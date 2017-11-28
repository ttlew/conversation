---

copyright:
  years: 2015, 2017
lastupdated: "2017-11-28"

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

# About the Improve component

The Improve component of {{site.data.keyword.conversationshort}} provides a history of interactions with users of your workspace. You can use this history to improve your workspace's understanding of users' inputs.
{: shortdesc}

Sections of the **Improve** panel:

* [Overview](logs_oview.html): A summary of interactions of users with a workspace.
* [User conversations](logs_convo.html): A list of user utterances. You can update intents and entities while viewing a user utterance.
* [Recommendations](logs_recommend.html): Ways to improve your system. Available only to Premium users.

## Improving across workspaces
To understand how to use utterance data to make improvements across workspaces, it is helpful to review the following definitions associated with the {{site.data.keyword.conversationshort}} service:

* ***Instance:*** Your deployment of {{site.data.keyword.conversationshort}}, accessible with unique credentials. A {{site.data.keyword.conversationshort}} instance may be made up of multiple workspaces
* ***Workspace:*** A workspace is one model of your {{site.data.keyword.conversationshort}} content; often, this can equate to a bot
* ***Workspace ID:*** The unique identifier of a workspace, that allows you to call your content
* ***Deployment ID:*** Deployment IDs are unique labels that are passed with user utterances to help identify the deployment environment that the utterances come from
* ***Utterance:*** An utterance is a single message the user sends to the workspace

Creating a workspace is a very iterative process. While you develop your workspace, you use the *Try it out* pane to verify that your workspace recognizes the correct intents and entities in test inputs, and to make corrections as needed.

In the **Improve** panel, you can view information about actual interactions with your users, and make similar corrections to improve the accuracy with which intents and entities are recognized by your workspace. It is difficult to know exactly *how* your users will ask questions, or what random utterances they may make, so it is important to frequently visit the **Improve** panel, in order to improve your workspaces.

For a {{site.data.keyword.conversationshort}} instance that includes mutiple workspaces, there may be times when it is useful to use utterance data from one workspace to improve another workspace within that same instance.

As an example, say you have a {{site.data.keyword.conversationshort}} instance named *HelpDesk*. You may have both a Production workspace and a Development workspace in your HelpDesk instance. When working in the Development workspace, you can filter utterances based on the `Deployment ID` for the Production workspace, so that you are using Production workspace utterances to improve your Development workspace.

![Data source link](images/data_source_1.png)

Any edits you then make within the Development workspace will only affect the Development workspace, even if you’re using Production workspace utterances. See [Selecting a data source](logs_convo.html#select-source) for additional information.

To specify the deployment ID for an utterance sent using the `/message` API, include the deployment property inside the metadata object in your [context ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#send_message){: new_window}, as in this example:
{: #deploy_id}

```
"context" : {
  "username" : "jane_doe@myemail.com",
  "member_type" : "gold",
  "metadata" : {
       "deployment": "HelpDesk-Production"
  }
}
```
