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

# Absolute scoring and Mark as irrelevant

As of February 2017, there is a new algorithm for scoring intent confidence and returning intents. You can also mark inputs as "irrelevant". These changes require an [upgrade to your workspace](upgrading.html).
{:shortdesc}

## Absolute scoring

We now score each intent’s confidence on its own, not in relation to other intents. This allows the flexibility to have multiple intents returned. It also means the system may not return an intent at all. If the system has low confidence (<.2) that any intents relate to the user’s input, the system will not return an intent.

As intent confidence scores change, your dialogs may need restructuring. For example, if you conditioned your dialog with an intent that now has low confidence, the system’s response will no longer be correct.

## Mark as irrelevant

**Note** Refer to [Supported languages](lang-support.html) for this feature.

After upgrading your workspace, you can [test input in the Try it out panel](intents.html#testing-your-intents) to see the changes. You can  "Mark as irrelevant" to indicate that the input is not related to your application.

Inputs marked as irrelevant are stored in the workspace and are included as part of the training data. **They cannot be accessed or changed later in the tooling.**

**The only way to remove the "Irrelevant" tag would be to use the same input in the Try it out panel and then change the intent. Be positive you want to make this change.**

**Note** If you already have an intent for inputs that are out of scope or off topic, such as #off_topic, you should delete that intent and test your workspace with the use of "Irrelevant".
