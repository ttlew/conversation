---

copyright:
  years: 2015, 2017
lastupdated: "2017-08-08"

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

# 處理值的方法

使用這些方法，可處理從使用者詞語中擷取的值，而這些使用者詞語是您要在回應的環境定義變數、條件或其他位置中參照的詞語。
{: shortdesc}

>**附註：**針對涉及正規表示式的方法，請參閱 [RE2 語法參考](https://github.com/google/re2/wiki/Syntax)，以取得要在指定正規表示式時使用之語法的詳細資料。

## 陣列
{: #arrays}

您無法使用這些方法來檢查陣列中的值，而陣列位在您設定陣列值之相同節點的節點條件或回應條件中。

### JSONArray.append(object)

此方法會將新值附加至 JSONArray，並傳回已修改的 JSONArray。

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
{: screen}

### JSONArray.contains(object value)

如果輸入 JSONArray 包含輸入值，則此方法會傳回 true。

針對前一個節點或外部應用程式所設定的這個「對話」運行環境定義：

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

對話節點或回應條件：

```json
$toppings_array.contains('ham')
```
{: codeblock}

結果：`True`，因為陣列包含元素 ham。

### JSONArray.get(integer)

此方法會傳回來自 JSONArray 的輸入索引。

針對前一個節點或外部應用程式所設定的這個「對話」運行環境定義：

```json
{
  "context": {
    "name": "John",
    "nested": {
      "array": [ "one", "two" ]
    }
  }
}
```
{: codeblock}

對話節點或回應條件：

```json
$nested.array.get(0).getAsString().contains('one')
```
{: codeblock}

結果：`True`，因為巢狀陣列包含 `one` 作為值。

回應：

```json
"output": {
    "text": {
      "values": [
        "The first item in the array is <?$nested.array.get(0)?>"
      ],
      "selection_policy": "sequential"
    }
  }
```
{: codeblock}

### JSONArray.getRandomItem()

此方法會傳回來自輸入 JSONArray 的隨機項目。

針對此「對話」運行環境定義：

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

對話節點輸出：

```json
{
  "output": {
    "text": "<? $toppings_array.getRandomItem() ?> is a great choice!"
  }
}
```
{: codeblock}

結果：`"ham is a great choice!"`、`"onion is a great choice!"` 或 `"olives is a great choice!"`

**附註：**隨機選擇產生的輸出文字。

### JSONArray.join(string delimiter)

此方法會將這個陣列中的所有值都結合至字串。值會轉換為字串，並以輸入定界字元分隔。

針對此「對話」運行環境定義：

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

對話節點輸出：

```json
{
  "output": {
    "text": "This is the array: <? $toppings_array.join(';') ?>"
  }
}
```
{: codeblock}

結果：

```json
This is the array: onion;olives;ham;
```
{: screen}

### JSONArray.remove(integer)

此方法會從 JSONArray 中移除索引位置中的元素，並傳回已更新的 JSONArray。

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
{: screen}

### JSONArray.removeValue(object)

此方法會從 JSONArray 中移除第一次出現的值，並傳回已更新的 JSONArray。

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
{: screen}

### JSONArray.set(integer index, object value)

此方法會將 JSONArray 的輸入索引設為輸入值，並傳回已修改的 JSONArray。

針對此「對話」運行環境定義：

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

對話節點輸出：

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.set(1,'ketchup')?>"
  }
}
```
{: codeblock}

結果：

```json
{
  "context": {
    "toppings_array": ["onion", "ketchup", "ham"]
  }
}
```
{: screen}

### JSONArray.size()

此方法會將 JSONArray 的大小傳回為整數。

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
    "toppings_array_size": "<? $toppings_array.size() ?>"
  }
}
```
{: codeblock}

結果：

```json
{
  "context": {
    "toppings_array_size": 2
  }
}
```
{: screen}

### JSONArray split(String regexp)

