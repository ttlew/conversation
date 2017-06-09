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

# Defining entities

An *entity* represents a class of object or a data type that is relevant to a user's purpose. By recognizing the entities that are mentioned in the user's input, the Conversation service can choose the specific actions to take to fulfill an intent.
{:shortdesc}

@[youtube](oSNF-QCbuDc?rel=0)

## Planning your entities

An *entity* represents a term or object in the user's input that provides clarification or specific context for a particular intent. If intents represent verbs (something a user wants to do), entities represent nouns (such as the object of, or the context for, an action). Entities make it possible for a single intent to represent multiple specific actions. An entity defines a class of objects, with specific values representing possible objects in that class.

For example, suppose your application is a cognitive car dashboard that enables users to turn accessories on or off. Consider the following utterances:

- `Headlights off`
- `Turn the radio off`
- `Stop the air conditioner`

These requests all represent the same intent (to turn something off), which you might call `#turn_off`. You would then use an entity to represent what the user wants to turn off. To do this, you might create an entity called `@accessory`, and give it the following possible values:

- `headlights`
- `radio`
- `air conditioner`

For each value, you can also specify synonyms. For example, you might specify `AC` and `cooling` as synonyms for `air conditioner`. All three strings are treated as the same value. Listing multiple ways that users might refer to the same value helps improve your application's accuracy.

When the user's input is received, the conversation recognizes both intents and entities. Your dialog flow can then use these to provide the best answer. You should only create an entity for something that matters in terms of changing the way the application responds to an intent.

