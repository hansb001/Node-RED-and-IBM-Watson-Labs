# Friday Coffee & Code: Develop an IoT app with Watson & Node Red, June 1, 2018

In this workshop, you learn the basics of using IBM Watson API's in Node-RED

This workshop is originally made by Emma Dawson and Soheel Chughtai for [INDEX 2018] (https://github.com/watson-developer-cloud/node-red-labs/tree/master/index_2018)


All the Watson API's are available on Node-RED. In this session I will show you how you can make use of the full capabilities of the API's to create cognitive models and quickly make use of them in real applications with little coding.

## The workshop

In this workshop I will guide you as you build a web application making use of several Watson API's. You will start by constructing basic components, which you will then assemble into parts, which you will then assemble into an application. We start simple and gradually build a more intricate flow. 

## Getting Started
You will need and instance of Node-RED to start this workshop. This can be an instance running on IBM Cloud, or on your own machine, or on any other platform that Node-RED supports, for example a Rasberry Pi.
I will demonstrate how to create a Node-RED instance on IBM Cloud

You will also need credentials for Watson services, which you can obtain using your IBM Cloud account. Because of time constraints, I will provide the credentials. You can find them [here](https://github.com/hansb001/Node-RED-and-IBM-Watson-Labs/blob/master/credentials). These credentials will only be good for the day of the workshop, and only given to those attending the workshop.

### Node-RED
If you don't have one already, you will need an instance of Node-RED.

#### IBM cloud
If you are going to be using IBM Cloud, then create either an instance of [The Node-RED Starter](https://ibm.biz/BdZC7L). I will demonstrate this.

#### (Optional) Your own machine
If you are going to be running on your own machine, then let's do this properly.
1. Install nvm
2. Use nvm to install Node.js (Verson 6 a minimum)
3. Use nvm to set the Node.js Version
4. `npm install node-red`
5. `npm start`

#### Watson Nodes
You are going to be making use of the following Watson services.
* Translation
* Speech To Text
* Text To Speech
* Visual Recognition
* Assistant (Conversation)

If you are running on Node-RED, feel free to bind in those services into your instance. For the workshop you don't have to provision them, I created those services and the credentials can be found here

If you have Node-RED running on your own machine, then use `Manage Palette` to install the nodes.

#### Utility Nodes
You will need some additional nodes in this workshop. Use `Manage Palette` to install.
* node-red-contrib-browser-utils
* node-red-contrib-play-audio
* node-red-contrib-file-buffer
* node-red-contrib-http-multipart
* node-red-contrib-extract-keyframes

## Components
You are now ready to build the basic components that you will be using in your application.

### Node-RED Hello World
Take an inject node and wire it into a debug node. Configure the Inject node to send a string, and see it appear in the debug panel

### Translation
Add a Watson Translator node in between the inject and debug node. Configure the Translator node to use neural translation and pick a language. Trigger the inject to see the translation.

You can use the flows on the [language translator basic example]https://github.com/watson-developer-cloud/node-red-labs/blob/master/basic_examples/language_translator/README.md as a guide.

It is possible to dynamically configure the Watson Translation node, by setting `msg.srclang` and `msg.destlang`. Node-RED [function nodes](https://nodered.org/docs/writing-functions) allow you to add snippets of javascript into the flow. Add a function node upstream of the Translation node and set the source and destination language in the function node.

You can also use Node-RED [Context](https://nodered.org/docs/writing-functions#storing-data) to remember settings. Use a separate flow to set the translation language. Modify your translation flow to use the global context to determine source and destination languages for the translation. Try it out.

The neural translation models all use English, but you can wire two translation nodes to go from for example *Japanese* to *French*. Wire two translation nodes to use English as a hop. Use the global context to determine how your translation initial source and final destination. You will need to add a function node between the two translation nodes.

### Speech to Text
Create a new Node-RED tab. Drop a microphone node, delay node and play audio node onto the flow canvas. Wire the microphone to the delay to the play audio. Configure the delay node to delay for 5 seconds. Trigger the microphone node, and hear yourself speaking. This will gauge the quality of your microphone. If it is undecipherable then use a better quality microphone. In most cases the microphone of the laptop is sufficient.

Add a Watson Speech to Text node to your flow. Wire the microphone to the Speech to Text node to a debug node. Configure the Speech to Text node for your language. Trigger the microphone and check the transcription.

The Speech to Text node can be dynamically configured by setting `msg.srclang`. Use a function node and the global context to switch the language setting for the Speech to Text node.

Wire the `Speech To Text` component to the `Translation` component to get a translation.

### Text to Speech
Create a new Node-RED tab. Drop an inject node, Watson Text to Speech node and a play audio node onto the flow canvas. Wire the inject node to the Text to Speech node to the play audio node. Configure the Text to Speech for language and voice. Configure the inject node to send a string. Trigger the inject node to hear the text.

The Text to Speech node can be dynamically configured by setting msg.voice. 

Wire the `Translation` component to the `Text To Speech` component so that you can now hear the translation.

### Visual Recognition
Create a new Node-RED tab. Drop a camera node, Watson Visual Recognition and a debug node on to the flow canvas. Wire the camera node to the Visual Recognition node to the debug node. Trigger the camera to take a selfie and see the output in the debug node.

### Assistant (Conversation)
For this you will need a Assistant workspace. This is a demo workspace around Customer service. For instance, you can ask questions around opening hours and directions to the shop. 

Create a new Node-RED tab. Drop an inject node, Watson Assistant node and a debug node onto the flow canvas. Configure the Watson Conversation node for the Conversation Workspace. 

Use the inject node to send in text, and the debug node to see the response.

Connect the `Speech To Text` component into the `Assistant` component to allow you to speak to your Conversation. Connect the `Text to Speech` component to hear the response from the Conversation. Connect the `Translation` component to allow you to converse with the service in another language.

Add the `Visual Recognition` component to recognise you and start the conversation accordingly.

This is now your `Working Assistant` part.

## HTML Components
Upto now your application has been confined to the Node-RED flow editor. You will now build components that will make your application available as a web page.

### HTML Hello World
Create a new Node-RED tab. Drop a HTTP In node, a Template node and a HTTP Out node on to the flow canvas. Wire the HTTP In node to the Template node to the HTTP Out node. Configure the HTTP In node as a `GET`. Edit the Template node to for a basic *hello world* HTML template.

Open a new browser tab and test your *hello world* web page.

If there is time left you can have a look at these [basic samples]https://github.com/watson-developer-cloud/node-red-labs/tree/master/basic_examples around all the Watson Api's and here are more [advanced labs]https://github.com/watson-developer-cloud/node-red-labs/tree/master/advanced_examples where different Watson API's are combined to make more advanced applications.
