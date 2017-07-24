---

copyright:
  years: 2015, 2017
lastupdated: "2017-07-24"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Building a client application

So you have a working dialog. Now you want to develop the application that will interact with your users and communicate with the {{site.data.keyword.conversationfull}} service.
{: shortdesc}

The code examples in this tutorial are written in JavaScript using the Node.js Watson SDK, but you can use other languages. Install the Watson [SDK ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/developer-tools.html){: new_window} for your programming language and review the SDK documentation for more information.
{: tip}

## Setting up the Conversation service

The example application we will create in this section implements several functions of a cognitive personal assistant. The application code will connect to a {{site.data.keyword.conversationshort}} workspace, where the cognitive processing (such as the detection of user intents) takes place.

Before continuing with this example, you need to set up the required {{site.data.keyword.conversationshort}} workspace:

1.  Download the workspace <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/conversation/conversation-simple-example.json" download="conversation-simple-example.json">JSON file</a>.
1.  [Import the workspace](configure-workspace.html#creating-workspaces) into an instance of the {{site.data.keyword.conversationshort}} service.

## Getting service information

To access the {{site.data.keyword.conversationshort}} service REST APIs, your application needs to be able to authenticate with {{site.data.keyword.Bluemix}} and connect to the right {{site.data.keyword.conversationshort}} workspace. You'll need to copy the service credentials and workspace ID and paste them into your application code.

You can access the service credentials and the workspace ID from your workspace by selecting the ![Menu](images/Menu_16.png) menu and choosing **Credentials**.

You can also access the service credentials from your Bluemix dashboard.

## Communicating with the Conversation service

Interacting with the {{site.data.keyword.conversationshort}} service is simple. Let's take a look at a Node.js example that connects to the service, sends a single message, and logs the output to the console:

```javascript
// Example 1: sets up service wrapper, sends initial message, and
// receives response.

var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Set up Conversation service wrapper.
var conversation = new ConversationV1({
  username: 'USERNAME', // replace with username from service key
  password: 'PASSWORD', // replace with password from service key
  path: { workspace_id: 'WORKSPACE_ID' }, // replace with workspace ID
  version_date: '2016-07-11'
});

// Start conversation with empty message.
conversation.message({}, processResponse);

// Process the conversation response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  // Display the output from dialog, if any.
  if (response.output.text.length != 0) {
      console.log(response.output.text[0]);
  }
}
```
{: codeblock}

The first step is to create a wrapper for the {{site.data.keyword.conversationshort}} service.

The wrapper is an object you will use to send input to, and receive output from, the service. When you create the service wrapper, specify the authentication credentials from the service key, as well as the version of the {{site.data.keyword.conversationshort}} API you are using.

In this Node.js example, the wrapper is an instance of `ConversationV1`, stored in the variable `conversation`. The Watson SDKs for other languages provide equivalent mechanisms for instantiating a service wrapper.

After creating the service wrapper, we use it to send a message to the {{site.data.keyword.conversationshort}} service. In this example, the message is empty; we just want to trigger the conversation_start node in the dialog, so we don't need any input text.

Use the `node <filename.js>` command to run the example application.

**Note:** Make sure you have installed the Node.js Watson SDK using `npm install watson-developer-cloud`.

Assuming everything works as expected, the {{site.data.keyword.conversationshort}} service returns the output from the dialog, which is then logged to the console:

```
Welcome to the Conversation example!
```
{: screen}

This output tells us that we have successfully communicated with the {{site.data.keyword.conversationshort}} service and received the welcome message specified by the conversation_start node in the dialog. Now we can add a user interface, making it possible to process user input.

## Processing user input to detect intents

To be able to process user input, we need to add a user interface to our application. For this example, we'll keep things simple and use standard input and output. We can use the Node.js prompt-sync module to do this. (You can install prompt-sync using `npm install prompt-sync`.)

```javascript
// Example 2: adds user input and detects intents.

var prompt = require('prompt-sync')();
var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Set up Conversation service wrapper.
var conversation = new ConversationV1({
  username: 'USERNAME', // replace with username from service key
  password: 'PASSWORD', // replace with password from service key
  path: { workspace_id: 'WORKSPACE_ID' }, // replace with workspace ID
  version_date: '2016-07-11'
});

// Start conversation with empty message.
conversation.message({}, processResponse);

// Process the conversation response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  // If an intent was detected, log it out to the console.
  if (response.intents.length > 0) {
    console.log('Detected intent: #' + response.intents[0].intent);
  }

  // Display the output from dialog, if any.
  if (response.output.text.length != 0) {
      console.log(response.output.text[0]);
  }

  // Prompt for the next round of input.
  var newMessageFromUser = prompt('>> ');
  conversation.message({
    input: { text: newMessageFromUser }
    }, processResponse)
}
```
{: codeblock}

This version of the application begins the same way as before: sending an empty message to the {{site.data.keyword.conversationshort}} service to start the conversation.

The `processResponse()` function now displays any intent detected by the dialog along with the output text, and then it then prompts for the next round of user input.

But something still isn't right:

```
Welcome to the Conversation example!
>> hello
Detected intent: #hello
Welcome to the Conversation example!
>> what time is it?
Detected intent: #time
Welcome to the Conversation example!
>> goodbye
Detected intent: #goodbye
Welcome to the Conversation example!
>>
```
{: screen}

The {{site.data.keyword.conversationshort}} service is detecting the correct intents, and yet every turn of the conversation returns the welcome message from the conversation_start node (`Welcome to the Conversation example!`).

This is happening because the {{site.data.keyword.conversationshort}} service is stateless; it is the responsibility of the application to maintain state information. Because we are not yet doing anything to maintain state, the {{site.data.keyword.conversationshort}} service sees every round of user input as the first turn of a new conversation, triggering the conversation_start node.

## Maintaining state

State information for your conversation is maintained using the *context*. The context is a JSON object that is passed back and forth between your application and the {{site.data.keyword.conversationshort}} service. It is the responsibility of your application to maintain the context from one turn of the conversation to the next.

The context includes a unique identifier for each conversation with a user, as well as a counter that is incremented with each turn of the conversation. Our previous version of the example did not preserve the context, which means that each round of input appeared to be the start of a new conversation. We can fix that by saving the context and sending it back to the {{site.data.keyword.conversationshort}} service each time.

In addition to maintaining our place in the conversation, the context can also be used to store any other data you want to pass back and forth between your application and the {{site.data.keyword.conversationshort}} service. This can include persistent data you want to maintain throughout the conversation (such as a customer's name or account number), or any other data you want to track (such as the current status of option settings).

```javascript
// Example 3: maintains state.

var prompt = require('prompt-sync')();
var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Set up Conversation service.
var conversation = new ConversationV1({
  username: 'USERNAME', // replace with username from service key
  password: 'PASSWORD', // replace with password from service key
  path: { workspace_id: 'WORKSPACE_ID' }, // replace with workspace ID
  version_date: '2016-07-11'
});

// Start conversation with empty message.
conversation.message({}, processResponse);

// Process the conversation response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  // If an intent was detected, log it out to the console.
  if (response.intents.length > 0) {
    console.log('Detected intent: #' + response.intents[0].intent);
  }

  // Display the output from dialog, if any.
  if (response.output.text.length != 0) {
      console.log(response.output.text[0]);
  }

  // Prompt for the next round of input.
    var newMessageFromUser = prompt('>> ');
    // Send back the context to maintain state.
    conversation.message({
      input: { text: newMessageFromUser },
      context : response.context,
    }, processResponse)
}
```
{: codeblock}

The only change from the previous example is that with each round of the conversation, we now send back the `response.context` object we received in the previous round:

```javascript
    conversation.message({
      input: { text: newMessageFromUser },
      context : response.context,
    }, processResponse)
```
{: codeblock}

This ensures that the context is maintained from one turn to the next, so the {{site.data.keyword.conversationshort}} service no longer thinks every turn is the first:

```
>> hello
Detected intent: #hello
Good day to you.
>> what time is it?
Detected intent: #time
>> goodbye
Detected intent: #goodbye
OK! See you later.
>>
```
{: screen}

Now we're making progress! The {{site.data.keyword.conversationshort}} service is correctly recognizing our intents, and the dialog is returning the correct output text (where provided) for each intent.

However, nothing else is happening. When we ask for the time, we get no answer; and when we say goodbye, the conversation does not end. That's because those intents require additional actions to be taken by the app.

## Implementing app actions

In addition to the output text to be displayed to the user, our dialog uses the `output` object in the response JSON to signal when the application needs to carry out an action, based on the detected intents.

These action flags are sent using the `action` property, which our dialog defines as part of the response JSON. When the dialog determines that the application needs to do something, it sets the value of `action` to the appropriate value, either `display_time` or `end_conversation`.

Keep in mind that `output` is just a JSON object, and you can add any valid content to it that you want. For a more complex application, you might use an array with multiple action flags.

But in our example, we're using a simple key/value pair that supports a single action flag. Our application code needs to check the value of the `action` property in the response and then carry out any specified action. (This version also removes the display of detected intents, now that we're sure those are being correctly identified.)

```javascript
// Example 4: implements app actions.

var prompt = require('prompt-sync')();
var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Set up Conversation service.
var conversation = new ConversationV1({
  username: 'USERNAME', // replace with username from service key
  password: 'PASSWORD', // replace with password from service key
  path: { workspace_id: 'WORKSPACE_ID' }, // replace with workspace ID
  version_date: '2016-07-11'
});

// Start conversation with empty message.
conversation.message({}, processResponse);

// Process the conversation response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  var endConversation = false;

  // Check for action flags.
  if (response.output.action === 'display_time') {
    // User asked what time it is, so we output the local system time.
    console.log('The current time is ' + new Date().toLocaleTimeString());
  } else if (response.output.action === 'end_conversation') {
    // User said goodbye, so we're done.
    console.log(response.output.text[0]);
    endConversation = true;
  } else {
    // Display the output from dialog, if any.
    if (response.output.text.length != 0) {
        console.log(response.output.text[0]);
    }
  }

  // If we're not done, prompt for the next round of input.
  if (!endConversation) {
    var newMessageFromUser = prompt('>> ');
    conversation.message({
      input: { text: newMessageFromUser },
      // Send back the context to maintain state.
      context : response.context,
    }, processResponse)
  }
}
```
{: codeblock}

The processResponse() function now checks the value of the `action` property of the `output` object received from the {{site.data.keyword.conversationshort}} service. If the value is either `display_time` or `end_conversation`, the application carries out the appropriate action.

```
Welcome to the Conversation example!
>> hello
Good day to you.
>> what time is it?
The current time is 12:40:42 PM.
>> goodbye
OK! See you later.
```
{: screen}

Success! The application now uses the {{site.data.keyword.conversationshort}} service to identify the intents in natural-language input, displays the appropriate responses, and implements the requested actions.

Of course, a real-world application would use a more sophisticated user interface, such as a web chat window. And it would implement more complex actions, possibly integrating with a customer database or other business systems. But the basic principles of how the application interacts with the {{site.data.keyword.conversationshort}} service would remain the same.

For some more complex examples, take a look at the Sample apps in the Navigation pane.