此方法使用輸入正規表示式來分割輸入字串。結果是字串的 JSONArray。

針對此輸入：

```
"bananas;apples;pears"
```
{: codeblock}

此語法：

```json
{
  "context": {
    "array": "<?input.text.split(";")?>
  }
}
```
{: codeblock}

此輸出中的結果：

```json
{
  "context": {
    "array": [ "bananas", "apples", "pears" ]
  }
}
```
{: screen}

## 日期和時間
{: #date-time}

您可以利用數種方法來處理日期和時間。

### .after(String date/time)

- 判定日期/時間值是否在日期/時間引數之後。
- 類似 `.before()`。

### .before(String date or time)

- 例如：
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')
- 判定日期/時間值是否在日期/時間引數之前。
- 如果比較不同的項目（例如 `time vs. date`、`date vs. time` 及 `time vs. date and time`），則此方法會傳回 false，並在回應 JSON 日誌 `output.log_messages` 中列印異常狀況。
    - 例如，`@sys-date.before(@sys-time)`。
- 如果比較 `date and time vs. time`，則此方法會忽略日期，而僅比較時間。

### now()

- 靜態函數。
- 傳回含有現行日期和時間的字串，格式為 `yyyy-MM-dd HH:mm:ss`。
- 您可以對此函數所傳回的日期-時間值呼叫其他日期/時間方法，因此可將其傳入作為其引數。
- 如果已設定環境定義變數 `$timezone`，則此函數會傳回用戶端時區的日期和時間。

輸出欄位中使用 `now()` 的對話節點範例：

```json
{
  "conditions": "#what_time_is_it",
  "output": {
    "text": "<? now() ?>"
   }
}
```
{: codeblock}

節點條件中的 `now()` 範例（決定是否仍是早上）：

```json
{
  "conditions": "now().before('12:00:00')",
  "output": {
    "text": "Good morning!"
   }
}
```
{: codeblock}

### .reformatDateTime(String format)

將日期和時間字串格式化為使用者輸出所要的格式。

根據指定的格式，傳回格式化的字串：

- `MM/dd/yyyy` 表示 12/31/2016
- `h a` 表示 10pm

若要傳回星期幾，請指定：

- `E` 表示星期二
- `u` 表示日期索引（1 = 星期一、...、7 = 星期日）

例如，此環境定義變數定義會建立 $time 變數，用來將值 17:30:00 儲存為 *5:30 PM*。

```json
{
  "context": {
    "time": "<? @sys-time.reformatDateTime('h:mm a') ?>"
  }
}
```
{: codeblock}

格式遵循 Java [SimpleDateFormat](http://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html) 規則。

>附註：嘗試僅格式化時間時，會將日期視為 `1970-01-01`。

### .sameMoment(String date/time)

- 判定日期/時間值是否與日期/時間引數相同。

### .sameOrAfter(String date/time)

- 判定日期/時間值是在日期/時間引數之後還是與其相同。
- 類似 `.after()`。

### .sameOrBefore(String date/time)

- 判定日期/時間值是在日期/時間引數之前還是與其相同。

如需用於擷取日期和時間值之系統實體的相關資訊，請參閱 [@sys-date 及 @sys-time 實體](system-entities.html#sys-datetime)。

## 數字
{: #numbers}

### Random()

傳回亂數。請使用下列其中一個語法選項：

- 若要傳回隨機布林值（true 或 false），請使用 `<?new Random().nextBoolean()?>`。
- 若要傳回 0（包括）與 1（排除）之間的隨機倍精準數，請使用 `<?new Random().nextDouble()?>`
- 若要傳回 0（包括）與所指定數字之間的隨機整數，請使用 `<?new Random().nextInt(n)?>`，其中 n 是您要 + 1 的數字範圍頂端。
  例如，如果您要傳回介於 0 與 10 之間的亂數，請指定 `<?new Random().nextInt(11)?>`。
- 若要傳回完整整數值範圍（-2147483648 到 2147483648）的隨機整數，請使用 `<?new Random().nextInt()?>`。

例如，您可以建立 #random_number 目的所觸發的對話節點。第一個回應條件看起來可能像這樣：

```json
Condition = @sys-number
{
  "context": {
    "answer": "<? new Random().nextInt(@sys-number.numeric_value + 1) ?>"
  },
  "output": {
    "text": {
      "values": [
        "Here's a random number between 0 and @sys-number.literal: $answer."
      ],
      "selection_policy": "sequential"
    }
  }
}
```
{: codeblock}

### toDouble()

  將物件或欄位轉換為 Double 數字類型。您可以在任何物件或欄位上呼叫此方法。如果轉換失敗，則會傳回*空值*。

### toInt()

  將物件或欄位轉換為 Integer 數字類型。您可以在任何物件或欄位上呼叫此方法。如果轉換失敗，則會傳回*空值*。

### toLong()

  將物件或欄位轉換為 Long 數字類型。您可以在任何物件或欄位上呼叫此方法。如果轉換失敗，則會傳回*空值*。

如需用於擷取數字之系統實體的相關資訊，請參閱 [@sys-number 實體](system-entities.html#sys-number)。

## 物件
{: #objects}

### JSONObject.has(string)

如果複雜 JSONObject 具有輸入名稱的內容，則此方法會傳回 true。

針對此「對話」運行環境定義：

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

對話節點輸出：

```json
{
  "conditions": "$user.has('first_name')"
}
```
{: codeblock}

結果：條件是 true，因為使用者物件包含 `first_name` 內容。

### JSONObject.remove(string)

此方法會從輸入 `JSONObject` 中移除名稱的內容。此方法所傳回的 `JSONElement` 是要移除的 `JSONElement`。

針對此「對話」運行環境定義：

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

對話節點輸出：

```json
{
  "context": {
    "attribute_removed": "<? $user.remove('first_name') ?>"
  }
}
```
{: codeblock}

結果：

```json
{
  "context": {
    "user": {
      "last_name": "Snow"
    },
    "attribute_removed": {
      "first_name": "John"
    }
  }
}
```
{: screen}

## 字串
{: #strings}

### String.append(object)

此方法會將輸入物件作為字串附加至字串，並傳回已修改的字串。

針對此「對話」運行環境定義：

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

此語法：

```json
{
  "context": {
    "my_text": "<? $my_text.append(' More text.') ?>"
  }
}
```
{: codeblock}

此輸出中的結果：

```json
{
  "context": {
    "my_text": "This is a text. More text."
  }
}
```
{: screen}

### String.contains(string)

如果字串包含輸入子字串，則此方法會傳回 true。

輸入："Yes, I'd like to go."

此語法：

```json
{
  "conditions": "input.text.contains('Yes')"
}
```
{: codeblock}

結果：條件是 `true`。

### String.endsWith(string)

如果字串的結尾是輸入子字串，則此方法會傳回 true。

針對此輸入：

```
"What is your name?".
```
{: codeblock}

此語法：

```json
{
  "conditions": "input.text.endsWith('?')"
}
```
{: codeblock}

結果：條件是 `true`。

### String.extract(String regexp, Integer groupIndex)

此方法會傳回輸入正規表示式的指定群組索引所擷取的字串。

針對此輸入：

```
"Hello 123456".
```
{: codeblock}

此語法：

```json
{
  "context": {
    "number_extract": "<? input.text.extract('[\\d]+',0) ?>"
  }
}
```
{: codeblock}

  **重要事項：**若要處理 `\\d` 作為正規表示式，您必須新增另一個 `\\` 來跳出這兩條反斜線：`\\\\d`

結果：

```json
{
  "context": {
    "number_extract": "123456"
  }
}
```
{: codeblock}

### String.find(string regexp)

如果字串的任何區段符合輸入正規表示式，則此方法會傳回 true。您可以針對 JSONArray 或 JSONObject 元素呼叫此方法，而在進行比較之前，會將陣列或物件轉換為字串。

針對此輸入：

```
"Hello 123456".
```
{: screen}

此語法：

```json
{
  "conditions": "input.text.find('^[^\d]*[\d]{6}[^\d]*$')"
}
```
{: codeblock}

結果：條件是 true，因為輸入文字的數值部分符合正規表示式 `^[^\d]*[\d]{6}[^\d]*$`。

### String.isEmpty()

如果字串是空字串，而非空值，則此方法會傳回 true。

針對此「對話」運行環境定義：

```json
{
  "context": {
    "my_text_variable": ""
  }
}
```
{: codeblock}

此語法：

```json
{
  "conditions": "$my_text_variable.isEmpty()"
}
```
{: codeblock}

結果：條件是 `true`。

### String.length()

此方法會傳回字串的字元長度。

針對此輸入：

```
"Hello"
```
{: codeblock}

此語法：

```json
{
  "context": {
    "input_length": "<? input.text.length() ?>"
  }
}
```
{: codeblock}

此輸出中的結果：

```json
{
  "context": {
    "input_length": 5
  }
}
```
{: screen}

### String.matches(string regexp)

如果字串符合輸入正規表示式，則此方法會傳回 true。

針對此輸入：

```
"Hello".
```
{: codeblock}

此語法：

```json
{
  "conditions": "input.text.matches('^Hello$')"
}
```
{: codeblock}

結果：條件是 true，因為輸入文字符合正規表示式 `\^Hello\$`。

### String.startsWith(string)

如果字串的開頭是輸入子字串，則此方法會傳回 true。

針對此輸入：

```
"What is your name?".
```
{: codeblock}

此語法：

```json
{
  "conditions": "input.text.startsWith('What')"
}
```
{: codeblock}

結果：條件是 `true`。

### String.substring(int beginIndex, int endIndex)

此方法會取得子字串，而子字串的字元位於 `beginIndex` 且最後一個字元設為 `endIndex` 之前的索引。不包括 endIndex 字元。

針對此「對話」運行環境定義：

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

此語法：

```json
{
  "context": {
    "my_text": "<? $my_text.substring(5, $my_text.length()) ?>"
  }
}
```
{: codeblock}

此輸出中的結果：

```json
{
  "context": {
    "my_text": "is a text."
  }
}
```
{: screen}

### String.toLowerCase()

此方法會傳回轉換為小寫字母的原始字串。

針對此輸入：

```
"This is A DOG!"
```
{: codeblock}

此語法：

```json
{
  "context": {
    "input_lower_case": "<? input.text.toLowerCase() ?>"
  }
}
```
{: codeblock}

此輸出中的結果：

```json
{
  "context": {
    "input_upper_case": "this is a dog!"
  }
}
```
{: screen}

### String.toUpperCase()

此方法會傳回轉換為大寫字母的原始字串。

針對此輸入：

```
"hi there".
```
{: codeblock}

此語法：

```json
{
  "context": {
    "input_upper_case": "<? input.text.toUpperCase() ?>"
  }
}
```
{: codeblock}

此輸出中的結果：

```json
{
  "context": {
    "input_upper_case": "HI THERE"
  }
}
```
{: screen}

### String.trim()

此方法會修除字串開頭及結尾的任何空格，並傳回已修改的字串。

針對此「對話」運行環境定義：

```json
{
  "context": {
    "my_text": "   something is here    "
  }
}
```
{: codeblock}

此語法：

```json
{
  "context": {
    "my_text": "<? $my_text.trim() ?>"
  }
}
```
{: codeblock}

此輸出中的結果：

```json
{
  "context": {
    "my_text": "something is here"
  }
}
```
{: screen}

如需用於擷取字串之系統實體的相關資訊，請參閱[系統實體](system-entities.html)。
