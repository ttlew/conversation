---

copyright:
  years: 2015, 2017
lastupdated: "2017-09-15"

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

# 存取及評估物件

條件中的有效表示式是以「Spring 表示式 (SpEL)」語言所撰寫的。如需相關資訊，請參閱 [Spring 表示式語言 (SpEL) 語言 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html){: new_window}。
{: shortdesc}

## 內建廣域變數

下列是可用的廣域變數：

| 廣域變數             | 定義       |
|----------------------|------------|
| *anything_else*      | 整個對話的最後一個節點。若使用者輸入與目的不相符，則會執行此節點。|
| *context*            | 已處理交談訊息的 JSON 物件部分。|
| *conversation_start* | 第一個對話交談要求中為 true 的布林值（可用在對話節點的條件中，以定義對話歡迎訊息）。|
| *entities[ ]*        | 支援第一個元素之預設存取的實體清單。|
| *input*              | 已處理交談訊息的 JSON 物件部分。|
| *intents[ ]*         | 支援第一個元素之預設存取的目的清單。|
| *output*             | 已處理交談訊息的 JSON 物件部分。|

## 存取實體

entity 陣列包含一個以上的實體。

測試對話時，您可以藉由在對話節點回應中指定此表示式，來查看使用者輸入中所辨識實體的詳細資料：

```json
<? entities ?>
```
{: codeblock}

針對使用者輸入 *Hello now*，服務會辨識 @sys-date 及 @sys-time 系統實體，因此回應會包含以下實體物件：

```json
[
{"entity":"sys-date","location":[6,9],"value":"2017-08-07",
  "confidence":1,"metadata":{"calendar_type":"GREGORIAN",
  "timezone":"America/New_York"}},
{"entity":"sys-time","location":[6,9],"value":"15:01:00",
  "confidence":1,"metadata":{"calendar_type":"GREGORIAN",
  "timezone":"America/New_York"}}
]
```
{: codeblock}

### 實體的速記語法

下表顯示在參照實體時可使用的速記語法範例。

| 速記語法            | SpEL 中的完整語法                        |
|---------------------|------------------------------------------|
| `@year`             | `entities['year']?.value`                |
| `@year == 2016`     | `entities['year']?.value == 2016`        |
| `@year != 2016`     | `entities['year']?.value != 2016`        |
| `@city == 'Boston'` | `entities['city']?.value == 'Boston'`    |
| `@city:Boston`      | `entities['city']?.contains('Boston')`   |
| `@city:(New York)`  | `entities['city']?.contains('New York')` |

在 SpEL 中，問號 `(?)` 會防止在實體物件為空值時觸發空值指標異常狀況。

如果您要檢查的實體值包含 `)` 字元，則無法使用 `:` 運算子進行比較。例如，如果您要檢查城市實體是否為 `Dublin (Ohio)`，則必須使用 `@city == 'Dublin (Ohio)'`，而非 `@city:(Dublin (Ohio))`。

### 在輸入事件中放置實體時

如果在輸入事件中放置實體，請使用完整 SpEL 表示式。不論如何放置，在所有 @city 實體中至少發現一個 'Boston' 城市實體時，條件 `entities['city']?.contains('Boston')` 會傳回 true。

例如，使用者提交 `"I want to go from Toronto to Boston."`。會偵測到 `@city:Toronto` 及 `@city:Boston` 實體，並以這些實體表示：

- `entities.city[0].value = 'Toronto'`
- `entities.city[1].value = 'Boston'`

對話節點中的 `@city.contains('Boston')` 條件會傳回 true，即使 Boston 是偵測到的第二個實體。

### 實體內容

每一個實體都有一組相關聯的內容。您可以透過實體內容來存取實體的相關資訊。

