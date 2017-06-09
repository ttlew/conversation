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

# Configuring a Conversation workspace

The natural-language processing for the {{site.data.keyword.conversationshort}} service happens inside a *workspace*, which is a container for all of the artifacts that define the conversation flow for an application.
{:shortdesc}

A single {{site.data.keyword.conversationshort}} service instance can contain multiple workspaces. A workspace contains the following types of artifacts:

- [**Intents**](intents.html): An *intent* represents the purpose of a user's input, such as a question about business locations or a bill payment. You define an intent for each type of user request you want your application to support. In the tool, the name of an intent is always prefixed with the `#` character. To train the workspace to recognize your intents, you supply lots of examples of user input and indicate which intents they map to.
- [**Entities**](entities.html); An *entity* represents a term or object that is relevant to your intents and that provides a specific context for an intent. For example, an entity might represent a city where the user wants to find a business location, or the amount of a bill payment. In the tool, the name of an entity is always prefixed with the `@` character. To train the workspace to recognize your entities, you list the possible values for each entity and synonyms that users might enter.
- [**Dialog**](dialog-build.html): A *dialog* is a branching conversation flow that defines how your application responds when it recognizes the defined intents and entities. You use the dialog builder in the tool to create conversations with users, providing responses based on the intents and entities that you recognize in their input.

As you add information, the workspace trains itself, so you don't have to do anything to initiate the training.

## Workspace limits

The number of workspaces you can create in a single service instance depends on your {{site.data.keyword.conversationshort}} service plan. The sample workspace does not count toward your workspace limit unless you edit or duplicate the sample:

| Service plan     | Workspaces per service instance |
|------------------|--------------------------------:|
| Standard/Premium |                              20 |
| Lite*            |                               5 |

*After 30 days of inactivity, an unused Lite workspace might be deleted to free up space.

## Creating workspaces

You can create a workspace from scratch, use the provided sample workspace, or import a workspace from a JSON file. You can also duplicate an existing workspace within the same service instance.

You use the {{site.data.keyword.conversationshort}} tool to create workspaces. Follow these steps to create a workspace:

1.  If the "Service Details" page is not already open, click your {{site.data.keyword.conversationshort}} service instance on the Bluemix console. (When you create a service instance, the "Service Details" page displays.)

1.  On the "Service Details" page, scroll down to **Conversation tooling** and click **Launch tool**.

1.  Do one of the following things:
    - To create a workspace from scratch, click **Create**.
    - To use the sample as a starting point for your own workspace, edit the sample workspace. If you want to use it for multiple workspaces, then duplicate it instead.
    - To import a workspace from a JSON file, click the ![Import workspace](images/workspace_import.png) icon, and select the JSON file you want to import from.

    > **Important**:
    >  - The imported JSON file must use UTF-8 encoding.
    >  - The maximum size for a workspace JSON file is 10MB. If you need to import a larger workspace, consider importing the intents and entities separately after you have imported the workspace. (You can also import larger workspaces using the REST API. For more information, see the [API Reference](../../conversation/api/v1/#create_workspace).)

    Specify the data you want to include:
        - Select **Everything (Intents, Entities, and Dialog)** if you want to import a complete copy of the exported workspace, including the dialog.
        - Select **Intents and Entities** if you want to use the intents and entities from the exported workspace, but you plan to build a new dialog.

1.  Specify the details for the new workspace:
    - **Name**: A name no more than 64 characters in length. This value is required.
    - **Description**: A description no more than 128 characters in length.
    - **Language**: The language of the user input the workspace will be trained to understand.

After you create the workspace, it appears as a tile on the Workspaces page.

## Exporting and copying workspaces

You can use the Workspaces page to export workspaces and to copy a workspace to a new workspace.

Exporting a workspace saves a copy of all workspace data in a JSON file. Use this option if you want to import the workspace into a different service instance or save a copy of the workspace data offline. When you import a workspace, you can choose to import only the intents and entities, which can be useful if you want to build a new dialog using the same training data.

Copying a workspace makes a complete copy of the workspace within the same service instance. Use this option if you want to adapt an existing workspace while preserving the original version.

- To export an existing workspace to a JSON file, click the menu icon in the workspace tile and then select **Download as JSON**:

    ![Screen capture showing Download as JSON menu choice](images/workspace_export.png)
- To create a copy of an existing workspace within the same service instance, click the menu icon in the workspace tile and then select **Duplicate**:

    ![Screen capture showing Duplicate menu choice](images/workspace_duplicate.png)

    Specify the name, description, and language for the new workspace. All data (including intents, entities, and dialog) is included in the copy.
