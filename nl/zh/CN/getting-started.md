---

copyright:
  years: 2015, 2017
lastupdated: "2017-08-10"

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

# 入门教程
{: #gettingstarted}

在本简短教程中，我们将介绍 {{site.data.keyword.conversationshort}} 工具，并完成创建第一个会话的过程。
{: shortdesc}

## 开始之前
{: #prerequisites}

如果已经创建服务实例，那么您已经满足以下所有先决条件。请移至步骤 1。
{: tip}

1.  转至 [{{site.data.keyword.conversationshort}} 服务 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.{DomainName}/catalog/services/conversation/){: new_window}，然后注册免费 {{site.data.keyword.Bluemix_notm}} 帐户或登录。
1.  登录后，在 {{site.data.keyword.conversationshort}} 页面的**服务名称**字段中输入 `conversation-tutorial`，然后单击**创建**。

## 步骤 1：启动工具
{: #launch-tool}

创建服务实例后，您将转至该实例的仪表板。从此处可启动 {{site.data.keyword.conversationshort}} 工具。

单击**管理**，然后单击**启动工具**。

![显示可启动工具的 IBM Bluemix Watson“管理”页面](images/gs-launch-tool.png)

系统可能会提示您单独登录到该工具。如果是这样，请提供您的 IBM Bluemix 凭证进行登录。

## 步骤 2：创建工作空间
{: #create-workspace}

在 {{site.data.keyword.conversationshort}} 工具中的第一步是创建工作空间。

[*工作空间*](configure-workspace.html)是一种容器，用于容纳定义会话流的工件。

1.  在 {{site.data.keyword.conversationshort}} 工具中，单击**创建**。
1.  为工作空间指定名称`会话教程`，然后单击**创建**。您将转至新工作空间的**意向**选项卡。

![显示用户创建“会话教程”工作空间的动画。](images/gs-create-workspace-animated.gif)

## 步骤 3：创建意向
{: #create-intents}

[意向](intents.html)表示用户输入的目的。可以将意向视为用户可能希望应用程序执行的操作。

对于此示例，我们将简化工作，仅定义两个意向：一个用于打招呼，一个用于说再见。

1.  确保位于“意向”选项卡上。（如果您刚刚创建工作空间，那么应该已经位于该选项卡上。）
1.  单击**新建**。
1.  将意向命名为 `hello`。
1.  输入 `hello` 作为**用户示例**，然后按 Enter 键。

   *示例*告知 {{site.data.keyword.conversationshort}} 服务您希望意向与哪种类型的用户输入匹配。提供的示例越多，服务识别用户意向的准确性就越高。
1.  再添加四个示例，然后单击**完成**以完成创建 #hello 意向：
    - `good morning`
    - `greetings`
    - `hi`
    - `howdy`

   ![显示用户使用示例发声创建 #hello 意向的动画。](images/gs-add-intents-animated.gif)

1.  使用以下五个示例创建另一个名为 #goodbye 的意向：
    - `bye`
    - `farewell`
    - `goodbye`
    - `I'm done`
    - `see you later`

您已创建了两个意向 #hello 和 #goodbye，并提供了示例用户输入来培训 {{site.data.keyword.watson}}，以在用户输入中识别这些意向。

![显示列出 #goodbye 和 #hello 意向的“意向”页面](images/gs-add-intents-result.png)

## 步骤 4：构建对话
{: #build-dialog}

[对话](dialog-build.html)以逻辑树的形式定义会话流。树的每个节点都有一个根据用户输入进行触发的条件。

我们将创建一个简单的对话，用于处理 #hello 和 #goodbye 意向，每个意向通过单个节点处理。

### 添加开始节点

1.  在 {{site.data.keyword.conversationshort}} 工具中，单击**对话**选项卡。
1.  单击**创建**。您将看到两个节点：
    - **欢迎**：包含用户首次使用机器人时向用户显示的问候语。
    - **其他**：包含用于在无法识别用户输入时对用户进行回复的短语。

    ![显示具有“欢迎”和“其他”节点的对话树](images/gs-add-dialog-node-animated-cover.png)
1.  单击**欢迎**节点以在编辑视图中将其打开。
1.  将缺省响应替换为文本`欢迎使用会话教程！`。

    ![显示在编辑视图中打开的“欢迎”节点](images/gs-edit-welcome-node.png)
1.  单击 ![关闭](images/close.png) 以关闭编辑视图。

您已创建由 `welcome` 条件触发的对话节点，这是一种特殊条件，用于指示用户启动了新会话。节点指定新会话开始时，系统应该使用欢迎消息进行响应。

### 测试开始节点

可以随时测试对话以验证对话。现在我们将测试对话。

- 单击 ![Ask Watson](images/ask_watson.png) 图标以打开“试用”窗格。您应该会看到欢迎消息。

    ![测试对话节点](images/gs-tryitout-welcome-node.png)

### 添加节点以处理意向

现在，我们将添加节点来处理`欢迎`节点和`其他`节点之间的意向。

1.  单击**欢迎**节点上的“更多”图标 ![“更多”选项](images/kabob.png)，然后选择**在下方添加节点**。
1.  在此节点的**输入条件**字段中输入 `#hello`。然后选择 **#hello** 选项。
1.  添加响应：`您好。`
1.  单击 ![关闭](images/close.png) 以关闭编辑视图。

   ![显示用户向对话添加 hello 节点的动画](images/gs-add-dialog-node-animated.gif)
1.  单击此节点上的“更多”图标 ![“更多”选项](images/kabob.png)，然后选择**在下方添加节点**以创建对等节点。在对等节点中，指定 `#goodbye` 作为条件，指定`好的，再会。` 作为响应。

    ![为意向添加节点](images/gs-add-dialog-nodes-result.png)

### 测试意向识别

您已构建了一个简单的对话来识别并响应 hello 和 goodbye 输入。下面我们来看看效果如何。

1.  单击 ![Ask Watson](images/ask_watson.png) 图标以打开“试用”窗格。将再次显示欢迎消息。
1.  在窗格的底部，输入 `Hello`，然后按 Enter 键。输出指示已识别到 #hello 意向，并显示相应的响应（`您好。`）。
1.  请尝试进行以下输入：
    - `bye`
    - `howdy`
    - `see ya`
    - `good morning`
    - `sayonara`

   ![显示用户在“试用”面板中测试对话的动画](images/gs-test-dialog-animated.gif)

即便您的输入与所包含的示例不完全匹配，{{site.data.keyword.watson}} 也可以识别到您的意向。此对话使用意向来识别用户输入的目的，而不考虑使用的措辞是否准确，然后以您指定的方式进行响应。

### 构建对话的结果

好了。您已创建具有两个意向的简单会话，并创建了一个对话来识别它们。

## 步骤 5：查看样本工作空间
{: #review-sample-workspace}

打开样本工作空间可查看与刚才创建的意向类似的意向以及其他更多意向，还能查看意向在复杂对话中的使用方式。

1.  返回到“工作空间”页面。可以单击导航菜单中的 ![“返回到工作空间”按钮](images/workspaces-button.png) 按钮。
1.  在**汽车仪表板 - 样本**工作空间磁贴上，单击**编辑样本**按钮。

    ![显示“工作空间”页面上的“汽车仪表板”样本磁贴](images/gs-workspace-car-sample.png)

## 后续操作
{: #next-steps}

本教程围绕一个简单的示例构建。对于真正的应用程序，您将需要定义一些更有意思的意向、一些实体和更复杂的对话。

- 尝试高级[教程](tutorial.html)以添加实体并弄清楚用户的目的。
- 通过将工作空间连接到前端用户界面、社交媒体或消息传递频道，[部署](deploy.html)工作空间。
- 查看[样本应用程序](sample-applications.html)。
