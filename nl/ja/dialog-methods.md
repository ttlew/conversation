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

# 値を処理するためのメソッド

これらのメソッドを使用して、応答の中のコンテキスト変数や条件などで参照する、ユーザー発話から抽出された値を処理します。
{: shortdesc}

>**注:** 正規表現を必要とするメソッドの場合、[RE2 構文リファレンス](https://github.com/google/re2/wiki/Syntax)で正規表現を指定する際に使用する構文についての詳細を参照してください。

## 配列
{: #arrays}

これらのメソッドを使用して、配列値を設定するその同じノード内のノード条件または応答条件に含まれる配列の値を調べることはできません。

### JSONArray.append(オブジェクト)

このメソッドは、JSONArray に新しい値を付加し、変更後の JSONArray を返します。

このダイアログ実行時のコンテキスト:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

適用する更新:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.append('ketchup', 'tomatoes') ?>"
  }
}
```
{: codeblock}

結果:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ketchup", "tomatoes"]
  }
}
```
{: screen}

### JSONArray.contains(オブジェクト値)

このメソッドは、入力 JSONArray に入力値が含まれていれば true を返します。

直前のノードまたは外部アプリケーションによって設定されたダイアログ・ランタイム・コンテキスト:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

ダイアログのノード条件または応答条件:

```json
$toppings_array.contains('ham')
```
{: codeblock}

結果: `True`。配列にエレメント「ham」が含まれています。

### JSONArray.get(整数)

このメソッドは、JSONArray から入力添字を返します。

直前のノードまたは外部アプリケーションによって設定されたダイアログ・ランタイム・コンテキスト:

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

ダイアログのノード条件または応答条件:

```json
$nested.array.get(0).getAsString().contains('one')
```
{: codeblock}

結果:
`True`。ネストされた配列に値として `one` が含まれています。

応答:

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

このメソッドは、入力 JSONArray からランダム項目を返します。

このダイアログ実行時のコンテキスト:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

ダイアログ・ノード出力:

```json
{
  "output": {
    "text": "<? $toppings_array.getRandomItem() ?> is a great choice!"
  }
}
```
{: codeblock}

結果: `"ham is a great choice!"` または `"onion is a great choice!"` または `"olives is a great choice!"`

**注:** 結果の出力テキストはランダムに選択されます。

### JSONArray.join(ストリング区切り文字)

このメソッドは、この配列内のすべての値をストリングに結合します。値はストリングに変換され、入力区切り文字で区切られます。

このダイアログ実行時のコンテキスト:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

ダイアログ・ノード出力:

```json
{
  "output": {
    "text": "This is the array: <? $toppings_array.join(';') ?>"
  }
}
```
{: codeblock}

結果:

```json
This is the array: onion;olives;ham;
```
{: screen}

### JSONArray.remove(整数)

このメソッドは、JSONArray から添字位置のエレメントを削除し、更新後の JSONArray を返します。

このダイアログ実行時のコンテキスト:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

適用する更新:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.remove(0) ?>"
  }
}
```
{: codeblock}

結果:

```json
{
  "context": {
    "toppings_array": ["olives"]
  }
}
```
{: screen}

### JSONArray.removeValue(オブジェクト)

このメソッドは、JSONArray に最初に現れる値を削除し、更新後の JSONArray を返します。

このダイアログ実行時のコンテキスト:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

適用する更新:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.removeValue('onion') ?>"
  }
}
```
{: codeblock}

結果:

```json
{
  "context": {
    "toppings_array": ["olives"]
  }
}
```
{: screen}

### JSONArray.set(添字整数, オブジェクト値)

このメソッドは、JSONArray の入力添字を入力値に設定し、変更後の JSONArray を返します。

このダイアログ実行時のコンテキスト:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

