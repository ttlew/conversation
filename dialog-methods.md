---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-18"

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

# Methods to process values

Use these methods to process values extracted from user utterances that you want to reference in a context variable, condition, or elsewhere in the response.
{: shortdesc}

>**Note:** For methods that involve regular expressions, see [RE2 Syntax reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/google/re2/wiki/Syntax){: new_window} for details about the syntax to use when you specify the regular expression.

## Arrays
{: #arrays}

You cannot use these methods to check for a value in an array in a node condition or response condition within the same node in which you set the array values.

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
{: codeblock}

Make this update:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.append('ketchup', 'tomatoes') ?>"
  }
}
```
{: codeblock}

Result:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ketchup", "tomatoes"]
  }
}
```
{: screen}

### JSONArray.contains(object value)

This method returns true if the input JSONArray contains the input value.

For this Dialog runtime context which is set by a previous node or external application:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Dialog node or response condition:

```json
$toppings_array.contains('ham')
```
{: codeblock}

Result: `True` because the array contains the element ham.

### JSONArray.get(integer)

This method returns the input index from the JSONArray.

For this Dialog runtime context which is set by a previous node or external application:

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

Dialog node or response condition:

```json
$nested.array.get(0).getAsString().contains('one')
```
{: codeblock}

Result:
`True` because the nested array contains `one` as a value.

Response:

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

This method returns a random item from the input JSONArray.

For this Dialog runtime context:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Dialog node output:

```json
{
  "output": {
    "text": "<? $toppings_array.getRandomItem() ?> is a great choice!"
  }
}
```
{: codeblock}

Result: `"ham is a great choice!"` or `"onion is a great choice!"` or `"olives is a great choice!"`

**Note:** The resulting output text is randomly chosen.

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
{: codeblock}

Dialog node output:

```json
{
  "output": {
    "text": "This is the array: <? $toppings_array.join(';') ?>"
  }
}
```
{: codeblock}

Result:

```json
This is the array: onion;olives;ham;
```
{: screen}

### new JsonArray().append('value')

To define a new array that will be filled in with values that are provided by users, you can instantiate an array. You must also add a placeholder value to the array when you instantiate it. You can use the following syntax to do so:

```json
{
  "context":{
    "answer": "<? output.answer?:new JsonArray().append('temp_value') ?>"
  }
```
{: codeblock}

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
{: codeblock}

Make this update:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.remove(0) ?>"
  }
}
```
{: codeblock}

Result:

```json
{
  "context": {
    "toppings_array": ["olives"]
  }
}
```
{: screen}

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
{: codeblock}

Make this update:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.removeValue('onion') ?>"
  }
}
```
{: codeblock}

Result:

```json
{
  "context": {
    "toppings_array": ["olives"]
  }
}
```
{: screen}

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
{: codeblock}

