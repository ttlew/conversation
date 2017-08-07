---

copyright:
  years: 2015, 2017
lastupdated: "2017-08-07"

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

# Building a dialog
{: #dialog-build}

The dialog uses the intents and entities that are identified in the user's input, plus context from the application, to interact with the user and ultimately provide a useful response.
{: shortdesc}

The response might be the answer to a question such as `Where can I get some gas?` or the execution of a command, such as turning on the radio. The intent and entity might be enough information to identify the correct response, or the dialog might ask the user for more input that is needed to respond correctly. For example, if a user asks, "Where can I get some food?" you might want to clarify whether they want a restaurant or a grocery store, to dine in or take out, and so on. You can ask for more details in a text response and create one or more child nodes to process the new input.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/3HSaVfr3ty0?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Dialog overview
{: #overview}

Your dialog is represented graphically in the tool as a tree. Create a branch to process each intent that you want your conversation to handle. A branch is composed of multiple nodes.

### Dialog nodes

Each dialog node contains, at a minimum, a condition and a response.

![Shows user input going to a box that contains the statement If: CONDITION, Then: RESPONSE](images/node1-empty.png)

- Condition: Specifies the information that must be present in the user input for this node in the dialog to be triggered. The information might be a specific intent, an entity value, or a context variable value. See [Conditions](#conditions) for more information.
- Response: The utterance that the service uses to respond to the user. The response can also be configured to trigger programmatic actions. See [Responses](#responses) for more information.

You can think of the node as having an if/then construction: if this condition is true, then return this response.

For example, the following node is triggered if the natural language processing function of the service determines that the user input contains the `#cupcake-menu` intent. As a result of the node being triggered, the service responds with an appropriate answer.

![Shows the user asking about cupcake flavors. the If condition is #cupcake-menu and the Then response is a list of cupcake flavors.](images/node1-simple.png)

A single node with one condition and response can handle simple user requests. But, more often than not, users have more sophisticated questions or want help with more complex tasks. You can add child nodes that ask the user to provide any additional information that the service needs.

![Shows that the first node in the dialog asks which type of cupcake the user wants, gluten-free or regular, and has two child nodes that provide a different response depending on the user's answer.](images/node1-children.png)

### Dialog flow

The dialog that you create is processed by the service from top to bottom.

![Arrow points down next to 3 nodes to show that dialog flows from top to bottom](images/node-flow-down.png)

As it travels down the tree, if the service finds a condition that is met, it triggers that node. It then moves from left to right on the triggered node to check the user input against any child node conditions. As it checks the child nodes it moves again from top to bottom.

The services continues to work its way through the dialog tree from top to bottom, left to right, then top to bottom and left to right until it reaches the last node in the branch it is following.

![Shows arrow 1 pointing from top to bottom, arrow 2 pointing from left to right, and arrow 3 pointing from top to bottom one node level over.](images/node-flow.png)

When you start to build the dialog, you must determine the branches to include, and where to place them. The order of the branches is important because nodes are evaluated from top to bottom. The first base node whose condition matches the input is used; any nodes that come lower in the tree are not triggered.

## Conditions
{: #conditions}

A node condition determines whether that node is used in the conversation. Response conditions determine which response to display to a user.
You can use one or more of the following artifacts in any combination to define a condition:

- **Context variable**: The node is used if the context variable equation that you specify is true. Use the syntax `$variable_name:value` or `$variable_name == 'value'`. See [Context variables](#context).
- **Entity**: The node is used when any value or synonym for the entity is recognized in the user input. Use the syntax `@{entity_name}`.

  Be sure to create a peer node to handle the case where none of the entity's values or synonyms are recognized.
  {: tip}

- **Entity value**: The node is used if the entity value is detected in the user input. Use the syntax `@{entity_name}:{value}`. Specify a defined value for the entity, not a synonym.

  If you check for the associated entity in a peer node, be sure to place the node that checks for this particular entity value above it.
{: tip}

- **Intent**: The simplest condition is a single intent. The node is used if the user's input maps to that intent. Use the sytnax `#{intent-name}`. For example, `#weather` checks if the intent detected in the user input is `weather`. If so, the node is processed.

- **Special condition**: Conditions that are provided with the service that you can use to perform common dialog functions.

  <table>
  <tr>
    <td>Condition name</td>
    <td>Description</td>
  </tr>
  <tr>
    <td>anything_else</td>
    <td>You can use this condition at the end of a dialog, to be processed when the user input does not match any other dialog nodes. The **Anything else** node is triggered by this condition.</td>
  </tr>
  <tr>
    <td>conversation_start</td>
    <td>Like **welcome**, this condition is evaluated as true during the first dialog turn. Unlike **welcome**, it is true whether or not the initial request from the application contains user input. A node with the **conversation_start** condition can be used to initialize context variables or perform other tasks at the beginning of the dialog.</td>
  </tr>
  <tr>
    <td>false</td>
    <td>This condition is always evaluated to false. You might use this at the top of a branch that is under development, to prevent it from being used, or as the condition for a node that provides a common function and is used only as the target of a **Jump to** action.</td>
  </tr>
  <tr>
    <td>irrelevant</td>
    <td>This condition will evaluate to true if the user’s input is determined to be irrelevant by the Conversation service.</td>
  </tr>
  <tr>
    <td>true</td>
    <td>This condition is always evaluated to true. You can use it at the end of a list of nodes or responses to catch any responses that did not match any of the previous conditions.</td>
  </tr>
  <tr>
    <td>welcome</td>
    <td>This condition is evaluated as true during the first dialog turn (when the conversation starts), only if the initial request from the application does not contain any user input. It is evaluated as false in all subsequent dialog turns. The **Welcome** node is triggered by this condition. Typically, a node with this condition is used to greet the user, for example, to display a message such as "Welcome to our Pizza ordering app."</td>
  </tr>
  </table>

### Condition syntax

Use one of these syntax options to create valid expressions in conditions:

- Spring Expression (SpEL) language, which is an expression language that supports querying and manipulating an object graph at runtime. See [Spring Expression Language (SpEL) language ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html){: new_window} for more information.

- Shorthand notations to refer to intents, entities, and context variables. See [Shorthand syntax](dialog-shorthand.html).

Use regular expressions to check for values to condition against.  To find a matching string, for example, you can use the `String.find` method. See  [Methods](dialog-methods.html) for more details.

### Condition usage tips

- If you want to evaluate only the value of the first detected instance of an entity type, you can use the syntax  `@entity == 'specific-value'` instead of the `@entity:(specific-value)` format. For example, when you use `@appliance == 'air conditioner'`, you are evaluating only the value of the first detected `@appliance` entity. But, using `@appliance:(air conditioner)` gets expanded to `entity['appliance'].contains('air conditioner')`, which matches whenever there is at least one `@appliance` entity of value 'air conditioner' detected in the user input.
- When using numeric variables, make sure the variables have values. If a variable does not have a value, it is treated as having a null value (0) in a numeric comparison. For example, if you check the value of a variable with the condition `@price < 100`, and the @price entity is null, then the condition is evaluated as `true` because 0 is less than 100, even though the price was never set. To prevent the checking of null variables, use a condition such as `@price AND @price < 100`. If @price has no value, then this condition correctly returns false.
- If you use an entity as the condition and fuzzy matching is enabled, then `@entity_name` evaluates to true only if the confidence of the match is greater than 30%. That is, only if `@entity_name.confidence > .3`.

## Responses
{: #responses}

The dialog response defines how to reply to the user.

You can reply with one of these response types:

- [Simple text response](#simple-text)
- [Multiple conditioned responses](#multiple)
- [Complex response](#complex)

### Simple text response
{: #simple-text}

If you want to provide a text response, simply enter the text that you want the service to display to the user.

![Shows a node that shows a user ask, "Where are you located" and the dialog response is, "We have no brick and mortar stores! But, with an internet connection, you can shop us from anywhere"](images/response-simple.png)

#### Adding variety
{: #variety}

If your users return to your conversation service frequently, they might be bored to hear the same greetings and responses every time.  You can add *variations* to your responses so that your conversation can respond to the same condition in different ways.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/nAlIW3YPrAs?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

In this example, the answer that the service provides in response to questions about store locations differs from one interaction to the next:

![Shows a node that shows a user ask, "Where are you located" and the dialog has three different responses defined"](images/variety.png)

You can choose to rotate through the response variations sequentially or in random order. By default, responses are rotated sequentially, as if they were chosen from an ordered list.

### Multiple conditioned responses
{: #multiple}

A single dialog node can provide different responses, each one triggered by a different condition.  Use this approach to address multiple scenarios in a single node.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/KcvVQAsnhLM?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

The node still has a main condition, which is the condition for using the node and processing the conditions and responses that it contains.

In this example, the service uses information that it collected earlier about the user's location to tailor its response, and provide information about the store nearest the user. See [Context variables](#context) for more information about how to store information collected from the user.

![Shows a node that shows a user ask, "Where are you located" and the dialog has three different responses depending on conditions that use info from the $state context variable to specify locations in those states"](images/multiple-responses.png)

This single node now provides the equivalent function of four separate nodes.

The conditions within a node are evaluated in order, just as nodes are.  Be sure that your conditions and responses are listed in the correct order.  If you need to change the order, select a condition and move it up or down in the list using the arrows that appear. If you want to update the context, you must do so in each individual response.  There is not a common response section. The **Jump to** action is processed after a response is selected and delivered. If you add a **Jump to** action, it is run after any response that is returned from the node.
{: tip}

### A complex response
{: #complex}

To specify a more complex response, you can use the JSON editor to specify the response in the `"output":{}` property.

- To include a context variable value in the response, use the syntax `$variable-name` to specify it. See [Context variables](#context) for more information.

    ```json
    {
      "output": {
        "text": "Hello $user"
      }
    }
    ```
    {: codeblock}

- To specify more than one statement that you want to display on separate lines, define the output as a JSON array.

    ```json
    {
      "output": {
        "text": ["Hello there.", "How are you?"]
      }
    }
    ```
    {: codeblock}
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

### Defining what to do next
{: #jump-to}

After making the specified response, you can instruct the service to do one of the following things:

- **Wait for user input**: The service waits for the user to provide new input that the response elicits. For example, the response might ask the user a yes or no question. The dialog will not progress until the user provides more input.
- **Jump to another dialog node**: Use this option when you want to bypass waiting for user input and want the conversation to go directly to child nodes or to an entirely different dialog node. You can use a Jump to action to route the flow to a common dialog node from multiple locations in the tree, for example.
  >Note: The target node that you want to jump to must exist before you can configure the jump to action to use it.

#### Configuring the Jump to action

If you choose to jump to another node, you must specify whether the action targets the **response** or the **condition** of the selected dialog node

- **Response**: If the statement targets the response part of the selected dialog node, it is run immediately. That is, the system does not evaluate the condition part of the selected dialog node and runs the response part of the selected dialog node immediately.

  Targeting the response is useful for chaining several dialog nodes together. The response part of the selected dialog node is processed as if the condition of this dialog node is true. If the selected dialog node has another **Jump to** action, that action is run immediately, too.
- **Condition**: If the statement targets the condition part of the selected dialog node, the service checks first whether the condition of the targeted node evaluates to true.
    - If the condition evaluates to true, the system processes this node immediately by updating the context with the dialog node context and the output with the dialog node output.
    - If the condition does not evaluate to true, the system continues the evaluation process of a condition of the next sibling node of the target dialog node and so on, until it finds a dialog node with a condition that evaluates to true.
    - If the system processes all the siblings and no condition evaluates to true, the basic fallback strategy is used, and the dialog evaluates the nodes at the top level too.

    Targeting the condition is useful for chaining the conditions of dialog nodes. For example, you might want to first check whether the input contains an intent, such as `#turn_on`, and if it does, you might want to check whether the input contains entities, such as `@lights`, `@radio`, or `@wipers`. Chaining conditions helps to structure larger dialog trees.

> Note: The processing of **Jump to** actions changed with the February 3, 2017 release. See [Upgrading your workspace ![External link icon](../../icons/launch-glyph.svg "External link icon")](upgrading.html){: new_window} for more details.

#### Processing order

The service processes the following functions in this order, regardless of the order in which you specify them.

1.  Updates the dialog context.
1.  Provides a text response to the user.
1.  Routes the conversation as directed by waiting for a user response or following a **Jump to** action.

#### More information

For information about the expression language used by dialog, plus methods, system entities, and other useful details, see the Reference section in the Navigation pane.

## Context variables
{: #context}

The dialog is stateless, meaning that it does not retain information from one interchange with the user to the next. Your application is responsible for maintaining any continuing information that it needs. However, the application can pass information to the dialog, and the dialog can update this information and pass it back to the application. It does so by using context variables.

A context variable is a variable that you define in a node, and optionally specify a default value for. Other nodes or application logic can subsequently set or change the value of the context variable. You can use context variable values within a dialog as node conditions that show different reponses depending on a value provided by the user.

### Passing context from the application

Pass information from the application to the dialog by setting a context variable and passing the context variable to the dialog.

For example, your application can set a $time_of_day context variable, and pass it to the dialog which can use the information to tailor the greeting it displays to the user.

![Shows a Welcome node that uses response conditions to check for the value of the $time_of_day context variable that is passed to the dialog from the application.](images/set-context.png)

In this example, the dialog knows that the application sets the variable to one of these values: *morning*, *afternoon*, or *evening*. It can check for each value, and depending on which value is present, return the appropriate greeting. If the variable is not passed or has a value that does not match one of the expected values, then a more generic greeting is displayed to the user.

### Passing context from node to node

The dialog can also add context variables to pass information from one node to another or to update the values of context variables. As the dialog asks for and gets information from the user, it can keep track of the information and reference it later in the conversation.

For example, in one node you might ask users for their name, and in a later node address them by name.

![Shows an introductions node that asks the user for their name, and stores it as a context variable. The next node refers to the user by name by using the $username context variable.](images/set-context-username.png)

In this example, the system entity @sys-person is used to extract the user's name from the input if the user provides one. In the JSON editor, the username context variable is defined and set to the @sys-person value. In a subsequent node, the $username context variable is included in the response to address the user by name.

### Defining a context variable

Define a context variable by adding a `name` and `value` pair to the `{context}` section of the JSON dialog node definition. The pair must meet these requirements:

- The `name` can contain any upper- and lower-case alphabetic characters, numeric characters (0-9), and underscores.

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

1.  From the edit view of the node, open the JSON editor by clicking the ![Advanced response](images/kabob.png) icon, and then selecting **JSON**.

1.  In front of the `"output":{}` block, add a `"context":{}` block if one is not present.

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
    ...
    }
    ```
    {: codeblock}

  To subsequently reference the context variable, use the syntax `$name` where *name* is the name of the context variable that you defined.

Other common tasks include:

- To store the entire string that was input by the user, use `input.text`:

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

### Updating a context variable value
{: #updating-a-context-variable-value}

If a node sets the value of a context variable that is already set, then the previous value is overwritten.

#### Updating a complex JSON object

Previous values are overwritten for all JSON types except a JSON object. If the context variable is a complex type such as JSON object, a JSON merge procedure is used to update the variable. The merge operation adds any newly defined properties and overwrites any existing properties of the object.

In this example, a name context variable is defined as a complex object.

```json
{
  "context": {
    ...
    "complex_object": {
      "user_firstname" : "Paul",
      "user_lastname" : "Pan"
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
    "string_array": "string array: $array"
  }
)
```
{: codeblock}

If you check the values of these context variables in the Try it out pane, you will see their values specified as follows:

**$array** : `["one","two"]`

**$string_array** : `"string array: [\"one\",\"two\"]"`

You can subsequently perform array methods on the $array variable, such as `<? $array.removeValue('two') ?>` but not the $string_array variable.

## Creating a dialog
{: #create}

Use the {{site.data.keyword.conversationshort}} tool to create a dialog.

### Dialog node limits
{: #dialog-node-limits}

The number of dialog nodes you can create depends on your service plan.

| Service plan     | Dialog nodes per workspace |
|------------------|---------------------------:|
| Standard/Premium |                    100,000 |
| Lite             |                     25,000 |

Tree depth limit: Service supports 2,000 dialog node descendants; tooling performs best with 20 or fewer.

### Procedure

To create a dialog, complete the following steps:

1.  Open the **Build** page from the navigation bar, click the **Dialog** tab, and then click **Create**.

    When you open the dialog builder for the first time, the following nodes are created for you:
    - **Welcome**: The first node. It contains a greeting that is displayed to your users when they first engage with the service. You can edit the greeting.
    - **Anything else**: The final node. It contains phrases that are used to reply to users when their input is not recognized. You can replace the responses that are provided or add more responses with a similar meaning to add variety to the conversation. You can also choose whether you want the service to return each response that is defined in turn or return them in random order.
1.  To add more nodes to the dialog tree, click the **More** ![More icon](images/kabob.png) icon on the **Welcome** node, and then select **Add node below**.
1.  Enter a condition that, when met, triggers the service to process the node.

    The condition builder helps you find valid values to specify. To use it, enter one of the following characters, and then pick a value from the list of options that is displayed.

    <table>
    <tr>
      <td>Character</td>
      <td>Lists defined values for these artifact types</td>
    </tr>
    <tr>
      <td>`#`</td>
      <td>intents</td>
    </tr>
    <tr>
      <td>`@`</td>
      <td>entities</td>
    </tr>
    <tr>
      <td>`@{entity-name}:`</td>
      <td>{entity-name} values</td>
    </tr>
    </table>

    > Note: The condition builder cannot show a list of defined context variables when you enter the `$` character.

    Even though the condition builder cannot show a list of them, you can use context variables in the condition.

    You can create a new intent, entity, entity value, or context variable by defining a new condition that uses it. If you create an artifact this way, be sure to go back and complete any other steps that are necessary for the artifact to be created completely, such as defining sample utterances for an intent.

    To define a node that triggers based on more than one condition, enter one condition, and then click the plus sign (+) icon next to it.

    ![Shows the user clicking the plus sign (Add condition) icon after the first condition, which adds another field where the user can add another condition.](images/add-condition.gif)

    If you want to apply an `OR` operator to the multiple conditions instead of `AND`, click the `and` that is displayed between the fields to change the operator type.

    The condition you define must be under 500 characters in length.

    For more information about how to test for values in conditions, see [Conditions](#conditions).
1.  **Optional**: If you want to collect multiple pieces of information from the user in this node, then click **Customize** and enable **Slots**. See [Gathering information with slots](#slots) for more details.
1.  Enter a response.
    - Add the text that you want the service to display to the user as a response.
    - For information about conditional responses, how to add variety to responses, or how to specify what should happen after the node is triggered, see [Responses](#responses).
1.  **Optional**: Name the node. 

    The dialog node name can contain letters (in Unicode), numbers, spaces, underscores, hyphens, and periods.

    Naming the node makes it easier for you to remember its purpose and to locate the node when it is minimized. If you don't provide a name, the node condition is used as the name.

1.  To add more nodes, select a node in the tree, and then click the **More** ![More icon](images/kabob.png) icon.
    - To create a peer node that is checked next if the condition for the existing node is not met, select **Add node below**.
    - To create a peer node that is checked before the condition for the existing node is checked, select **Add node above**.
    - To create a child node to the selected node, select **Add child node**. A child node is processed after its parent node.

    For more information about the order in which dialog nodes are processed, see [Dialog overview](#overview).
1.  Test the dialog as you build it.
   See [Testing your dialog](#test) for more information.

## Gathering information with slots
{: #slots}

Add slots to a dialog node to gather multiple pieces of information from a user within that node. Slots collect information at the users' pace. Details the user provides upfront are saved, and the service asks only for the details they do not.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/3unhxZUKZtk?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

### Why add slots?

Use slots to get the information you need before you can respond accurately to the user. For example, if users ask about operating hours, but the hours differ by store location, you could ask a follow-up question about which store location they plan to visit before you answer. You can then add response conditions that take the provided location information into account.

![Asks for location information before answering the question, When do you open?.](images/op-hours.png)

Slots can help you to collect multiple pieces of information that you need to complete a complex task for a user, such as making a dinner reservation.

![Shows four slots that prompt for the information needed to make a dinner reservation.](images/reservation.png)

The user might provide values for mutliple slots at once. For example, the input might include the information, "There will be 6 of us dining at 7 PM." This one input contains two of the missing required values: the number of guests and time of the reservation. The service recognizes and stores both of them, each one in its corresponding slot. It then displays the prompt that is associated with the next empty slot.

![Shows that two slots are filled, and the service prompts for the remaining one.](images/pass-in-info.png)

Slots make it possible for the service to answer follow-up questions without having to re-establish the user's goal. For example, a user might ask for a weather forecast, then ask a follow-up question about weather in another location or on a different day. If you save the required forecast variables, such as location and day, in slots, then if a user asks a follow-up question with new variable values, you can overwrite the slot values with the new values provided, and give a response that reflects the new information.

![Shows someone asking for a weather forecast, and then following up with a question about weather for a different location and time.](images/follow-up.png)

Using slots produces a more natural dialog flow between the user and the service, and is easier for you to manage than trying to collect the information by using many separate nodes.

### How slots work

Here's what you do:

- Add a slot for each piece of information that you want to collect.
- Write a prompt that elicits the piece of the information from the user.
- Identify the type of information you want to extract from the user's answer.
- Provide a name for the context variable in which the user's answer will be stored.
- Define responses that are executed after the user provides input as a result of the prompt. The responses must cover these cases:
    - Found:  Triggered when the user answers the question as expected.
    - Not found: Triggered when the user does not answer the question as expected. Here, you can clarify the type of information you need from the user.

The service cycles through the prompts for each slot to ask for the missing information, and saves the answers as they are provided. It repeats the process - starting from the first empty slot - until all of the slots are filled, meaning that all of the slot context variables contain valid values. It then executes the node-level response.

#### Adding slots

1.  Identify the units of information that you want to collect. For example, to order a pizza for someone, you might want to collect the following information:
    - Delivery time
    - Size
1.  From the dialog node edit view, click **Customize**, and then select the **Slots** checkbox.
1.  **Add a slot for each unit of required information**.

    For each slot, specify what to look for in the user input, how to save the slot value when it is provided, and the text to display to users to prompt them to provide the information you need. You can also specify follow-up statements to display both when the value you want is provided and when it is not.

    <table>
    <tr>
      <td>Information</td>
      <td>Check for</td>
      <td>Save as</td>
      <td>Prompt</td>
      <td>Follow-up if found</td>
      <td>Follow-up if not found</td>
    </tr>
    <tr>
      <td>Size</td>
      <td>@size</td>
      <td>$size</td>
      <td>"What size pizza would you like?"</td>
      <td>"$size it is."</td>
      <td>"What size did you want? We have small, medium, and large."</td>
    </tr>
    <tr>
      <td>DeliverBy</td>
      <td>@sys-time</td>
      <td>$time</td>
      <td>"When do you need the pizza by?"</td>
      <td>"For delivery by $time."</td>
      <td>"What time did you want it delivered? We need at least a half hour to prepare it."</td>
    </tr>
    </table>

    **Optional slots**: If you want to add a slot that captures information but is optional, do not specify a prompt for it.

    For example, you might add a slot that captures dietary restriction informations in case the user specifies any. However, you don't want to ask all users for dietary information since it is irrelevant in most cases.

    <table>
    <tr>
      <td>Information</td>
      <td>Check for</td>
      <td>Save as</td>
    </tr>
    <tr>
      <td>Wheat restriction</td>
      <td>@dietary</td>
      <td>$dietary</td>
    </tr>
    </table>

    When you add a slot without a prompt, the service treats the slot as optional.

    If you make a slot optional, only reference its context variable in the node-level response text if you can word it such that it makes sense even if no value is provided for the slot. For example, you might word a summary statement like this, "I am ordering a $dietary $size pizza for delivery at $time." The resulting text still makes sense if the dietary restruction information, such as `gluten-free` or `dairy-free` is not provided, "I am ordering a large pizza for delivery at 3:00PM."
    {: tip}
1.  **Keep users on track**.
    You can optionally define node-level handlers that provide responses to questions users might ask during the interaction that are tangential to the purpose of the node.

    For example, the user might ask about the tomato sauce recipe or where you get your ingredients. To handle such digressions, click the **Manage handlers** link and add a condition and response for each anticipated question.

    ![Shows a user ask about the sauce recipe. The response is, I'll take it to my grave'.](images/sauce.png)

    After responding to the digression, the prompt associated with the current empty slot is displayed.

    This condition is triggered if the user provides input that matches the handler conditions at any time during the dialog node flow up until the node-level response is displayed.
1.  **Add a node-level response**.
    This node-level response is not executed until after all of the required slots are filled. You can add a response that summarizes the information you collected. For example, "A `$size` pizza is scheduled for delivery at `$time`. Enjoy!"

1.  **Add logic that resets the slot context variables**.
    As you collect answers from the user per slot, they are saved in context variables. You can use the context variables to pass the information to another node or to an application or external service for use. However, after passing the information, you must set the context variables to null to reset the node so it can start collecting information again. You cannot null the context variables within the current node because the service will not exit the node until the required slots are filled. Instead, consider using one of the following methods:
    - Add processing to the external application that nulls the variables.
    - Add a child node that nulls the variables.
    - Insert a parent node that nulls the variables, and then jumps to the node with slots.

#### Tips for using slots

Consider these suggested approaches for handling common tasks.

- **Ask for everything at once**: Include an initial prompt for the whole node that clearly tells users which units of information you want them to provide. Displaying this prompt first gives users the opportunity to provide all the details at once and not have to wait to be prompted for each piece of information one at a time.

    For example, when the node is triggered because a customer wants to order a pizza, you can respond with the preliminary prompt, "I can take your pizza order. Tell me what size pizza you want and the time that you want it delivered."

    If the user provides even one piece of this information in their initial request, then the prompt is not displayed. For example, the initial input might be, "I want to order a large pizza." When the service analyzes the input, it recognizes "large" as the pizza size and fills the **Size** slot with the value provided. Because one of the slots is filled, it skips displaying the initial prompt to avoid asking for the pizza size information again. Instead, it displays the prompts for any remaining slots with missing information.

    From the Customize pane where you enabled the Slots feature, select the **Prompt for everything** checkbox to enable the intial prompt. This setting adds the **If no slots are pre-filled, ask this first** field to the node, where you can specify the text that prompts the user for everything.

- **Capturing multiple values**: You can ask for a list of items and save them in one slot.

    For example, you might want to ask users whether they want toppings on their pizza. To do so define an entity (@toppings), and the accepted values for it (pepperoni, cheese, mushroom, and so on). Add a slot that asks the user about toppings. Use the values property of the entity type to capture multiple values, if provided.

    <table>
    <tr>
      <td>Information</td>
      <td>Check for</td>
      <td>Save as</td>
      <td>Prompt</td>
      <td>Follow-up if found</td>
      <td>Follow-up if not found</td>
    </tr>
    <tr>
      <td>Toppings</td>
      <td>@toppings.values</td>
      <td>$toppings</td>
      <td>Any toppings on that?</td>
      <td>"Great addition."</td>
      <td>"What toppings would you like? We offer ..."</td>
    </tr>
    </table>

    To reference the user-specified toppings later, use the `<? $entity-name.join(',') ?>` syntax to list each item in the toppings array and separate the values with a comma. For example, "I am ordering you a $size pizza with `<? $toppings.join(',') ?>` that is scheduled for delivery by $time."

- **Reformatting values**: Because you are asking for information from the user and need to reference their input in responses, consider reformatting the values so you can display them in a friendlier format.

    For example, time values are saved in the `hh:mm:ss` format. You can use the JSON editor for the slot to reformat the time value as you save it so it uses the `hour:minutes AM/PM` format instead:

    ```json
    {
      "context":{
        "time": "<? @sys-time.reformatDateTime('h:mm a') ?>"
      }
    }
    ```
    {: codeblock}

    See [Methods to process values](dialog-methods.html) for other reformatting ideas.

- **Getting confirmation**: Add a slot below the others that asks the user to confirm that the information you have collected is accurate and complete. The slot can look for responses that match the #yes intent.

    <table>
    <tr>
      <td>Information</td>
      <td>Check for</td>
      <td>Save as</td>
      <td>Prompt</td>
      <td>Follow-up if found</td>
      <td>Follow-up if not found</td>
    </tr>
    <tr>
      <td>Confirmation</td>
      <td>#yes</td>
      <td>$confirmation</td>
      <td>"I'm going to order you a `$size` pizza for delivery at `$time`. Should I go ahead?"</td>
      <td>"Your pizza is on its way!"</td>
      <td>see below</td>
    </tr>
    </table>

    Because users might include affirmative statements at other times during the dialog (*Oh yes, we want the pizza delivered at 5pm*), use the `slot_in_focus` property to make it clear in the slot condition that you are looking for a Yes response to the prompt for this slot only.

    ```json
    #yes && slot_in_focus
    ```
    The `slot_in_focus` property always evaluates to a Boolean (true or false) value. Only include it in a condition for which you want a boolean result. Do not use it in slot conditions that checks for an entity type and then save the entity value, for example.
    {: tip}
    In the **Not Found** prompt, ask for the information all over again and reset the context variables that you saved earlier.

    ```json
    {
      "output":{
        "text": {
          "values": [
            "Let's try this again. Tell me what size pizza you want and the time..."
          ]
        }
      },
      "context":{
        "size": null,
        "time": null
      }
    }
    ```
    {: codeblock}

- **Avoid number confusion**: Some values that are provided by users can be identified as more than one entity type.

    You might have two slots that store the same type of value, such as an arrival date and departure date, for example. Build logic into your slot conditions to distinguish such similar values from one another.

    In addition, the service can recognize multiple entity types in a single user input. For example, when a user provides a currency, it is recognized as both a @sys-currency and @sys-number entity type. Do some testing in the "Try it out" pane to understand how the system will interpret different user inputs, and build logic into your conditions to prevent possible misinterpretations.

    **Tip**: In logic that is unique to the slots feature, when two entities are recognized in a single user input, the one with the larger span is used. For example, if the user enters *May 2*, even though the Conversation service recognizes both @sys-date (05022017) and @sys-number (2) entities in the text, only the entity with the longer span (@sys-date) is registered and applied to a slot, if applicable.

- **Prevent a Found response from displaying when it's not needed**: If you specify Found responses for multiple slots, then if a user provides values for multiple slots at once, the Found response for at least one of the slots will be displayed. You probably want either the Found response for all of them or none of them to be returned.

    To prevent just one of the Found responses from being displayed, you can add a condition to the Found response that prevents it from being displayed if more than one slot value is filled. For example, use the JSON editor to add the following condition to the last slot with a Found response:
    ```json
    "conditions": "!($size && $time)"
    ```

    This condition prevents the response from being displayed if the $size and $time context variables are both provided.

- **Handle requests to exit the process**: Add at least one node-level handler that can recognize it when a user wants to exit the node.

    For example, in a node that collects information to schedule a pet grooming appointment, you can add a node-level handler that conditions on the #cancel intent, which recognizes utterances such as, "Forget it. I changed my mind."
    1.  In the JSON editor for the handler, fill all of the slot context variables with dummy values to prevent the node from continuing to ask for any that are missing. And in the handler response, add a message such as, "Ok, we'll stop there. No appointment will be scheduled."
    1.  In the node-level response, add a condition that checks for a dummy value in one of the slot context variables. If found, show a final message such as, "If you decide to make an appointment later, I'm here to help." If not found, it displays the standard summary message for the node, such as "I am making a grooming appointment for your $animal at $time on $date."
    1.  Take into account the logic used in conditions that are evaluated before this node-level handler so you can build distinct conditions into them. When a user input is received, the conditions are evaluated in the following order:
        - Current slot level If Found conditions.
        - Node-level handlers in the order they are listed.
        - Current slot level If Not Found conditions.

        For example, you groom all animals except cats. For the Animal slot, you might be tempted to use the following slot condition to prevent `cat` from being saved in the Animal slot:

        ```json
        Check for @animal && !@animal:cat, then save it as $animal.
        ```
        {: codeblock}

        And to let users know that you do not accept cats, you might specify the following value in the Not Found condition of the Animal slot:

        ```json
        If @animal && !@animal:cat then, "I'm sorry. We do not groom cats."

        ```
        {: codeblock}

        While logical, if you also define a node-level exit request handler, then - given the order of condition evaluation - this Not Found condition will likely never get triggered. Instead, you can use this slot condition:

        ```json
        Check for @animal, then save it as $animal.
        ```
        {: codeblock}

        And to deal with a possible `cat` response, add this value to the Found condition:

        ```josn
        If @animal:cat then, "I'm sorry. We do not groom cats."
        ```
        {: codeblock}

        In the JSON editor for the Found condition, reset the value of the $animal context variable because it is currently set to cat and should not be.

        ```json
        {
          "output":{
            "text": {
              "values": [
                "I'm sorry. We do not groom cats."
              ]
            }
          },
          "context":{
            "animal": null
          }
        }
        ```
        {: codeblock}

    Here's a sample of JSON that defines a node-level handler for the pizza example:

    ```json
    {
    "conditions": "#cancel",
    "output": {
      "text": {
        "values": [
          "Ok, we'll stop there. No pizza delivery will be scheduled."
        ],
    "selection_policy": "sequential"
    }
    },
    "context": {
      "time": "12:00:00",
      "size": "large",
      "confirmation":"true"
    }
    }
    ```

#### Slots examples

To access JSON files that implement different common slot usage scenarios, go to the community [conversation repo ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/watson-developer-cloud/community/tree/master/conversation){: new_window} in GitHub.

To explore an example, download an example JSON file, and then import it as a new workspace. Go to the Dialog tab, and review the dialog nodes to see how slots were implemented to address different use cases.

### Moving a dialog node

Each node that you create can be moved elsewhere in the dialog tree.

You might want to move a previously created node to another area of the flow to change the conversation. You can move nodes to become siblings or peers in another branch.

1.  On the node you want to move, click the **More** ![More icon](images/kabob.png) icon, and then select **Move**.
1.  Select a target node that is located in the tree near where you want to move this node. Choose whether to place this node above or below the target node, or to make it a child of the target node.

## Testing your dialog
{: #test}

As you make changes to your dialog, you can test it at any time to see how it responds to input.

1.  From the Dialog tab, click the ![Ask Watson](images/ask_watson.png) icon.
1.  In the chat pane, type some text and then press Enter.

    Make sure the system has finished training on your most recent changes before you start to test the dialog. If the system is still training, a message appears at the top of the chat pane:
    {: tip}

    ![Screen capture of training message](images/training.png)
1.  Check the response to see if the dialog correctly interpreted your input and chose the right response.

    The chat window indicates what intents and entities were recognized in the input:

    ![Screen capture of test dialog output](images/test_dialog_output.png)

    In the dialog editor pane, the currently active node is highlighted.
1.  To check or set the value of a context variable, click the **Manage context** link.

    Any context variables that you defined in the dialog are displayed.

    In addition, a `$timezone` context variable is listed. The "Try it out" pane user interface gets user locale information from the web browser and uses it to set the `$timezone` context variable. This context variable makes it easier to deal with time references in test dialog exchanges. Consider doing something similar in your user application. If not specified, Greenwich Mean Time (GMT) is used.

    You can add a variable and set its value to see how the dialog responds in the next test dialog turn. This capability is helpful if, for example, the dialog is set up to show different responses based on a context variable value that is provided by the user.

    1.  To add a context variable, specify the variable name, and press **Enter**.
    1.  To define a default value for the context variable, find the context variable you added in the list, and then specify a value for it.

    See [Context variables](#context) for more information.

1.  Continue to interact with the dialog to see how the conversation flows through it.
    - To find and resubmit a test utterance, you can press the Up key to cycle through your recent inputs.
    - To remove prior test utterances from the chat pane and start over, click the **Clear** link. Not only are the test utterances and responses removed, but this action also clears the values of any context variables that were set as a result of your interactions with the dialog. Context variable values that you explicitly set or change are not cleared.

### What to do next

If you determine that the wrong intents or entities are being recognized, you might need to modify your intent or entity definitions.

If the right intents and entities are being recognized, but the wrong nodes are being triggered in your dialog, make sure your conditions are written correctly.
