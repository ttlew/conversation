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

# Filter query reference

The {{site.data.keyword.conversationshort}} service REST API offers powerful log search capabilities through filter queries. You can use the /logs API `filter` parameter to search your workspace log for events that match a specified query. 
{:shortdesc}

The `filter` parameter is a cacheable query that limits the results to those matching the specified filter. You can filter on any object that is part of the JSON response model (for example, the user input text, the detected intents and entities, or the confidence score).

For more information about the /logs `GET` method and its response model, refer to the [API Reference](../../conversation/api/v1/#get_logs).

**Note:** The filter query syntax uses some characters that are not allowed in HTTP queries. Make sure that all special characters, including spaces and quotation marks, are URL encoded when sent as part of an HTTP query. For example, the filter `request.input.text::"IBM Watson"` would be specified as `request.input.text::%22IBM%20Watson%22`.

## Operators
{:#operators}

Operators are the separators between different parts of a query. Theses are the available operators:

| Operator | Description | Examples |
|:-------------------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| `.` | Separator for levels of nested objects in the JSON schema (not including fields within intents or entities) | `request.input.text` |
| `:` | Matches if the specified field contains the query term, or a grammatical variant of the query term. Also used to specify fields within intents or entities. Useful for filtering on user input text. | `request.input.text:order` <br/> `response.intents:intent:place_order`|
| `::` | Matches if the specified field contains an exact match to the query term. Useful for filtering on entities or intents. | `response.entities:entity::beverage` |
| `:!` | Matches if the specified field does not contain any variant of the query term. | `request.input.text:!order` |
| `::!` | Matches if the specified field does not contain an exact match to the query term. | `response.intents:intent::!hello` |
| `\` | Escape character for queries that include control characters. For example, `::\!hello` would match the text `!hello`. | `request.input.text::\!hello` |
| `" "` | Encloses a phrase that contains spaces or other special characters. No special characters within a phrase query are parsed by the API. | `request.input.text:"IBM Watson"` |
| `~ + Number` | Specifies an approximate match by indicating the maximum number of single-character differences allowed between the query string and a match in the response object. For example, `car~1` would match `car`, `cat`, or `cars`, but it would not match `cats`. | `request.input.text:Watson~3` |
| `(), []` | Encloses a logical grouping of multiple expressions, using Boolean operators. |  |
| <code>&#124;</code> | Boolean operator for "or". | <code>response.intents:intent:(hello&#124;goodbye)</code> |
| `,` | Boolean operator for "and". | `[response.intents:intent::order,response.entities:entity::beverage]` |
| `<=, >=, >, <` | Comparison operators. | `response.intents:confidence>0.8` |
| `*` | Wildcard that matches any sequence of characters. | `request.input.text:IBM*` |
