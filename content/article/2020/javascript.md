---
title: "JavaScript Notes"
date: 2020-07-21T14:00:00+11:00
draft: false
ads: true
categories:
  - Notes
tags:
  - javascript
  - cheatsheet
---
# JavaScript Notes

There are three ways of using JavaScript in HTML document.

External:

```html
<head>
  <!-- import JavaScript in head -->
  <script src="index.js"></script>
<body>
  <!-- import JavaScript in body -->
  <script src="index.js"></script>
</body>
```

Internal:

```html
<body>
  <script> // full form: <script type="text/javascript">
    <!-- JavaScript code here -->
  </script>
</body>
```

Inline:

```html
<head>
  <script>
    function showAlert()
    {
        alert("Clicked on a button!")
    }
  </script>
</head>
<body>
  <!-- display an alert on button click -->
  <input type="button" value="aButton" onclick="alert('Whats up?')" />

  <!-- call another function -->
  <input type="button" value="anotherButton" onclick="showAlert()" />
</body>
```

## Basics

- JavaScript has a list of keywords and reserved words
- a line of code in JavaScript usually ends with semicolon
- `// single line comment`
- `/* multiline comments */`

### Variables and Constants

- variables in JavaScript consists of letters, underscores, dollar sign or digits
- first letter of variable name must be either letter, underscore or dollar sign
- variable names cannot be keyword or reserved word
- variable names are case sensitive

```javascript
var a = 10;
var b = 20, c = 30;
a = a + 1

// constants:
var DEBUG = 1;
```

### Data Types

```javascript
var number = 100;
var string1 = 'Single can be wrapped by single quote.'
var string2 = "String can also be wrapped by double quote."
var string3 = "String can have escape value like \" or \'."
if (number < 200) // boolean condition
{
    document.write("number is less than 200.")
}
// undefined:
var n;
document.write(n)
// null value
var m = null;
```

### Operations

| Operator | Meaning                   |
|----------|---------------------------|
| +        | addition                  |
| -        | subtraction               |
| *        | multiplication            |
| /        | division                  |
| %        | remainder                 |
| ++       | add one                   |
| --       | minus one                 |
| =        | plain assignment          |
| -=       | subtraction assignment    |
| +=       | addition assignment       |
| *=       | multiplication assignment |
| /=       | division assignment       |

- addition operation applies for numbers and string
  - number + number = number
  - string + string = string (concatenation)
  - string + number = string

### Comparison

| Operator | Meaning                  |
|----------|--------------------------|
| >        | greater than             |
| <        | less than                |
| >=       | greater than or equal to |
| <=       | less than or equal to    |
| ==       | equal to                 |
| !=       | not equal to             |

### Logic Operation

| Operator | Meaning               |
|----------|-----------------------|
| &&       | AND                   |
| \|\|     | OR                    |
| !        | NOT                   |

### Condition Operation

```javascript
var a = condition ? expression1 : expression2;
```

### Type Conversion

- use `Number()` to convert string to number
  - `Number("123")`: 123
  - `Number("3.14")`: 3.14
  - `Number("hao123")`: `NaN` (Not a Number)
  - `Number("100px")`: `NaN` (Not a Number)
- use `parseInt()` to convert a string to integer
  - `parseInt("+123")`: 123
  - `parseInt("3.14")`: 3
  - `parseInt("hao123")`: `NaN`
  - `parseInt("100px")`: 100
- use `parseFloat()` to convert a string to floating point number
- use `toString()` to convert a number to a string

## Flow Control

### if…else…

```javascript
if (condition) {
    // statement
}

if (condition) {
    // statement1
} else {
    // statement2
}

if (condition1) {
    // statement1
} else if (condition2) {
    // statement2
} else {
    // statement3
}
```

### switch

```javascript
var day = 3;
var week;

switch (day)
{
    case 1:
        week = "Monday"; break;
    case 2:
        week = "Tuesday"; break;
    case 3:
        week = "Wednesday"; break;
    case 4:
        week = "Thursday"; break;
    case 5:
        week = "Friday"; break;
    case 6:
        week = "Saturday"; break;
    default:
        week = "Sunday";
}
```

### loops

```javascript
while (condition)
{
    // statement executed when condition is true
}
```

```javascript
do
{
    // statement
} while(condition);
```

- the body of `while` must be enclosed by braces
- inside the loop there must be a way to make the condition no longer true, in order to prevent dead loop

```javascript
for (initial expression; condition; post-loop expression)
{
    // ...
}

// e.g.
for (var i = 0 ; i < 5 ; i++)
{
    // ...
}
```

## Function

```javascript
function name(argument1, argument2, …, argumentN)
{
    // function body
    return value; // return a value if the function does have one
}
```

- function in JavaScript can either return a value or without return value
- variables defined inside a function is called local variable, which has a scope of the defining function
- function can be invoked directly, as part of an expression, as a hyperlink, or as an event
- a function can be embedded inside another function