| 內容                  | 定義       | 用法提示   |
|-----------------------|------------|------------|
| *confidence*          | 十進位百分比，代表已辨識實體中的服務信任。除非您已啟動實體的模糊符合，否則實體的信任是 0 或 1。已啟用模糊符合時，預設信任層次臨界值為 0.3。不論是否已啟用模糊符合，系統實體一律具有信任層次 1.0。| 如果信任層次未高於指定的百分比，您可以在條件中使用此內容，使其傳回 false。|
| *location*            | 以零為起始的字元偏移，指出偵測到的實體值在輸入文字中的開始及結束位置。| 使用 `.literal` 以擷取 location 內容中所儲存之開始與結束索引值之間的文字段。|
| *value*               | 輸入中識別的實體值。| 此內容會傳回訓練資料中定義的實體值，即使比對是針對其中一個關聯同義字進行。您可以使用 `.values` 擷取可能在使用者輸入中多次出現的實體。|

### 實體內容用法範例
在下列範例中，工作區包含內含 JFK 值及同義字 'Kennedy Airport" 的機場實體。使用者輸入是 *I want to go to Kennedy Aiport*。

- 若要在使用者輸入中辨識到 'JFK' 實體時傳回特定回應，您可以將此表示式新增至回應條件：
  `entities.airport[0].value == 'JFK'`
  或
  `@airport = "JFK"`
- 若要傳回使用者在對話回應中所指定的實體名稱，請使用 .literal 內容：`So you want to go to <?entities.airport[0].literal?>...`
  或
  `So you want to go to @airport.literal ...`

  在回應中，這兩種格式都會評估為 'So you want to go to Kennedy Airport...'。

- 表示式（如 `@airport:(JFK)` 或 `@airport.contains('JFK')`）一律會參照實體的 **value**（在此範例中為 `JFK`）。
- 若要更嚴格地限制在已啟用模糊符合時將哪些術語識別為機場，您可以在節點條件中指定此表示式，例如：`@airport && @airport.confidence > 0.7`。只有在服務有 70% 確信輸入文字包含機場參照時，才會執行此節點。

在此範例中，使用者輸入是 *Are there places to exchange currency at JFK, Logan, and O'Hare?*

- 若要擷取使用者輸入中多次出現的實體類型，請使用下列這類語法：

    ```json
    "context":{
      "airports":"@airport.values"
    }
    ```

  稍後若要在對話回應中參照擷取的清單，請使用此語法：`You asked about these airports: <? $airports.join(', ') ?>.`
  這會顯示如下：`You asked about these airports: JFK, Logan, O'Hare.`

## 存取目的

intent 陣列包含使用者輸入中所辨識的一個以上目的，並依信任遞減順序排序。每一個目的都只有一個內容：confidence 內容。confidence 內容是一個十進位百分比，代表已辨識目的中的服務信任。

測試對話時，您可以藉由在對話節點回應中指定此表示式，來查看使用者輸入中所辨識目的的詳細資料：

```json
<? intents ?>
```
{: codeblock}

針對使用者輸入 *Hello now*，服務確信 #greeting 目的是使用者輸入的最佳相符項，因此會先列出 #greeting 目的物件詳細資料。回應也會包括工作區中所定義的所有其他目的，即使其信任很低而四捨五入到 0。

```json
[{"intent":"greeting","confidence":1},
{"intent":"yes","confidence":0},
{"intent":"pizza-order","confidence":0}]
```
{: codeblock}

下列範例顯示如何檢查目的值：

- `intents[0] == 'Help'`
- `intent == 'Help'`

`intent == 'help'` 與 `intents[0] == 'help'` 不同，因為 `intent == 'help'` 在偵測不到目的時不會擲出異常狀況。只有在目的信任超出臨界值時，它才會評估為 true。如果想要的話，您可以指定條件的自訂信任層次，例如，`intents.size() > 0 && intents[0] == 'help' && intents[0].confidence > 0.1`

### 目的的速記語法

下表顯示在參照目的時可使用的速記語法範例。

