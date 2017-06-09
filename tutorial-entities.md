---
layout: documentation
title: Conversation | Tutorial step 4
category: Language
---

# Stage 4: Add entities

An entity definition includes a set of entity *values* that can be used to trigger different responses.
Each entity value can have multiple *synonyms*, which define different ways that the same value might be specified in user input.

Create entities to represent what the user wants to turn on.

1.  On the Car tutorial workspace page, click the Entities tab.
If **Entities** is not visible, use the
![Menu](images/Menu_16.png) menu to open the page.

1.  On the Entities tab, click **Create** and add an entity name:

    ```none
    appliance
    ```

  The @appliance entity represents an appliance in the car that a user might want to turn on.
1.  Add a value in the **Value** field

    ```none
    music
    ```


    The value represents a specific appliance that users might want to turn on.
1.  Add another way to specify the music appliance entity in the **Synonyms** field:

    ```none
    radio
    ```


1.  Click the plus sign **(+)** to define additional values for @appliance.
    -  Value: `headlights`. Synonym: `lights`.
    -  Value: `air conditioning`. Synonyms: `air`.
1.  Repeat the process to create the @`genre` entity with 2 values and synonyms:
    -  Value: `classical`. Synonym: `symphonic`.
    -  Value: `rhythm and blues` Synonym: `r&b`
    -  Value: `rock`. Synonym: `pop`

You defined two entities: `@appliance` (representing an appliance the can turn on) and `@genre` (representing a genre of music the user can choose).

When the user's input is received, the {{site.data.keyword.conversationshort}} service identifies both the intents and entities. You can now define a dialog that uses intents and entities to choose the correct response.
