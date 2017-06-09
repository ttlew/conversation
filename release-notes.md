---

copyright:
  years: 2015, 2017
lastupdated: "2017-06-09"

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

# Release notes

### Service API Versioning
{:shortdesc}

API requests require a version parameter that takes a date in the format `version=YYYY-MM-DD`. Whenever we change the API in a backwards-incompatible way, we release a new minor version of the API.

Send the version parameter with every API request. The service uses the API version for the date you specify, or the most recent version before that date. Don't default to the current date. Instead, specify a date that matches a version that is compatible with your app, and don't change it until your app is ready for a later version.

The current version is `2017-05-26`.

### Beta features

IBM will release services, features, and language support that are classified as in beta (formerly _experimental_). These services may be unstable, may change frequently, and may be discontinued with short notice. Beta features will be supported via our Bluemix forum only.

### The following new features and changes to the service are available.

## 6 June 2017

- **Learn** - A new *Learn about Watson Conversation* page is available that provides getting started information and links to service documentation and other useful resources. To open the page, click the ![i for information.](images/info.png) icon in the page header.

- **Bulk export and delete** - You can now simultaneously export a number of intents or entities to a CSV file, so you can then import and reuse them for another Conversation application. You can also simultaneously select a number of entities or intents for deletion in bulk.

- **Updates to Korean** - Korean tokenizers have been updated to address informal language support. IBM continues to work on improvements to entity recognition and classification.

- **Workspace language** - You can no longer change the language of a workspace after you create it by editing the workspace details. If you need to change the language, you can export the workspace as a JSON file, update the language property, and then import the JSON file as a new workspace.

- **Emoji support** - Emojis added to intent examples, or as entity values, will now be correctly classified/extracted. **Note**: Only emojis that are included in your training data will be correctly and consistently identified; emoji support may not correctly classify similar emojis with different color tones or other variations.

- **Entity stemming (Beta - English only)** - The fuzzy matching beta feature recognizes entities and matches them based on the stem form of the entity value. For example, this feature correctly recognizes 'bananas' as being similar to 'banana', and 'run' being similar to 'running' as they share a common stem form. For more information, see [Fuzzy matching](entities.html#fuzzy-matching).

- **Workspace import progress** - When you import a workspace from a JSON file, a tile for the workspace is displayed immediately, in which information about the progress of the import is displayed.

- **Reduced training time** - Multiple models are now trained in parallel, which noticeably reduces the training time for large workspaces.

## 26 May 2017

- The current API version is now `2017-05-26`. This version introduces the following changes:

    - The schema of ErrorResponse objects has changed. This change affects all endpoints and methods. For more information, see the [API Reference](../../conversation/api/v1/).

    - The internal schema used to represent dialog nodes in exported workspace JSON has changed. If you use the `2017-05-26` API to import a workspace that was exported using an earlier version, some dialog nodes might not import correctly. For best results, always import a workspace using the same version that was used to export it.

## 25 May 2017

- You can now manage context variables in the "Try it out" pane. Click the **Manage context** link to open a new pane where you can set and check the values of context variables as you test the dialog. See [Testing your dialog](dialog-test.html) for more information.

## 16 May 2017

- A **Car Dashboard** sample workspace is now available when you open the tool. To use the sample as a starting point for your own workspace, edit the workspace. If you want to use it for multiple workspaces, then duplicate it instead. The sample workspace does not count toward your subscription workspace total unless you use it.

- It is now easier to navigate the tool. The navigation menu options are available from the side of the main page instead of the top. At the top of the page, Breadcrumb links display that show you where you are. You can now switch between service instances from the Workspaces page. To get there quickly, click **Back to workspaces** from the navigation menu. If you have multiple service instances, the name of the current instance is displayed. You can click the **Change** link beside it to choose another instance.

- When you create a dialog, two nodes are now added to it for you: 1) a **Welcome** node at the top of the dialog tree that contains the greeting to display to the user and 2) an **Anything else** node at the bottom of the tree that catches any user inquiries that are not recognized by other nodes in the dialog and responds to them. See [Creating a dialog ](dialog-create.html) for more details.
  
- When you are testing a dialog in the "Try it out" pane, you can now find and resubmit a recent test utterance by pressing the Up key to cycle through your previous inputs.

- Experimental Korean language support for 5 system entities (`@sys-date`, `@sys-time`, `@sys-currency`, `@sys-number`, `@sys-percentage`) is now available. There are known issues for some of the numeric entities, and limited support for informal language input.