> **Tip:** In addition to the entities you define, the service provides a set of system entities that are already defined and available for you to use. For more information, see [Enabling system entities](#enable_system_entities).

## Entity limits
The number of entities, entity values, and synonyms that you can create in a single service instance depends on your {{site.data.keyword.conversationshort}} service plan:

| Service plan      | Entities per service instance | Entity values per service instance | Entity synonyms per service instance |
|-------------------|------------------------------:|-----------------------------------:|-------------------------------------:|
| Standard/ Premium |                          1000 |                            100,000 |                              100,000 |
| Lite              |                            25 |                            100,000 |                              100,000 |

System entities that you enable for use count toward your plan usage totals.

## Creating an entity

You use the Conversation tool to create entities.

1.  In the Conversation tool, open your workspace and then click the **Entities** tab. If **Entities** is not visible, use the ![Menu](images/Menu_16.png) menu to open the page.
1.  Click **Create new**.
1.  In the **Add the entity name** field, type a descriptive name for the entity.

    The entity name can contain letters (in Unicode), numbers, underscores, and hyphens. For example:
    - `@location`
    - `@menu_item`
    - `@product`

    > **Tip**:
    > - Don't include the `@` character in the entity names when you create them in the Conversation tool.
    > - Entity names can't contain spaces or be longer than 64 characters. And entity names can't begin with the string `sys-`, which is reserved for system entities.
1.  **Fuzzy Matching (BETA)**: Increases the ability of the service to recognize user input terms with syntax that is similar to the entity, but without requiring an exact match. Click the button to select either on or off; fuzzy matching is off by default. This feature is available for languages noted in the [Supported languages](lang-support.html) topic.
1.  In the **Value** field, type the text of a possible value for the intent. An entity value can be any string up to 64 characters in length.
    > **Important**: Don't include sensitive or personal information in entity names or values. The names and values can be exposed in URLs in an app.
1.  In the **Synonyms** field, type any synonyms for the entity value. A synonym can be any string up to 64 characters in length. Press Enter to save each synonym.
    ![Screen capture of defining an entity](images/define_entity.png)
1.  Click **+** and repeat the process to add more entity values.
1.  When you are finished adding values and synonyms, click **Create**.

### Fuzzy matching

You can turn on fuzzy matching per entity to improve the ability of the service to recognize terms in user input with syntax that is similar to the entity, without requiring an exact match. There are two components to fuzzy matching: stemming and misspelling:

-  *Stemming* - The feature recognizes the stem form of entity values that have several grammatical forms. For example, the stem of 'bananas' would be 'banana', while the stem of 'running' would be 'run'.

-  *Misspelling* - The feature is able to map user input to the appropriate corresponding entity despite the presence of misspellings or slight syntactical differences. For example, if you define "giraffe" as a synonym for an animal entity, and the user input contains the terms "giraffes" or "girafe", the fuzzy match is able to map the term to the animal entity correctly.

### Results

The entity you created is added to the **Entities** tab, and the system begins to train itself on the new data.

You can click any entity in the list to open it for editing. You can rename or delete entities, and you can add, edit, or delete values and synonyms.

## Importing entities

If you have a large number of entities, you might find it easier to import them from a comma-separated value (CSV) file than to define them one by one in the Conversation tool.

1.  Collect the entities into a CSV file, or export them from a spreadsheet to a CSV file. The required format for each line in the file is as follows:

    ```xml
    <entity>,<value>,<synonyms>
    ```
    {:codeblock}

    where &lt;entity&gt; is the name of an entity, &lt;value&gt; is a value for the entity, and &lt;synonyms&gt; is a comma-separated list of synonyms for that value.

    ```
    weekday,Monday,Mon
    weekday,Tuesday,Tue,Tues
    weekday,Wednesday,Wed
    weekday,Thursday,Thur,Thu,Thurs
    weekday,Friday,Fri
    weekday,Saturday,Sat
    weekday,Sunday,Sun
    month,January,Jan
    month,February,Feb
    month,March,Mar
    month,April,Apr
    month,May
    ```
    {:screen}

    > **Important**:
    > - Save the CSV file with UTF-8 encoding and no byte order mark (BOM).
    > - The maximum CSV file size is 10MB. If your CSV file is larger, consider splitting it into multiple files and importing them separately.
1.  In the Conversation tool, open your workspace and then click the **Entities** tab.
1.  Click ![Import](images/importGA.png) and then drag a file, or browse to select a file from your computer. The file is validated and imported, and the system begins to train itself on the new data.

### Results

You can view the imported entities on the Entities tab. You might need to refresh the page to see the new entities.

## Exporting entities
{:#export_entities}

You can export a number of entities to a CSV file, so you can then import and reuse them for another Conversation application.

1. On the Entities tab, select ![Export icon](images/ExportIcon.png)

    ![Export and Delete options](images/ExportEntity1.png)

2. Select the entities you want, and click the **Export** button.
    ![Entity selection for Export and Delete buttons](images/ExportEntity2.png)

## Deleting entities
{:#delete_entities}

You can select a number of entities for deletion.

**IMPORTANT**: By deleting entities you are also deleting all associated examples or values, and synonyms, and these items can not be retrieved later. All dialog nodes that reference these entities or values must be updated manually to no longer reference the deleted content.

1. On the Entities tab, select ![Delete icon](images/DeleteIcon.png)

    ![Export and Delete options](images/ExportEntity1.png)

2. Select the entities you want to delete, and click the **Delete** button. **Note**: The delete feature supports bulk delete of entities.

## Enabling system entities
{:#enable_system_entities}

The {{site.data.keyword.conversationshort}} service provides a number of *system entities*, which are common entities that you can use for any application. Enabling a system entity makes it possible to quickly populate your workspace with training data that is common to many use cases.

System entities can be used to recognize a broad range of values for the object types they represent. For example, the `@sys-number` system entity matches any numerical value, including whole numbers, decimal fractions, or even numbers written out as words.

System entities are centrally maintained, so any updates are available automatically. You cannot modify system entities.

1.  On the Entities tab, click **System entities**:

    ![Screen capture of "System entities" tab](images/system_entities_1.png)
    If you haven't created any entities, click **Use system entities**:

    ![Screen capture of "Use system entities" button](images/system_entities_2.png)
1.  Browse through the list of system entities to choose the ones that are useful for your application.
    - To see more information about a system entity, including examples of matching input, click the entity in the list.
    - For details about the available system entities, see [System entities reference](system-entities.html).
1.  Click the toggle switch next to a system entity to enable or disable it. You can also enable or disable all system entities by clicking the **All toggle** switch at the top of the list.

### Results

After you enable system entities, the {{site.data.keyword.conversationshort}} service begins automatically retraining. After training is complete, you can use the entities.
