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

# 값을 처리하기 위한 메소드

컨텍스트 변수, 조건 또는 응답의 다른 위치에서 참조할 사용자 표현에서 추출된 값을 처리하려면 다음 메소드를 사용하십시오.
{: shortdesc}

>**참고:** 정규식과 관련된 메소드의 경우 정규식을 지정할 때 사용할 구문에 대한 세부사항은 [RE2 구문 참조](https://github.com/google/re2/wiki/Syntax)를 참조하십시오.

## 배열
{: #arrays}

배열 값을 설정하는 동일한 노드 내 노드 조건 또는 응답 조건에서 배열의 값을 검사하는 데 다음 메소드를 사용할 수 없습니다.

### JSONArray.append(object)

이 메소드는 JSONArray에 새 값을 추가하고 수정된 JSONArray를 리턴합니다.

다음 대화 상자 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

다음 업데이트를 작성하십시오.

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.append('ketchup', 'tomatoes') ?>"
  }
}
```
{: codeblock}

결과:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ketchup", "tomatoes"]
  }
}
```
{: screen}

### JSONArray.contains(object value)

이 메소드는 입력 JSONArray에 입력 값이 포함되어 있는 경우 true를 리턴합니다.

이전 노드 또는 외부 애플리케이션에서 설정된 대화 상자 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

대화 상자 노드 또는 응답 조건:

```json
$toppings_array.contains('ham')
```
{: codeblock}

결과: 배열에 요소 햄이 포함되어 있으므로 `True`입니다.

### JSONArray.get(integer)

이 메소드는 JSONArray에서 입력 인덱스를 리턴합니다.

이전 노드 또는 외부 애플리케이션에서 설정된 대화 상자 런타임 컨텍스트의 경우:

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

대화 상자 노드 또는 응답 조건:

```json
$nested.array.get(0).getAsString().contains('one')
```
{: codeblock}

결과:
중첩된 배열에 `one`이 값으로 포함되어 있으므로 `True`입니다.

응답:

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

이 메소드는 입력 JSONArray에서 랜덤 항목을 리턴합니다.

다음 대화 상자 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

대화 상자 노드 출력:

```json
{
  "output": {
    "text": "<? $toppings_array.getRandomItem() ?> is a great choice!"
  }
}
```
{: codeblock}

결과: `"ham is a great choice!"` 또는 `"onion is a great choice!"` 또는 `"olives is a great choice!"`

**참고:** 결과 출력 텍스트는 임의로 선택됩니다.

### JSONArray.join(string delimiter)

이 메소드는 이 배열의 모든 값을 문자열에 결합합니다. 값은 문자열로 변환되고 입력 구분 기호로 구분됩니다.

다음 대화 상자 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

대화 상자 노드 출력:

```json
{
  "output": {
    "text": "This is the array: <? $toppings_array.join(';') ?>"
  }
}
```
{: codeblock}

결과:

```json
This is the array: onion;olives;ham;
```
{: screen}

### JSONArray.remove(integer)

이 메소드는 JSONArray에서 인덱스 위치의 요소를 제거하고 업데이트된 JSONArray를 리턴합니다.

다음 대화 상자 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

다음 업데이트를 작성하십시오.

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.remove(0) ?>"
  }
}
```
{: codeblock}

결과:

```json
{
  "context": {
    "toppings_array": ["olives"]
  }
}
```
{: screen}

### JSONArray.removeValue(object)

이 메소드는 JSONArray에서 값의 첫 번째 발생을 제거하고 업데이트된 JSONArray를 리턴합니다.

다음 대화 상자 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

다음 업데이트를 작성하십시오.

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.removeValue('onion') ?>"
  }
}
```
{: codeblock}

결과:

```json
{
  "context": {
    "toppings_array": ["olives"]
  }
}
```
{: screen}

### JSONArray.set(integer index, object value)

이 메소드는 JSONArray의 입력 인덱스를 입력 값으로 설정하고 수정된 JSONArray를 리턴합니다.

다음 대화 상자 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