ダイアログ・ノード出力:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.set(1,'ketchup')?>"
  }
}
```
{: codeblock}

結果:

```json
{
  "context": {
    "toppings_array": ["onion", "ketchup", "ham"]
  }
}
```
{: screen}

### JSONArray.size()

このメソッドは、JSONArray 配列のサイズを整数として返します。

このダイアログ実行時のコンテキスト:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

適用する更新:

```json
{
  "context": {
    "toppings_array_size": "<? $toppings_array.size() ?>"
  }
}
```
{: codeblock}

結果:

```json
{
  "context": {
    "toppings_array_size": 2
  }
}
```
{: screen}

### JSONArray split(正規表現ストリング)

このメソッドは、入力正規表現を使用して、入力ストリングを分割します。結果はストリングの JSONArray です。

入力:

```
"bananas;apples;pears"
```
{: codeblock}

構文:

```json
{
  "context": {
    "array": "<?input.text.split(";")?>
  }
}
```
{: codeblock}

結果の出力:

```json
{
  "context": {
    "array": [ "bananas", "apples", "pears" ]
  }
}
```
{: screen}

## 日付と時刻
{: #date-time}

いくつかのメソッドは、日時の処理に使用できます。

### .after(日付/時刻ストリング)

- 日付/時刻値が日付/時刻引数より後かどうかを判別します。
- `.before()` に似ています。

### .before(日付または時刻ストリング)

- 例:
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')
- 日付/時刻値が日付/時刻引数より前かどうかを判別します。
- `時刻と日付`、`日付と時刻`、`時刻と日時`など、異なる項目を比較した場合、メソッドは false を返し、応答 JSON ログの `output.log_messages` に例外が出力されます。
    - 例えば、`@sys-date.before(@sys-time)` などです。
- `日時と時刻`を比較した場合、メソッドは日付を無視して時刻だけ比較します。

### now()

- 静的関数。
- `yyyy-MM-dd HH:mm:ss` 形式の現在の日付と時刻を含むストリングを返します。
- この関数から返される日時値で他の日付/時刻メソッドを呼び出して、それらのメソッドの引数として渡すことができます。
- コンテキスト変数 `$timezone` が設定されている場合、この関数はクライアントのタイム・ゾーンでの日付と時刻を返します。

出力フィールドで `now()` が使用されるダイアログ・ノードの例:

```json
{
  "conditions": "#what_time_is_it",
  "output": {
    "text": "<? now() ?>"
   }
}
```
{: codeblock}

(まだ午前かどうかを決定するための) ノードの条件における `now()` の例:

```json
{
  "conditions": "now().before('12:00:00')",
  "output": {
    "text": "Good morning!"
   }
}
```
{: codeblock}

### .reformatDateTime(形式ストリング)

日時ストリングをユーザー出力に必要な形式にフォーマット設定します。

指定された次の形式に従ってフォーマット設定されたストリングを返します。

- `MM/dd/yyyy` (12/31/2016 とする場合)
- `h a` (10pm とする場合)

曜日を返すには次を指定します。

- `E` (火曜日とする場合)
- `u` (曜日指標とする場合) (1 = 月曜日、...、7 = 日曜日)

例えば、次のコンテキスト変数定義の場合は、値 17:30:00 を *5:30 PM* として保存する $time 変数が作成されます。

```json
{
  "context": {
    "time": "<? @sys-time.reformatDateTime('h:mm a') ?>"
  }
}
```
{: codeblock}

形式は、Java [SimpleDateFormat](http://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html) ルールに従います。

>注: 時刻のみをフォーマット設定しようとすると、日付は `1970-01-01` として扱われます。

### .sameMoment(日付/時刻ストリング)

- 日付/時刻値が日付/時刻引数と同じかどうかを判別します。

### .sameOrAfter(日付/時刻ストリング)

- 日付/時刻値が日付/時刻引数以降かどうかを判別します。
- `.after()` に似ています。

### .sameOrBefore(日付/時刻ストリング)

- 日付/時刻値が日付/時刻引数以前かどうかを判別します。

日時値を取り出すシステム・エンティティーについては、[@sys-date および @sys-time エンティティー](system-entities.html#sys-datetime)を参照してください。

## 数値
{: #numbers}

### Random()

乱数を返します。以下の構文オプションのいずれかを使用します。

- ランダム・ブール値 (true または false) を返すには、`<?new Random().nextBoolean()?>` を使用します。
- 0 (含まれる) から 1 (含まれない) までのランダム倍精度浮動小数点数を返すには、`<?new Random().nextDouble()?>` を使用します。
- 0 (含まれる) から指定数値までのランダム整数を返すには、`<?new Random().nextInt(n)?>` を使用します。ここで、n は目的の数値範囲の限界に 1 を加えた数値です。
例えば、0 から 10 までの乱数を返す場合は、`<?new Random().nextInt(11)?>` を指定します。
- 整数値の全範囲 (-2147483648 から 2147483648) のランダム整数を返すには、`<?new Random().nextInt()?>` を使用します。

例えば、#random_number インテントでトリガーされるダイアログ・ノードを作成することもできます。この場合、最初の応答条件は、例えば次のようになります。

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

  オブジェクトまたはフィールドを Double 数値型に変換します。任意のオブジェクトまたはフィールドに対してこのメソッドを呼び出すことができます。変換に失敗した場合は、*null* が返されます。

### toInt()

  オブジェクトまたはフィールドを Integer 数値型に変換します。任意のオブジェクトまたはフィールドに対してこのメソッドを呼び出すことができます。変換に失敗した場合は、*null* が返されます。

### toLong()

  オブジェクトまたはフィールドを Long 数値型に変換します。任意のオブジェクトまたはフィールドに対してこのメソッドを呼び出すことができます。変換に失敗した場合は、*null* が返されます。

数値を取り出すシステム・エンティティーについては、[@sys-number エンティティー](system-entities.html#sys-number)を参照してください。

## オブジェクト
{: #objects}

### JSONObject.has(ストリング)

このメソッドは、複合 JSONObject に入力名のプロパティーがあれば true を返します。

このダイアログ実行時のコンテキスト:

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

ダイアログ・ノード出力:

```json
{
  "conditions": "$user.has('first_name')"
}
```
{: codeblock}

結果: 条件は true です。user オブジェクトにプロパティー `first_name` が含まれています。

### JSONObject.remove(ストリング)

このメソッドは、入力 `JSONObject` から名前のプロパティーを削除します。このメソッドから返される `JSONElement` が、削除される `JSONElement` です。

このダイアログ実行時のコンテキスト:

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

ダイアログ・ノード出力:

```json
{
  "context": {
    "attribute_removed": "<? $user.remove('first_name') ?>"
  }
}
```
{: codeblock}

結果:

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

## ストリング
{: #strings}

### String.append(オブジェクト)

このメソッドは、ストリングに入力オブジェクトをストリングとして付加し、変更後のストリングを返します。

このダイアログ実行時のコンテキスト:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

構文:

```json
{
  "context": {
    "my_text": "<? $my_text.append(' More text.') ?>"
  }
}
```
{: codeblock}

結果の出力:

```json
{
  "context": {
    "my_text": "This is a text. More text."
  }
}
```
{: screen}

### String.contains(ストリング)

このメソッドは、ストリングに入力サブストリングが含まれていれば true を返します。

入力: "Yes, I'd like to go."

構文:

```json
{
  "conditions": "input.text.contains('Yes')"
}
```
{: codeblock}

結果: 条件は `true` です。

### String.endsWith(ストリング)

このメソッドは、ストリングが入力サブストリングで終わっていれば true を返します。

入力:

```
"What is your name?".
```
{: codeblock}

構文:

```json
{
  "conditions": "input.text.endsWith('?')"
}
```
{: codeblock}

結果: 条件は `true` です。

### String.extract(正規表現ストリング, グループ添字整数)

このメソッドは、入力正規表現の指定したグループ添字によって取り出されたストリングを返します。

入力:

```
"Hello 123456".
```
{: codeblock}

構文:

```json
{
  "context": {
    "number_extract": "<? input.text.extract('[\\d]+',0) ?>"
  }
}
```
{: codeblock}

  **重要:** `\\d` を正規表現として処理するには、別の `\\` を追加して両方の円記号 (\) をエスケープする必要があります。つまり、`\\\\d` にします。

結果:

```json
{
  "context": {
    "number_extract": "123456"
  }
}
```
{: codeblock}

### String.find(正規表現ストリング)

このメソッドは、ストリングのいずれかのセグメントが入力正規表現と一致すれば true を返します。JSONArray または JSONObject エレメントに対してこのメソッドを呼び出すことができます。このメソッドは、配列またはオブジェクトをストリングに変換してから比較を行います。

入力:

```
"Hello 123456".
```
{: screen}

構文:

```json
{
  "conditions": "input.text.find('^[^\d]*[\d]{6}[^\d]*$')"
}
```
{: codeblock}

結果: 条件は true です。入力テキストの数値部分が正規表現 `^[^\d]*[\d]{6}[^\d]*$` と一致します。

### String.isEmpty()

このメソッドは、ストリングが空ストリングであるがヌル以外の場合に true を返します。

このダイアログ実行時のコンテキスト:

```json
{
  "context": {
    "my_text_variable": ""
  }
}
```
{: codeblock}

構文:

```json
{
  "conditions": "$my_text_variable.isEmpty()"
}
```
{: codeblock}

結果: 条件は `true` です。

### String.length()

このメソッドは、ストリングの文字長を返します。

入力:

```
"Hello"
```
{: codeblock}

構文:

```json
{
  "context": {
    "input_length": "<? input.text.length() ?>"
  }
}
```
{: codeblock}

結果の出力:

```json
{
  "context": {
    "input_length": 5
  }
}
```
{: screen}

### String.matches(正規表現ストリング)

このメソッドは、ストリングが入力正規表現と一致すれば true を返します。

入力:

```
"Hello".
```
{: codeblock}

構文:

```json
{
  "conditions": "input.text.matches('^Hello$')"
}
```
{: codeblock}

結果: 条件は true です。入力テキストが正規表現 `\^Hello\$` と一致します。

### String.startsWith(ストリング)

このメソッドは、ストリングが入力サブストリングで始まっていれば true を返します。

入力:

```
"What is your name?".
```
{: codeblock}

構文:

```json
{
  "conditions": "input.text.startsWith('What')"
}
```
{: codeblock}

結果: 条件は `true` です。

### String.substring(beginIndex 整数, endIndex 整数)

このメソッドは、`beginIndex` の位置にある文字と `endIndex` の前の添字に設定された最後の文字を含むサブストリングを取得します。
endIndex の位置の文字は含まれません。

このダイアログ実行時のコンテキスト:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

構文:

```json
{
  "context": {
    "my_text": "<? $my_text.substring(5, $my_text.length()) ?>"
  }
}
```
{: codeblock}

結果の出力:

```json
{
  "context": {
    "my_text": "is a text."
  }
}
```
{: screen}

### String.toLowerCase()

このメソッドは、小文字に変換された元のストリングを返します。

入力:

```
"This is A DOG!"
```
{: codeblock}

構文:

```json
{
  "context": {
    "input_lower_case": "<? input.text.toLowerCase() ?>"
  }
}
```
{: codeblock}

結果の出力:

```json
{
  "context": {
    "input_upper_case": "this is a dog!"
  }
}
```
{: screen}

### String.toUpperCase()

このメソッドは、大文字に変換された元のストリングを返します。

入力:

```
"hi there".
```
{: codeblock}

構文:

```json
{
  "context": {
    "input_upper_case": "<? input.text.toUpperCase() ?>"
  }
}
```
{: codeblock}

結果の出力:

```json
{
  "context": {
    "input_upper_case": "HI THERE"
  }
}
```
{: screen}

### String.trim()

このメソッドは、ストリングの先頭と末尾にスペースがあればそれを切り取り、変更後のストリングを返します。

このダイアログ実行時のコンテキスト:

```json
{
  "context": {
    "my_text": "   something is here    "
  }
}
```
{: codeblock}

構文:

```json
{
  "context": {
    "my_text": "<? $my_text.trim() ?>"
  }
}
```
{: codeblock}

結果の出力:

```json
{
  "context": {
    "my_text": "something is here"
  }
}
```
{: screen}

ストリングを取り出すシステム・エンティティーについては、[システム・エンティティー](system-entities.html)を参照してください。
