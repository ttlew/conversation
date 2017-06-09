---

copyright:
  years: 2015, 2017
lastupdated: "2017-06-09"

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

# Methods

Use these methods to process values extracted from user utterances that you want to reference in a context variable, condition, or elsewhere in the response.
{:shortdesc}

## Arrays  
{:#arrays}

### JSONArray.append(object)

This method appends a new value to the JSONArray and returns the modified JSONArray.

For this Dialog runtime context:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{:codeblock}

Make this update:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.append('ketchup', 'tomatoes') ?>"
  }
}
```
{:codeblock}

Result:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ketchup", "tomatoes"]
  }
}
```
{:codeblock}

### JSONArray.contains(object value)

This method returns true if the input JSONArray contains the input value.

For this Dialog runtime context:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{:codeblock}

Dialog node output:

```json
{
  "conditions": "$toppings_array.contains('ham')"
}
```
{:codeblock}

Result: `True` because the array contains the element ham.

### JSONArray.get(integer)

This method returns the input index from the JSONArray.

For this Dialog runtime context:

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
{:codeblock}

Dialog node output:

```json
{
  "conditions": "$nested.array.get(0).getAsString().contains('one')"
}
```
{:codeblock}

Result:
`True` because the nested array contains `one` as a value.

### JSONArray.getRandomItem()

This method returns a random item from the input JSONArray.

For this Dialog runtime context:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{:codeblock}

Dialog node output:

```json
{
  "output": {
    "text": "<? $toppings_array.getRandomItem() ?> is a great choice!"
  }
}
```
{:codeblock}

Result: `"ham is a great choice!"` or `"onion is a great choice!"` or `"olives is a great choice!"`

**Note**: The resulting output text will be randomly chosen:

### JSONArray.join(string delimiter)

This method joins all values in this array to a string. Values are converted to string and delimited by the input delimiter.

For this Dialog runtime context:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{:codeblock}

Dialog node output:

```json
{
  "output": {
    "text": "This is the array: <? $toppings_array.join(';') ?>"
  }
}
```
{:codeblock}

Result:

```
This is the array: onion;olives;ham;
```
{:screen}

### JSONArray.remove(integer)

This method removes the element in the index position from the JSONArray and returns the updated JSONArray.

For this Dialog runtime context:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{:codeblock}

Make this update:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.remove(0) ?>"
  }
}
```
{:codeblock}

Result:

```json
{
  "context": {
    "toppings_array": ["olives"]
  }
}
```
{:codeblock}

### JSONArray.removeValue(object)

This method removes the first occurrence of the value from the JSONArray and returns the updated JSONArray.

For this Dialog runtime context:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{:codeblock}

Make this update:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.removeValue('onion') ?>"
  }
}
```
{:codeblock}

Result:

```json
{
  "context": {
    "toppings_array": ["olives"]
  }
}
```
{:codeblock}

### JSONArray.set(integer index, object value)

This method sets the input index of the JSONArray to the input value and returns the modified JSONArray.

For this Dialog runtime context:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{:codeblock}

