---

copyright:
  years: 2015, 2017
lastupdated: "2017-07-11"

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

# Deploying

After you have configured your workspace and built a working dialog, you can deploy your application by connecting the workspace to the interface your customers will use. You can also connect your workspace to other services (including other {{site.data.keyword.watson}} services). There are various ways you can do this.

- [**Testing in Slack**](test-deploy.html): You can use the test deployment tool to deploy your workspace as a chat bot in a Slack channel in just a few steps. This option is the fastest and easiest way to deploy your workspace for testing, but has limitations.
- [**Building a client application**](develop-app.html): You can use the {{site.data.keyword.watson}} SDK for the language of your choice to develop your own application front end. Your application can then use the {{site.data.keyword.conversationshort}} APIs to communicate with the service, as well as integratating with third-party services and your own business systems.
- [**Building a bot with Botkit**](integrations.html): You can use the Botkit framework to integrate your workspace with social media and messaging channels.
