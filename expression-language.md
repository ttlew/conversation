---

copyright:
  years: 2015, 2017
lastupdated: "2017-07-03"

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

# Accessing and evaluating objects

Valid expressions in conditions are written in the Spring Expression (SpEL) language. For more information, see [Spring Expression Language (SpEL) language ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html){: new_window}.
{: shortdesc}

## Built-in global variables

The following global variables are available:

**Table 1. Built-in global variables**

| Global variable       | Definition |
|-----------------------|------------|
| *intents[ ]*         | List of intents that supports default access to first element. |
| *entities[ ]*        | List of entities that supports default access to 1st element. |
| *input*              | JSON object part of the processed conversation message. |
| *output*             | JSON object part of the processed conversation message. |
| *context*            | JSON object part of the processed conversation message. |
| *conversation_start* | A boolean value that is true in the first dialog conversation turn (can be used in a condition of a dialog node to define a dialog welcome message). |
| *anything_else*      | The last node of the entire dialog. When use input cannot be matched an intent, the output of this node is issued. |

## Accessing entities

The following example shows how to access entities:

- `entities.airport[0].literal == 'Kennedy Airport'`
- `entities.airport[0].value == 'JFK Airport'`

To get the exact text that was recognized as an entity in the user input, use your own variation of the following example: `<? entities.city.literal ?>`.

## Accessing input

The following example shows how to access input:

- `input.text == 'yes'`
- `input.text.contains( 'yes' )`
- `input.text.matches( '[0-9]+' )`

You can use the `length()` function to access the length of the input string. For example, to check whether the input string contains ten characters, add this to your condition: `input.text.length() == 10`.

## Accessing intents

The following examples show how to access intents:

`intents[0] == 'Help'` or `intents == 'Help'`

## Evaluation

To expand variable values inside other variables, glue strings, add numbers, and add items to a list, use expression evaluation in string values of the JSON update structures.

Use the `<? expression ?>` expression syntax:

- **Expanding a property**
    - `"output":{"text":"Your name is <? context.userName ?>"}`
    or
    - `"output":{"text":"Your name is $userName"}` in shorthand syntax
- **Incrementing a numeric property**
    - `"output":{"number":"<? output.number + 1 ?>"}`
- **Invoking methods on properties and global objects**
    - `"context":{"toppings": "<? context.toppings.append( 'onions' ) ?>"}`
