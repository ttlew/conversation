---

copyright:
  years: 2015, 2017
lastupdated: "2017-12-11"

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
{:table: .aria-labeledby="caption"}

# Dialog overview
{: #dialog-overview}

The dialog uses the intents and entities that are identified in the user's input, plus context from the application, to interact with the user and ultimately provide a useful response.
{: shortdesc}

The response might be the answer to a question such as `Where can I get some gas?` or the execution of a command, such as turning on the radio. The intent and entity might be enough information to identify the correct response, or the dialog might ask the user for more input that is needed to respond correctly. For example, if a user asks, `Where can I get some food?` you might want to clarify whether they want a restaurant or a grocery store, to dine in or take out, and so on. You can ask for more details in a text response and create one or more child nodes to process the new input.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/oQUpejt6d84?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

The dialog is represented graphically in the {{site.data.keyword.conversationshort}} tool as a tree. Create a branch to process each intent that you want your conversation to handle. A branch is composed of multiple nodes.

## Dialog nodes

Each dialog node contains, at a minimum, a condition and a response.

![Shows user input going to a box that contains the statement If: CONDITION, Then: RESPONSE](images/node1-empty.png)

- Condition: Specifies the information that must be present in the user input for this node in the dialog to be triggered. The information might be a specific intent, an entity value, or a context variable value. See [Conditions](#conditions) for more information.
- Response: The utterance that the service uses to respond to the user. The response can also be configured to trigger programmatic actions. See [Responses](#responses) for more information.

You can think of the node as having an if/then construction: if this condition is true, then return this response.

For example, the following node is triggered if the natural language processing function of the service determines that the user input contains the `#cupcake-menu` intent. As a result of the node being triggered, the service responds with an appropriate answer.

![Shows the user asking about cupcake flavors. the If condition is #cupcake-menu and the Then response is a list of cupcake flavors.](images/node1-simple.png)

A single node with one condition and response can handle simple user requests. But, more often than not, users have more sophisticated questions or want help with more complex tasks. You can add child nodes that ask the user to provide any additional information that the service needs.

![Shows that the first node in the dialog asks which type of cupcake the user wants, gluten-free or regular, and has two child nodes that provide a different response depending on the user's answer.](images/node1-children.png)

## Dialog flow

The dialog that you create is processed by the service from the first node in the tree to the last.

![Arrow points down next to 3 nodes to show that dialog flows from the first node to the last](images/node-flow-down.png)

As it travels down the tree, if the service finds a condition that is met, it triggers that node. It then moves along the triggered node to check the user input against any child node conditions. As it checks the child nodes it moves again from the first child node to the last.

The services continues to work its way through the dialog tree from first to last node, along each triggered node, then from first to last child node, and along each triggered child node until it reaches the last node in the branch it is following.

![Shows arrow 1 pointing from the first base node to the last, arrow 2 pointing from along the length of a triggered node, and arrow 3 pointing from the first to the last child nodes of the triggered node.](images/node-flow.png)

When you start to build the dialog, you must determine the branches to include, and where to place them. The order of the branches is important because nodes are evaluated from first to last. The first base node whose condition matches the input is used; any nodes that come later in the tree are not triggered.

When the service reaches the end of a branch, or cannot find a condition that evaluates to true from the current set of child nodes it is evaluating, it jumps back out to the base of the tree. And once again, the service processes the base nodes from first to the last. If none of the conditions evaluates to true, then the response from the last node in the tree, which typically has a special `anything_else` condition that always evaluates to true, is returned.

You can disrupt the standard first-to-last flow by customizing what happens after a node is processed. For example, you can configure a node to jump directly to another node after it is processed, even if the other node is defined earlier in the tree. See [Defining what to do next](dialog-overview.html#jump-to) for more details.

## Conditions
{: #conditions}

A node condition determines whether that node is used in the conversation. Response conditions determine which response to display to a user.
You can use one or more of the following artifacts in any combination to define a condition:

- **Context variable**: The node is used if the context variable expression that you specify is true. Use the syntax, `$variable_name:value` or `$variable_name == 'value'`. For example, `$city:Boston` checks whether the `$city` context variable contains the value, `Boston`. If so, the node or response is processed.

  Do not define a node or response condition based on the value of a context variable in the same dialog node in which you set the context variable value.
  {: tip}

  For more information about context variables, see [Context variables](#context).

- **Entity**: The node is used when any value or synonym for the entity is recognized in the user input. Use the syntax, `@entity_name`. For example, `@city` checks whether any of the city names that are defined for the @city entity were detected in the user input. If so, the node or response is processed.

  Be sure to create a peer node to handle the case where none of the entity's values or synonyms are recognized.
  {: tip}

  For more information about entities, see [Defining entities](entities.html).

- **Entity value**: The node is used if the entity value is detected in the user input. Use the syntax, `@entity_name:value` and specify a defined value for the entity, not a synonym. For example: `@city:Boston` checks whether the specific city name, `Boston`, was detected in the user input.

  If the entity is a pattern entity with capture groups, then you can check for a certain group value match. For example, you can use the syntax: `@us_phone.groups[1] == '617'`
  See [Storing pattern entity values in context variables](dialog-overview.html#context-pattern-entities) for more information.

  If you check for the presence of the entity, without specifying a particular value for it, in a peer node, be sure to position this node (which checks for a particular entity value) before the peer node that checks only for the presence of the entity. Otherwise, this node will never be evaluated.
  {: tip}

- **Intent**: The simplest condition is a single intent. The node is used if the user's input maps to that intent. Use the syntax, `#intent_name`. For example, `#weather` checks if the intent detected in the user input is `weather`. If so, the node is processed.

  For more information about intents, see [Defining intents](intents.html).

- **Special condition**: Conditions that are provided with the service that you can use to perform common dialog functions.

| Condition syntax     | Description |
|----------------------|-------------|
| `anything_else`      | You can use this condition at the end of a dialog, to be processed when the user input does not match any other dialog nodes. The **Anything else** node is triggered by this condition. |
| `conversation_start` | Like **welcome**, this condition is evaluated as true during the first dialog turn. Unlike **welcome**, it is true whether or not the initial request from the application contains user input. A node with the **conversation_start** condition can be used to initialize context variables or perform other tasks at the beginning of the dialog. |
| `false`              | This condition is always evaluated to false. You might use this at the start of a branch that is under development, to prevent it from being used, or as the condition for a node that provides a common function and is used only as the target of a **Jump to** action. |
| `irrelevant`         | This condition will evaluate to true if the user’s input is determined to be irrelevant by the Conversation service. |
| `true`               | This condition is always evaluated to true. You can use it at the end of a list of nodes or responses to catch any responses that did not match any of the previous conditions. |
| `welcome`            | This condition is evaluated as true during the first dialog turn (when the conversation starts), only if the initial request from the application does not contain any user input. It is evaluated as false in all subsequent dialog turns. The **Welcome** node is triggered by this condition. Typically, a node with this condition is used to greet the user, for example, to display a message such as `Welcome to our Pizza ordering app.`|
{: caption="Special conditions" caption-side="top"}

### Condition syntax details

Use one of these syntax options to create valid expressions in conditions:

- Shorthand notations to refer to intents, entities, and context variables. See [Accessing and evaluating objects](expression-language.html).

- Spring Expression (SpEL) language, which is an expression language that supports querying and manipulating an object graph at runtime. See [Spring Expression Language (SpEL) language ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html){: new_window} for more information.

Use regular expressions to check for values to condition against.  To find a matching string, for example, you can use the `String.find` method. See  [Methods](dialog-methods.html) for more details.

### Condition usage tips

- **Checking for values with special characters**: If you want to check whether an entity or context variable contains a value, and the value includes a special character, such as an apostrophe ('), then you must surround the value that you want to check with parentheses. For example, to check if an entity or context variable contains the name `O'Reilly`, you must surround the name with parentheses.

  `@person:(O'Reilly)` and `$person:(O'Reilly)`

  The service converts these shorthand references into these full SpEL expressions:

  `entities['person']?.contains('O''Reilly')` and `context['person'] == 'O''Reilly'`

  **Note**: SpEL uses a second apostrophe to escape the single apostrophe in the name.

- **Checking for number values**: When using numeric variables, make sure the variables have values. If a variable does not have a value, it is treated as having a null value (0) in a numeric comparison.

  For example, if you check the value of a variable with the condition `@price < 100`, and the @price entity is null, then the condition is evaluated as `true` because 0 is less than 100, even though the price was never set. To prevent the checking of null variables, use a condition such as `@price AND @price < 100`. If `@price` has no value, then this condition correctly returns false.

- **How fuzzy matching impacts entity recognition**: If you use an entity as the condition and fuzzy matching is enabled, then `@entity_name` evaluates to true only if the confidence of the match is greater than 30%. That is, only if `@entity_name.confidence > .3`.

- **Handling multiple entities in input**: If you want to evaluate only the value of the first detected instance of an entity type, you can use the syntax  `@entity == 'specific-value'` instead of the `@entity:(specific-value)` format.

  For example, when you use `@appliance == 'air conditioner'`, you are evaluating only the value of the first detected `@appliance` entity. But, using `@appliance:(air conditioner)` gets expanded to `entity['appliance'].contains('air conditioner')`, which matches whenever there is at least one `@appliance` entity of value 'air conditioner' detected in the user input.

## Responses
{: #responses}

The dialog response defines how to reply to the user.

You can reply with one of these response types:

- [Simple text response](#simple-text)
- [Conditional responses](#multiple)
- [Complex response](#complex)

### Simple text response
{: #simple-text}

If you want to provide a text response, simply enter the text that you want the service to display to the user.

![Shows a node that shows a user ask, Where are you located, and the dialog response is, We have no brick and mortar stores! But, with an internet connection, you can shop us from anywhere.](images/response-simple.png)

#### Adding variety
{: #variety}

If your users return to your conversation service frequently, they might be bored to hear the same greetings and responses every time.  You can add *variations* to your responses so that your conversation can respond to the same condition in different ways.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/nAlIW3YPrAs?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

In this example, the answer that the service provides in response to questions about store locations differs from one interaction to the next:

![Shows a node that shows a user ask, Where are you located, and the dialog has three different responses defined.](images/variety.png)

You can choose to rotate through the response variations sequentially or in random order. By default, responses are rotated sequentially, as if they were chosen from an ordered list.

### Conditional responses
{: #multiple}

A single dialog node can provide different responses, each one triggered by a different condition.  Use this approach to address multiple scenarios in a single node.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/KcvVQAsnhLM?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

The node still has a main condition, which is the condition for using the node and processing the conditions and responses that it contains.

In this example, the service uses information that it collected earlier about the user's location to tailor its response, and provide information about the store nearest the user. See [Context variables](#context) for more information about how to store information collected from the user.

![Shows a node that shows a user ask, Where are you located, and the dialog has three different responses depending on conditions that use info from the $state context variable to specify locations in those states.](images/multiple-responses.png)

This single node now provides the equivalent function of four separate nodes.

To add conditional responses to a node, click **Customize** and then click the **Multiple responses** toggle to turn it **On**.

The conditions within a node are evaluated in order, just as nodes are.  Be sure that your conditions and responses are listed in the correct order.  If you need to change the order, select a condition and move it up or down in the list using the arrows that are displayed. If you want to update the context, you must do so in the JSON editor for each individual response.  There is not a common JSON editor for all responses. If you associated a **Jump to** action with the node, the jump does not occur until after any responses are processed.
{: tip}

### A complex response
{: #complex}

To specify a more complex response, you can use the JSON editor to specify the response in the `"output":{}` property.

To include a context variable value in the response, use the syntax `$variable-name` to specify it. See [Context variables](#context) for more information.

```json
{
  "output": {
    "text": "Hello $user"
  }
}
```
{: codeblock}

To specify more than one statement that you want to display on separate lines, define the output as a JSON array.

```json
{
  "output": {
    "text": ["Hello there.", "How are you?"]
  }
}
```
{: codeblock}

The first sentence is displayed on one line, and the second sentence is displayed as a new line after it.

To implement more complex behavior, you can define the output text as a complex JSON object. For example, you can use a complex object in the JSON output to mimic the behavior of adding response variations to the node. You can include the following properties in the complex object:

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
        {: codeblock}

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
        {: codeblock}

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
    {: codeblock}

    In this case, all other output text is overwritten by this output text.

The default behavior assumes `selection_policy = random` and `append = true`. When the values array contains more than one item, the output text is randomly selected from its elements.

## Defining what to do next
{: #jump-to}

After making the specified response, you can instruct the service to do one of the following things:

- **Wait for user input**: The service waits for the user to provide new input that the response elicits. For example, the response might ask the user a yes or no question. The dialog will not progress until the user provides more input.
- **Skip user input**:  Use this option when you want to bypass waiting for user input and go directly to the first child node of the current node instead.

  **Note**: The current node must have at least one child node for this option to be available.

- **Jump to another dialog node**: Use this option when you want to bypass waiting for user input and want the conversation to go directly to an entirely different dialog node. You can use a *Jump to* action to route the flow to a common dialog node from multiple locations in the tree, for example.

  **Note**: The target node that you want to jump to must exist before you can configure the jump to action to use it.

### Configuring the Jump to action
{: #jump-to-config}

If you choose to jump to another node, you must specify whether the action targets the **response** or the **condition** of the selected dialog node.

- **Response**: If the statement targets the response section of the selected dialog node, it is run immediately. That is, the system does not evaluate the condition of the selected dialog node; it processes the response of the selected dialog node immediately.

  Targeting the response is useful for chaining several dialog nodes together. The response is processed as if the condition of this dialog node is true. If the selected dialog node has another **Jump to** action, that action is run immediately, too.

- **Condition**: If the statement targets the condition section of the selected dialog node, the service checks first whether the condition of the targeted node evaluates to true.
    - If the condition evaluates to true, the system processes the target node immediately.
    - If the condition does not evaluate to true, the system moves to the next sibling node of the target node to evaluate its condition, and repeats this process until it finds a dialog node with a condition that evaluates to true.
    - If the system processes all the siblings and none of the conditions evaluate to true, the basic fallback strategy is used, and the dialog evaluates the nodes at the base level of the dialog tree.

    Targeting the condition is useful for chaining the conditions of dialog nodes. For example, you might want to first check whether the input contains an intent, such as `#turn_on`, and if it does, you might want to check whether the input contains entities, such as `@lights`, `@radio`, or `@wipers`. Chaining conditions helps to structure larger dialog trees.

**Note**: The processing of **Jump to** actions changed with the February 3, 2017 release. See [Upgrading your workspace ![External link icon](../../icons/launch-glyph.svg "External link icon")](upgrading.html){: new_window} for more details.

## Context variables
{: #context}

The dialog is stateless, meaning that it does not retain information from one interchange with the user to the next. Your application is responsible for maintaining any continuing information that it needs. However, the application can pass information to the dialog, and the dialog can update this information and pass it back to the application. It does so by using context variables.

A context variable is a variable that you define in a node, and optionally specify a default value for. Other nodes or application logic can subsequently set or change the value of the context variable.

You can condition against context variable values by referencing a context variable from a dialog node condition to determine whether to execute a node. And you can reference a context variable from dialog node response conditions to show different reponses depending on a value provided by an external service or by the user.

Learn more:

- [Passing context from the application](dialog-overview.html#context-from-app)
- [Passing context from node to node](dialog-overview.html#context-node-to-node)
- [Defining a context variable](dialog-overview.html#context-var-define)
- [Common context variable tasks](dialog-overview.html#context-common-tasks)
- [Deleting a context variable](dialog-overview.html#context-delete)
- [Order of operation](dialog-overview.html#context-order-of-ops)
- [Storing pattern entity values](dialog-overview.html#context-pattern-entities)
- [Updating a context variable value](dialog-overview.html#context-update)

### Passing context from the application
{: #context-from-app}

Pass information from the application to the dialog by setting a context variable and passing the context variable to the dialog.

For example, your application can set a $time_of_day context variable, and pass it to the dialog which can use the information to tailor the greeting it displays to the user.

![Shows a Welcome node that uses response conditions to check for the value of the $time_of_day context variable that is passed to the dialog from the application.](images/set-context.png)

In this example, the dialog knows that the application sets the variable to one of these values: *morning*, *afternoon*, or *evening*. It can check for each value, and depending on which value is present, return the appropriate greeting. If the variable is not passed or has a value that does not match one of the expected values, then a more generic greeting is displayed to the user.

### Passing context from node to node
{: #context-node-to-node}

The dialog can also add context variables to pass information from one node to another or to update the values of context variables. As the dialog asks for and gets information from the user, it can keep track of the information and reference it later in the conversation.

For example, in one node you might ask users for their name, and in a later node address them by name.

![Shows an introductions node that asks the user for their name, and stores it as a context variable. The next node refers to the user by name by using the $username context variable.](images/set-context-username.png)

In this example, the system entity @sys-person is used to extract the user's name from the input if the user provides one. In the JSON editor, the username context variable is defined and set to the @sys-person value. In a subsequent node, the $username context variable is included in the response to address the user by name.

### Defining a context variable
{: #context-var-define}

Define a context variable by adding a `name` and `value` pair to the `{context}` section of the JSON dialog node definition. The pair must meet these requirements:

- The `name` can contain any upper- and lowercase alphabetic characters, numeric characters (0-9), and underscores.

  **Note**: You can include other characters, such as periods and hyphens, in the name. However, if you do, then you must use one of the following approaches to reference the variable:
  - context['variable-name']: The full SpEL expression syntax.
  - $(variable-name): Shorthand syntax with the variable name enclosed in parentheses.

  See [Accessing and evaluating objects](expression-language.html#shorthand-syntax-for-context-variables) for more details.

- The `value` can be any supported JSON type, such as a simple string variable, a number, a JSON array, or a JSON object.

The following JSON sample defines values for the $dessert string, $toppings_array array, and $age number context variables:

```json
{
  "context": {
    "dessert": "ice-cream",
    "toppings_array": ["onion", "olives"],
    "age": 18
  }
}
```
{: codeblock}

To define a context variable, complete the following steps:

1.  You define the context variable in a JSON editor.

    The editor is surfaced in multiple places in the dialog node edit view. Where it surfaces reflects the time at which the variable will be set during dialog node evaluation.

    -To add a context variable that is processed after the node response is processed, click the **Edit response** ![Edit response](images/edit-slot.png) icon. Click the **Options**  ![Advanced response](images/kabob.png) icon that is displayed in the response section, and then click **Open JSON editor**.

    If the **Multiple responses** setting is disabled for the node, then you do not need to click the **Edit response** icon first.
    {:tip}

    -To add a context variable that is processed after a slot condition is met, click the **Edit slot** ![Edit response](images/edit-slot.png) icon. From the *Configure slot* view header, click the **Options** ![Advanced response](images/kabob.png) icon, and then click **Open JSON editor**. (For more information about slots, see [Gathering information with slots](dialog-slots.html).)

    -To add a context variable that is processed after a slot response condition is met, click the **Edit slot** ![Edit response](images/edit-slot.png) icon. Click the **Options** ![Advanced response](images/kabob.png) icon in the response section, and then click **Open JSON editor**.

    If the **Enable conditional responses** setting is enabled for the slot, then you must click the **Edit response** ![Edit response](images/edit-slot.png) icon before you will see the **Options** ![Advanced response](images/kabob.png) icon in the response section.
    {: tip}

1.  Add a `"context":{}` block if one is not present.

    ```json
    {
      "context":{},
      "output":{}
    }
    ```
    {: codeblock}

1.  In the context block, add a name and value pair for each context variable that you want to define.

    ```json
    {
      "context":{
        "name": "value"
    },
      "output": {}
    }
    ```
    {: codeblock}

    In this example, a variable named `new_variable` is added to a context block that already contains a variable.

    ```json
    {
      "context":{
        "existing_variable": "value",
        "new_variable":"value"
      }
    }
    ```
    {: codeblock}

  To subsequently reference the context variable, use the syntax `$name` where *name* is the name of the context variable that you defined. For example, `$new_variable`.

### Common context variable tasks
{: #context-common-tasks}

- To store the entire string that was provided by the user as input, use `input.text`:

    ```json
    {
      "context": {
        "repeat": "<?input.text?>"
      }
    }
    ```
    {: codeblock}

- To store the value of an entity in a context variable, use this syntax:

    ```json
    {
      "context": {
        "place": "@place"
      }
    }
    ```
    {: codeblock}

- To store in a context variable the value of a string that you extract from the user's input by using a regular expression, use this syntax:

    ```json
    {
      "context": {
         "number": "<?input.text.extract('^[^\\d]*[\\d]{11}[^\\d]*$',0)?>"
      }
    }
    ```
    {: codeblock}

### Deleting a context variable
{: #context-delete}

To delete a context variable, set the variable to null.

```json
{
  "context": {
    "order_form": "null"
  }
}
```
{: codeblock}

If you want to remove all trace of the context variable, you can use the JSONObject.remove(string) method to delete it from the context object. However, you must use a variable to perform the removal. Define the new variable in the message output so it will not be saved beyond the current call.

```json
{
  "output": {
    "text" : {},
    "deleted_variable" : "<? context.remove('order_form') ?>"
  }
}
```
{: codeblock}

Alternatively you can delete the context variable in your application logic.

### Order of operation
{: #context-order-of-ops}

The order in which you define the context variables does not determine the order in which they are evaluated by the service. The service evaluates the variables, which are defined as JSON name and value pairs, in random order. Do not set a value in the first context variable and expect to be able to use it in the second, because there is no guarantee that the first context variable in your list will be executed before the second one in your list. For example, do not use two context variables to implement logic that returns a random number between zero and some higher value that is passed to the node.

```json
"context": {
    "upper": "<? @sys-number.numeric_value + 1?>",
    "answer": "<? new Random().nextInt($upper) ?>"
}
```
{: codeblock}

Use a slightly more complex expression to avoid having to rely on the value of the $upper context variable being evaluated before the $answer context variable is evaluated.

```json
"context": {
    "answer": "<? new Random().nextInt(@sys-number.numeric_value + 1) ?>"
}
```
{: codeblock}

### Storing pattern entity values
{: #context-pattern-entities}

To store the value of a pattern entity in a context variable, append .literal to the entity name. Using this syntax ensures that the exact span of text from user input that matched the specified pattern is stored in the variable.

```json
{
  "context": {
    "email": "<? @email.literal ?>"
  }
}
```
{: codeblock}

To store the text from a single group in a pattern entity with groups defined, specify the array number of the group that you want to store. For example, assume that the entity pattern is defined as follows for the @phone_number entity. (Remember, the parentheses denote pattern groups):

`\b((958)|(555))-(\d{3})-(\d{4})\b`

To store only the area code from the phone number that is specified in user input, you can use the following syntax:

```json
{
  "context": {
    "area_code": "<? @phone_number.groups[1] ?>"
  }
}
```
{: codeblock}

The groups are delimited by the regular expression that is used to define the group pattern. For example, if the user input that matches the pattern defined in the entity `@phone_number` is: `958-234-3456`, then the following groups are created:

| Group number | Regex engine value  | Dialog value   | Explanation |
|--------------|---------------------|----------------|-------------|
| groups[0]    | `958-234-3456`      | `958-234-3456` | The first group is always the full matching string. |
| groups[1]    | `((958)`l`(555))`   | `958`          | String that matches the regex for the first defined group, which is `((958)`l`(555))`. |
| groups[2]    | `(958)`             | `958`          | Match against the group that is included as the first operand in the OR expression `((958)`l`(555))` |
| groups[3]    | `(555)`             | `null`         | No match against the group that is included as the second operand in the OR expression `((958)`l`(555))` |
| groups[4]    | `(\d{3})`           | `234`          | String that matches the regular expression that is defined for the group. |
| groups[5]    | `(\d{4})`           | `3456`         | String that matches the regular expression that is defined for the group. |
{: caption="Group details" caption-side="top"}

**Note**: The 'l' used in the examples in the table above replaces the vertical bar character (|), which represents an `or` operator in the regular expression. For example, `((958)`l`(555))` is equivalent to `((958)|(555))`, which means `(958) or (555)`.

To help you decipher which group number to use to capture the section of input you are interested in, you can extract information about all the groups at once. Use the following syntax to create a context variable that returns an array of all the grouped pattern entity matches:

```json
{
  "context": {
    "array_of_matched_groups": "<? @phone_number.groups ?>"
  }
}
```
{: codeblock}

Use the "Try it out" pane to enter some test phone number values. For the input `958-123-2345`, this expression sets `$array_of_matched_groups` to `["958-123-2345","958","958",null,"123","2345"]`.

You can then count each value in the array starting with 0 to get the group number for it.

| Array element value | Array element number |
|---------------------|----------------------|
| "958-123-2345"      | 0 |
| "958"               | 1 |
| "958"               | 2 |
| null                | 3 |
| "123"               | 4 |
| "2345"              | 5 |
{: caption="Array elements" caption-side="top"}

It is easy to determine that, to capture the last four digits of the phone number, you need group #5, for example.

To return the JSONArray structure that is created to represent the grouped pattern entity, use the following syntax:

```json
{
  "context": {
    "json_matched_groups": "<? @phone_number.groups_json ?>"
  }
}
```
{: codeblock}

This expression sets `$json_matched_groups` to the following JSON array:

```json
[
  {"group": "group_0","location": [0, 12]},
  {"group": "group_1","location": [0, 3]},
  {"group": "group_2","location": [0, 3]},
  {"group": "group_3"},
  {"group": "group_4","location": [4, 7]},
  {"group": "group_5","location": [8, 12]}
]
```
{: codeblock}

**Note**: `location` is a property of an entity that uses a zero-based character offset to indicate where the detected entity value begins and ends in the input text.

If you expect two phone numbers to be supplied in the input, then you can check for two phone numbers. If present, use the following syntax to capture the area code of the second number, for example.

```json
{
  "context": {
    "second_areacode": "<? entities['phone_number'][1].groups[1] ?>"
  }
}
```
{: codeblock}

If the input is `I want to change my phone number from 958-234-3456 to 555-456-5678`, then `$second_areacode` equals `555`.

### Updating a context variable value
{: #context-update}

If a node sets the value of a context variable that is already set, then the previous value is overwritten.

#### Updating a complex JSON object

Previous values are overwritten for all JSON types except a JSON object. If the context variable is a complex type such as JSON object, a JSON merge procedure is used to update the variable. The merge operation adds any newly defined properties and overwrites any existing properties of the object.

In this example, a name context variable is defined as a complex object.

```json
{
  "context": {
    "complex_object": {
      "user_firstname" : "Paul",
      "user_lastname" : "Pan",
      "has_card" : false
    }
  }
}
```
{: codeblock}

A dialog node updates the context variable JSON object with the following values:

```json
{
  "complex_object": {
    "user_firstname": "Peter",
    "has_card": true
  }
}
```
{: codeblock}

The result is this context:

```json
{
  "complex_object": {
    "user_firstname": "Peter",
    "user_lastname": "Pan",
    "has_card": true
  }
}
```
{: codeblock}

#### Updating arrays

If your dialog context data contains an array of values, you can update the array by appending values, removing a value, or replacing all the values.

Choose one of these actions to update the array. In each case, we see the array before the action, the action, and the array after the action has been applied.

- **Append**: To add values to the end of an array, use the `append` method.

    For this Dialog runtime context:

    ```json
    {
      "context": {
        "toppings_array": ["onion", "olives"]
      }
    }
    ```
    {: codeblock}

    Make this update:

    ```json
    {
      "context": {
        "toppings_array": "<? $toppings_array.append('ketchup', 'tomatoes') ?>"
      }
    }
    ```
    {: codeblock}

    Result:

    ```json
    {
      "context": {
        "toppings_array": ["onion", "olives", "ketchup", "tomatoes"]
      }
    }
    ```
    {: codeblock}

- **Remove**: To remove an element, use the `remove` method and specify its value or position in the array.

    - **Remove by value** removes an element from an array by its value.

        For this Dialog runtime context:

        ```json
        {
          "context": {
            "toppings_array": ["onion", "olives"]
          }
        }
        ```
        {: codeblock}

        Make this update:

        ```json
        {
          "context": {
            "toppings_array": "<? $toppings_array.removeValue('onion') ?>"
          }
        }
        ```
        {: codeblock}

        Result:

        ```json
        {
          "context": {
            "toppings_array": ["olives"]
          }
        }
        ```
        {: codeblock}

    - **Remove by position**: Removing an element from an array by its index position:

        For this Dialog runtime context:

        ```json
        {
          "context": {
            "toppings_array": ["onion", "olives"]
          }
        }
        ```
        {: codeblock}

        Make this update:

        ```json
        {
          "context": {
            "toppings_array": "<? $toppings_array.remove(0) ?>"
          }
        }
        ```
        {: codeblock}

        Result:

        ```json
        {
          "context": {
            "toppings_array": ["olives"]
          }
        }
        ```
        {: codeblock}

- **Overwrite**: To overwrite the values in an array, simply set the array to the new values:

    For this Dialog runtime context:

        ```json
        {
          "context": {
            "toppings_array": ["onion", "olives"]
          }
        }
        ```
        {: codeblock}

    Make this update:

        ```json
        {
          "context": {
            "toppings_array": ["ketchup", "tomatoes"]
          }
        }
        ```
        {: codeblock}

    Result:

        ```json
        {
          "context": {
            "toppings_array": ["ketchup", "tomatoes"]
          }
        }
        ```
        {: codeblock}

**Attention**: If you save the array as part of a string, it becomes a String object instead of an Array. For example, the following $array context variable is an array, but the $string_array context variable is a string.

```json
{
  "context": {
    "array": [
      "one",
      "two"
    ],
    "array_in_string": "this is my array: $array"
  }
}
```
{: codeblock}

If you check the values of these context variables in the Try it out pane, you will see their values specified as follows:

**$array** : `["one","two"]`

**$array_in_string** : `"this is my array: [\"one\",\"two\"]"`

You can subsequently perform array methods on the $array variable, such as `<? $array.removeValue('two') ?>` but not the $array_in_string variable.

## More information

For information about the expression language used by dialog, plus methods, system entities, and other useful details, see the **Reference** section in the navigation pane.