| 速記語法                | SpEL 中的完整語法   |
|-------------------------|---------------------|
| `#help`                 | `intent == 'help'`  |
| `! #help`               | `intent != 'help'`  |
| `NOT #help`             | `intent != 'help'`  |
| `#help` 或 `#i_am_lost` | <code>(intent == 'help' &#124;&#124; intent == 'I_am_lost')</code> |

## 存取輸入

輸入 JSON 物件只包含一個內容：text 內容。text 內容代表使用者輸入的文字。

### 輸入內容用法範例

下列範例顯示如何存取輸入：

- 若要在使用者輸入為 "Yes" 時執行節點，請將此表示式新增至節點條件：`input.text == 'Yes'`

您可以使用任何[字串方法](/docs/services/conversation/dialog-methods.html#strings)來評估或操作使用者輸入中的文字。例如：

- 若要檢查使用者輸入是否包含 "Yes"，請使用：`input.text.contains( 'Yes' )`。
- 如果使用者輸入是數字，則會傳回 true：`input.text.matches( '[0-9]+' )`。
- 若要檢查輸入字串是否包含 10 個字元，請使用：`input.text.length() == 10`。

## 環境定義變數的速記語法

下表顯示可用來在條件表示式中撰寫環境定義變數的速記語法範例。

| 速記語法                   | SpEL 中的完整語法                       |
|----------------------------|-----------------------------------------|
| `$card_type`               | `context['card_type']`                  |
| `$(card-type)`             | `context['card-type']`                  |
| `$card_type:VISA`          | `context['card_type'] == 'VISA'`        |
| `$card_type:(MASTER CARD)` | `context['card_type'] == 'MASTER CARD'` |

您可以在環境定義變數名稱中包括特殊字元（例如連字號或句點）。不過，在評估 SpEL 表示式時，這麼做可能會導致問題。例如，連字號可能會解譯為減號。若要避免這類問題，請使用完整表示式語法或速記語法 `$(variable-name)` 來參照變數，而且不要在名稱中使用下列特殊字元：

- 括弧 ()
- 多個單引號 ''
- 引號 "

## 評估

若要展開其他變數內的變數值，或將方法套用至變數，請使用 `<? expression ?>` 表示式語法。例如：

- **展開內容**
    - `"output":{"text":"Your name is <? context.userName ?>"}`
    或
    - `"output":{"text":"Your name is $userName"}`（速記語法）
- **將數值內容增量**
    - `"output":{"number":"<? output.number + 1 ?>"}`
- **在內容及廣域物件上呼叫方法（例如 append）**
    - `"context":{"toppings": "<? context.toppings.append( 'onions' ) ?>"}`

### JSON 物件或字串格式

在對話回應中使用完整 SpEL 語法時，請用 `<?` 及 `?>` 括住表示式，以「字串」格式予以呈現。在條件中使用完整 SpEL 語法時，請不要包括圍繞 `<? ?>` 的語法。

如果您在條件中指定一個 SpEL 表示式，則會以物件格式傳回資訊，因此產生的值可以保留其資料類型，並用於方程式或其他表示式中。如果您在條件中指定多個 SpEL 表示式，或將表示式包括為字串的一部分，則會改為以「字串」格式傳回資訊。 

例如，您可以將此表示式新增至對話節點回應，以傳回使用者輸入中所辨識的實體：

```json
The entities are <? entities ?>.
```
{: codeblock}

如果使用者將 *Hello now* 指定為輸入，則會以「字串」格式提供實體資訊。

```json
The entities are 2017-08-07, 15:09:49.
```
{: codeblock}

您可以將此表示式新增至對話節點回應，以傳回使用者輸入中所辨識的目的：

```json
The intents are <? intents ?>.
```
{: codeblock}

如果使用者將 *Hello now* 指定為輸入，則會以「字串」格式提供目的資訊。

```json
The intents are [
{"intent":"greeting","confidence":0.9331061244010925},
{"intent":"yes","confidence":0.06050306558609009},
{"intent":"pizza-order","confidence":0.052069634199142456},
...
]
```
{: codeblock}
