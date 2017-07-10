---

copyright:
  years: 2015, 2017
lastupdated: "2017-07-03"

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

With the {{site.data.keyword.conversationfull}} service, you can create an application that understands natural-language input and uses machine learning to respond to customers in a way that simulates a conversation between humans.
{: shortdesc}

## How to use the service

This diagram shows the overall architecture of a complete solution:![Flow diagram of the service](images/conversation_arch_overview.png)

- Users interact with your application through the user **interface** that you implement. For example, A simple chat window or a mobile app, or even a robot with a voice interface.

- The **application** sends the user input to the {{site.data.keyword.conversationshort}} service.
    - The application connects to a *workspace*, which is a container for your dialog flow and training data.
    - The service interprets the user input, directs the flow of the conversation and gathers information that it needs.
    - You can connect additional {{site.data.keyword.watson}} services to analyze user input, such as {{site.data.keyword.toneanalyzershort}} or {{site.data.keyword.speechtotextshort}}.

- The application can interact with your **back-end systems** based on the user's intent and additional information. For example, answer question, open tickets, update account information, or place orders. There is no limit to what you can do.

### Implementation

You complete these steps to implement your application:

- **Configure a workspace.** With the easy-to-use graphical environment, you set up the dialog flow and training data for your application.
- **Develop your application.** You code your application to connect to the {{site.data.keyword.conversationshort}} workspace through API calls. You then integrate your app with other systems that you need, including back-end systems and third-party services such as chat services or social media.

## Language support

Language support by feature is detailed in the [Supported languages](lang-support.html) topic.

## Next steps

- [Get started](getting-started.html) with the service
- Try out some [demos](sample-applications.html).
- View the list of [SDKs ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/developer-tools.html){: new_window}.
