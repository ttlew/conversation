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

# Responses 
{:#dialog-responses}

The dialog response defines how to reply to the user. 
{:shortdesc}

You can reply with one of these response types:

- [Simple text response](dialog-responses.html#simple-text)
- [Multiple conditioned responses](dialog-responses.html#multiple)
- [Complex response](dialog-responses.html#complex)

## Simple text response 
{:#simple-text}

If you want to provide a text response, simply enter the text that you want the bot to display to the user.

![Shows a node that shows a user ask, "Where are you located" and the dialog response is, "We have no brick and mortar stores! But, with an internet connection, you can shop us from anywhere"](images/response-simple.png)

### Adding variety  
{:#variety}

If your users return to your bot frequently, they might be bored to hear the same greetings and responses every time.  You can add *variations* to your responses so that your bot can respond to the same condition in different ways.

@[youtube](nAlIW3YPrAs?rel=0)

In this example, the answer that the bot provides in response to questions about store locations differs from one interaction to the next:

![Shows a node that shows a user ask, "Where are you located" and the dialog has three different responses defined"](images/variety.png)

You can choose to rotate through the response variations sequentially or in random order. By default, responses are rotated sequentially, as if they were chosen from an ordered list.

## Multiple conditioned responses
{:#multiple}

A single dialog node can provide different responses, each one triggered by a different condition.  Use this approach to address multiple scenarios in a single node.

@[youtube](KcvVQAsnhLM?rel=0)

The node still has a main condition, which is the condition for using the node and processing the conditions and responses that it contains.

In this example, the bot uses information that it collected earlier about the user's location to tailor its response, and provide information about the store nearest the user.

![Shows a node that shows a user ask, "Where are you located" and the dialog has three different responses depending on conditions that use info from the $state context variable to specify locations in those states"](images/multiple-responses.png)

This single node now provides the equivalent function of four separate nodes.

**Tips**:
* The conditions within a node are evaluated in order, just as nodes are.  Be sure that your conditions and responses are listed in the correct order.  If you need to change the order, select a condition and move it up or down in the list using the arrows that appear.
* If you want to update the context, you must do so in each individual response.  There is not a common response section.
* The **Jump to** action is processed after a response is selected and delivered. If you add a **Jump to** action, it is run after any response that is returned from the node.

## A complex response 
{:#complex}

To specify a more complex response, you can use the JSON editor to specify the response in the `"output":{}` property.

- To include a context variable value in the response, use the syntax `$variable-name` to specify it. See [Context variables](dialog-context.html) for more information.

    ```json
    {
      "output": {
        "text": "Hello $user"
      }
    }
    ```
    {:codeblock}

- To specify more than one statement that you want to display on separate lines, define the output as a JSON array.

    ```json
    {
      "output": {
        "text": ["Hello there.", "How are you?"]
      }
    }
    ```
    {:codeblock}
    The first sentence is displayed on one line, and the second sentence is displayed as a new line below it.

- To implement more complex behavior, you can define the output text as a complex JSON object. For example, you can use a complex object in the JSON output to mimic the behavior of adding response variations to the node.

  You can include the following properties in the complex object:
  - **values**: A JSON array of strings that holds multiple versions of output text that this dialog node can return. The order in which values in the array are returned depends on the attribute `selection_policy`.
  - **selection_policy**: The following values are valid:
    - **random**: The system randomly selects output text from the `values` array and does not repeat them consecutively. For example, consider output.text that contains three values. For the first three times, a random value is selected but not repeated another time. After all the output values are given, the system randomly selects another value and repeats the process.

        ```json
        {
            "output":{
                "text":{
                    "values":["Hello.","Hi.","Howdy!"],
                    "selection_policy":"random"
                }
            }
        }
        ```
        {:codeblock}

        The system returns one greeting from these three options that it picks at random. The next time the response is triggered, another greeting from the list is displayed. The greeting is again chosen at random, except the previously used greeting is intentionally not repeated.
    - **sequential**: The system returns the first output text the first time the dialog node is triggered, the second output text the second time the node is triggered, and so on.

        ```json
        {
            "output":{
                "text":{
                    "values":["Hello.", "Hi.", "Howdy!"],
                    "selection_policy":"sequential"
                }
            }
        }
        ```
        {:codeblock}

  - **append**: Specifies whether to append a value to an array or overwrite the values in the array with the new value or values. When set to false, the output collected in previously executed dialog nodes is overwritten by the text value specified in this particular node.

    ```json
    {
        "output":{
            "text":{
                "values": ["Hello."],
                "append":false
            }
        }
    }
    ```
    {:codeblock}

    In this case, all other output text is overwritten by this output text.

  The default behavior assumes `selection_policy = random` and `append = true`. When the values array contains more than one item, the output text is randomly selected from its elements.

## Defining what to do next 
{:#jump-to}

After making the specified response, you can instruct the bot to do one of the following things:

- **Wait for user input**: The bot waits for the user to provide new input that the response elicits. For example, the response might ask the user a yes or no question. The dialog will not progress until the user provides more input.

- **Jump to another dialog node**: Use this option when you want to bypass waiting for user input and want the conversation to go directly to child nodes or to an entirely different dialog node. You can use a Jump to action to route the flow to a common dialog node from multiple locations in the tree, for example.
  >Note: The target node that you want to jump to must exist before you can configure the jump to action to use it.

### Configuring the Jump to action

If you choose to jump to another node, you must specify whether the action targets the **response** or the **condition** of the selected dialog node

- **Response**: If the statement targets the response part of the selected dialog node, it is run immediately. That is, the system does not evaluate the condition part of the selected dialog node and runs the response part of the selected dialog node immediately.

  Targeting the response is useful for chaining several dialog nodes together. The response part of the selected dialog node is processed as if the condition of this dialog node is true. If the selected dialog node has another **Jump to** action, that action is run immediately, too.
- **Condition**: If the statement targets the condition part of the selected dialog node, the service checks first whether the condition of the targeted node evaluates to true.
    - If the condition evaluates to true, the system processes this node immediately by updating the context with the dialog node context and the output with the dialog node output. 
    - If the condition does not evaluate to true, the system continues the evaluation process of a condition of the next sibling node of the target dialog node and so on, until it finds a dialog node with a condition that evaluates to true.
    - If the system processes all the siblings and no condition evaluates to true, the basic fallback strategy is used, and the dialog evaluates the nodes at the top level too.

  Targeting the condition is useful for chaining the conditions of dialog nodes. For example, you might want to first check whether the input contains an intent, such as `#turn_on`, and if it does, you might want to check whether the input contains entities, such as `@lights`, `@radio`, or `@wipers`. Chaining conditions helps to structure larger dialog trees.

> Note: The processing of **Jump to** actions changed with the February 3, 2017 release. See [Updating the workspace](updating.html) for more details.

### Processing order

The service processes the following functions in this order, regardless of the order in which you specify them.

1. Updates the dialog context.
1. Provides a text response to the user.
1. Routes the conversation as directed by waiting for a user response or following a **Jump to** action.

### More information

For information about the expression language used by dialog, plus methods, system entities, and other useful details, see the [Reference](reference.html) section.

### What to do next

If you are ready to start building a dialog, see [Creating a dialog](dialog-create.html).
