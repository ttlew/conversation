---

copyright:
  years: 2015, 2017
lastupdated: "2017-07-07"

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

# Recommendations
This page presents recommendations for ways to improve your system.
{: shortdesc}

This feature is Beta only.
{: tip}

This feature is available only to Premium users.
{: tip}

By analyzing the conversations that users have had with your bot, and taking into account your system's current training data and response certainty, you will be presented with actions you can take to easily and efficiently improve your bot.
![Recommendations tab](images/RecommendTop.png)

Recommendations are generated nightly, and require a large volume of user messages, for example, more than 50.
{: tip}

## Improve existing intents
This recommendation involves taking individual phrases entered by users, that the system does not recognize, and then presenting them to you to select an intent for each phrase. This will help your bot better understand what your users are saying.

Click **Start** to begin identifying intents.
![Improve existing intents page](images/rec_improve_intent.png)

When you enter or exit **Improve existing intents**, the progress bar shows how many phrases you have acted on in the current session, out of the total phrases left for the day. Note that if you exit and reenter, the progress bar will start from "empty" again, but that does not mean your previous work was lost - it just won’t count towards the progress of the current session.

Select the best intent for a phrase from the list provided, or select *Mark as irrelevant*. The phrases are added to intents as examples (added as training data) as soon as you click **Save**.

The *Skip to Next* button allows you to skip the current phrase, and move to the next one. The skipped phrase will not be shown again if you exit and reenter **Improve existing intents** during the same day, but it may appear again in subsequent days.

![Improve existing intents edit page](images/rec_improve_intent2.png)
