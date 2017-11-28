---

copyright:
  years: 2015, 2017
lastupdated: "2017-07-06"

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

# 升级工作空间

Conversation 服务会定期添加和更新功能。虽然其中某些更改会自动应用于工作空间，但那些影响较大的更新仍需要手动进行。
{: shortdesc}

如果您有在 2017 年 2 月 3 日之前创建的实例，并且您的工作空间有可用更新，那么会显示“升级”图标 (![“升级”图标](images/upgrade.png)) 以指示有可用的升级。选择此图标可打开“升级工作空间”对话框。

升级工作空间后，工具中会启用 API 的最新版本，并且“试用”面板开始使用最新的功能。

升级工作空间后，即无法将工作空间还原为先前版本。

## 升级工作空间
要避免工作空间中的功能受到影响，请执行以下操作：

1.  [复制工作空间](configure-workspace.html#exporting-and-copying-workspaces)。
2.  升级复制的工作空间。
3.  在升级后的工作空间中测试是否有问题。

完成问题测试后，通过将消息 API 调用更改为使用 **2017-02-03** 或更高版本，将升级应用于应用程序。

## “跳转至”操作中的更改
**跳转至**操作的处理已于 2017 年 2 月 3 日发行版中进行了更改。

先前，如果跳转至节点的条件，但是该节点或其任何对等节点都没有求值为 true 的条件，那么系统将跳转至根级别节点，并查找其条件与输入匹配的节点。在某些情境中，此处理会产生循环，导致无法继续处理对话。

在新过程下，如果目标节点或其同级都未求值为 true，那么对话将结束。已生成的任何响应都会返回给用户，并且会向应用程序返回错误消息：

```
Goto failed from node DIALOG_NODE_ID.
Did not match the condition of the target node and any of the conditions of its subsequent siblings.
```
{: screen}

下一个用户输入将在对话的根级别进行处理。

如果有目标节点条件求值为 false 的**跳转至**操作，那么此更新可能会更改对话的行为。

如果要复原旧的处理模型，请添加条件为 `true` 的最终对等节点。在响应中，使用将对话树根级别的第一个节点条件作为目标的**跳转至**操作。