Dialog node output:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.set(1,'ketchup')?>"
  }
}
```
{: codeblock}

Result:

```json
{
  "context": {
    "toppings_array": ["onion", "ketchup", "ham"]
  }
}
```
{: screen}

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
{: codeblock}

Make this update:

```json
{
  "context": {
    "toppings_array_size": "<? $toppings_array.size() ?>"
  }
}
```
{: codeblock}

Result:

```json
{
  "context": {
    "toppings_array_size": 2
  }
}
```
{: screen}

### JSONArray split(String regexp)

This method splits the input string by using the input regular expression. The result is a JSONArray of strings.

For this input:

```
"bananas;apples;pears"
```
{: codeblock}

This syntax:

```json
{
  "context": {
    "array": "<?input.text.split(";")?>
  }
}
```
{: codeblock}

Results in this output:

```json
{
  "context": {
    "array": [ "bananas", "apples", "pears" ]
  }
}
```
{: screen}

## Date and Time
{: #date-time}

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
{: codeblock}

Example of `now()` in node's conditions (to decide if it is still morning):

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

Formats date and time strings to the format desired for user output.

Returns a formatted string according to the specified format:

- `MM/dd/yyyy` for 12/31/2016
- `h a` for 10pm

To return the day of the week:

- `E` for Tuesday
- `u` for day index (1 = Monday, ..., 7 = Sunday)

For example, this context variable definition creates a $time variable that saves the value 17:30:00 as *5:30 PM*.

```json
{
  "context": {
    "time": "<? @sys-time.reformatDateTime('h:mm a') ?>"
  }
}
```
{: codeblock}

Format follows the Java [SimpleDateFormat ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html){: new_window} rules.

>Note: When trying to format time only, the date is treated as `1970-01-01`.

### .sameMoment(String date/time)

- Determines whether the date/time value is the same as the date/time argument.

### .sameOrAfter(String date/time)

- Determines whether the date/time value is after or the same as the date/time argument.
- Analogous to `.after()`.

### .sameOrBefore(String date/time)

- Determines whether the date/time value is before or the same as the date/time argument.

For information about system entities that extract date and time values, see [@sys-date and @sys-time entities](system-entities.html#sys-datetime).

## Numbers
{: #numbers}

### Random()

Returns a random number. Use one of the following syntax options:

- To return a random boolean value (true or false), use `<?new Random().nextBoolean()?>`.
- To return a random double number between 0 (included) and 1 (excluded), use `<?new Random().nextDouble()?>`
- To return a random integer between 0 (included) and a number you specify, use `<?new Random().nextInt(n)?>`  where n is the top of the number range you want + 1. 
  For example, if you want to return a random number between 0 and 10, specify `<?new Random().nextInt(11)?>`.
- To return a random integer from the full Integer value range (-2147483648 to 2147483648), use `<?new Random().nextInt()?>`.

For example, you might create a dialog node that is triggered by the #random_number intent. The first response condition might look like this:

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

  Converts the object or field to the Double number type. You can call this method on any object or field. If the conversion fails, *null* is returned.

### toInt()

  Converts the object or field to the Integer number type. You can call this method on any object or field. If the conversion fails, *null* is returned.

### toLong()

  Converts the object or field to the Long number type. You can call this method on any object or field. If the conversion fails, *null* is returned.

  If you specify a Long number type in a SpEL expression, you must append an `L` to the number to identify it as such. For example, `5000000000L`. This syntax is required for any numbers that do not fit into the 32-bit Integer type. For example, numbers that are greater than 2^31 (2,147,483,648) or lower than -2^31 (-2,147,483,648) are considered Long number types. Long number types have a minimum value of -2^63 and a maximum value of 2^63-1.

For information about system entities that extract numbers, see [@sys-number entity](system-entities.html#sys-number). If you want the service to recognize specific number formats in user input, such as order number references, consider creating a pattern entity. See [Creating entities](entities.html#creating-entities) for more details.

## Objects
{: #objects}

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
{: codeblock}

Dialog node output:

```json
{
  "conditions": "$user.has('first_name')"
}
```
{: codeblock}

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
{: codeblock}

Dialog node output:

```json
{
  "context": {
    "attribute_removed": "<? $user.remove('first_name') ?>"
  }
}
```
{: codeblock}

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
{: screen}

## Strings
{: #strings}

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
{: codeblock}

This syntax:

```json
{
  "context": {
    "my_text": "<? $my_text.append(' More text.') ?>"
  }
}
```
{: codeblock}

Results in this output:

```json
{
  "context": {
    "my_text": "This is a text. More text."
  }
}
```
{: screen}

### String.contains(string)

This method returns true if the string contains the input substring.

Input: "Yes, I'd like to go."

This syntax:

```json
{
  "conditions": "input.text.contains('Yes')"
}
```
{: codeblock}

Results: The condition is `true`.

### String.endsWith(string)

This method returns true if the string ends with the input substring.

For this input:

```
"What is your name?".
```
{: codeblock}

This syntax:

```json
{
  "conditions": "input.text.endsWith('?')"
}
```
{: codeblock}

Results: The condition is `true`.

### String.extract(String regexp, Integer groupIndex)

This method returns a string extracted by specified group index of the input regular expression.

For this input:

```
"Hello 123456".
```
{: codeblock}

This syntax:

```json
{
  "context": {
    "number_extract": "<? input.text.extract('[\\d]+',0) ?>"
  }
}
```
{: codeblock}

  **Important:** To process `\\d` as the regular expression, you have to escape both the backslashes by adding another `\\`: `\\\\d`

Result:

```json
{
  "context": {
    "number_extract": "123456"
  }
}
```
{: codeblock}

### String.find(string regexp)

This method returns true if any segment of the string matches the input regular expression.  You can call this method against a JSONArray or JSONObject element, and it will convert the array or object to a string before making the comparison.

For this input:

```
"Hello 123456".
```
{: screen}

This syntax:

```json
{
  "conditions": "input.text.find('^[^\d]*[\d]{6}[^\d]*$')"
}
```
{: codeblock}

Result: The condition is true because the numeric portion of the input text matches the regular expression `^[^\d]*[\d]{6}[^\d]*$`.

### T(java.lang.String).format()

You can apply a standard Java String format method to text. See [java.util.formatter reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://docs.oracle.com/javase/7/docs/api/java/util/Formatter.html#syntax){: new_window} for information about the syntax to use in the format() method to specify the format details.

For example, the following expression takes three decimal integers (1, 1, and 2) and adds them to a sentence.

```json
{
  "formatted String": "<? T(java.lang.String).format('%d + %d equals %d', 1, 1, 2) ?>"
}
```
{: codeblock}

Result: `1 + 1 equals 2`.

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
{: codeblock}

This syntax:

```json
{
  "conditions": "$my_text_variable.isEmpty()"
}
```
{: codeblock}

Results: The condition is `true`.

### String.length()

This method returns the character length of the string.

For this input:

```
"Hello"
```
{: codeblock}

This syntax:

```json
{
  "context": {
    "input_length": "<? input.text.length() ?>"
  }
}
```
{: codeblock}

Results in this output:

```json
{
  "context": {
    "input_length": 5
  }
}
```
{: screen}

### String.matches(string regexp)

This method returns true if the string matches the input regular expression.

For this input:

```
"Hello".
```
{: codeblock}

This syntax:

```json
{
  "conditions": "input.text.matches('^Hello$')"
}
```
{: codeblock}

Result: The condition is true because the input text matches the regular expression `\^Hello\$`.

### String.startsWith(string)

This method returns true if the string starts with the input substring.

For this input:

```
"What is your name?".
```
{: codeblock}

This syntax:

```json
{
  "conditions": "input.text.startsWith('What')"
}
```
{: codeblock}

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
{: codeblock}

This syntax:

```json
{
  "context": {
    "my_text": "<? $my_text.substring(5, $my_text.length()) ?>"
  }
}
```
{: codeblock}

Results in this output:

```json
{
  "context": {
    "my_text": "is a text."
  }
}
```
{: screen}

### String.toLowerCase()

This method returns the original String converted to lowercase letters.

For this input:

```
"This is A DOG!"
```
{: codeblock}

This syntax:

```json
{
  "context": {
    "input_lower_case": "<? input.text.toLowerCase() ?>"
  }
}
```
{: codeblock}

Results in this output:

```json
{
  "context": {
    "input_upper_case": "this is a dog!"
  }
}
```
{: screen}

### String.toUpperCase()

This method returns the original String converted to upper case letters.

For this input:

```
"hi there".
```
{: codeblock}

This syntax:

```json
{
  "context": {
    "input_upper_case": "<? input.text.toUpperCase() ?>"
  }
}
```
{: codeblock}

Results in this output:

```json
{
  "context": {
    "input_upper_case": "HI THERE"
  }
}
```
{: screen}

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
{: codeblock}

This syntax:

```json
{
  "context": {
    "my_text": "<? $my_text.trim() ?>"
  }
}
```
{: codeblock}

Results in this output:

```json
{
  "context": {
    "my_text": "something is here"
  }
}
```
{: screen}

For information about system entities that extract strings, see the [System entities](system-entities.html).