대화 상자 노드 출력:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.set(1,'ketchup')?>"
  }
}
```
{: codeblock}

결과:

```json
{
  "context": {
    "toppings_array": ["onion", "ketchup", "ham"]
  }
}
```
{: screen}

### JSONArray.size()

이 메소드는 JSONArray의 크기를 정수로 리턴합니다.

다음 대화 상자 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

다음 업데이트를 작성하십시오.

```json
{
  "context": {
    "toppings_array_size": "<? $toppings_array.size() ?>"
  }
}
```
{: codeblock}

결과:

```json
{
  "context": {
    "toppings_array_size": 2
  }
}
```
{: screen}

### JSONArray split(String regexp)

이 메소드는 입력 정규식을 사용하여 입력 문자열을 분할합니다. 결과는 문자열의 JSONArray입니다.

다음 입력의 경우:

```
"bananas;apples;pears"
```
{: codeblock}

다음 구문:

```json
{
  "context": {
    "array": "<?input.text.split(";")?>
  }
}
```
{: codeblock}

다음 출력이 결과입니다.

```json
{
  "context": {
    "array": [ "bananas", "apples", "pears" ]
  }
}
```
{: screen}

## 날짜 및 시간
{: #date-time}

날짜 및 시간에 대한 작업에 몇 가지 메소드를 사용할 수 있습니다.

### .after(String date/time)

- 날짜/시간 값이 날짜/시간 인수 다음에 있는지 여부를 판별합니다.
- `.before()`와 비슷합니다.

### .before(String date or time)

- 예:
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')
- 날짜/시간 값이 날짜/시간 인수 앞에 있는지 여부를 판별합니다.
- 여러 항목(`time vs. date`, `date vs. time` 및 `time vs. date and time`)을 비교하는 경우 메소드는 false를 리턴하며 예외가 응답 JSON 로그 `output.log_messages`에 인쇄됩니다.
    - 예를 들어, `@sys-date.before(@sys-time)`입니다.
- `date and time vs. time`을 비교하는 경우 메소드가 날짜는 무시하고 시간만 비교합니다.

### now()

- 정적 함수.
- `yyyy-MM-dd HH:mm:ss` 형식의 현재 날짜 및 시간이 포함된 문자열을 리턴합니다.
- 다른 날짜/시간 메소드는 이 함수를 통해 리턴되는 날짜-시간 값에서 호출될 수 있으며 해당 인수로 전달될 수 있습니다.
- 컨텍스트 변수 `$timezone`이 설정되어 있으면 이 함수가 고객 시간대의 날짜 및 시간을 리턴합니다.

출력 필드에 사용되는 `now()`가 있는 대화 상자 노드 예제:

```json
{
  "conditions": "#what_time_is_it",
  "output": {
    "text": "<? now() ?>"
   }
}
```
{: codeblock}

노드 조건의 `now()` 예제(아직 오전인지 여부 결정):

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

날짜 및 시간 문자열을 사용자 출력에 대해 원하는 형식으로 형식화합니다.

지정된 형식에 따라 형식화된 문자열을 리턴합니다.

- 12/31/2016의 경우 `MM/dd/yyyy`
- 10pm의 경우 `h a`

요일을 다음과 같이 리턴합니다.

- 화요일의 경우 `E`
- 요일 인덱스(1 = 월요일, ..., 7 = 일요일)의 경우 `u`

예를 들어, 이 컨텍스트 변수 정의는 17:30:00 값을 *5:30 PM*으로 저장하는 $time 변수를 작성합니다.

```json
{
  "context": {
    "time": "<? @sys-time.reformatDateTime('h:mm a') ?>"
  }
}
```
{: codeblock}

형식은 Java [SimpleDateFormat](http://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html) 규칙을 따릅니다.

>참고: 시간만 형식화하려면 날짜를 `1970-01-01`로 처리합니다.

### .sameMoment(String date/time)

- 날짜/시간 값이 날짜/시간 인수와 같은지 여부를 판별합니다.

### .sameOrAfter(String date/time)

- 날짜/시간 값이 날짜/시간 인수 이후인지 여부를 판별합니다.
- `.after()`와 유사합니다.

### .sameOrBefore(String date/time)

- 날짜/시간 값이 날짜/시간 인수 이전인지 여부를 판별합니다.

날짜 및 시간 값을 추출하는 시스템 엔티티에 대한 정보는 [@sys-date 및 @sys-time 엔티티](system-entities.html#sys-datetime)를 참조하십시오.

## 숫자
{: #numbers}

### Random()

난수를 리턴합니다. 다음 구문 옵션 중 하나를 사용하십시오.

- 랜덤 부울 값(true 또는 false)을 리턴하려면 `<?new Random().nextBoolean()?>`를 사용하십시오.
- 0(포함됨)과 1(제외됨) 사이의 실수형 난수를 리턴하려면 `<?new Random().nextDouble()?>`를 사용하십시오.
- 0(포함됨)과 지정한 수 사이의 정수형 난수를 리턴하려면 `<?new Random().nextInt(n)?>`를 사용하십시오. 여기서 n은 숫자 범위(1을 더함)의 최대 수입니다.
  예를 들어, 0과 10 사이의 난수를 리턴하려면 `<?new Random().nextInt(11)?>`를 지정하십시오.
- 전체 정수 값 범위(-2147483648에서 2147483648까지)에서 정수형 난수를 리턴하려면 `<?new Random().nextInt()?>`를 사용하십시오.

예를 들어, #random_number 인텐트로 트리거되는 대화 상자 노드를 작성할 수 있습니다. 첫 번째 응답 조건은 다음과 같습니다.

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

  오브젝트 또는 필드를 실수 유형으로 변환합니다. 오브젝트 또는 필드에서 이 메소드를 호출할 수 있습니다. 변환에 실패하면 *null*이 리턴됩니다.

### toInt()

  오브젝트 또는 필드를 정수 유형으로 변환합니다. 오브젝트 또는 필드에서 이 메소드를 호출할 수 있습니다. 변환에 실패하면 *null*이 리턴됩니다.

### toLong()

  오브젝트 또는 필드를 숫자(Long) 유형으로 변환합니다. 오브젝트 또는 필드에서 이 메소드를 호출할 수 있습니다. 변환에 실패하면 *null*이 리턴됩니다.

숫자를 추출하는 시스템 엔티티에 대한 정보는 [@sys-number 엔티티](system-entities.html#sys-number)를 참조하십시오.

## 오브젝트
{: #objects}

### JSONObject.has(string)

이 메소드는 복합 JSONObject에 입력 이름의 특성이 있는 경우 true를 리턴합니다.

다음 대화 상자 런타임 컨텍스트의 경우:

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

대화 상자 노드 출력:

```json
{
  "conditions": "$user.has('first_name')"
}
```
{: codeblock}

결과: 사용자 오브젝트에 `first_name` 특성이 있으므로 조건은 true입니다.

### JSONObject.remove(string)

이 메소드는 입력 `JSONObject`에서 이름의 특성을 제거합니다. 이 메소드에서 리턴되는 `JSONElement`는 제거되는 `JSONElement`입니다.

다음 대화 상자 런타임 컨텍스트의 경우:

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

대화 상자 노드 출력:

```json
{
  "context": {
    "attribute_removed": "<? $user.remove('first_name') ?>"
  }
}
```
{: codeblock}

결과:

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

## 문자열
{: #strings}

### String.append(오브젝트)

이 메소드는 입력 오브젝트를 문자열에 문자열로 추가하고 수정된 문자열을 리턴합니다.

다음 대화 상자 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

다음 구문:

```json
{
  "context": {
    "my_text": "<? $my_text.append(' More text.') ?>"
  }
}
```
{: codeblock}

다음 출력이 결과입니다.

```json
{
  "context": {
    "my_text": "This is a text. More text."
  }
}
```
{: screen}

### String.contains(string)

이 메소드는 문자열에 입력 하위 문자열이 포함되어 있는 경우 true를 리턴합니다.

입력: "Yes, I'd like to go."

다음 구문:

```json
{
  "conditions": "input.text.contains('Yes')"
}
```
{: codeblock}

결과: 조건이 `true`입니다.

### String.endsWith(string)

이 메소드는 문자열이 입력 하위 문자열로 끝나는 경우 true를 리턴합니다.

다음 입력의 경우:

```
"What is your name?".
```
{: codeblock}

다음 구문:

```json
{
  "conditions": "input.text.endsWith('?')"
}
```
{: codeblock}

결과: 조건이 `true`입니다.

### String.extract(String regexp, Integer groupIndex)

이 메소드는 입력 정규식의 지정된 그룹 인덱스에 따라 추출된 문자열을 리턴합니다.

다음 입력의 경우:

```
"Hello 123456".
```
{: codeblock}

다음 구문:

```json
{
  "context": {
    "number_extract": "<? input.text.extract('[\\d]+',0) ?>"
  }
}
```
{: codeblock}

  **중요:** `\\d`를 정규식으로 처리하려면 다른 `\\`: `\\\\d`를 추가하여 백슬래시를 둘 다 이스케이프 처리해야 합니다.

결과:

```json
{
  "context": {
    "number_extract": "123456"
  }
}
```
{: codeblock}

### String.find(string regexp)

이 메소드는 문자열의 세그먼트가 입력 정규식과 일치하는 경우 true를 리턴합니다. 이 메소드를 JSONArray 또는 JSONObject 요소에 대해 호출할 수 있으며, 비교하기 전에 배열 또는 오브젝트를 문자열로 변환합니다.

다음 입력의 경우:

```
"Hello 123456".
```
{: screen}

다음 구문:

```json
{
  "conditions": "input.text.find('^[^\d]*[\d]{6}[^\d]*$')"
}
```
{: codeblock}

결과: 입력 텍스트의 숫자 부분이 정규식 `^[^\d]*[\d]{6}[^\d]*$`와 일치하므로 조건은 true입니다.

### String.isEmpty()

이 메소드는 문자열이 널이 아닌 빈 문자열인 경우 true를 리턴합니다.

다음 대화 상자 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "my_text_variable": ""
  }
}
```
{: codeblock}