```html
<script>
  // function that doesn't return
  function showMessage(msg)
  {
      document.write(msg)
  }
  showMessage("Hi") // direct invocation

  // function that returns
  function addSum(a, b)
  {
      var sum = a + b;
      return sum;
  }
  var n = addSum(1, 2) + 100; // function call as part of expression
  document.write(n);
</script>

<!-- function call as hyperlink -->
<a href="javascript:showMessage('Hello, world!')">Click Me</a>

<body>
  <!-- function call as an event -->
  <input type="button" onclick="showMessage('Submitted')" value="Submit" />
</body>
```

## String Operation

Please look for more detailed documents on this top, below are just a few operations for string.

- use `string.function` to call a function on `string`
- `length`: get length of string
- `toUpperCase()`, `toLowerCase()`: case conversion
- `charAt(n)`: returns the (n+1)th character in the string (index starts with 0)
- `substring(start, end)`: returns a substring from index `start` to `end` (not including the character at `end`)
- `substring(start)`: returns a substring from `start` to end of the string
- `replace(string, replacement)`: returns a new string replacing first occurence of `string` by `replacement`
- `replace(RegEx, replacement)`: returns a new string replacing substrings matching `RegEx` with `replacement`
- `split(separator)` splits a string by `separator` into an array of string components
- `indexOf(substring)`, `lastIndexOf(substring)`: return index of first or last occurrence of `substring`

## Array

To create an array in JavaScript:

```javascript
var arrayName = new Array(element1, element2, …, elementN); // full form
var arrayName = [element1, element2, …, elementN]; // short form
var emptyArray = []; // empty array

arrayName[i] = value; // value assignment to index i
```

- `array.length` gives you the number of elements in `array`
- `array.slice(start, end)` returns a subarray from index `start` to (but not include) `end`
- `array.unshift(new element1, new element2, …)` appends new elements to the head of `array`
- `array.push(new element1, new element2, …)` appends new elements to the end of `array`
- `array.shift()` removes the first element in `array`
- `array.pop()` removes the last element in `array`
- `array.sort(functionName)` sorts the array using the `functionName` as a comparator
- `array.reverse()` reverses the order of elements in `array`
- `array.join(separator)` joins elements in `array` into a string using `separator`
  - by default, comma is used as separator

## Date and Time

Create a new `Date` object:

```javascript
var date = new Date();
```

Please refer to JavaScript documentation for functions of `Date`, basically it has a list of `getXxx` and `setXxx` for retrieving and setting details of the date.

### Mathmatics

In JavaScript we can use `Math` object to perform a list of mathematical calculations. It has constants commonly used in Maths like `PI`, and functions like `abs(x)`, `floor(x)`, `ceil(x)` and `random()`.

## DOM

DOM stands for Document Object Model, the document structure is modeled as an object, different parts of the document can be read and manipulated through the concept of "node", with three commonly used nodes:

```html
<div id="wrapper">text</div>
```

- element (nodeType = 1): the whole line above
- attribute (nodeType = 2): `id="wrapper"`
- text (nodeType = 3): `text`

### Element Node

- `getElementById(idName)` used on `document`, cannot operate on dynamic DOM object
  - `var div = document.getElementById("div1");`: return a single element with ID `div1`
  - change color: `div.style.color = "red";`
- `getElementByTagName(tagName)` used on `document` or other DOM object
  - `var list = document.getElementById("list");`: get single element with ID `list`
  - `var items = list.getElementsByTagName("li");`: get all items under `list`
  - `items[1].style.color = "yellow"`: update color of 2nd item
- `getElementsByClassName(className)`: returns elements by class name
- `querySelector(selector)`: return first element matching `selector`
- `querySelectorAll(selector)`: return all elements matching `selector`
  - `document.querySelectorAll(".test")` returns all elements whose `class` is `test`
  - `document.querySelector("#list li:nth-child(3)")` returns third element under element `list`, see following example

```html
<ul id="list">
  <li>HTML</li>
  <li>CSS</li>
  <li>JavaScript</li>
  <li>jQuery</li>
</ul>
```

- `getElementsByName(name)` returns all elements by name
  - `document.getElementsByName("fruit");` see sample below

```html
<body>
  Favourite Fruit:
  <label><input type="radio" name="fruit" value="Apple" />Apple</label>
  <label><input type="radio" name="fruit" value="Orange" />Orange</label>
  <label><input type="radio" name="fruit" value="Banana" />Banana</label>
</body>
```

#### document.title and document.body

JavaScript provides an easy way to access the title and body of `document`:

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8" />
  <title></title>
  <script>
    window.onload = function()
    {
        document.title = "Title";
        document.body.innerHTML = "<p>A paragraph of text.</p>";
    }
  </script>