- An Overview page is available from the Improve tab. The page provides a summary of interactions with your bot. You can view the amount of traffic for a given time period, as well as the intents and entities that were recognized most often in user conversations. For additional information, see [Using the Overview page](logs.html#using-the-overview-page).

## 27 April 2017

- The following system entities are now available as beta features in English only:

  - sys-location: Recognizes references to locations, such as towns, cities, and countries, in user utterances. 
  - sys-person: Recognizes references to people's names, first and last, in user utterances. 

  For more information, see the [System entities reference](system-entities.html).
  
- Fuzzy matching for entities is a beta feature that is now available in English. You can turn on fuzzy matching per entity to improve the ability of the service to recognize terms in user input with syntax that is similar to the entity, without requiring an exact match. The feature is able to map user input to the appropriate corresponding entity despite the presence of misspellings or slight syntactical differences. For examples, if you define **giraffe** as a synonym for an animal entity, and the user input contains the terms *giraffes* or *girafe*, the fuzzy match is able to map the term to the animal entity correctly. See [Planning your entities](entities.html#planning-your-entities) for details. 

## 18 April 2017

- The Conversation REST API now supports access to the following resources:

  - entities
  - entity values
  - entity value synonyms
  - logs

  For more information, see the [API Reference](https://www.ibm.com/watson/developercloud/conversation/api/v1/).

- The behavior of the /messages `POST` method has changed the handling of entities and intents specified as part of the message input:

  - If you specify intents on input, the service uses the intents you specify, but uses natural language processing to detect entities in the user input.

  - If you specify entities on input, the service uses the entities you specify, but uses natural language processing to detect intents in the user input.

  The behavior has not changed for messages that specify both intents and entities, or for messages that specify neither.

- The option to mark user input as irrelevant is now available for all supported languages. This is a beta feature.

- A new Credentials tab provides a single place where you can find all of the information you need for connecting your application to a workspace (such as the service credentials and workspace ID), as well as other deployment options. To access the Credentials tab for your workspace, click the ![Menu](images/Menu_16.png) icon and select **Credentials**.

## 9 March 2017

The Conversation REST API now supports access to the following resources:

 - workspaces
 - intents
 - examples
 - counterexamples

For more information, see the [API Reference](https://www.ibm.com/watson/developercloud/conversation/api/v1/).

## 7 March 2017

- The use of `.` or `..` as an intent name causes problems and is no longer supported.
You cannot rename or delete an intent with this name; to change the name, export your intents to a file, rename the intent in the file, and import the updated file into your workspace.
Paying customers can contact support for a database change.

## 1 March 2017

- System entities are now enabled in German.

## 22 February 2017

- Messages are now limited to 2,048 characters.

## 3 February 2017

- In this release, we change how [intents are scored and add the ability to mark input as irrelvant to your application](irrelevant_utterance.html).

    - These features are available in the tooling by [upgrading your workspace](upgrading.html).
    - These features are available for your application by changing the message API call to use **2017-02-03.**

- The processing of **Jump to** actions has been changed to prevent loops that can occur under certain conditions.
See [Jump to actions](dialog-responses.html#jump-to-actions) for details.

## 11 January 2017

- In this release, you can customize node titles in dialog.

## 22 December 2016

- In this release, dialog nodes display a new section for `node title`. The ability to customize the `node title` is not available. When collapsed, the `node title` displays the `node condition` of the dialog node. If there is not a `node condition`, “Untitled Node” is displayed as the title.

## 19 December 2016

Several changes make the dialog builder easier and more intuitive to use:
* A larger editing view makes it easier to view all the details of a node as you work on it.
* A node can contain multiple responses, each triggered by a separate condition. For more information see [Multiple responses](dialog-responses.html#defining-a-response).

## 5 December 2016

- New languages are supported, all in Experimental mode: German, Traditional Chinese, Simplified Chinese, and Dutch.
- Two new system entities are available: @sys-date and @sys-time. For details, see [System entities](system-entities.html).

## 21 October 2016

- The {{site.data.keyword.conversationshort}} service now provides system entities, which are
    common entities that can be used across any use case. For details, see [Defining entities](entities.html) and search for "Enabling system entities".

- You can now view a history of conversations with users on the Improve page. You can use this  to understand your bot's behavior. For details, see [Improving accuracy](logs.html).

- You can now import entities from a comma-separated value (CSV) file, which helps with when you have a large number of entities. For details, see [Defining Entities](entities.html) and search for "Importing entities".

## 20 September 2016

**New version**: 2016-09-20

To take advantage of the changes in a new version, change the value of the `version` parameter to the new date. If you're not ready to update to this version, don't change your version date.

- version **2016-09-20**: `dialog_stack` changed from an array of strings to an array of JSON objects.

## 29 August 2016

-   You can move dialog nodes from one branch to another, as siblings or peers. For details, see [Building a dialog](dialog-build.html) and search for "Moving a dialog node".

- You can expand the JSON editor window.

- You can view chat logs of your bot's conversations to help you understand it's behavior. You can filter by intents, entities, date, and time. For details, see [Improving accuracy](logs.html)

## 11 July 2016

- This General Availability release enables you to work with entities and dialogs to create a fully functioning bot.

## 18 May 2016

- This Experimental release of the {{site.data.keyword.conversationshort}} introduces the user interface and enables you to work with workspaces, intents, and examples.