다음 구문:

```json
{
  "conditions": "$my_text_variable.isEmpty()"
}
```
{: codeblock}

결과: 조건이 `true`입니다.

### String.length()

이 메소드는 문자열의 문자 길이를 리턴합니다.

다음 입력의 경우:

```
"Hello"
```
{: codeblock}

다음 구문:

```json
{
  "context": {
    "input_length": "<? input.text.length() ?>"
  }
}
```
{: codeblock}

다음 출력이 결과입니다.

```json
{
  "context": {
    "input_length": 5
  }
}
```
{: screen}

### String.matches(string regexp)

이 메소드는 문자열이 입력 정규식과 일치하는 경우 true를 리턴합니다.

다음 입력의 경우:

```
"Hello".
```
{: codeblock}

다음 구문:

```json
{
  "conditions": "input.text.matches('^Hello$')"
}
```
{: codeblock}

결과: 입력 텍스트가 정규식 `\^Hello\$`와 일치하므로 조건은 true입니다.

### String.startsWith(string)

이 메소드는 문자열이 입력 하위 문자열로 시작되는 경우 true를 리턴합니다.

다음 입력의 경우:

```
"What is your name?".
```
{: codeblock}

다음 구문:

```json
{
  "conditions": "input.text.startsWith('What')"
}
```
{: codeblock}

