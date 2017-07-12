---

copyright:
  years: 2015, 2017
lastupdated: "2017-07-11"

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

# Tutorial
{: #tutorial}

In this tutorial, you will use the {{site.data.keyword.conversationshort}} service to create a dialog that helps users interact with a cognitive car dashboard.
{: shortdesc}

## Duration
Steps 1-6 of this tutorial will take approximately 1 to 2 hours to complete.
Step 7 might take many hours to complete, depending on the method you choose to use to deploy the workspace.

## Step 1: Create the service
{: #login}

To begin the tutorial, you need to create a new instance of the {{site.data.keyword.conversationshort}} service and launch the tool.

1.  Log in to {{site.data.keyword.Bluemix}} and navigate to the {{site.data.keyword.conversationshort}} service on {{site.data.keyword.Bluemix}}:
    - Don't have a {{site.data.keyword.Bluemix_notm}} account? [Start free ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.ng.bluemix.net/registration/?target=/catalog/services/conversation/){: new_window}.
    - Have a {{site.data.keyword.Bluemix_notm}} account? [Log in ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.ng.bluemix.net/catalog/services/conversation){: new_window}.
1.  In the **Service name** field, specify `Conversation-tutorial`.
1.  Click **Create**.
1.  On the "Service Details" page, click **Launch tool**.

    ![Launch tool](images/launch_tool.png)

## Step 2: Create a workspace
{: #workspace}

A *workspace* is a container for all the artifacts that define the behavior of your service.

1.  On the Create workspace page, click **Create**.
1.  In the **Name** field, type `Car tutorial`.
1.  Select **Create**. The new workspace is displayed as a tile on your dashboard.

## Step 3: Add intents and examples
{: #intents}

Add some intents on the Intents tab. An intent is the purpose or goal expressed in user input.

1.  On the Intents page of the Car tutorial workspace, click **Create new**.
1.  Add the following intent name, and then press Enter:

    ```
    turn_on
    ```
    {: codeblock}

    A `#` is prepended to the intent name you specify. The `#turn_on` intent indicates that the user wants to turn on an appliance such as the radio, windshield wipers, or headlights.
1.  In the **User example** field, type the following utterance, and then press Enter:

    ```
    I need lights
    ```
    {: codeblock}

1.  Add these 5 more examples to help Watson recognize the `#turn_on` intent.

    ```
    Play some tunes
    Turn on the radio
    turn on
    Air on please
    Crank up the AC
    Turn on the headlights
    ```
    {: codeblock}

1.  Click **Done** to add the intent.

1.  Repeat steps 2-5 to create a `#greeting` intent with these utterances:

    ```
    Hello
    Hi
    Good morning
    Good afternoon
    Good evening
    Howdy
    ```
    {: codeblock}

You defined two intents, `#turn_on` and `#greeting`, with example utterances. These examples help train Watson to recognize the intents in user input.

## Step 4: Add entities
{: #entities}

An entity definition includes a set of entity *values* that can be used to trigger different responses. Each entity value can have multiple *synonyms*, which define different ways that the same value might be specified in user input.

Create entities that might occur in user input that has the #turn_on intent to represent what the user wants to turn on.

1.  Click the **Entities** tab to open the Entities page.
1.  Click **Create new**.
1.  Add the following entity name, and then press Enter:

    ```
    appliance
    ```
    {: codeblock}

    A `@` is prepended to the entity name you specify. The `@appliance` entity represents an appliance in the car that a user might want to turn on.
1.  Click the toggle to turn fuzzy matching **On**.
    This setting helps the service recognize references to entities in user input even when the entity is specified in a way that does not exactly match the syntax you use here.
1.  Add the following value to the **Value** field, but do not press Enter:

    ```
    radio
    ```
    {: codeblock}

    The value represents a specific appliance that users might want to turn on.
1.  Add other ways to specify the radio appliance entity in the **Synonyms** field. Press tab to give the the field focus, and then enter the following synonyms. Press Enter after each synonym.

    ```
    music
    tunes
    ```
    {: codeblock}

1.  Click the **Add a new value** icon ![plus sign](images/add.png) to add other types of appliances.
    - Value: `headlights`. Synonym: `lights`.
    - Value: `air conditioning`. Synonyms: `air` and `AC`.
1.  Click **Done** to add the **@appliance** entity.
1.  Repeat Steps 2-8 to create the @`genre` entity with these values and synonyms:
    - Value: `classical`. Synonym: `symphonic`.
    - Value: `rhythm and blues` Synonym: `r&b`.
    - Value: `rock`. Synonym: `rock & roll`, `rock and roll`, and `pop`.

You defined two entities: `@appliance` (representing an appliance the bot can turn on) and `@genre` (representing a genre of music the user can choose to listen to).

When the user's input is received, the {{site.data.keyword.conversationshort}} service identifies both the intents and entities. You can now define a dialog that uses intents and entities to choose the correct response.

## Step 5: Create a simple dialog
{: #dialog}

A dialog is a set of conversational nodes that are contained in a workspace. Together the set of nodes makes a dialog tree, on which every branch is a conversation that can be had with a user.

### Start the dialog
Create a dialog and add a node:

1.  On the Car tutorial workspace page, click the **Dialog** tab.
1.  Click **Create**. A dialog is created with the following nodes:
    - **Welcome**: Contains a greeting that is displayed to your users when they first engage with the bot. You can edit the greeting.
    - **Anything else**: Contains phrases that are used to reply to users when their input is not recognized. You can replace the responses that are provided or add more responses with a similar meaning to the list.
1.  Click the **Welcome** node to open it in edit view.
1.  Replace the existing response (Hello. How can I help you?) with the text, `Welcome to the car tutorial!`
1.  Click ![Close](images/close.png) to close the node edit view.
1.  Click the More icon ![More options](images/kabob.png) on the **Welcome** node, and select **Add node below**.
1.  In the edit view, specify the condition and response for this node of the conversation.

    1.  In the condition field, start typing `#greeting`, and then select it from the list.

    1.  In the response field, enter the following text:

        ```
        Hi, there. What can I do for you?
        ```
        {: codeblock}

        Your bot will show this response when the specified condition is true (in this case, when someone greets the bot).

1.  Click ![Close](images/close.png) to close the node edit view.

    The tree view now shows the node you added.

    ![Dialog nodes](images/tut-dialog-greeting.png)

### Test the simple dialog

1.  Select the ![Ask Watson](images/ask_watson.png) icon.
    In the chat pane, you should see the bot welcome message, `Welcome to the car tutorial!`.
1.  Type a greeting, such as "Hi", and then press Enter.
    The output shows that the #greeting intent is recognized, and the appropriate response is displayed.

    ![Shows a greeting and response in Try it out pane](images/tut-first-chat.png)

    Congratulations! You have had a successful first chat with your bot.

1.  Close the chat pane.

## Step 6: Create a complex dialog
{: #complex-dialog}

In this complex dialog, you will create dialog branches that handle the #turn_on intent you defined earlier.

### Add a root-level node for #turn_on
Create a dialog branch to respond to the #turn_on intent. Start by creating the root-level node:

1.  Click the More icon ![More options](images/kabob.png) on the **#greeting** node, and then select **Add node below**.
1.  Start typing `#turn_on` in the condition field, and then select it from the list.
    This condition is triggered by any input that matches the #turn_on intent.
1.  Do not enter a response in this node. Click ![Close](images/close.png) to close the node edit view.

### Scenarios
The dialog needs to determine which appliance the user wants to turn on. To handle this, create multiple responses based on additional conditions.

There are three possible scenarios, based on the intents and entities that you defined:

**Scenario 1**: The user wants to turn on the music, in which case the bot must ask for the genre.

**Scenario 2**: The user wants to turn on any other valid appliance, in which case the bot echos the name of the requested appliance in a message that indicates it is being turned on.

**Scenario 3**: The user does not specify a recognizable appliance name, in which case the bot must ask for clarification.

Add nodes that check these scenario conditions in this order so the dialog evaluates the most specific condition first.

### Address Scenario 1

Add nodes that address scenario 1, which is that the user wants to turn on the music. In response, the bot must ask for the music genre.

#### Add a child node that checks whether the appliance type is music

1.  Click the More icon ![More options](images/kabob.png) on the **#turn_on** node, and select **Add child node**.
1.  In the condition field, enter `@appliance:radio`.
    This condition is true if the value of the @appliance entity is `radio` or one of its synonyms, as defined on the Entities tab.
1.  In the response field, enter `What kind of music would you like to hear?`
1.  Name the node `Music`.
1.  Click ![Close](images/close.png) to close the node edit view.

#### Add a jump from the #turn_on node to the Music node

Jump directly from the `#turn on` node to the `Music` node without asking for any more user input. To do this, you can use a **Jump to** action.

1.  Click the More icon ![More options](images/kabob.png) on the **#turn_on** node, and select **Jump to**.
1.  Select the **Music** child node, and then select **If bot recognizes (condition)** to indicate that you want to process the condition of the Music node.

![Jump to before](images/tut-dialog-jumpto.png)

Note that you had to create the target node (the node to which you want to jump) before you added the **Jump to** action.

After you create the Jump to relationship, you see a new entry in the tree:

![Jump to after](images/tut-dialog-jump2.png)

#### Add a child node that checks the music genre

Now add a node to process the type of music that the user requests.

1.  Click the More icon ![More options](images/kabob.png) on the **Music** node, and select **Add child node**.
    This child node is evaluated only after the user has responded to the question about the type of music they want to hear. Because we need a user input before this node, there is no need to use a **Jump to** action.
1.  Add `@genre` to the condition field.  This condition is true whenever a valid value for the @genre entity is detected.
1.  Enter `OK! Playing @genre.` as the response. This response reiterates the genre value that the user provides.

#### Add a node that handles unrecognized genre types in user responses

Add a node to respond when the user does not specify a recognized value for @genre.

1.  Click the More icon ![More options](images/kabob.png) on the *@genre* node, and select **Add node below** to create a peer node.
1.  Enter `true` in the condition field, and then select **true (add new condition)** from the list.
    This condition specifies that if the dialog flow reaches this node, it should always evaluate as true. (If the user specifies a valid @genre value, this node will never be reached.)
1.  Enter `I'm sorry, I don't understand. I can play classical, rhythm and blues, or rock music.` as the response.

That takes care of all the cases where the user asks to turn on the music.

#### Test the dialog for music

1.  Select the ![Ask Watson](images/ask_watson.png) icon to open the chat pane.
1.  Type `Play music`.
    The bot recognizes the #turn_on intent and the @appliance:music entity, and it responds by asking for a musical genre.

1.  Type a valid @genre value (for example, `rock`).
    The bot recognizes the @genre entity and responds appropriately.

    ![Shows a successful request to play music](images/tut-test-music.png)

1.  Type `Play music` again, but this time specify an invalid response for the genre. The bot responds that it does not understand.

### Address Scenario 2

We will add nodes that address scenario 2, which is that the user wants to turn on another valid appliance. In this case, the bot echos the name of the requested appliance in a message that indicates it is being turned on.

#### Add a child node that checks for any appliance

Add a node that is triggered when any other valid value for @appliance is provided by the user.
For the other values of @appliance, the bot doesn't need to ask for any more input. It just returns a positive response.

1.  Click the More icon ![More options](images/kabob.png) on the **Music** node, and then select **Add node below** to create a peer node that is evaluated after the @appliance:music condition is evaluated.
1.  Enter `@appliance` as the node condition.
    This condition is triggered if the user input includes any recognized value for the @appliance entity besides music.
1.  Enter `OK! Turning on the @appliance.` as the response.
    This response reiterates the appliance value that the user provided.

#### Test the dialog with other appliances

1.  Select the ![Ask Watson](images/ask_watson.png) icon to open the chat pane.
1.  Type `lights on`.

    The bot recognizes the #turn_on intent and the @appliance:headlights entity, and it responds with `OK, turning on the headlights`.

    ![Shows a successful request to turn on the lights](images/tut-test-lights.png)

1.  Type `turn on the air`.

    The bot recognizes the #turn_on intent and the @appliance:(air conditioning) entity, and it responds with `OK, turning on the air conditioning.`

1.  Try variations on all of the supported commands based on the example utterances and entity synonyms you defined.

### Address Scenario 3

Now add a peer node that is triggered if the user does not specify a valid appliance type.

1.  Click the More icon ![More options](images/kabob.png) on the **@appliance** node, and then select **Add node below** to create a peer node that is evaluated after the @appliance condition is evaluated.
1.  Enter `true` in the condition field, and then select **true (add new condition)** from the list.
    This condition specifies that if the dialog flow reaches this node, it should always evaluate as true. (If the user specifies a valid @appliance value, this node will never be reached.)
1.  Enter `I'm sorry, I'm not sure I understood you. I can turn on music, headlights, or air conditioning.` as the response.

#### Test some more

1.  Try more utterance variations to test the dialog.

    If the bot fails to recognize the correct intent, you can retrain it directly from the chat window. Select the arrow next to the incorrect intent and choose the correct one from the list.

    ![Shows choosing a different intent and retraining](images/tut-change-intent.gif)

#### What to do next

Optionally review the **Car Dashboard - Sample** workspace to see this same use case fleshed out even more with a longer dialog and additional functionality.

1.  Click the **Back to workspaces** button ![Shows Back to workspaces button in menu](images/workspaces-button.png) from the navigation menu.

1.  On the **Car Dashboard - Sample** tile, click **Edit sample**.

## Step 7: Deploy the tutorial workspace
{: #deploy}

Now that you have built and tested your workspace, you can deploy it by connecting it to a user interface. There are several ways you can do this.

### Test in Slack

You can use the test deployment tool to [deploy your workspace](test-deploy.html) as a chat bot in a Slack channel in just a few steps. This option is the fastest and easiest way to deploy your workspace for testing, but it has limitations.

### Build your own front-end application

You can use the Watson SDKs to [build your own](develop-app.html) front-end application that connects to your workspace using the Conversation REST API.

### Deploy to social media or messaging channels

You can use the [Botkit framework](integrations.html) to build a bot application that you can integrate with social media and messaging channels such as Slack, Facebook, and Twilio.