</head>
<body></body>
</html>
```

### Create Element

In JavaScript we can dynamically create new elements and modify existings elements, this is called "Dynamic DOM Operation".

- `createElement(elementName)` creates new element node
- `createTextNode(text)` creates new text node
- `appendChild(element)` appends a new element to parent node
- `insertBefore(newElement, refElement)` inserts `newElement` before `refElement`
- `removeChild(element)` removes `element`
- `node.cloneNode(bool)` make a clone of `node`, boolean argument indicates whether sub-elements are cloned as well
- `replaceChild(new, old);` replaces `old` from its parent element by `new`

Sample (add a `strong` text):

```javascript
window.onload = function()
{
    var oDiv = document.getElementById("content");
    var oStrong = document.createElement("strong");
    var oText = document.createTextNode("Text");

    oStrong.appendChild(oText);
    oDiv.appendChild(oStrong);
}
```

Another sample, creating an image equivalent to HTML:

```html
<img class="pic" src="images/photo.png" style="border:1px solid silver" />
```

```javascript
window.onload = function()
{
    var oImg = document.createElement("img");
    oImg.className = "pic"; // className is used instead of class because class is a keyword
    oImg.src = "images/photo.png";
    oImg.style.border = "1px solid silver";
    document.body.appendChild(oImg);
}
```

## DOM Advanced

### HTML Attributes

Access attribute of object via: `obj.attr`, consider the following HTML code:

```html
<input id="btn" type="button" value="Submit" />
```

To retrieve the button:

```javascript
// get the button and assign it `onclick` function
var button = document.getElementById("btn")
button.onclick = function()
{
    alert(button.id);
};
```

To create the button dynamically:

```javascript
var input = documenmt.createElement("input");
input.id = "submit";
input.type = "button";
input.value = "Submit";
document.body.appendChild(input);

input.onclick = function()
{
    alert(input.id);
};
```

For custom attribute we have to use `getAttribute(name)` and `setAttribute(value, name)`, consider this code with custom attribute `data`:

```html
<input id="btn" type="button" value="Insert" data="JavaScript" />
```

To retrieve `data` attribute:

```javascript
var button = document.getElementById("btn");
button.getAttribute("data");
// button.data is undefined
```

- there's also `removeAttribute(name)` and `hasAttribute(name)`
- to get CSS attributes, use `getComputedStyle(object).attr`
  - `var box = document.getElementById("box")`
  - `getComputedStyle(box).backgroundColor`
- to set CSS attributes, use `obj.style.attr`
  - `box.style.backgroundColor = "blue";`
- to set multiple CSS attributes, use `obj.style.cssText`
  - `div.style.cssText = "width:100px;height:100px;border:1px solid gray;";`

### DOM Traversal

- look for parent element: `obj.parentNode`
- look for child elements: `childNodes`, `firstChild`, `lastChild`
  - `children`, `firstElementChild`, `lastElementChild`
- look for sibling elements
  - `previousSibling`, `nextSibling`
  - `previousElementSibling`, `nextElementSibling`

## Events

In JavaScript, an event consists of three parts: Subject, Type and Action. Please refer to JavaScript documentation for list of events for different elements.

### Event Invocation

Trigger event in script tag:

```javascript
object.eventName = function()
{
    // event body
};
```

e.g.

```javascript
window.onload = function()
{
    // event body
};
```

Trigger event in element:

```html
<input type="button" onclick="showAlert()" value="Alert" />
```

### Event Listener

- To add event listener to (or remove from) an object `obj`:

```javascript
obj.addEventListener(type, function, false);
obj.removeEventListener(type, function, false);
```

```javascript
window.onload = function()
{
    var button = document.getElementById("btn");
    button.addEventListener("click", function(){
        alert("First Click");
    }, false);
    button.addEventListener("click", function(){
        alert("Second Click");
    }, false);
    button.addEventListener("click", function(){
        alert("Third Click");
    }, false);
}
```

### event object

An object for event is passed to event function.

| Attribute | Meaning                 |
|-----------|-------------------------|
| type      | type of event           |
| keyCode   | code of key stroke      |
| shiftKey  | if Shift key is pressed |
| ctrlKey   | if Ctrl key is pressed  |
| altKey    | if Alt key is pressed   |

## window object

In JavaScript, every browser window/tab is a `window` object, each `window` object has a list of sub-objects and methods. Please refer to JavaScript documentation for detailed explanation of children objects and methods.

Child objects of `window`:

| sub object | meaning                    |
|------------|----------------------------|
| document   | document model of the page |
| location   | URL related object         |
| navigator  | browser related object     |
| history    | browsing history           |
| screen     | screen related object      |

Methods of `window`:

| method          | meaning                    |
|-----------------|----------------------------|
| alert()         | show an alert dialog       |
| confirm()       | show a confirmation dialog |
| prompt()        | show a prompt dialog       |
| open()          | open a new window          |
| close()         | close the window           |
| setTimeout()    | turn on "once off" timer   |
| clearTimeout()  | turn off "once off" timer  |
| setInterval()   | turn on repeating timer    |
| clearInterval() | turn off repeating time    |