결과: 조건이 `true`입니다.

### String.substring(int beginIndex, int endIndex)

이 메소드는 `beginIndex`의 문자와 `endIndex` 앞에 인덱스로 설정된 마지막 문자가 포함된 하위 문자열을 가져옵니다.
endIndex 문자는 포함되지 않습니다.

다음 대화 상자 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

다음 구문:

```json
{
  "context": {
    "my_text": "<? $my_text.substring(5, $my_text.length()) ?>"
  }
}
```
{: codeblock}

다음 출력이 결과입니다.

```json
{
  "context": {
    "my_text": "is a text."
  }
}
```
{: screen}

### String.toLowerCase()

이 메소드는 소문자로 변환되는 원래 문자열을 리턴합니다.

다음 입력의 경우:

```
"This is A DOG!"
```
{: codeblock}

다음 구문:

```json
{
  "context": {
    "input_lower_case": "<? input.text.toLowerCase() ?>"
  }
}
```
{: codeblock}

다음 출력이 결과입니다.

```json
{
  "context": {
    "input_upper_case": "this is a dog!"
  }
}
```
{: screen}

### String.toUpperCase()

이 메소드는 대문자로 변환되는 원래 문자열을 리턴합니다.

다음 입력의 경우:

```
"hi there".
```
{: codeblock}

다음 구문:

```json
{
  "context": {
    "input_upper_case": "<? input.text.toUpperCase() ?>"
  }
}
```
{: codeblock}

다음 출력이 결과입니다.

```json
{
  "context": {
    "input_upper_case": "HI THERE"
  }
}
```
{: screen}

### String.trim()

이 메소드는 문자열의 시작과 끝에 있는 공백을 자르고 수정된 문자열을 리턴합니다.

다음 대화 상자 런타임 컨텍스트의 경우:

```json
{
  "context": {
    "my_text": "   something is here    "
  }
}
```
{: codeblock}

다음 구문:

```json
{
  "context": {
    "my_text": "<? $my_text.trim() ?>"
  }
}
```
{: codeblock}

다음 출력이 결과입니다.

```json
{
  "context": {
    "my_text": "something is here"
  }
}
```
{: screen}

문자열을 추출하는 시스템 엔티티에 대한 정보는 [시스템 엔티티](system-entities.html)를 참조하십시오.
