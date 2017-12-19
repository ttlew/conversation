---

copyright:
  years: 2015, 2017
lastupdated: "2017-12-19"

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

# How the dialog is processed
{: #dialog-runtime}

Understand how your dialog is processed when a person interacts with your instance of the deployed {{site.data.keyword.conversationshort}} service at run time.
{: shortdesc}

## Anatomy of a dialog call
{: message-anatomy}

Each user utterance is passed to the dialog as a /message API call. This includes utterances that users make in reply to prompts from the dialog that ask them for more information. Some subscription plans include a set number of API calls, so it helps to understand what constitutes a call. A single /message API call is equivalent to a single dialog turn, which consists of an input from the user and a corresponding response from the dialog.

The body of the /message API call request and response includes the following objects:

- `context`: Contains variables that are meant to be persisted. To pass information from one call to the next, the application developer must pass the previous API call's response context in with each subsequent API call. For example, the dialog can collect the user's name and then refer to the user by name in subsequent nodes.

  ```json
  {
    "context" : {
      "user_name" : "<? @sys-person.literal ?>"
    }
  ```
  {: codeblock}

  See [Retaining information across dialog turns](dialog-runtime.html#context) for more information.

- `input`: The string of text that was submitted by the user. The text string can contain up to 2,048 characters.

  ```json
  {
    "input" : {
      "text" : "Where's your nearest store?"
    }
  ```
  {: codeblock}

- `output`: The dialog response to display to the user. You can use this section to define objects, such as variables, that are not meant to be persisted. For example, if you want to permanently delete a context variable named `temp` that you defined elsewhere in the dialog, you can use the following expression to do so.

  ```json
  {
  "output": {
    "text" : {},
    "deleted_variable" : "<? context.remove('temp') ?>"
  ```
  {: codeblock}

  See [A complex response](dialog-overview.html#complex) for more information about the output object.

You can learn more about the /message API call from the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.

## Retaining information across dialog turns
{: #context}

The dialog is stateless, meaning that it does not retain information from one interaction with the user to the next. It is the responsibility of the application developer to maintain any continuing information that the application needs. The application must look for, and store the context object in the message API response, and pass it in the context object with the next /message API request that is made as part of the conversation flow.

The simplest way to retain the information is to store the entire context object in memory in the client application - a web browser, for example. As an application becomes more complex, or if it needs to pass and store personally identifiable information, then you can store and retrieve the information from a database.

The application can pass information to the dialog, and the dialog can update this information and pass it back to the application, or to a subsequent node. The dialog does so by using context variables.

A context variable is a variable that you define in a node, and optionally specify a default value for. Other nodes or application logic can subsequently set or change the value of the context variable.

You can condition against context variable values by referencing a context variable from a dialog node condition to determine whether to execute a node. And you can reference a context variable from dialog node response conditions to show different reponses depending on a value provided by an external service or by the user.

## Passing context from the application
{: #context-from-app}

Pass information from the application to the dialog by setting a context variable and passing the context variable to the dialog.

For example, your application can set a $time_of_day context variable, and pass it to the dialog which can use the information to tailor the greeting it displays to the user.

![Shows a Welcome node that uses response conditions to check for the value of the $time_of_day context variable that is passed to the dialog from the application.](images/set-context.png)

In this example, the dialog knows that the application sets the variable to one of these values: *morning*, *afternoon*, or *evening*. It can check for each value, and depending on which value is present, return the appropriate greeting. If the variable is not passed or has a value that does not match one of the expected values, then a more generic greeting is displayed to the user.

## Passing context from node to node
{: #context-node-to-node}

The dialog can also add context variables to pass information from one node to another or to update the values of context variables. As the dialog asks for and gets information from the user, it can keep track of the information and reference it later in the conversation.

For example, in one node you might ask users for their name, and in a later node address them by name.

![Shows an introductions node that asks the user for their name, and stores it as a context variable. The next node refers to the user by name by using the $username context variable.](images/set-context-username.png)

In this example, the system entity @sys-person is used to extract the user's name from the input if the user provides one. In the JSON editor, the username context variable is defined and set to the @sys-person value. In a subsequent node, the $username context variable is included in the response to address the user by name.

## Defining a context variable
{: #context-var-define}

Define a context variable by adding a `name` and `value` pair to the `{context}` section of the JSON dialog node definition. The pair must meet these requirements:

- The `name` can contain any upper- and lowercase alphabetic characters, numeric characters (0-9), and underscores.

  **Note**: You can include other characters, such as periods and hyphens, in the name. However, if you do, then you must use one of the following approaches every time you subsequently reference the variable:

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

    -To add a context variable that is processed after the node response is processed. Click the **Options**  ![Advanced response](images/kabob.png) icon that is displayed in the response section, and then click **Open JSON editor**.

    If the **Multiple responses** setting is **On** for the node, then you must click the **Edit response** ![Edit response](images/edit-slot.png) icon before the **Options**  ![Advanced response](images/kabob.png) icon will be displayed in the response section.
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

## Common context variable tasks
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

## Deleting a context variable
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

## Order of operation
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

## Storing pattern entity values
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

## Updating a context variable value
{: #context-update}

If a node sets the value of a context variable that is already set, then the previous value is overwritten.

### Updating a complex JSON object

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

### Updating arrays

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
