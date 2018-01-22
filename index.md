---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-22"

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

# About

With the {{site.data.keyword.conversationfull}} service, you can build a solution that understands natural-language input and uses machine learning to respond to customers in a way that simulates a conversation between humans.
{: shortdesc}

## How to use the service

This diagram shows the overall architecture of a complete solution:![Flow diagram of the service](images/conversation_arch_overview.png)

- Users interact with your application through the user **interface** that you implement. For example, a simple chat window or a mobile app, or even a robot with a voice interface.

- The **application** sends the user input to the {{site.data.keyword.conversationshort}} service.
    - The application connects to a *workspace*, which is a container for your dialog flow and training data.
    - The service interprets the user input, directs the flow of the conversation and gathers information that it needs.
    - You can connect additional {{site.data.keyword.watson}} services to analyze user input, such as {{site.data.keyword.toneanalyzershort}} or {{site.data.keyword.speechtotextshort}}.

- The application can interact with your **back-end systems** based on the user's intent and additional information. For example, answer question, open tickets, update account information, or place orders. There is no limit to what you can do.

## Implementation

Here's how you will implement your conversation:

- **Configure a workspace.** With the easy-to-use graphical environment, set up the training data and dialog for your conversation.

    The training data consists of the following artifacts:
    - **Intents**: Goals that you anticipate your users will have when they interact with the service. Define one intent for each goal that can be identified in a user's input. For example, you might define an intent named *store_hours* that answers questions about store hours. For each intent, you add sample utterances that reflect the input customers might use to ask for the information they need, such as, `What time do you open?`
    - **Entities**: An entity represents a term or object that provides context for an intent. For example, an entity might be a city name that helps your dialog to distinguish which store the user wants to know store hours for.

      As you add training data, a natural language classifier is automatically added to the workspace, and is trained to understand the types of requests that you have indicated the service should listen for and respond to.

    Use the dialog tool to build a dialog flow that incorporates your intents and entities. The dialog flow is represented graphically in the tool as a tree. You can add a branch to process each of the intents that you want the service to handle. You can then add branch nodes that handle the many possible permutations of a request based on other factors, such as the entities found in the user input or information that is passed to the service from your application or another external service.

- **Deploy your workspace.** Deploy the configured workspace to users by connecting it to a front-end user interface, social media, or a messaging channel. Your deployed instance of the {{site.data.keyword.conversationshort}} service is hosted by {{site.data.keyword.cloud_notm}}, the IBM cloud computing platform. (See [Platform overview ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/overview/ibm-cloud.html#overview) for more information.)

Read more about these implementation steps by following these links:

- [Planning your intents and entities](intents-entities.html#planning-your-entities)
- [Dialog overview](dialog-overview.html)
- [Deployment overview](deploy.html)

## Browser support

The {{site.data.keyword.conversationshort}} service tooling requires the same level of browser software as is required by {{site.data.keyword.Bluemix_notm}}. See the {{site.data.keyword.Bluemix_notm}} [Prerequisites ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/overview/prereqs.html#browsers){: new_window} topic for details.

## Language support

Language support by feature is detailed in the [Supported languages](lang-support.html) topic.

## Next steps

- [Get started](getting-started.html) with the service
- Try out some [demos](sample-applications.html).
- View the list of [SDKs ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/developer-tools.html){: new_window}.

Still have questions? Contact [IBM Sales](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970).
