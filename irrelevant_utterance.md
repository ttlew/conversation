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

# Absolute scoring and Irrelevant input

As of February 2017, there is a new technique for scoring intent confidence and returning intents. You can also mark inputs as "irrelevant". 
{:shortdesc}

In the tooling, these changes require an [upgrade to your workspace](upgrading.html). 

For your application, these changes require the message api call to use **2017-02-03**.

## Absolute scoring

We now score each intent’s confidence on its own, not in relation to other intents. Previously, the sum of all intent confidences added to 1. Now, each intent’s confidence can be 1 by itself. As a result, the confidence returned for some inputs may change in relation to what was returned previously.

This allows the flexibility to have multiple intents returned. It also means the system may not return an intent at all. If the system has low confidence (<.2) that any intents relate to the user’s input, the system will not return an intent.

As intent confidence scores change, your dialogs may need restructuring. If you previously conditioned your dialog with an intent that now has low confidence, the system’s response may no longer be correct.

For example, you may have a condition that looks like this:

```json
<?intents[n].confidence >= x?>
```
{:codeblock}

If so, you should retest the dialog after upgrading to ensure the condition is evaluating as expected. This could have changed due to absolute scoring.

If you have `#intent_name` in a condition, that condition may not be matched now, as the confidence could have changed (to <.2). If this happens, you need to add more examples to the intent in question.

## Irrelevant input

**Note** This feature only works for English workspaces.

After upgrading your workspace, you can [test input in the Try it out panel](intents.html#testing-your-intents) to see the changes. You can  "Mark as irrelevant" to indicate that the input is not related to your application.

Inputs marked as irrelevant are stored in the workspace and are included as part of the training data. **They cannot be accessed or changed later in the tooling.**

**Note** If you already have an intent for inputs that are out of scope or off topic, such as #off_topic, you should delete that intent and test your workspace with the use of "Irrelevant".

You are allowed 25,000 irrelevant examples per service instance.
