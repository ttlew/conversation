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

# Dialog tree runtime evaluation 
{:#dialog-eval}

When a user interacts with an application that calls the conversation service, the service pocesses the user's input against the dialog tree that you create. Dialog trees are evaluated in rounds, and a round always starts on a specific node.
{:shortdesc}

At the beginning of each conversation, evaluation begins at the top level of dialog nodes.

Each time the dialog returns a response and waits for user input, it stores the ID of the node at which the conversation should resume. This node is called the `contextual node`, and its ID is added to the `context.system.dialog_stack` property, which contains a JSON array of dialog node IDs that are currently on the dialog stack. The last node on this stack is the contextual node at which evaluation begins in the next round.

The evaluation round works in two stages:

- Stage one: The dialog tries to find an answer in the child nodes of the contextual node. That is, it tries to match all the conditions of the child nodes of this contextual node. 
  - If the final child node has a condition of "true", meaning that it is to be used if none of its siblings have been matched, then that node's response is processed. 
  - Otherwise, if no match is found, the dialog continues to the second stage. 

- Stage two: The dialog tries to find an answer to a particular input by matching the top level of dialog nodes. 
  The top level of dialog nodes should contain an *anything_else* node as the last node, which is hit when no match occurred in the conditions of the top level nodes. Typically, if an *anything_else* node is defined, the dialog returns an answer to every user input.
  The next contextual node to be selected in the dialog depends on whether the last dialog node that was selected has any child nodes. 
  - If it does not have children, it is a `leaf node`. In this case, the next contextual node is set to the *Conversation starts* virtual root node, and the evaluation in the next round starts from the top level again. 
  - If the last selected dialog node has children, it is set as the contextual node for the next round, and the dialog evaluation in the next round starts from the child nodes of this contextual node.
