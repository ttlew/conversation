---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-24"

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

# Upgrading workspaces

The {{site.data.keyword.conversationshort}} service regularly adds and updates features. While some of these changes are automatically applied to your workspace, updates that have a large impact do require a manual update.
{: shortdesc}

If you have an instance that was created before February 3, 2017 and your workspace has an available update, the upgrade icon (![upgrade icon](images/upgrade.png)) is displayed to indicate that and upgrade is available. Select this icon to open the Upgrade workspace dialog.

When you upgrade your workspace, the latest version of the API is enabled in the tool, and the "Try it out" pane begins to use the newest features.

After you upgrade a workspace, you cannot revert your workspace to a previous version.

## Upgrading your workspace
To avoid hindering functionality in your workspace:

1.  [Duplicate your workspace](configure-workspace.html#exporting-and-copying-workspaces).
2.  Upgrade the duplicate workspace.
3.  Test for issues in the upgraded workspace.

When you complete testing for issues, apply the upgrade to your application by changing the message API call to use **2017-02-03** or later.

## Change in Jump to actions
The processing of **Jump to** actions changed with the February 3, 2017 release.

Previously, if you jumped to the condition of a node and neither that node nor any of its peer nodes had a condition that was evaluated as true, the system would jump to the root-level node and look for a node whose condition matched the input. In some situations this processing created a loop, which prevented the dialog from progressing.

Under the new process, if neither the target node nor its peers is evaluated as true, the dialog turn is ended. Any response that has been generated is returned to the user, and an error message is returned to the application:

```
Goto failed from node DIALOG_NODE_ID.
Did not match the condition of the target node and any of the conditions of its subsequent siblings.
```
{: screen}

The next user input is handled at the root level of the dialog.

This update might change the behavior of your dialog if you have **Jump to** actions that target nodes whose conditions are false.

If you want to restore the old processing model, add a final peer node with a condition of `true`. In the response, use a **Jump to** action that targets the condition of the first node at the root level of your dialog tree.
