---

copyright:
  years: 2015, 2017
lastupdated: "2017-08-04"

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

### Table 1. Built-in global variables

| Global variable      | Definition |
|----------------------|------------|
| *anything_else*      | The last node of the entire dialog. When user input cannot be matched to an intent, this node is executed. |
| *context*            | JSON object part of the processed conversation message. |
| *conversation_start* | A boolean value that is true in the first dialog conversation turn (can be used in a condition of a dialog node to define a dialog welcome message). |
| *entities[ ]*        | List of entities that supports default access to 1st element. |
| *input*              | JSON object part of the processed conversation message. |
| *intents[ ]*         | List of intents that supports default access to first element. |
| *output*             | JSON object part of the processed conversation message. |

## Accessing entities

The entities array contains one or more entities. Each entity has a set of properties associate with it. You can access information about an entity through its properties.

### Table 2. Entity properties

| Property              | Definition | Usage tips |
|-----------------------|------------|------------|
| *confidence*          | A decimal percentage that represents the service's confidence in the recognized entity. | You can use this property in a condition to have it return false if the confidence level is not higher than a percent you specify. When fuzzy matching is enabled, the default confidence level threshold is 0.3. Whether or not fuzzy matching is enabled, system entities always have a confidence level of 1.0. |
| *location*            | A zero-based character offsets that indicates where the detected entity values begin and end in the input text. | Use `.literal` to extract the span of text between start and end index values that are stored in the location property. |
| *value*               | The entity value identified in the input. | This property returns the entity value as defined in the training data, even if the match was made against one of its associated synonyms.

### Entity property usage examples
The following example shows how to make use of property values:

- To return a specific response if the 'JFK' entity is recognized in the user input, you could add this equation to the response condition:
  `entities.airport[0].value == 'JFK'`
- To return the entity name as it was specified by the user in the dialog response, use the .literal property because the input might have matched against one of the synonyms defined for the JFK entity:
  `So you want to go to <?entities.airport[0].literal?>...`

  It might evaluate to something like `So you want to go to Kennedy Airport...' in the response.
- To be more restrictive about which terms are identified as cities in the input when fuzzy matching is enabled, you can specify this equation in a node condition, for example: `@city && @city.confidence > 0.7`. The node will only execute if the service is 70% confident that the input text contains a city reference.

## Accessing input

The input JSON object contains one property only: the text property. The text property represents the text of the user input.

### Input property usage examples

The following example shows how to access input:

- To execute a node if the user input is "Yes", add this equation to the node condition:
  `input.text == 'Yes'`

You can use any of the [String methods](/docs/services/conversation/dialog-methods.html#strings) to evaluate or manipulate text from the user input. For example:

- To check whether the user input contains "Yes", use: `input.text.contains( 'Yes' )`.
- Returns true if the user input is a number: `input.text.matches( '[0-9]+' )`.
- To check whether the input string contains ten characters, use: `input.text.length() == 10`.

## Accessing intents

The intents array contains one or more intents that were recognized in the user input, sorted in descending order of confidence. Each intent has one property only: the confidence property. The confidence property is a decimal percentage that represents the service's confidence in the recognized intent.

The following examples show how to access intents:

- `intents[0] == 'Help'`
- `intents == 'Help'`

## Evaluation

To expand variable values inside other variables, or apply methods to variables, use the `<? expression ?>` expression syntax. For example:

- **Expanding a property**
    - `"output":{"text":"Your name is <? context.userName ?>"}`
    or
    - `"output":{"text":"Your name is $userName"}` in shorthand syntax
- **Incrementing a numeric property**
    - `"output":{"number":"<? output.number + 1 ?>"}`
- **Invoking methods, such as append, on properties and global objects**
    - `"context":{"toppings": "<? context.toppings.append( 'onions' ) ?>"}`
