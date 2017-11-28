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

# 升級工作區

Conversation 服務會定期新增及更新特性。其中某些變更自動套用至工作區時，需要手動更新具有大型影響的更新項目。
{: shortdesc}

如果您有一個在 2017 年 2 月 3 日之前建立的實例，而且您的工作區有可用的更新，則會顯示升級圖示 (![升級圖示](images/upgrade.png)) 以指出有升級可用。選取此圖示以開啟「升級工作區」對話框。

在升級工作區時，該工具會啟用 API 的最新版本，而且「試用」畫面會開始使用最新的特性。

升級工作區之後，就無法將工作區回復成舊版本。

## 升級工作區
若要避免阻礙工作區的功能，請執行下列動作：

1.  [複製工作區](configure-workspace.html#exporting-and-copying-workspaces)。
2.  升級重複工作區。
3.  測試已升級工作區中的問題。

在完成問題的測試時，請變更訊息 API 呼叫來使用 **2017-02-03** 或更新版本，以將升級套用至應用程式。

## 跳至動作的變更
2017 年 2 月 3 日的版本已變更**跳至**動作的處理。

先前，如果您跳至某個節點的條件，而且該節點及其任何對等節點都沒有評估為 true 的條件，則系統會跳至根層次節點，並尋找其條件符合輸入的節點。在某些情況下，此處理已建立迴圈，因而導致對話無法進行。

在新的處理程序下，如果目標節點及其對等節點都未評估為 true，則會結束對話。任何已產生的回應都會傳回給使用者，並將錯誤訊息傳回給應用程式：

```
Goto failed from node DIALOG_NODE_ID.
Did not match the condition of the target node and any of the conditions of its subsequent siblings.
```
{: screen}

下一個使用者輸入是在對話的根層次進行處理。

如果您的**跳至**動作是將目標設為其條件為 false 的節點，則這項更新可能會變更對話的行為。

如果您要還原舊的處理模型，請新增具有 `true` 條件的最終對等節點。在回應中，使用**跳至**動作，將目標設為對話樹狀結構中根層次之第一個節點的條件。
