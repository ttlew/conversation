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

# Creating a dialog
{:#dialog-create}

To create a dialog, complete the following steps:
{:shortdesc}

1.  Open the **Build** page from the navigation bar, and then click the **Dialog** tab. When you open the dialog builder for the first time, the following nodes are created for you:

    - **Welcome**: The first node. It contains a greeting that is displayed to your users when they first engage with the bot. You can edit the greeting.

    - **Anything else**: The final node. It contains phrases that are used to reply to users when their input is not recognized. You can replace the responses that are provided or add more responses with a similar meaning to add variety to the conversation. You can also choose whether you want the bot to return each response that is defined in turn or return them in random order.

1.  To add more nodes to the dialog tree, select the **Welcome** node, and then click the **Add node** icon ![Add node](images/Add_Create_24.png) below it.

1.  Enter a condition that, when met, triggers the service to process the node.

    The condition builder helps you find valid values to specify. To use it, enter one of the following characters, and then pick a value from the list of options that is displayed.

    | Character         | Lists defined values for these artifact types |
    |-------------------|----------------------------------------------:|
    | `#`               |                                       intents |
    | `@`               |                                      entities |
    | `@{entity-name}:` |                          {entity-name} values |

    > Note: The context builder cannot display a list of defined context variables when you enter the `$` character.

    Even though the condition builder cannot display a list of them, you can use context variables in the condition.

    You can create a new intent, entity, entity value, or context variable by defining a new condition that uses it. If you create an artifact this way, be sure to go back and complete any other steps that are necessary for the artifact to be created completely, such as defining sample utterances for an intent.

    To define a node that triggers based on more than one condition, enter one condition, and then click the plus sign (+) icon next to it.

    ![Shows the user clicking the plus sign (Add condition) icon after the first condition, which adds another field where the user can add another condition.](images/add-condition.gif)

    If you want to apply an `OR` operator to the multiple conditions instead of `AND`, click the `and` that is displayed between the fields to change the operator type.

    For more information about how to test for values in conditions, see [Conditions](dialog-conditions.html).

1.  Enter a response.
    - Add the text that you want the bot to display to the user as a response.
    - For information about conditional responses, how to add variety to responses, or how to specify what should happen after the node is triggered, see [Responses](dialog-responses.html).
1.  **Optional**: Name the node.

    Naming the node makes it easier for you to remember its purpose and to locate the node when it is minimized. If you don't provide a name, the name *Untitled node* is used.
1.  To add more nodes, select a node in the tree, and then click the **Add node** icon ![Add node](images/Add_Create_24.png).
    - To create a peer node to the selected node, click the icon below it. A peer node is an alternative to the existing node; it is checked next if the condition for the existing node is not met.
    - To create a child node to the selected node, click the icon next to it. A child node is processed after its parent node.

    As you create dialog branches, you can minimize the view of the branches that you have already created to help you focus on the new branch. To minimize a branch, select the minimize icon ![Minimize icon](images/DropDown_16.png) in its base node.
    For more information about the order in which dialog nodes are processed, see [Dialogs](dialog-overview.html).
1.  Test the dialog as you build it.
   See [Testing your dialog](dialog-test.html) for more information.

## Moving a dialog node

Each node that you create can be moved elsewhere in the dialog tree.

You might want to move a previously created node to another area of the flow to change the conversation. You can move nodes to become siblings or peers in another branch.

1.  On the node you want to move, select the ![the Move icon](images/moveGA.png) **Move** icon.
1.  Select where you want to move this node. Two icons appear on the destination node. You can choose to place the node that you are moving as a peer to this node, or as a child of it.
1.  Choose one of the icons. Depending on which you choose, the source node is moved to appear as a peer or a sibling of the target node and is used in that branch during a conversation.
