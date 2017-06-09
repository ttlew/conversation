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

# Stage 3: Add intents and examples

Add some intents on the Intents tab. Intents are used in your dialog flow.
{:shortdesc}

1.  On the Workspaces page, select the *Car tutorial* workspace tile that you created.
1.  On the Intents tab, select **Create** and add an intent name:

    ```
    turn_on
    ```
    {:screen}

    This intent indicates that the user wants to turn on an appliance such as the radio, windshield wipers, or headlights.
1.  In the **User example** field, type an utterance and press Enter:

    ```
    I need lights
    ```
    {:screen}

1.  Add these 4 more examples to help Watson recognize the #turn_on intent and hit Enter:

    ```
    Listen to some music
    Play some tunes
    Air on please
    Turn on the headlights
    ```
    {:screen}

1.  Repeat the process to create the `#greeting` intent with 5 examples:

    ```
    Hello
    Hi
    Good morning
    Good afternoon
    Good evening
    ```
    {:screen}

You defined two intents, `#turn_on` and `#greeting`, with example utterances. These examples help train Watson to recognize these intents in user input.