Dialog node output:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.set(1,'ketchup')?>"
  }
}
```
{:codeblock}

Result:

```json
{
  "context": {
    "toppings_array": ["onion", "ketchup", "ham"]
  }
}
```
{:codeblock}

### JSONArray.size()

This method returns the size of the JSONArray as an integer.

For this Dialog runtime context:

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{:codeblock}

Make this update:

```json
{
  "context": {
    "toppings_array_size": "<? $toppings_array.size() ?>"
  }
}
```
{:codeblock}

Result:

```json
{
  "context": {
    "toppings_array_size": 2
  }
}
```
{:codeblock}

### JSONArray split(String regexp)

This method splits the input string by using the input regular expression. The result is a JSONArray of strings.

For this input:

```
"bananas;apples;pears"
```
{:screen}

This syntax:

```json
{
  "context": {
    "array": "<?input.text.split(";")?>
  }
}
```
{:codeblock}

Results in this output:

```json
{
  "context": {
    "array": [ "bananas", "apples", "pears" ]
  }
}
```
{:codeblock}

## Date and Time 
{:#date-time}

Several methods are available to work with date and time.

### .after(String date/time)

  - Determines whether the date/time value is after the date/time argument.
  - Analogous to `.before()`.

### .before(String date or time)

- For example:
  - @sys-time.before('12:00:00')
  - @sys-date.before('2016-11-21')
- Determines whether the date/time value is before the date/time argument.
- If comparing different items, such as `time vs. date`, `date vs. time`, and `time vs. date and time`, the method returns false and an exception is printed in the response JSON log `output.log_messages`.
    - For example, `@sys-date.before(@sys-time)`.
- If comparing `date and time vs. time` the method ignores the date and only compares times.

### now()

- Static function.
- Returns a string with the current date and time in the format `yyyy-MM-dd HH:mm:ss`.
- The other date/time methods can be invoked on date-time values that are returned by this function and it can be passed in as their argument.
- If the context variable `$timezone` is set, this function returns dates and times in the client's time zone.

Example of a dialog node with `now()` used in the output field:

```json
{
  "conditions": "#what_time_is_it",
  "output": {
    "text": "<? now() ?>" 
   }
}
```
{:codeblock}

Example of `now()` in node's conditions (to decide if it is still morning):

```json
{
  "conditions": "now().before('12:00:00')",
  "output": {
    "text": "Good morning!"
   }
}
```
{:codeblock}

### .reformatDateTime(String format)

  - Formats date and time strings to the format desired for user output.
  - Returns a formatted string according to the specified format:
    - `MM/dd/yyy` for 12/31/2016
    - `h a` for 10pm
  - To return the day of the week:
    - `E` for Tuesday
    - `u` for day index (1 = Monday, ..., 7 = Sunday)
  - Format follows the Java [SimpleDateFormat](http://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html) rules.
  - When trying to format time only, the date is treated as `1970-01-01`.

### .sameMoment(String date/time)

  - Determines whether the date/time value is the same as the date/time argument.

### .sameOrAfter(String date/time)

  - Determines whether the date/time value is after or the same as the date/time argument.
  - Analogous to `.after()`.

### .sameOrBefore(String date/time)

  - Determines whether the date/time value is before or the same as the date/time argument.

For information about system entities that extract date and time values, see [@sys-date and @sys-time entities](system-entities.html#sys-datetime).
  
## Numbers 
{:#numbers}

### toDouble()

  Converts the object or field to the Double number type. You can call this method on any object or field. If the conversion fails, *null* is returned.

### toInt()

  Converts the object or field to the Integer number type. You can call this method on any object or field. If the conversion fails, *null* is returned.

### toLong()

  Converts the object or field to the Long number type. You can call this method on any object or field. If the conversion fails, *null* is returned.

For information about system entities that extract numbers, see [@sys-number entity](system-entities.html#sys-number).

## Objects 
{:#objects}

### JSONObject.has(string)

This method returns true if the complex JSONObject has a property of the input name.

For this Dialog runtime context:

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
{:codeblock}

Dialog node output:

```json
{
  "conditions": "$user.has('first_name')"
}
```
{:codeblock}

Result: The condition is true because the user object contains the property `first_name`.

### JSONObject.remove(string)

This method removes a property of the name from the input `JSONObject`. The `JSONElement` that is returned by this method is the `JSONElement` that is being removed.

For this Dialog runtime context:

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
{:codeblock}

Dialog node output:

```json
{
  "context": {
    "attribute_removed": "<? $user.remove('first_name') ?>"
  }
}
```
{:codeblock}

Result:

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
{:codeblock}

## Strings 
{:#strings}

### String.append(object)

This method appends an input object to the string as a string and returns a modified string.

For this Dialog runtime context:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{:codeblock}

This syntax:

```json
{
  "context": {
    "my_text": "<? $my_text.append(' More text.') ?>"
  }
}
```
{:codeblock}

Results in this output:

```json
{
  "context": {
    "my_text": "This is a text. More text."
  }
}
```
{:codeblock}

### String.endsWith(string)

This method returns true if the string ends with the input substring.

For this input:

```
"What is your name?".
```
{:screen}

This syntax:

```json
{
  "conditions": "input.text.endsWith('?')"
}
```
{:codeblock}

Results: The condition is `true`.

### String.extract(String regexp, Integer groupIndex)

This method returns a string extracted by specified group index of the input regular expression.

For this input:

```
"Hello 123456".
```
{:screen}

This syntax:

```json
{
  "context": {
    "number_extract": "<? input.text.extract('[\\d]+',0) ?>"
  }
}
```
{:codeblock}

  **Important**: To process `\\d` as the regular expression, you have to escape both the backslashes by adding another `\\`: `\\\\d`

Result:

```json
{
  "context": {
    "number_extract": "123456"
  }
}
```
{:codeblock}

### String.find(string regexp)

This method returns true if any segment of the string matches the input regular expression.  You can call this method against a JSONArray or JSONObject element, and it will convert the array or object to a string before making the comparison.

For this input:

```
"Hello 123456".
```
{:screen}

This syntax:

```json
{
  "conditions": "input.text.find('^[^\d]*[\d]{6}[^\d]*$')"
}
```
{:codeblock}

Result: The condition is true because the numeric portion of the input text matches the regular expression `^[^\d]*[\d]{6}[^\d]*$`.

### String.isEmpty()

This method returns true if the string is an empty string, but not null.

For this Dialog runtime context:

```json
{
  "context": {
    "my_text_variable": ""
  }
}
```
{:codeblock}

This syntax:

```json
{
  "conditions": "$my_text_variable.isEmpty()"
}
```
{:codeblock}

Results: The condition is `true`.

### String.length()

This method returns the character length of the string.

For this input:

```
"Hello"
```
{:screen}

This syntax:

```json
{
  "context": {
    "input_length": "<? input.text.length() ?>"
  }
}
```
{:codeblock}

Results in this output:

```json
{
  "context": {
    "input_length": 5
  }
}
```
{:codeblock}

### String.matches(string regexp)

This method returns true if the string matches the input regular expression.

For this input:

```
"Hello".
```
{:screen}

This syntax:

```json
{
  "conditions": "input.text.matches('^Hello$')"
}
```
{:codeblock}

Result: The condition is true because the input text matches the regular expression `\^Hello\$`.

### String.startsWith(string)

This method returns true if the string starts with the input substring.

For this input:

```
"What is your name?".
```
{:screen}

This syntax:

```json
{
  "conditions": "input.text.startsWith('What')"
}
```
{:codeblock}

Results: The condition is `true`.

### String.substring(int beginIndex, int endIndex)

This method gets a substring with the character at `beginIndex` and the last character set to index before `endIndex`.
The endIndex character is not included.

For this Dialog runtime context:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{:codeblock}

This syntax:

```json
{
  "context": {
    "my_text": "<? $my_text.substring(5, $my_text.length()) ?>"
  }
}
```
{:codeblock}

Results in this output:

```json
{
  "context": {
    "my_text": "is a text."
  }
}
```
{:codeblock}

### String.toLowerCase()

This method returns the original String converted to lowercase letters.

For this input:

```
"This is A DOG!"
```
{:screen}

This syntax:

```json
{
  "context": {
    "input_lower_case": "<? input.text.toLowerCase() ?>"
  }
}
```
{:codeblock}

Results in this output:

```json
{
  "context": {
    "input_upper_case": "this is a dog!"
  }
}
```
{:codeblock}

### String.toUpperCase()

This method returns the original String converted to upper case letters.

For this input:

```
"hi there".
```
{:screen}

This syntax:

```json
{
  "context": {
    "input_upper_case": "<? input.text.toUpperCase() ?>"
  }
}
```
{:codeblock}

Results in this output:

```json
{
  "context": {
    "input_upper_case": "HI THERE"
  }
}
```
{:codeblock}

### String.trim()

This method trims any spaces at the beginning and the end of the string and returns the modified string.

For this Dialog runtime context:

```json
{
  "context": {
    "my_text": "   something is here    "
  }
}
```
{:codeblock}

This syntax:

```json
{
  "context": {
    "my_text": "<? $my_text.trim() ?>"
  }
}
```
{:codeblock}

Results in this output:

```json
{
  "context": {
    "my_text": "something is here"
  }
}
```
{:codeblock}

For information about system entities that extract strings, see the [System entities reference](system-entities.html).
