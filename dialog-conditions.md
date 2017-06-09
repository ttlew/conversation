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

# Conditions 
{:#dialog-conditions}

A node condition determines whether that node is used in the conversation. Response conditions determine which response to display to a user. 
{:shortdesc}

You can use one or more of the following artifacts in any combination to define a condition:

- **Context variable**: The node is used if the context variable equation that you specify is true. Use the syntax `$variable_name:value` or `$variable_name == 'value'`. See [Context variables](dialog-context.html).

- **Entity**: The node is used when any value or synonym for the entity is recognized in the user input. Use the syntax `@{entity_name}`.
>Note: If fuzzy matching is enabled, then `@entity_name` evaluates to true only if the confidence of the match is greater than 30%. That is, only if `@entity_name.confidence > .3`.
>Tip: Be sure to create a peer node to handle the case where none of the entity's values or synonyms are recognized.

- **Entity value**: The node is used if the entity value is detected in the user input. Use the syntax `@{entity_name}:{value}`. Specify a defined value for the entity, not a synonym.

- **Intent**: The simplest condition is a single intent. The node is used if the user's input maps to that intent. Use the sytnax `#{intent-name}`. For example, `#weather` checks if the intent detected in the user input is `weather`. If so, the node is processed.

- **Special condition**: Conditions that are provided with the service that you can use to perform common dialog functions. 
 
  | Condition name       | Description                       |
  |----------------------|-----------------------------------|
  | anything_else        | You can use this condition at the end of a dialog, to be   processed when the user input does not match any other dialog nodes. The             **Anything else** node is triggered by this condition. |
  | conversation_start   | Like **welcome**, this condition is evaluated as true       during the first dialog turn. Unlike **welcome**, it is true whether or not the     initial request from the application contains user input. A node with the           **conversation_start** condition can be used to initialize context variables or     perform other tasks at the beginning of the dialog. |
  | false | This condition is always evaluated to false.  You might use this at the   top of a branch that is under development, to prevent it from being used, or as     the condition for a node that provides a common function and is used only as the     target of a **Jump to** action. |
  | irrelevant | This condition will evaluate to true if the user’s input is           determined to be irrelevant by the Conversation service. |
  | true | This condition is always evaluated to true.  You can use it at the end of   a list of nodes or responses to catch any responses that did not match any of the   previous conditions. |
  | welcome | This condition is evaluated as true during the first dialog turn (when   the conversation starts), only if the initial request from the application does     not contain any user input.  It is evaluated as false in all subsequent dialog       turns. The **Welcome** node is triggered by this condition. Typically, a node with   this condition is used to greet the user, for example, to display a message such     as "Welcome to our Pizza ordering app." |

### Condition syntax

Use one of these syntax options to create valid expressions in conditions:

- Spring Expression (SpEL) language, which is an expression language that supports querying and manipulating an object graph at runtime. See <a target="_blank" href="http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html" title="Spring Expression Language (SpEL) language">Spring Expression Language (SpEL) language</a> for more information.  

- Shorthand notations to refer to intents, entities, and context variables. See [Shorthand syntax](dialog-shorthand.html).

Use regular expressions to check for values to condition against.  To find a matching string, for example, you can use the `String.find` method. See  [Methods](dialog-methods.html) for more details.

### Condition usage tips

- As an alternative to the `@appliance:(air conditioner)` format, you can use `@appliance == 'air conditioner'`. When you use `==` you are evaluating only the value of the first detected `@appliance` entity. Using `@appliance:(air conditioner)` gets expanded to `entity['appliance'].contains('air conditioner')`, which matches whenever there is at least one `@appliance` entity of value 'air conditioner' detected in the user input.

- When using numeric variables, make sure the variables have values. If a variable does not have a value, it is treated as having value 0 in a numeric comparison. For example, if you check the value of a variable with the condition `@price < 100`, and the @price entity is null, then the condition is evaluated as `true`, even though the price was never set. To prevent the checking of null variables, use a condition such as `@price AND @price < 100`. If @price has no value, then this condition correctly returns false.

## What to do next

If you are ready to create a dialog, see [Creating a dialog](dialog-create.html). To learn about responses first, see [Responses](dialog-responses.html).
