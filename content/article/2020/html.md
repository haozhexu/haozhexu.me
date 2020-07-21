---
title: "HTML Cheatsheet"
date: 2020-07-21T11:00:00+11:00
draft: false
ads: true
categories:
  - Notes
tags:
  - html
  - cheatsheet
---

# HTML Cheatsheet

Basic components of HTML document:

```html
<!DOCTYPE html>
<html>
    <head></head>
    <body>
    </body>
</html>
```

Specify W3C XHTML standard:

```html
<html xmlns="http://www.w3.org/1999/xhtml">
```

Comments:

```html
<!-- comments -->
```

## Inside head

### meta

- `<title></title>`: name of the page, displayed on title bar, also used for bookmark
- `<meta></meta>`: provide extra information of this page, commonly used:
  - `<meta name="keywords" content="key, word, tag">`: keyword tags of the page
  - `<meta name="description" content="">`: description
  - `<meta name="author" content="Author Name">`: author
  - `<meta name="copyright" content="Copyright Info">`: copyright information
  - `<meta http_equiv="Content-Type" content="text/html; charset="utf-8"/>`: encoding of the page
  - `<meta charset="utf-8">`: short form in HTML5
  - `<meta http-equiv="refresh" content="5;url=URL">`: redirect to URL after 5 seconds
- **note:** `<meta charset="utf-8">` has to be put before `title` or other `meta` tags

### style, script and link

- `style`: used to define CSS style
- `script`: wrapping JavaScript code or import external JavaScript file
- `link`: import external CSS file

```html
<head>
    <style type="text/css">
    </style>
    <script>
    </script>
    <link type="text/css" rel="stylesheet" href="css/index.css">
</head>
```

## Text

- `<h1></h1>` --> `<h6></h6>`: headlines, 6 levels from largest to smallest
- `<p></p>`: paragraph, multiple paragraphs have default spacing between paragraphs
- `<br/>`: line break, not the same as paragraph spacing

### Text Tags

- `<strong></strong>` and `<b></b>`: bold text
- `<em></em>` and `<i></i>`: italic text
- `<cite></cite>`: citation, usually the same as italic
- `<sup></sup>`: superscripted, e.g. `a<sup>2</sup>` is a to the power of 2
- `<sub></sub>`: subscripted, e.g. `H<sub>2</sub>O` hydro-oxygen
- `<s></s>`: stike through line
- `<u></u>`: underline, usually replaced by CSS
- `<big></big>` and `<small></small>`: big and small font, usually replaced by CSS
- `<hr/>`: horizontal line
- `<div></div>`: division of HTML body, used to format content with CSS

Self-closing tags can have opening tag without closing tag, a few examples:

- `<meta/>`
- `<link/>`
- `<br/>`
- `<hr/>`
- `<img/>`
- `<input/>`

### Block and Inline Element

In HTML, block elements occupy at least a whole line and don't co-exist next to other elements on the same line. Inside block elements there can be other block or inline elements.

```html
<body>
    <div>
        <h3>Title 3</h3>
        <p>A paragraph of <em>text</em></p>
        <p>Another paragraph of <i>text</i></p>
    </div>
</body>
```

### Special Symbols

| Symbol | Description        | Code     |
|--------|--------------------|----------|
| "      | double quote       | &quot;   |
| ‘      | left single quote  | &lsquo;  |
| ’      | right single quote | &rsquo;  |
| x      | multiply           | &times;  |
| ÷      | division           | &divide; |
| >      | greater than       | &gt;     |
| <      | less than          | &lt;     |
| &      | ampersand          | &amp;    |
| —      | long dash          | &mdash;  |
| \|     | pipe               | &#124;   |
| §      | section            | &sect;   |
| ©      | copyright          | &copy;   |
| ®      | registration       | &reg;    |
| ™      | trademark          | &trade;  |
| €      | euro               | &euro;   |
| £      | pound              | &pound;  |
| ¥      | Yen                | &yen;    |
| °      | Degree             | &deg;    |
|        | Space              | &nbsp;   |

## List

### Ordered List

```html
<ol type="attribute">
    <li>Item 1</li>
    <li>Item 2</li>
</ol>
```

- `type` is optional, specifying order prefix, default is digit
- `1`: digit
- `a`: lower case alphabets
- `A`: upper case alphabets
- `i`: lower case Rome characters
- `I`: upper case Rome characters

### Unordered List

