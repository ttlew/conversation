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

# Supported languages
The {{site.data.keyword.conversationshort}} service supports the languages listed. Individual features of the service are supported to a greater or lesser extent for each language. In the following table, the level of language and feature support is indicated by these codes:
- **GA** - The feature is generally available and supported for this language. Note that features may continue to be updated even after they are generally available.
- **Ex** - Language support is only Experimental.
- **Beta** - The feature is supported only as a Beta release, and is still undergoing testing before it is made generally available in this language.
- **Blank** - Absence of any code indicates that a feature is not available in this language.
{:shortdesc}

|                  | **[Defining intents](intents.html)**, **[entities](entities.html)**, and **[dialog](dialog-build.html)** | **[Absolute scoring and 'Mark as irrelevant'](intents.html#mark-as-irrelevant)** | **[Entity fuzzy matching](entities.html#fuzzy-matching)** | **System entities ([number](system-entities.html#sys-number), [currency](system-entities.html#sys-currency), [percentage](system-entities.html#sys-percentage), [date, time](system-entities.html#sys-datetime))** |
|:---|:---:|:---:|:---:|:---:|
| **English (en)**                   | GA | GA | Beta (Stemming and misspelling) | GA </br> Beta ([location](system-entities.html#sys-location), [person](system-entities.html#sys-person))|
| **Arabic (ar)**                    | Ex |  |  |  |
| **Brazilian Portuguese (pt-br)**   | GA | Beta |  | GA |
| **Chinese - Simplified (zh-cn)**   | Ex | Beta |  |  |
| **Chinese - Traditional (zh-tw)**  | Ex | Beta |  |  |
| **Dutch (nl)**                     | Ex | Beta |  |  |
| **French (fr)**                    | GA | Beta |  | GA |
| **German (de)**                    | GA | Beta |  | GA |
| **Italian (it)**                   | GA | Beta |  | GA |
| **Japanese (ja)**                  | GA | Beta |  | GA |
| **Korean (ko)**                    | Ex | Beta |  | Beta |
| **Spanish (es)**                   | GA | Beta |  | GA ||

**Note**: Although the {{site.data.keyword.conversationshort}} service supports multiple languages as noted, the tooling interface itself is in English; other languages can be trained through the English interface. The current exception is Arabic, which is supported through the use of the Conversation API, but not through the tooling interface.

## Changing a workspace language

Once a workspace has been created, its language cannot be modified. If it is necessary to change the supported language of a workspace, the user should download the workspace where the language change is required. Then, edit the resulting JSON file in a text editor, searching for a JSON property called `language`.

The `language` property should be set to the original language of the workspace; for example, English would be `en`. Modify the value of this property, changing it to the desired language (`fr` for French, `de` for German, etc.). Save the changes to the JSON file, and import the modified file into your {{site.data.keyword.conversationshort}} service instance.
