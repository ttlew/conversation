---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-08"

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

# Upgrading

## Upgrading your plan
{: #plan-upgrade}

You can explore the {{site.data.keyword.conversationshort}} service extensively with the Lite plan. However, there are limits to what you can do.
{: shortdesc}

### Limits by artifact
For information about artifact limits per plan, see these topics:

- [Workspaces](configure-workspace.html#workspace-limits)
- [Dialog nodes](dialog-build.html#dialog-node-limits)
- [Intents](intents.html#intent-limits)
- [Entities](entities.html#entity-limits)
- [Logs](logs_convo.html#log-limits)

### API call limits
The number of API calls allowed per instance depends on your service plan. See your plan description for details.

If you have a Lite plan and reach your API call limit, but the logs show that you have made fewer calls than expected, remember that the Lite plan stores log information for only 7 days.

To upgrade your plan, complete these steps:

1.  From the {{site.data.keyword.Bluemix_notm}} menu, select **Services** > **Dashboard**.
1.  Select the service instance that you want to upgrade to open it.
1.  Click **Plan** from the navigation pane.
   From here, you can see your current plan and other available plan options, and make changes.

For answers to common questions about subscriptions, see the [Managing billing and usage ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/billing-usage/how_charged.html){: new_window}.

You can learn more about services hosted by IBM Cloud from the following links:

- [Cloud Services terms](http://www.ibm.com/software/sla/sladb.nsf/sla/saas)
- [Cloud Services data security and privacy](http://www.ibm.com/software/sla/sladb.nsf/sla/csdsp)

## Upgrading your workspace
{: #upgrade-workspace}

The {{site.data.keyword.conversationshort}} service regularly adds and updates features. While some of these changes are automatically applied to your workspace, updates that have a large impact do require a manual update.

If an upgrade is available for your workspace, the upgrade icon (![upgrade icon](images/upgrade.png)) is displayed.

**Note**: After you upgrade a workspace, you cannot revert your workspace to a previous version.

To upgrade your workspace, complete the following steps:
1.  [Duplicate your workspace](configure-workspace.html#exporting-and-copying-workspaces).
2.  Upgrade the duplicate workspace.
3.  Test the upgraded workspace.
4.  After addressing any issues, apply the upgrade to your primary workspace by clicking the Upgrade (![upgrade icon](images/upgrade.png)) icon.

    When you upgrade your workspace, the latest version of the API is enabled in the tool, and the "Try it out" pane begins to use the newest features.
5.  Upgrade your application by changing the message API call to use the latest API version.