```html
<ul type="attribute">
    <li>Item 1</li>
    <li>Item 2</li>
</ol>
```

- `type` is also optional, default is solid dot
- `disc`: solid dot
- `circle`: circle
- `square`: solid square

### Definition List

```html
<dl>
    <dt>Term</dt>
    <dd>Description</dd>
</dl>
```

## Table

- an HTML table usually consists of `table`, `tr` for row and `td` for data cell.
- `<th>` stands for table header
- `<thead>`, `<tbody>` and `<tfoot>` isn't required but can make code more organized
- `<colspan>` and `<rowspan>` stand for column span and row span

```html
<table>
    <caption>Table Title</caption>
    <!-- table head -->
    <thead>
        <tr>
            <th>head title 1</th>
            <th>head title 2</th>
        </tr>
    </thead>
    <!-- table body -->
    <tbody>
        <tr>
            <td>data 1</td>
            <td>data 2</td>
        </tr>
        <tr>
            <td>data 3</td>
            <td>data 4</td>
        </tr>
    </tbody>
    <!-- table footer -->
    <tfoot>
        <tr>
            <td colspan="2">footer</td>
        </tr>
    </tfoot>
</table>
```


## Image

```html
<img src="path/image.png" title="Title" alt="alternative text" align=? border=? width=? height=? />
```

- `src`: source of image
- `title`: title of image for viewer, on mouse cursor
- `alt`: alternative text, displayed when image not found, also for search engine
- `align`: alignment for CSS
- `border`: size of border
- `width`, `height`: dimension of image

## Hyperlink

- `<a href="URL">clickable text</a>`: hyperlink to URL
- `<a href="mailto:EMAIL@ADDRESS">clickable email</a>`: hyperlink to email address
- `<a name="Name">`: an anchor location within a document
- `<a href="#Name">clickable text</a>`: a link to an anchor within text
- `<div id="Name">` also creates a target anchor

### target

`target` specifies how a links should be opened.

`<a href="URL" target="target">Clickable Text</a>`

- `_self`: default, open within the same window
- `_blank`: open in a new window
- `_parent`: open in a parent window
- `_top`: open in top window

## Form

Commonly used attributes for `<form>`:

- `name`: name of the form
  - `<form name="Form"></form>`
- `method`: HTTP method, e.g. `"get"`, `"post"`
  - `<form method="post"></form>`
- `action`: where should the form be submitted to
  - `<form action="index.php"></form>`
- `target`: how the window should be opened
  - `<form target="_blank"></form>`
- `enctype`: type of encoding, used for uploading files

### input

`<input type="TYPE" />`

```html
<form method="post">
  Name: <input type="text" />
  Password: <input type="password" maxlength="20" />
  Gender:
  <input type="radio" name="gender" value="Male" checked />Male
  <input type="radio" name="gender" value="Female" />Female
  Favourite Fruits:
  <input type="checkbox" name="fruit" value="Apple" />Apple
  <input type="checkbox" name="fruit" value="Orange" />Orange
  <input type="checkbox" name="fruit" value="Banana" />Banana
  Basic Information:<br/>
  <textarea rows="5" cols="20">Personal information</textarea>
  Skills:<br/>
  <select>
    <option selected>HTML</option>
    <option>CSS</option>
  </select>
</form>
```

- `text`: single line text field
  - `value`: default value of the text field
  - `size`: size of the text field
  - `maxlength`: the limit of characters for input
- `password`: password text field
  - `password` field has same attributes as `text`
- `radio`: single selection radio box
  - `name` and `value` are mandatory attributes
  - `name` the group name the radio belongs to
  - `value` value of the radio
  - use `checked` for default selection
- `checkbox`: multiple selection checkbox
  - `checkbox` has same attributes as `radio`
- `button`, `submit`, `reset`: button
  - `<input type="button" value="Value" />`
  - `<input type="submit" value="Value" />`
  - `<input type="reset" value="Value" />`
- `file`: file upload
  - `<input type="file" />`
- `textarea`: multi-line text input
  - `value`: value of the text input
  - `rows`: number of rows
  - `cols`: number of columns
- `select`: droplist
  - `<option></option>`: item in the droplist
    - `selected`: indicates the option is selected by default
    - `value`: value of the option
  - `multiple`: allow multiple selection
  - `size`: the number of options displayed at a time

## Frame

`iframe` tag is used to embed another web inside a web.

`<iframe src="URL" width="WIDTH" height="HEIGHT"></iframe>`