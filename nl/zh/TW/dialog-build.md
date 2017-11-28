---

copyright:
  years: 2015, 2017
lastupdated: "2017-10-19"

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

# 建置對話
{: #dialog-build}

此對話會使用使用者輸入中所識別的目的和實體以及應用程式的環境定義來與使用者互動，最後提供有用的回應。
{: shortdesc}

回應可能是 `Where can I get some gas?` 這類問題的回答，或是執行打開收音機這類的指令。目的和實體資訊可能足以識別正確的回應，或是對話可能要求使用者提供更多正確回應所需的輸入。例如，如果使用者詢問 "Where can I get some food?"，您可能會想要釐清他們是要在餐廳還是雜貨店吃晚餐還是帶走，依此類推。您可以要求文字回應中有更多詳細資料，並建立一個以上的子節點來處理新的輸入。

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/oQUpejt6d84?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## 對話概觀
{: #overview}

在工具中，您的對話會以圖形方式呈現為樹狀結構。建立一個分支來處理每一個您要交談處理的目的。分支由多個節點組成。

### 對話節點

每一個對話節點至少都包含一個條件及一個回應。

![顯示放入包含陳述式 If: CONDITION, Then: RESPONSE 之方框的使用者輸入](images/node1-empty.png)

- 條件：指定在要觸發的對話中，必須存在於此節點之使用者輸入中的資訊。資訊可能是特定目的、實體值或環境定義變數值。如需相關資訊，請參閱[條件](#conditions)。
- 回應：服務用來回應使用者的詞語。回應也可以配置成觸發程式化動作。如需相關資訊，請參閱[回應](#responses)。

您可以將節點視為具有 if/then 建構：如果此條件為 true，則傳回此回應。

例如，如果服務的自然語言處理程序功能判定使用者輸入包含 `#cupcake-menu` 目的，則會觸發下列節點。因此，在觸發節點時，服務會回應適當的回答。

![顯示使用者詢問 cupcake 特性。If 條件是 #cupcake-menu，而 Then 回應是 cupcake 特性的清單。](images/node1-simple.png)

具有一個條件及回應的單一節點可以處理簡單使用者要求。但是，使用者通常會有更複雜的問題，或需要協助來完成更複雜的作業。您可以新增子節點，要求使用者提供服務所需的任何其他資訊。

![顯示對話中的第一個節點詢問使用者想要的 cupcake 類型（gluten-free 或 regular），而且具有兩個子節點可根據使用者的回答來提供不同的回應。](images/node1-children.png)

### 對話流程

服務會從上到下處理所建立的對話。

![3 個節點旁的箭頭指向下方，顯示對話從上向下流動](images/node-flow-down.png)

在樹狀結構中向下流動時，如果服務發現符合的條件，則會觸發該節點。然後，會在觸發的節點上從左向右移動，以根據任何子節點條件來檢查使用者輸入。檢查子節點時，會再次從上到下移動。

服務會繼續在對話樹狀結構中從上到下、從左到右，然後從上到下、從左到右地運作，直到到達所追蹤分支中的最後一個節點。

![顯示從上指向下的箭頭 1、從左指向右的箭頭 2，以及從上指向下的箭頭 3，一次一個節點層次。](images/node-flow.png)

在開始建置對話時，您必須決定要併入的分支，以及放置它們的位置。分支的順序十分重要，因為會從上到下評估節點。會使用其條件符合輸入的第一個基本節點；不會觸發樹狀結構中較低位置的任何節點。

## 條件
{: #conditions}

節點條件決定是否在交談中使用該節點。回應條件決定要向使用者顯示的回應。
您可以使用任何組合中的下列其中一個以上構件來定義條件：

- **環境定義變數**：如果所指定的環境定義變數表示式為 true，則會使用節點。請使用語法 `$variable_name:value` 或 `$variable_name == 'value'`。請參閱[環境定義變數](#context)。

  請不要根據在其中設定環境定義變數值的相同對話節點中的環境定義變數值，來定義節點或回應條件。
  {: tip}

- **實體**：在使用者輸入中辨識到實體的任何值或同義字時，會使用節點。請使用語法 `@entity_name`。例如，`@city`。

  請務必建立對等節點來處理未辨識到任何實體值或同義字的情況。
  {: tip}

- **實體值**：如果在使用者輸入中偵測到實體值，則會使用節點。請使用語法 `@entity_name:value`。例如：`@city:Boston`。請指定針對實體所定義的值，而不是同義字。

  如果檢查對等節點中是否存在實體，但未指定實體的特定值，則請務必將檢查這個特定實體值的節點放在實體的上面。
{: tip}

- **目的**：最簡單的條件是單一目的。如果使用者的輸入對映至該目的，則會使用節點。請使用語法 `#intent-name`。例如，`#weather` 會檢查在使用者輸入中偵測到的目的是否為 `weather`。如果是，則會處理節點。

- **特殊條件**：可用來執行一般對話功能之服務所隨附的條件。

  <table>
  <tr>
    <td>條件名稱</td>
    <td>說明</td>
  </tr>
  <tr>
    <td>anything_else</td>
    <td>您可以在對話尾端使用此條件，以在使用者輸入不符合任何其他對話節點時進行處理。此條件會觸發 **Anything else** 節點。</td>
  </tr>
  <tr>
    <td>conversation_start</td>
    <td>如同 **welcome**，在第一次對話開啟期間，會將此條件評估為 true。與 **welcome** 不同的是，不論應用程式的起始要求是否包含使用者輸入，它都會是 true。具有 **conversation_start** 條件的節點可以用來起始設定環境定義變數，或在對話開頭執行其他作業。</td>
  </tr>
  <tr>
    <td>false</td>
    <td>一律將此條件評估為 false。您可以在開發中分支的頂端使用此條件，以避免它被使用，或是用作提供一般功能之節點的條件，而且只能用作**跳至**動作的目標。</td>
  </tr>
  <tr>
    <td>irrelevant</td>
    <td>如果 Conversation 服務判定使用者輸入不相關，則會將此條件評估為 true。</td>
  </tr>
  <tr>
    <td>true</td>
    <td>一律將此條件評估為 true。您可以在節點或回應清單尾端使用此條件，以捕捉任何不符合任何先前條件的回應。</td>
  </tr>
  <tr>
    <td>welcome</td>
    <td>只有在應用程式的起始要求不包含任何使用者輸入時，才會在第一次對話開啟期間（交談開始時），將此條件評估為 true。在所有後續對話開啟中，會將它評估為 false。此條件會觸發 **Welcome** 節點。一般而言，使用此條件的節點是用來歡迎使用者，例如，顯示 "Welcome to our Pizza ordering app." 這類訊息。</td>
  </tr>
  </table>

### 條件語法

請使用下列其中一個語法選項，在條件中建立有效的表示式：

- 「Spring 表示式 (SpEL)」語言是一種表示式語言，支援在運行環境查詢及操作物件圖形。如需相關資訊，請參閱 [Spring 表示式語言 (SpEL) 語言 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html){: new_window}。

- 參照目的、實體及環境定義變數的速記表示法。請參閱[存取及評估物件](expression-language.html)。

請使用正規表示式來檢查條件的值。例如，若要尋找相符字串，您可以使用 `String.find` 方法。如需詳細資料，請參閱[方法](dialog-methods.html)。

### 條件用法提示

- 如果只要評估實體類型之第一個偵測到實例的值，您可以使用 `@entity == 'specific-value'` 語法，而非 `@entity:(specific-value)` 格式。例如，在使用 `@appliance == 'air conditioner'` 時，只會評估第一個偵測到 `@appliance` 實體的值。但是，使用 `@appliance:(air conditioner)` 則會展開為 `entity['appliance'].contains('air conditioner')`，如此只要在使用者輸入中偵測到至少有一個值 'air conditioner' 的 `@appliance` 實體就會相符。
- 使用數值變數時，請確定變數具有值。如果變數沒有值，則在數值比較中會將它視為具有空值 (0)。例如，如果您檢查含條件 `@price < 100` 的變數值，而 @price 實體是空值，則會將條件評估為 `true`，因為 0 小於 100，即使未曾設定價格。若要防止檢查空值變數，請使用 `@price AND @price < 100` 這類條件。如果 @price 沒有值，則此條件會正確地傳回 false。
- 如果使用實體作為條件，並已啟用模糊符合，則只有在相符項的信任大於 30% 時，才會將 `@entity_name` 評估為 true。亦即，只有在 `@entity_name.confidence > .3` 時。

## 回應
{: #responses}

對話回應定義如何回覆使用者。

您可以回覆下列其中一種回應類型：

- [簡單文字回應](#simple-text)
- [多個條件式回應](#multiple)
- [複雜回應](#complex)

### 簡單文字回應
{: #simple-text}

如果要提供文字回應，則只需要輸入您要服務向使用者顯示的文字。

![顯示用於顯示使用者詢問 "Where are you located" 的節點，而且對話回應為 "We have no brick and mortar stores! But, with an internet connection, you can shop us from anywhere"](images/response-simple.png)

#### 新增變化
{: #variety}

如果使用者經常回到交談服務，他們可能會覺得每次都聽到相同的問候語及回應十分無聊。您可以對回應新增*變異*，讓交談能夠以不同方式回應相同的狀況。

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/nAlIW3YPrAs?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

在此範例中，服務針對回應商店位置問題所提供的回答，在不同互動之間會不同：

![顯示用於顯示使用者詢問 "Where are you located" 的節點，而且對話定義了三個不同的回應"](images/variety.png)

您可以選擇按順序或以隨機順序來輪替回應變異。依預設，會按順序輪替回應，就像它們是從排序清單中選擇的一樣。

### 條件式回應
{: #multiple}

單一對話節點可以提供不同的回應，每一個回應都是由不同的條件所觸發。您可以使用此方法來處理單一節點中的多種情境。

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/KcvVQAsnhLM?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

節點仍然有一個主要條件，這是使用節點以及處理其所包含條件和回應的條件。

在此範例中，服務會使用先前所收集的使用者位置資訊來修改其回應，並提供離使用者最近之商店的相關資訊。如需如何儲存收集自使用者之資訊的相關資訊，請參閱[環境定義變數](#context)。

![顯示用於顯示使用者詢問 "Where are you located" 的節點，而且對話有三個不同的回應（取決的條件是使用 $state 環境定義變數中的資訊來指定那些州中的位置）"](images/multiple-responses.png)

這個單一節點現在提供四個不同節點的對等功能。

會依序評估節點內的條件，就像評估節點一樣。請確定條件及回應是依正確順序列出。如果您需要變更順序，請選取條件，然後使用出現的箭頭在清單中上下移動。如果您要更新環境定義，則必須在每一個個別回應中執行此動作。沒有一般回應區段。選取並遞送回應之後，會處理**跳至**動作。如果您新增**跳至**動作，則會在任何從節點傳回的回應之後執行此動作。
{: tip}

### 複雜回應
{: #complex}

若要指定更複雜的回應，您可以使用 JSON 編輯器在 `"output":{}` 內容中指定回應。

若要在回應中包括環境定義變數值，請使用語法 `$variable-name` 進行指定。如需相關資訊，請參閱[環境定義變數](#context)。

```json
{
  "output": {
    "text": "Hello $user"
  }
}
```
{: codeblock}

若要指定多個要顯示在不同行的陳述式，請將輸出定義為 JSON 陣列。

```json
{
  "output": {
    "text": ["Hello there.", "How are you?"]
  }
}
```
{: codeblock}

第一個句子會顯示在一行，而第二個句子則會顯示為其下的新行。

若要實作更複雜的行為，您可以將輸出文字定義為複雜 JSON 物件。例如，您可以在 JSON 輸出中使用複雜物件，來模擬將回應變異新增至節點的行為。您可以在複雜物件中包括下列內容：

- **values**：JSON 字串陣列，保留這個對話節點可傳回之輸出文字的多個版本。陣列中值的傳回順序取決於屬性 `selection_policy`。

- **selection_policy**：下列是有效值：

    - **random**：系統會隨機從 `values` 陣列中選取輸出文字，而且不會連續重複。例如，考慮使用包含三個值的 output.text。前三次會選取隨機值，但下一次不會重複。指定所有輸出值之後，系統會隨機選取另一個值，並重複該處理程序。

        ```json
        {
            "output":{
                "text":{
                    "values":["Hello.","Hi.","Howdy!"],
                    "selection_policy":"random"
                }
            }
        }
        ```
        {: codeblock}

    系統會從這三個選項中傳回隨機挑選的某個問候語。下次觸發此回應時，會顯示清單中的另一個問候語。再次隨機選擇問候語，但故意不重複先前已使用的問候語。

    - **sequential**：系統在第一次觸發對話節點時會傳回第一個輸出文字、第二次觸發節點時會傳回第二個輸出文字，依此類推。

        ```json
        {
            "output":{
                "text":{
                    "values":["Hello.", "Hi.", "Howdy!"],
                    "selection_policy":"sequential"
                }
            }
        }
        ```
        {: codeblock}

- **append**：指定要將值附加至陣列，還是將陣列中的值改寫為新的值。設為 false 時，先前執行對話節點時所收集的輸出會改寫為這個特定節點中所指定的文字值。

    ```json
    {
        "output":{
            "text":{
                "values": ["Hello."],
                "append":false
            }
        }
    }
    ```
    {: codeblock}

    在此情況下，會將所有其他輸出文字都改寫為此輸出文字。

預設行為假設 `selection_policy = random` 及 `append = true`。values 陣列包含多個項目時，會從其元素中隨機選取輸出文字。

### 定義下一步
{: #jump-to}

做出指定的回應之後，您可以指示服務執行下列其中一項作業：

- **等待使用者輸入**：服務會等待使用者提供回應所引起的新輸入。例如，回應可能會向使用者詢問「是/否」問題。除非使用者提供其他輸入，否則不會進行對話。
- **跳至另一個對話節點**：如果您不要等待使用者輸入，而是想要直接與子節點或完全不同的對話節點交談，請使用此選項。例如，您可以使用「跳至」動作，將流程從樹狀結構中的多個位置遞送至一般對話節點。
  >附註：您要跳至的目標節點必須存在，才能配置「跳至」動作使用該節點。

#### 配置跳至動作

如果您選擇跳至另一個節點，則必須指定動作的目標是設為所選取對話節點的**回應**還是**條件**

- **回應**：如果陳述式的目標設為所選取對話節點的回應部分，則會立即執行。亦即，系統不會評估所選取對話節點的條件部分，且立即執行所選取對話節點的回應部分。

  將目標設為回應適用於將數個對話節點鏈結在一起。如果這個對話節點的條件是 true，則會處理所選取對話節點的回應部分。如果選取的對話節點有另一個**跳至**動作，則也會立即執行該動作。
- **條件**：如果陳述式的目標設為所選取對話節點的條件部分，則服務會先檢查目標節點的條件是否評估為 true。
    - 如果條件評估為 true，則系統會立即處理這個節點，方法是使用對話節點環境定義來更新環境定義，以及使用對話節點輸出來更新輸出。
    - 如果條件未評估為 true，則系統會繼續評估目標對話節點之下一個同層級節點的條件，依此類推，直到找到將條件評估為 true 的對話節點為止。
    - 如果系統處理所有同層級，而且沒有條件評估為 true，則會使用基本備用策略，而且對話也會評估最上層的節點。

    將目標設為條件適用於鏈結對話節點的條件。例如，您可能要先檢查輸入是否包含目的（例如 `#turn_on`），而且，如果包含目的，則您可能要檢查輸入是否包含實體（例如 `@lights`、`@radio` 或 `@wipers`）。鏈結條件有助於建構較大的對話樹狀結構。

**附註**：2017 年 2 月 3 日的版本已變更**跳至**動作的處理。如需詳細資料，請參閱[升級工作區 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](upgrading.html){: new_window}。

#### 相關資訊

如需對話所使用表示式語言的相關資訊，以及方法、系統實體和其他有用詳細資料，請參閱「導覽」窗格中的「參照」區段。

## 環境定義變數
{: #context}

對話是無狀態的，表示它不會在與使用者的不同交換之間保留資訊。您的應用程式負責維護任何所需的繼續資訊。不過，應用程式可以將資訊傳遞給對話，而對話可以更新此資訊，並將它傳回給應用程式。作法是使用環境定義變數。

環境定義變數是您在節點中定義的變數，並選擇性地指定其預設值。其他節點或應用程式邏輯可以隨後設定或變更環境定義變數的值。 

環境定義變數值的條件設定方式是從對話節點條件中參照環境定義變數以判斷是否執行節點。而且，您可以從對話節點回應條件中參照環境定義變數，以根據外部服務或使用者所提供的值來顯示不同的回應。

### 從應用程式傳遞環境定義

設定環境定義變數並將環境定義變數傳遞給對話，即可將資訊從應用程式傳遞給對話。

例如，您的應用程式可以設定 $time_of_day 環境定義變數，並將它傳遞給對話，而此對話可以使用該資訊來修改向使用者顯示的問候語。

![顯示 Welcome 節點，其使用回應條件來檢查從應用程式傳遞給對話的 $time_of_day 環境定義變數值。](images/set-context.png)

在此範例中，對話知道應用程式將變數設為下列其中一個值：*morning*、*afternoon* 或 *evening*。它可以檢查每一個值，並視呈現的值傳回適當的問候語。如果未傳遞變數，或變數的值不符合其中一個預期值，則會向使用者顯示更通用的問候語。

### 將環境定義從節點傳遞至節點

對話也可以新增環境定義變數以在不同的節點之間傳遞資訊，或是更新環境定義變數的值。對話詢問並取得使用者的相關資訊時，可以追蹤資訊，而且稍後可以在交談中進行參照。

例如，在某個節點中，您可能會詢問使用者的名稱，並在後面的節點中，依名稱來處理使用者。

![顯示簡介節點，其詢問使用者的名稱，並將名稱儲存為環境定義變數。下一個節點會使用 $username 環境定義變數，依名稱來參照使用者。](images/set-context-username.png)

在此範例中，如果使用者提供名稱，則會使用系統實體 @sys-person 從輸入中擷取使用者名稱。在 JSON 編輯器中，會定義 username 環境定義變數，並將其設為 @sys-person 值。在後續節點中，回應中會包括 $username 環境定義變數，以依名稱來處理使用者。

### 定義環境定義變數

藉由將 `name` 及 `value` 配對新增至 JSON 對話節點定義的 `{context}` 區段，即可定義環境定義變數。配對必須符合下列需求：

- `name` 可以包含任何大寫和小寫英文字母、數值字元 (0-9) 及底線。

  **附註**：您可以在名稱中包括其他字元（例如句點及連字號）。不過，如果您這麼做，則必須使用下列其中一種方法來參照變數：
  - context['variable-name']: The：完整 SpEL 表示式語法。
  - $(variable-name)：以括弧括住變數名稱的速記語法。

  如需詳細資料，請參閱[存取及評估物件](expression-language.html#shorthand-syntax-for-context-variables)。

- `value` 可以是任何支援的 JSON 類型，例如簡單字串變數、數字、JSON 陣列或 JSON 物件。

下列 JSON 範例定義 $dessert 字串、$toppings_array 陣列及 $age 數字環境定義變數的值：

```json
{
  "context": {
    "dessert": "ice-cream",
    "toppings_array": ["onion", "olives"],
    "age": 18
  }
}
```
{: codeblock}

若要定義環境定義變數，請完成下列步驟：

1.  從節點的編輯視圖中，按一下 ![進階回應](images/kabob.png) 圖示，然後選取 **JSON**，以開啟 JSON 編輯器。

1.  在 `"output":{}` 區塊的前面，新增 `"context":{}` 區塊（如果不存在的話）。

    ```json
    {
      "context":{},
      "output":{}
    }
    ```
    {: codeblock}

1.  在環境定義區塊中，針對您要定義的每一個環境定義變數，新增一個名稱/值配對。

    ```json
    {
      "context":{
        "name": "value"
    },
    ...
    }
    ```
    {: codeblock}

  隨後，若要參照環境定義變數，請使用語法 `$name`，其中 *name* 是已定義之環境定義變數的名稱。

其他一般作業包括：

- 若要儲存使用者輸入的整個字串，請使用 `input.text`：

    ```json
    {
      "context": {
        "repeat": "<?input.text?>"
      }
    }
    ```
    {: codeblock}

- 若要將實體的值儲存在環境定義變數，請使用下列語法：

    ```json
    {
      "context": {
        "place": "@place"
      }
    }
    ```
    {: codeblock}

- 若要將使用正規表示式從使用者輸入中擷取的字串值儲存在環境定義變數，請使用下列語法：

    ```json
    {
      "context": {
         "number": "<?input.text.extract('^[^\\d]*[\\d]{11}[^\\d]*$',0)?>"
      }
    }
    ```
    {: codeblock}

- 若要將型樣實體的值儲存在環境定義變數，請在實體名稱後面附加 .literal。使用此語法可確保使用者輸入中符合所指定型樣的確切文字段會儲存在變數。

    ```json
    {
      "context": {
        "email": "@email.literal"
      }
    }
    ```
    {: codeblock}

### 作業順序
{: #order-of-context-var-ops}

定義環境定義變數的順序不會決定服務的評估順序。服務會隨機評估定義為 JSON 名稱/值配對的變數。請不要在第一個環境定義變數中設定值，並預期可在第二個環境定義變數中使用該值，因為不保證會先執行清單中的第一個環境定義變數，再執行清單中的第二個環境定義變數。例如，請不要使用兩個環境定義變數來實作邏輯，以傳回零與傳遞至節點之某個較高值之間的亂數。

```json
"context": {
    "upper": "<? @sys-number.numeric_value + 1?>",
    "answer": "<? new Random().nextInt($upper) ?>"
}
```
{: codeblock}

使用略為複雜的表示式，避免必須依賴在評估 $answer 環境定義變數之前評估的 $upper 環境定義變數值。

```json
"context": {
    "answer": "<? new Random().nextInt(@sys-number.numeric_value + 1) ?>"
}
```
{: codeblock}

### 更新環境定義變數值
{: #updating-a-context-variable-value}

如果節點設定已設定之環境定義變數的值，則會改寫前一個值。

#### 更新複雜 JSON 物件

會改寫所有 JSON 類型的先前值，但 JSON 物件除外。如果 context 變數是複雜類型（例如 JSON 物件），則會使用 JSON 合併程序來更新變數。合併作業會新增任何新定義的內容，並改寫物件的任何現有內容。

在此範例中，會將名稱環境定義變數定義為複雜物件。

```json
{
  "context": {
    ...
    "complex_object": {
      "user_firstname" : "Paul",
      "user_lastname" : "Pan"
      "has_card" : false
    }
  }
}
```
{: codeblock}

對話節點會將環境定義變數 JSON 物件更新為下列值：

```json
{
  "complex_object": {
    "user_firstname": "Peter",
    "has_card": true
  }
}
```
{: codeblock}

結果是下列環境定義：

```json
{
  "complex_object": {
    "user_firstname": "Peter",
    "user_lastname": "Pan",
    "has_card": true
  }
}
```
{: codeblock}

#### 更新陣列

如果對話環境定義資料包含值的陣列，您可以藉由附加值、移除值或取代所有值來更新陣列。

選擇下列其中一個動作來更新陣列。在每一種情況下，我們都會看到在動作前面的陣列、動作，以及套用動作之後的陣列。

- **附加**：若要將值新增至陣列尾端，請使用 `append` 方法。

    針對此「對話」運行環境定義：

    ```json
    {
      "context": {
        "toppings_array": ["onion", "olives"]
      }
    }
    ```
    {: codeblock}

    進行下列更新：

    ```json
    {
      "context": {
        "toppings_array": "<? $toppings_array.append('ketchup', 'tomatoes') ?>"
      }
    }
    ```
    {: codeblock}

    結果：

    ```json
    {
      "context": {
        "toppings_array": ["onion", "olives", "ketchup", "tomatoes"]
      }
    }
    ```
    {: codeblock}

- **移除**：若要移除元素，請使用 `remove` 方法，並在陣列中指定其值或位置。

    - **依值移除**：會依元素值從陣列中移除元素。

        針對此「對話」運行環境定義：

        ```json
        {
          "context": {
            "toppings_array": ["onion", "olives"]
          }
        }
        ```
        {: codeblock}

        進行下列更新：

        ```json
        {
          "context": {
            "toppings_array": "<? $toppings_array.removeValue('onion') ?>"
          }
        }
        ```
        {: codeblock}

        結果：

        ```json
        {
          "context": {
            "toppings_array": ["olives"]
          }
        }
        ```
        {: codeblock}

    - **依位置移除**：會依元素的索引位置從陣列中移除元素。

        針對此「對話」運行環境定義：

        ```json
        {
          "context": {
            "toppings_array": ["onion", "olives"]
          }
        }
        ```
        {: codeblock}

        進行下列更新：

        ```json
        {
          "context": {
            "toppings_array": "<? $toppings_array.remove(0) ?>"
          }
        }
        ```
        {: codeblock}

        結果：

        ```json
        {
          "context": {
            "toppings_array": ["olives"]
          }
        }
        ```
        {: codeblock}

- **改寫**：若要改寫陣列中的值，只需要將陣列設為新值：

    針對此「對話」運行環境定義：

        ```json
        {
          "context": {
            "toppings_array": ["onion", "olives"]
          }
        }
        ```
        {: codeblock}

    進行下列更新：

        ```json
        {
          "context": {
            "toppings_array": ["ketchup", "tomatoes"]
          }
        }
        ```
        {: codeblock}

    結果：

        ```json
        {
          "context": {
            "toppings_array": ["ketchup", "tomatoes"]
          }
        }
        ```
        {: codeblock}

**注意**：如果您將陣列儲存為字串的一部分，則它會變成「字串」物件，而不是「陣列」。例如，下列 $array 環境定義變數是陣列，但 $string_array 環境定義變數是字串。

```json
{
  "context": {
    "array": [
      "one",
      "two"
    ],
    "array_in_string": "this is my array: $array"
  }
}
```
{: codeblock}

如果您在「試用」窗格中檢查這些環境定義變數的值，則會看到指定指下的值：

**$array**：`["one","two"]`

**$array_in_string**：`"this is my array: [\"one\",\"two\"]"`

您可以接著對 $array 變數執行陣列方法（例如 `<? $array.removeValue('two') ?>`），而不是對 $array_in_string 變數。

## 建立對話
{: #create}

使用 {{site.data.keyword.conversationshort}} 工具來建立對話。

### 對話節點限制
{: #dialog-node-limits}

您可以建立的對話節點數目，取決於您的服務方案。

| 服務方案         | 每個工作區的對話節點數   |
|------------------|---------------------------:|
| 標準/進階        |                    100,000 |
| 精簡             |                     25,000 |

樹狀結構深度限制：「服務」支援 2,000 個對話節點後代；工具的最佳執行效能是 20 個或以下。

### 程序

若要建立對話，請完成下列步驟：

1.  從導覽列中開啟**建置**頁面，按一下**對話**標籤，然後按一下**建立**。

    第一次開啟對話建置器時，會為您建立下列節點：
    - **Welcome**：第一個節點。它會包含使用者第一次與服務互動時向其顯示的問候語。您可以編輯問候語。
    - **Anything else**：最終節點。它會包含在無法辨識使用者輸入時，用來回覆使用者的詞組。您可以取代所提供的回應，或新增更多意義類似的回應，以新增交談變化。您也可以選擇讓服務逐一傳回每一個定義的回應，還是隨機傳回它們。
1.  若要將更多節點新增至對話樹狀結構，請按一下 **Welcome** 節點上的**其他** ![「其他」圖示](images/kabob.png) 圖示，然後選取**新增下面的節點**。
1.  輸入條件，以在符合時觸發服務來處理節點。

    當您開始定義條件時，會顯示一個用於顯示您的選項的方框。您可以輸入下列其中一個字元，然後從所顯示的選項清單中挑選一個值。

    <table>
    <tr>
      <td>字元</td>
      <td>列出這些構件類型的已定義值</td>
    </tr>
    <tr>
      <td>`#`</td>
      <td>目的</td>
    </tr>
    <tr>
      <td>`@`</td>
      <td>實體</td>
    </tr>
    <tr>
      <td>`@{entity-name}:`</td>
      <td>{entity-name} 值</td>
    </tr>
    <tr>
      <td>`$`</td>
      <td>您在對話的其他位置所定義或參照的環境定義變數</td>
    </tr>
    </table>

    您可以建立新的目的、實體、實體值或環境定義變數，方法是定義使用該項目的新條件。如果您使用此方式來建立構件，請務必返回，並完成完整建立構件所需的任何其他步驟（例如定義目的的範例詞語）。

    若要定義根據多個條件觸發的節點，請輸入一個條件，然後按一下它旁邊的加號 (+) 圖示。如果您要將 `OR` 運算子套用至多個條件，而不是 `AND`，請按一下顯示在欄位之間的 `and`，以變更運算子類型。AND 運算是在 OR 運算之前執行，但您可以使用括弧來變更順序。例如：`$isMember:true AND ($memberlevel:silver OR $memberlevel:gold)`

    所定義條件的長度必須小於 500 個字元。

    如需如何測試條件中之值的相關資訊，請參閱[條件](#conditions)。
1.  **選用**：如果您要向這個節點的使用者收集多個資訊片段，請按一下**自訂**，然後啟用**空位**。如需詳細資料，請參閱[使用空位收集資訊](#slots)。
1.  輸入回應。
    - 將要服務向使用者顯示的文字新增為回應。
    - 如需條件式回應、如何新增回應變化，或如何指定在觸發節點之後應該發生什麼情況的相關資訊，請參閱[回應](#responses)。
1.  **選用**：將節點命名。

    對話節點名稱可以包含字母（Unicode 形式）、數字、空格、底線、連字號及句點。

    將節點命名可讓您輕鬆地記住其用途，並且在節點最小化時可以輕鬆地找到節點。如果您未提供名稱，則會使用節點條件作為名稱。

1.  若要新增其他節點，請選取樹狀結構中的節點，然後按一下**其他** ![「其他」圖示](images/kabob.png) 圖示。
    - 若要建立在現有節點的條件不符時接著檢查的對等節點，請選取**新增下面的節點**。
    - 若要建立在檢查現有節點的條件之前檢查的對等節點，請選取**新增上面的節點**。
    - 若要建立所選取節點的子節點，請選取**新增子節點**。子節點是在其母節點之後進行處理。

    如需對話節點處理順序的相關資訊，請參閱[對話概觀](#overview)。
1.  在建置對話時進行測試。
   如需相關資訊，請參閱[測試對話](#test)。

## 使用空位收集資訊
{: #slots}

在對話節點中新增空位，以向該節點內的使用者收集多個資訊片段。空位會依使用者速度來收集資訊。系統會儲存使用者事先提供的詳細資料，而且服務只會要求他們未提供的詳細資料。

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/ES4GHcDsSCI?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

### 為什麼新增空位？
{: #why-add-slots}

使用空位可取得您精確回應使用者之前所需的資訊。例如，如果使用者詢問作業時數，但時數會依商店位置而不同，所以您可以在回答之前詢問他們打算造訪哪個商店位置的相關後續問題。然後，您可以新增將所提供位置資訊列入考量的回應條件。

![在回答 When do you open? 問題之前，詢問位置資訊。](images/op-hours.png)

空位可協助您收集完成使用者複雜作業所需的多個資訊片段（例如預約晚餐）。

![顯示四個空位，用於提示預約晚餐所需的資訊。](images/reservation.png)

使用者可以一次提供多個空位的值。例如，輸入可能包括 "There will be 6 of us dining at 7 PM." 資訊。此輸入包含兩個遺漏的必要值：訪客數及預約時間。服務可辨識並儲存這兩個項目，各自在其對應空位中。然後，服務會顯示與下一個空位相關聯的提示。

![顯示已填入兩個空位，而且服務提示輸入其餘空位。](images/pass-in-info.png)

空位可讓服務回答後續問題，而不需要重新建立使用者目標。例如，使用者可能會詢問天氣預報，然後詢問另一個位置或不同日期之天氣的相關後續問題。如果您在空位中儲存必要的預報變數（例如位置及日期），則在使用者使用新變數值詢問後續問題時，可以將空位值改寫為提供的新值，並提供反映新資訊的回應。

![顯示有人詢問天氣預報，然後追蹤不同位置及時間之天氣的相關問題。](images/follow-up.png)

使用空位會在使用者與服務之間產生更自然的對話流程，而且與嘗試使用許多個別節點來收集資訊比起來，可讓您更輕鬆地進行管理。

#### 新增空位
{: #add-slots}

1.  識別您要收集的資訊單元。例如，若要為某人訂購披薩，您可能要收集下列資訊：

    - 送餐時間
    - 大小

1.  從對話節點編輯視圖中，按一下**自訂**，然後選取**空位**勾選框。

    **附註**：如需**提示輸入所有資訊**欄位的相關資訊，請參閱[一次詢問所有資訊](dialog-build.html#slots-prompt-for-everything)。

1.  **新增每一個必要資訊單元的空位**。

    針對每一個空位，指定下列詳細資料：

    - **檢查**：識別您要從使用者對空位提示的回應中擷取的資訊類型。在大部分情況下，您可以檢查實體值，但也可以檢查目的。您可以在這裡使用 AND 及 OR 運算子，來定義更複雜的條件。

      **附註**：如果已定義實體的型樣，則會在新增實體名稱之後，附加 `.literal`。例如，在從已定義實體清單中選擇 `@email` 之後，請編輯*檢查* 欄位以包含 `@email.literal`。新增 `.literal` 內容，以指出您要擷取使用者所輸入的確切文字，並根據型樣將其識別為電子郵件位址。

      避免檢查環境定義變數值。*檢查* 值最開始是用作條件，但之後會變成您在*另存為* 欄位中所命名的環境定義變數值。如果您在條件中使用環境定義變數，則在環境定義中使用它時可能會導致非預期的行為。
      {: tip}

    - **另存為**：提供環境定義變數的名稱，以在其中儲存使用者對空位提示的回應中的感興趣值。請不要指定先前在對話中使用的環境定義變數，因此可能會有一個值。只有在空位的環境定義變數是空值時，才會顯示該空位的提示。

    - **提示**：撰寫陳述式，以向使用者取得您需要的資訊片段。在顯示此提示之後，交談會暫停，而且服務會等待使用者回應。

    - 如果您編輯空位，則也可以定義要在使用者回應空位提示之後顯示的回應。
      - **找到**：在使用者提供預期的資訊之後執行。
      - **找不到**：如果不瞭解使用者所提供的資訊，或未以預期的格式提供資訊，則會執行。您在這裡指定的文字可以更明確地陳述您需要使用者提供的資訊類型。如果已順利填入空位，或節點層次處理程式瞭解並處理使用者輸入，則永遠不會觸發此條件。

    此表格顯示用於協助使用者訂披薩之節點的範例空位值。

    <table>
    <tr>
      <td>資訊</td>
      <td>檢查</td>
      <td>另存為</td>
      <td>提示</td>
      <td>找到時的後續</td>
      <td>找不到時的後續</td>
    </tr>
    <tr>
      <td>Size</td>
      <td>@size</td>
      <td>$size</td>
      <td>"What size pizza would you like?"</td>
      <td>"$size it is."</td>
      <td>"What size did you want? We have small, medium, and large."</td>
    </tr>
    <tr>
      <td>DeliverBy</td>
      <td>@sys-time</td>
      <td>$time</td>
      <td>"When do you need the pizza by?"</td>
      <td>"For delivery by $time."</td>
      <td>"What time did you want it delivered? We need at least a half hour to prepare it."</td>
    </tr>
    </table>

    **選用空位**：如果您要新增用於擷取資訊但為選用項目的空位，請不要指定其提示。

    例如，您可以新增一個空位，用來在使用者指定任何飲食限制資訊時擷取這些資訊。不過，您不會想要詢問所有使用者的飲食資訊，因為大部分情況都不相關。

    <table>
    <tr>
      <td>資訊</td>
      <td>檢查</td>
      <td>另存為</td>
    </tr>
    <tr>
      <td>Wheat restriction</td>
      <td>@dietary</td>
      <td>$dietary</td>
    </tr>
    </table>

    在沒有提示的情況下新增空位時，服務會將該空位視為選用。

    如果您將空位設為選用，則只有在使用有意義的文字表達時，才會參照其在節點層次回應文字中的環境定義變數，即使未提供空位的任何值。例如，您可以使用 "I am ordering a $size $dietary pizza for delivery at $time." 這類摘要陳述。如果未提供 `gluten-free` 或 `dairy-free` 這類飲食限制資訊，則產生的文字仍然有其意義："I am ordering a large pizza for delivery at 3:00PM."。
    {: tip}
1.  **追蹤使用者**。
    您可以選擇性地定義節點層次處理程式，以提供使用者可能在互動期間詢問偏離節點用途之問題的回應。

    例如，使用者可能會詢問紅醬食譜或食材取得位置。若要處理這類離題，請按一下**管理處理程式**鏈結，然後新增每一個預期問題的條件及回應。

    ![顯示使用者詢問醬汁食譜。回應是：I'll take it to my grave。](images/sauce.png)

    回應離題之後，會顯示與現行空位相關聯的提示。

    在顯示節點層次回應之前，如果使用者所提供的輸入在對話節點流程期間的任何時間符合處理程式條件，則會觸發此條件。
1.  **新增節點層次回應**。
    除非已填入所有必要空位，否則不會執行此節點層次回應。您可以新增回應，以彙總所收集的資訊。例如，"A `$size` pizza is scheduled for delivery at `$time`. Enjoy!"

1.  **新增用於重設空位環境定義變數的邏輯**。
    當您向使用者收集每個空位的回答時，會將它們儲存在環境定義變數。您可以使用環境定義變數，將資訊傳遞給另一個節點或是傳遞給應用程式或外部服務，以供使用。不過，傳遞資訊之後，您必須將環境定義變數設為空值來重設節點，使其可重新開始收集資訊。您無法將現行節點內的環境定義變數設為空值，因為在填入必要空位之前，服務不會結束節點。相反地，請考慮使用下列其中一種方法：
    - 新增將變數設為空值之外部應用程式的處理。
    - 新增將變數設為空值的子節點。
    - 插入將變數設為空值的母節點，然後跳至含有空位的節點。

請考慮使用這些建議的方式來處理一般作業。

#### 一次詢問所有資訊
{: #slots-prompt-for-everything}

包括整個節點的起始提示，以清楚告知使用者您希望他們提供的資訊單元。顯示此提示會先讓使用者有機會一次提供所有詳細資料，而不需要等待提示一次輸入一個資訊片段。

例如，因為客戶想要訂披薩而觸發節點時，您可以回應初步提示 "I can take your pizza order. Tell me what size pizza you want and the time that you want it delivered."

只要使用者在起始要求中提供這項資訊的一個片段，就不會顯示提示。例如，起始輸入可能是 "I want to order a large pizza."。服務分析輸入時，會將 "large" 辨識為披薩大小，並將提供的值填入**大小**空位。因為已填入其中一個空位，所以它不會顯示起始提示，以避免再次詢問披薩大小資訊。反之，會針對遺漏資訊的任何其餘空位顯示提示。

從您已啟用「空位」特性的「自訂」窗格中，選取**提示輸入所有資訊**勾選框，以啟用起始提示。此設定會將**如果未預先填入任何空位，請先詢問此資訊**欄位新增至節點，您可以在其中指定用於提示使用者輸入所有資訊的文字。

#### 擷取多個值
{: #slots-multiple-entity-values}

您可以要求項目清單，並將它們儲存在一個空位。

例如，您可能想要詢問使用者的披薩是否要加上配料。若要這麼做，請定義一個實體 (@toppings) 及其接受值（pepperoni、cheese、mushroom 等）。請新增向使用者詢問配料的空位。請使用實體類型的 values 內容來擷取多個值（如果已提供的話）。

<table>
<tr>
  <td>資訊</td>
  <td>檢查</td>
  <td>另存為</td>
  <td>提示</td>
  <td>找到時的後續</td>
  <td>找不到時的後續</td>
</tr>
<tr>
  <td>Toppings</td>
  <td>@toppings.values</td>
  <td>$toppings</td>
  <td>Any toppings on that?</td>
  <td>"Great addition."</td>
  <td>"What toppings would you like? We offer ..."</td>
</tr>
</table>

若稍後要參照使用者指定的配料，請使用 `<? $entity-name.join(',') ?>` 語法列出 toppings 陣列中的每一個項目，並使用逗點區隔值。例如，"I am ordering you a $size pizza with `<? $toppings.join(',') ?>` that is scheduled for delivery by $time."

#### 重新格式化值
{: #slots-reformat-values}

因為您會向使用者詢問資訊，並且需要參照他們在回應中的輸入，所以請考慮重新格式化這些值，以較友善的格式顯示這些值。

例如，以 `hh:mm:ss` 格式儲存時間值。您可以在儲存時間值時對該空位使用 JSON 編輯器來重新格式化時間值，以改為使用 `hour:minutes AM/PM` 格式：

```json
{
  "context":{
    "time": "<? @sys-time.reformatDateTime('h:mm a') ?>"
  }
}
```
{: codeblock}

請參閱[處理值的方法](dialog-methods.html)，以瞭解其他重新格式化構想。

#### 取得確認
{: #slots-get-confirmation}

在其他空位下面新增一個空位，要求使用者確認您所收集的資訊正確且完整。這個空位可以尋找符合 #yes 目的的回應。

<table>
<tr>
  <td>資訊</td>
  <td>檢查</td>
  <td>另存為</td>
  <td>提示</td>
  <td>找到時的後續</td>
  <td>找不到時的後續</td>
</tr>
<tr>
  <td>Confirmation</td>
  <td>#yes</td>
  <td>$confirmation</td>
  <td>"I'm going to order you a `$size` pizza for delivery at `$time`. Should I go ahead?"</td>
  <td>"Your pizza is on its way!"</td>
  <td>請參閱下面</td>
</tr>
</table>

因為使用者可能在對話期間的其他時間包括肯定陳述 (*Oh yes, we want the pizza delivered at 5pm*)，所以請使用 `slot_in_focus` 內容，在空位條件中清楚指出您只要尋找這個空位之提示的 Yes 回應。

```json
#yes && slot_in_focus
```
`slot_in_focus` 內容一律會評估為布林值（true 或 false）。只需要將它併入您要有布林結果的條件中。例如，不要在空位條件中使用它來檢查實體類型，然後儲存實體值。
{: tip}

在**找不到**提示中，重新詢問所有資訊，並重設先前儲存的環境定義變數。

```json
{
  "output":{
    "text": {
      "values": [
        "Let's try this again. Tell me what size pizza you want and the time..."
      ]
    }
  },
  "context":{
    "size": null,
    "time": null
  }
}
```
{: codeblock}

#### 取代空位環境定義變數值
{: #slots-found-handler-event-properties}

在使用者結束含空位的節點之前的任何時間，如果使用者提供新的空位值，則會將新值儲存在空位環境定義變數，並取代先前指定的值。您的對話可以明確地確認這項取代是使用針對「找到」條件事件處理程式所定義的特殊內容所進行：

- `event.previous_value`：此空位之環境定義變數的先前值。
- `event.current_value`：此空位之環境定義變數的現行值。

例如，您的對話詢問目的地城市來進行航班預定。使用者提供 `Paris`。您會將 $destination 空位環境定義變數設為 *Paris*。然後，使用者說：`Oh wait. I want to fly to Madrid instead.`。如果您如下設定「找到」條件，則對話可以得體的處理這類型的變更。

```json
When user responds, if @destination is found:
Condition: event.previous_value != null
    Response: Ok, updating destination from <? event.previous_value ?> to <? event.current_value ?>.
Response: Ok, destination is $destination.literal.
```

此空位配置可讓您的對話反應使用者的目的地變更，方法是表明 `Ok, updating the destination from Paris to Madrid.`

#### 避免數字混淆
{: #slots-avoid-number-confusion}

使用者提供的部分值可以識別為多個實體類型。

例如，您可能有兩個空位來儲存相同類型的值（例如到達日期及出發日期）。請建置空位條件的邏輯，來區分這類不同的類似值。

此外，服務可以在單一使用者輸入中辨識到多個實體類型。例如，使用者提供貨幣時，會將貨幣同時辨識為 @sys-currency 及 @sys-number 實體類型。請在「試用」窗格中執行一些測試，以瞭解系統如何解譯不同的使用者輸入，以及建置條件的邏輯，避免可能的錯誤解譯。

在空位特性特有的邏輯中，於單一使用者輸入中辨識到兩個實體時，會使用跨距較大的實體。例如，即使 Conversation 服務在文字中同時辨識到 @sys-date (05022017) 及 @sys-number (2) 實體，但如果使用者輸入 *May 2*，則只會登錄跨距較大的實體 (@sys-date)，並將其套用至空位（適用時）。
{: tip}

#### 防止在不需要時顯示「找到」回應
{: #slots-stifle-found-responses}

如果您指定多個空位的「找到」回應，則使用者一次提供多個空位的值時，就會顯示至少一個空位的「找到」回應。您可能想要傳回所有空位的「找到」回應，或根本不傳回。

若要防止顯示「找到」回應，您可以對每一個「找到」回應執行下列其中一個動作：

- 新增回應的條件，以在填入特定空位時防止顯示回應。例如，您可以新增 `!($size && $time)` 這類條件，防止在同時提供 $size 及 $time 環境定義變數時顯示回應。
- 新增回應的 `!all_slots_filled` 條件。此設定會在填入所有空位時防止顯示回應。如果您要包括確認空位，請不要使用此方式。確認空位也是一個空位，而且您一般會防止在填入確認空位本身之前顯示「找到」回應。

#### 處理結束處理程序的要求
{: #slots-node-level-handler}

新增至少一個節點層次處理程式，以在使用者想要結束節點時辨識它。

例如，在收集資訊來預約寵物美容的節點中，您可以新增設定 #cancel 目的之條件的節點層次處理程式，此目的會辨識 "Forget it. I changed my mind." 這類詞語。

1.  在處理程式的 JSON 編輯器中，填入所有含虛擬值的空位環境定義變數，以防止節點繼續詢問任何遺漏的項目。在處理程式回應中，新增 "Ok, we'll stop there. No appointment will be scheduled." 這類訊息。
1.  在節點層次回應中，新增條件以檢查其中一個空位環境定義變數中的虛擬值。如果找到，則顯示 "If you decide to make an appointment later, I'm here to help." 這類最終訊息。如果找不到，它會顯示節點的標準摘要訊息，例如 "I am making a grooming appointment for your $animal at $time on $date."
1.  考量在此節點層次處理程式之前評估的條件中所用的邏輯，因此您可以在其中建置不同的條件。收到使用者輸入時，會依下列順序評估條件：

    - 現行空位層次「找到時」條件。
    - 依列出順序的節點層次處理程式。
    - 現行空位層次「找不到時」條件。

將一律評估為 true 的條件（例如 `true` 或 `anything_else` 特殊條件）新增為節點層次處理程式時，請小心。根據空位，如果節點層次處理程式評估為 true，則會完全跳過「找不到時」條件。因此，使用一律評估為 true 的節點層次處理程式，可有效地防止評估每個空位的「找不到時」條件。
{: tip}

例如，您可以為所有動物美容，但貓除外。在「動物」空位中，您可能很想使用下列空位條件，防止將 `cat` 儲存在「動物」空位：

```json
Check for @animal && !@animal:cat, then save it as $animal.
```
{: codeblock}

而且，若要讓使用者知道您不接受貓，您可以在「動物」空位的「找不到」條件中指定下列值：

```json
If @animal && !@animal:cat then, "I'm sorry. We do not groom cats."
```
{: codeblock}

邏輯上，如果您也定義節點層次結束要求處理程式，則在指定條件評估順序時，可能永遠不會觸發這個「找不到」條件。相反地，您可以使用此空位條件：

```json
Check for @animal, then save it as $animal.
```
{: codeblock}

而且，若要處理可能的 `cat` 回應，請將此值新增至「找到」條件：

```josn
If @animal:cat then, "I'm sorry. We do not groom cats."
```
{: codeblock}

在「找到」條件的 JSON 編輯器中，重設 $animal 環境定義變數值，因為它目前設為 cat，但不應該如此設定。

```json
{
  "output":{
    "text": {
      "values": [
        "I'm sorry. We do not groom cats."
      ]
    }
  },
  "context":{
    "animal": null
  }
}
```
{: codeblock}

下列 JSON 範例定義披薩範例的節點層次處理程式：

```json
{
"conditions": "#cancel",
 "output": {
   "text": {
     "values": [
       "Ok, we'll stop there. No pizza delivery will be scheduled."
     ],
    "selection_policy": "sequential"
    }
  },
"context": {
   "time": "12:00:00",
   "size": "dummy",
   "confirmation":"true"
}
}
```

#### 空位範例

若要存取用於實作不同常用空位使用情境的 JSON 檔案，請跳至 GitHub 中的社群[交談儲存庫 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/watson-developer-cloud/community/tree/master/conversation){: new_window}。

若要探索範例，請下載其中一個範例 JSON 檔案，然後將它匯入為新的工作區。在「對話」標籤中，您可以檢閱對話節點，以查看如何實作空位來處理不同的使用案例。

## 測試對話
{: #test}

在變更對話時，您可以隨時對它進行測試，以查看它回應輸入的方式。

1.  在「對話」標籤中，按一下 ![詢問 Watson](images/ask_watson.png) 圖示。
1.  在聊天窗格中，鍵入一些文字，然後按 Enter 鍵。

    開始測試對話之前，請確定系統已完成最新變更的訓練。如果系統仍在訓練，則聊天窗格頂端會出現訊息：
    {: tip}

    ![訓練訊息的畫面擷取](images/training.png)
1.  檢查回應，瞭解對話是否已正確地解譯您的輸入，並選擇正確的回應。

    聊天視窗指出輸入中辨識到的目的及實體：

    ![測試對話輸出的畫面擷取](images/test_dialog_output.png)

    在對話編輯器窗格中，會強調顯示目前作用中節點。
1.  若要檢查或設定環境定義變數的值，請按一下**管理環境定義**鏈結。

    即會顯示您在對話中定義的任何環境定義變數。

    此外，還會列出 `$timezone` 環境定義變數。「試用」窗格使用者介面會從 Web 瀏覽器取得使用者語言環境資訊，並用它來設定 `$timezone` 環境定義變數。此環境定義變數讓處理測試對話交換中的時間參照更為簡單。請考慮在使用者應用程式中執行類似的動作。如果未指定，則會使用「格林威治標準時間 (GMT)」。

    您可以新增變數，並設定其值，以查看對話在下一個測試對話要求中如何回應。例如，如果對話設定成根據使用者所提供的環境定義變數值來顯示不同的回應，則此功能十分有用。

    1.  若要新增環境定義變數，請指定變數名稱，然後按 **Enter** 鍵。
    1.  若要定義環境定義變數的預設值，請在清單中尋找所新增的環境定義變數，然後指定其值。

    如需相關資訊，請參閱[環境定義變數](#context)。

1.  繼續與對話互動，以瞭解如何透過對話進行交談。
    - 若要尋找並重新提交測試詞語，您可以按向上鍵循環瀏覽最近的輸入。
    - 若要從聊天窗格中移除先前的測試詞語，然後重新開始，請按一下**清除**鏈結。這個動作不只會移除測試詞語及回應，也會清除因為與對話互動而設定的任何環境定義變數值。但不會清除您明確設定或變更的環境定義變數值。

### 下一步

如果您判定辨識到錯誤的目的或實體，則可能需要修改目的或實體定義。

如果辨識到正確的目的及實體，但在對話中觸發錯誤的節點，請確定已正確撰寫您的條件。

## 移動對話節點
{: #move-node}

您建立的每一個節點都可以在對話樹狀結構的其他位置移動。

您可能想要將先前建立的節點移至流程的另一個區域，以變更交談。您可以移動節點，以變成另一個分支中的同層級或對等節點。

1.  在您要移動的節點上，按一下**其他** ![「其他」圖示](images/kabob.png) 圖示，然後選取**移動**。
1.  選取位於樹狀結構中接近您要移動此節點之位置的目標節點。選擇將此節點放在目標節點上面或下面，還是讓它成為目標節點的子節點。

## 依節點 ID 尋找對話節點
{: #get-node-id}

基於下列任何原因，您可能會想要尋找與已知節點 ID 相關聯的對話節點：

- 您要檢閱日誌，而且日誌依節點 ID 參照對話的某個區段。
- 您要將 API 訊息輸出的 `nodes_visited` 內容中所列出的節點 ID，對映至您可在對話樹狀結構中看到的節點。
- 對話運行環境錯誤訊息會通知您語法發生錯誤，並使用節點 ID 來識別您需要修正的節點。

若要根據節點 ID 來探索節點，請完成下列步驟：

1.  在工具的「對話」標籤中，選取對話樹狀結構中的任何節點。
1.  如果已開啟現行節點的編輯視圖，則請予以關閉。
1.  在 Web 瀏覽器的位置欄位中，URL 應顯示下列語法：

    ```json
    https://watson-conversation.ng.bluemix.net/space/instance-id/workspaces/workspace-id/build/dialog#node=node-id
    ```

1.  藉由將現行 `node-id` 值取代為您要尋找的節點 ID 來編輯 URL，然後提交新的 URL。
1.  必要的話，請重新強調顯示編輯過的 URL，並重新提交。

工具會重新整理，並將焦點移至具有所指定節點 ID 的對話節點。

**附註**：您目前無法使用此方法來尋找空位、空位處理程式或節點層次處理程式。若要依 ID 尋找這些類型的節點，您必須匯出工作區，使用 JSON 編輯器在 JSON 中尋找 node-id，然後記下其標題（指定時）或其條件。在工具的「對話」標籤中，使用瀏覽器搜尋功能來搜尋具有該標題或條件的對話節點。
