---

copyright:
  years: 2015, 2017
lastupdated: "2017-09-21"

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

# 使用 API 修改對話

{{site.data.keyword.conversationshort}} REST API 支援以程式設計方式修改對話，而不使用 {{site.data.keyword.conversationshort}} 工具。您可以使用 /dialog_nodes API 來建立、刪除或修改對話節點。

請記住，對話是交互連接節點的樹狀結構，且必須符合某些規則才有效。這表示您對對話節點所做的任何變更都可能會對其他節點或對話的結構造成重疊顯示影響。使用 /dialog_nodes API 修改對話之前，請確定您瞭解所做的變更對其餘對話的影響。

有效的對話一律會滿足下列準則：

- 每一個對話節點都有唯一的 ID（`dialog_node` 內容）。
- 子節點知道其母節點（`parent` 內容）。不過，母節點與其子項無關。
- 節點知道其直屬的上一個同層級（如果有的話）（`previous_sibling` 內容）。這表示給定母項的所有同層級會形成一份鏈結的清單，而且每一個節點都指向上一個節點。
- 給定母項中只能有一個子項是第一個同層級（表示其 `previous_sibling` 為空值）。
- 節點無法指向上一個作為不同母項之子項的同層級。
- 兩個節點都無法指向相同的上一個同層級。
- 節點可以指定另一個接下來要執行的節點（`next_step` 內容）。
- 節點不能是它自己的母項或它自己的同層級。
- 類型為 `response_condition` 的節點不能有子項。

下列範例顯示各種修改如何導致重疊顯示變更。

## 建立節點
{: #create-node}

請考量下列簡單對話樹狀結構：

![範例對話](images/dialog_api_1.png)

我們可以藉由向具有下列內文的 /dialog_nodes 提出 POST 要求，來建立新的節點：

```json
{
  "dialog_node": "node_8"
}
```

對話現在看起來像這樣：

![範例對話 2](images/dialog_api_2.png)

因為已建立 **node_8**，但未指定 `parent` 或 `previous_sibling` 的值，所以它現在是對話中的第一個節點。請注意，除了建立 **node_8** 之外，服務也會修改 **node_1**，讓其 `previous_sibling` 內容指向新的節點。

您可以指定母項及上一個同層級，以在對話中的某個位置建立節點：

```json
{
  "dialog_node": "node_9",
  "parent": "node_2",
  "previous_sibling": "node_5"
}
```

您指定給 `parent` 及 `previous_node` 的值必須有效：

- 兩個值都必須參照現有節點。
- 指定的母項必須與上一個同層級的母項相同（如果上一個同層級沒有母項，則為 `null`）。
- 母項不能是類型為 `response_condition` 的節點。

產生的對話看起來像這樣：

![範例對話 3](images/dialog_api_3.png)

除了建立 **node_9** 之外，服務也會自動更新 *node_6* 的 `previous_sibling` 內容，讓它指向新的節點。

## 將節點移至不同的母項
{: #change-parent}

使用具有下列內文的 POST /dialog_nodes/node_5 方法，將 **node_5** 移至不同的母項：

```json
{
  "parent": "node_1"
}
```

指定的 `parent` 值必須有效：
- 它必須參照現有節點。
- 它不得參照正在修改的節點（節點不能是它自己的母項）。
- 它不得參照正在修改之節點的後代。
- 它不得參照類型為 `response_condition` 的節點。

這會導致下列變更的結構：

![範例對話 4](images/dialog_api_4.png)

這裡發生幾個事件：
- 將 **node_5** 移至其新的母項之後，**node_7** 會隨著它一起移動（因為 **node_7** 的 `parent` 值未變更）。在移動節點時，該節點仍然會保留其所有後代。
- 因為我們未指定 **node_5** 的 `previous_sibling` 值，所以它現在是 **node_1** 下的第一個同層級。
- **node_4** 的 `previous_sibling` 內容已更新為 `node_5`。
- **node_9** 的 `previous_sibling` 內容已更新為 `null`，因為它現在是 **node_2** 下的第一個同層級。

## 重新排序同層級
{: #change-sibling}

現在，讓我們將 **node_5** 設為第二個同層級，而非第一個同層級。作法是使用具有下列內文的 POST /dialog_nodes/node_5 方法：

```json
{
  "previous_sibling": "node_4"
}
```

在修改 `previous_sibling` 時，新值必須有效：
- 它必須參照現有節點
- 它不得參照正在修改的節點（節點不能是它自己的同層級）。
- 它必須參照相同母項的子項（所有同層級都必須具有相同的母項）

結構變更如下：

![範例對話 5](images/dialog_api_5.png)

請注意，再次提醒，**node_7** 會隨著母項一起保留。此外，已修改 **node_4**，因此其 `previous_sibling` 是 `null`，因為它現在是第一個同層級。

## 刪除節點
{: #delete-node}

現在讓我們使用 DELETE /dialog_nodes/node_1 方法來刪除 **node_1**。

結果如下：

![範例對話 6](images/dialog_api_6.png)

請注意，已全部刪除 **node_1**、**node_4**、**node_5** 及 **node_7**。在刪除節點時，也會刪除該節點的所有後代。因此，如果刪除基本節點，則實際上是刪除對話樹狀結構的整個分支。對已刪除節點的任何其他參照（例如 `next_step` 參照）都會變更為 `null`。

此外，**node_2** 會更新為指向 **node_8**，以作為其新的上一個同層級。

## 重新命名節點
{: #rename-node}

最後，使用具有下列內文的 POST /dialog_nodes/node_2 方法，來重新命名 **node_2**：

```json
{
  "dialog_node": "node_X"
}
```

![範例對話 7](images/dialog_api_7.png)

對話的結構未變更，但再次修改多個節點以反映已變更的名稱：

- **node_9** 及 **node_6** 的 `parent` 內容
- **node_3** 的 `previous_sibling` 內容

也會變更對已刪除節點的任何其他參照（例如 `next_step` 參照）。
