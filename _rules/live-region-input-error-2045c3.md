---
id: 2045c3
name: alert role or live region identify input error
rule_type: atomic

description: |
  This rule checks that a container with `role=alert` or a live region are used to identify input errors

accessibility_requirements: # Remove whatever is not applicable
  wcag-technique:ARIA19: # Using ARIA role=alert or Live Regions to Identify Errors
    forConformance: false
    failed: not satisfied
    passed: satisfied
    inapplicable: further testing needed

input_aspects:
  - DOM Tree
  - CSS Styling
  - Language

authors:
  - Carlos Duarte
  - João Vicente
---

## Applicability

This rule applies to any [document](https://www.w3.org/TR/dom/#concept-document) where the [document element](https://www.w3.org/TR/dom/#document-element) is an HTML `html` element that has at least one HTML or SVG element that has one of the following [semantic roles][semantic role]: `checkbox`, `combobox` (`select` elements), `listbox`, `menuitemcheckbox`, `menuitemradio`, `radio`, `searchbox`, `slider`, `spinbutton`, `switch` and `textbox`, that may or may not belong to a [form element](https://www.w3.org/TR/html52/sec-forms.html#the-form-element)

**Note**: The list of applicable [semantic roles][semantic role] is derived by taking all the [ARIA 1.1](https://www.w3.org/TR/wai-aria-1.1/) roles that:

- inherit from the [abstract](https://www.w3.org/TR/wai-aria/#abstract_roles) `input` or `select` role, and
- do not have a [required context](https://www.w3.org/TR/wai-aria/#scope) role that itself inherits from one of those roles.

## Expectation 1

The target element contains an HTML element with either `role=alert` or `aria-live=assertive` attribute that is present in the [document](https://www.w3.org/TR/dom/#concept-document) at the time it [completes loading](https://www.w3.org/TR/html52/dom.html#dom-documentreadystate-complete).

## Expectation 2

The HTML element with either `role=alert` or `aria-live=assertive` attribute displays a message when an [input element](https://www.w3.org/TR/html52/sec-forms.html#the-input-element) is [completed](#completed-input-field) without meeting the instructions provided for it.

Note: Instructions for an [input element](https://www.w3.org/TR/html52/sec-forms.html#the-input-element) include information that is required by the Web page, the data format and possible values. The instructions can be presented to the user in several ways, including:

- Text content placed visually in the vicinity of the [input element](https://www.w3.org/TR/html52/sec-forms.html#the-input-element)
- The accessible description of the [input element](https://www.w3.org/TR/html52/sec-forms.html#the-input-element)
- A tooltip displayed when the [input element](https://www.w3.org/TR/html52/sec-forms.html#the-input-element) is [focused](#focusable)
- Text content accessible through an help button or similar

## Expectation 3

The content of the message is [visible](#visible) and [included in the accessibility tree](included-in-the-accessibility-tree) and identifies the [input error](https://www.w3.org/TR/WCAG21/#dfn-input-error).

## Assumptions

_There are currently no assumptions_

## Accessibility Support

_There are no major accessibility support issues known for this rule._

## Background

- [ARIA19: Using ARIA role=alert or Live Regions to Identify Errors](https://www.w3.org/WAI/WCAG21/Techniques/aria/ARIA19)

## Test Cases

### Passed

#### Passed Example 1

Element with `role="alert"` displays a message on [input error](https://www.w3.org/TR/WCAG21/#dfn-input-error).

```html
<html>
<head>
<meta charset="UTF-8">
<script src="http://code.jquery.com/jquery.js"></script>
<script>
$(document).ready(function(e) {
    $('#name').focusout(function () {
		$('#errors').html('');
        if ($('#name').val() === '') {
            $('#errors').append('<p>Please enter your name.</p>');
        }
    });

    $('#email').focusout(function () {
		$('#errors').html('');
        if ($('#email').val() === '') {
            $('#errors').append('<p>Please enter your email address.</p>');
        }
    });
});
</script>
</head>

<body>
<form name="signup" id="signup" method="post" action="">
  <p id="errors" role="alert" aria-atomic="true"></p>
  <p>
    <label for="name">Name (required)</label><br>
    <input type="text" name="name" id="name">
  </p>
  <p>
    <label for="email">Email (required)</label><br>
    <input type="text" name="email" id="email">
  </p>
  <p>
    <input type="submit" name="button" id="button" value="Submit">
  </p>
</form>
</body>
</html>
```

#### Passed Example 2

Element with `aria-live="assertive"` displays a message on [input error](https://www.w3.org/TR/WCAG21/#dfn-input-error).

```html
<html>
<head>
<meta charset="UTF-8">
<script src="http://code.jquery.com/jquery.js"></script>
<script>
$(document).ready(function(e) {
    $('#name').focusout(function () {
		$('#errors').html('');
        if ($('#name').val() === '') {
            $('#errors').append('<p>Please enter your name.</p>');
        }
    });

    $('#email').focusout(function () {
		$('#errors').html('');
        if ($('#email').val() === '') {
            $('#errors').append('<p>Please enter your email address.</p>');
        }
    });
});
</script>
</head>

<body>
<form name="signup" id="signup" method="post" action="">
  <p id="errors" aria-live="assertive" aria-atomic="true"></p>
  <p>
    <label for="name">Name (required)</label><br>
    <input type="text" name="name" id="name">
  </p>
  <p>
    <label for="email">Email (required)</label><br>
    <input type="text" name="email" id="email">
  </p>
  <p>
    <input type="submit" name="button" id="button" value="Submit">
  </p>
</form>
</body>
</html>
```

### Failed

#### Failed Example 1

Document does not have an element with `role="alert"` or a live region.

```html
<html>
<head>
<meta charset="UTF-8">
<script src="http://code.jquery.com/jquery.js"></script>
<script>
$(document).ready(function(e) {
    $('#name').focusout(function () {
		$('#errors').html('');
        if ($('#name').val() === '') {
            $('#errors').append('<p>Please enter your name.</p>');
        }
    });

    $('#email').focusout(function () {
		$('#errors').html('');
        if ($('#email').val() === '') {
            $('#errors').append('<p>Please enter your email address.</p>');
        }
    });
});
</script>
</head>

<body>
<form name="signup" id="signup" method="post" action="">
  <p id="errors" aria-atomic="true"></p>
  <p>
    <label for="name">Name (required)</label><br>
    <input type="text" name="name" id="name">
  </p>
  <p>
    <label for="email">Email (required)</label><br>
    <input type="text" name="email" id="email">
  </p>
  <p>
    <input type="submit" name="button" id="button" value="Submit">
  </p>
</form>
</body>
</html>
```

#### Failed Example 2

Document does not present a message on [input error](https://www.w3.org/TR/WCAG21/#dfn-input-error).

```html
<form name="signup" id="signup" method="post" action="">
  <p id="errors" aria-live="assertive" aria-atomic="true"></p>
  <p>
    <label for="name">Name (required)</label><br>
    <input type="text" name="name" id="name" required>
  </p>
  <p>
    <label for="email">Email (required)</label><br>
    <input type="text" name="email" id="email" required>
  </p>
  <p>
    <input type="submit" name="button" id="button" value="Submit">
  </p>
</form>
```

#### Failed Example 3

The message is not visible.

```html
<html>
<head>
<meta charset="UTF-8">
<script src="http://code.jquery.com/jquery.js"></script>
<script>
$(document).ready(function(e) {
    $('#name').focusout(function () {
		$('#errors').html('');
        if ($('#name').val() === '') {
            $('#errors').append('<p>Please enter your name.</p>');
        }
    });

    $('#email').focusout(function () {
		$('#errors').html('');
        if ($('#email').val() === '') {
            $('#errors').append('<p>Please enter your email address.</p>');
        }
    });
});
</script>
</head>

<body>
<form name="signup" id="signup" method="post" action="">
  <p id="errors" aria-live="assertive" aria-atomic="true" style="position: absolute; top: -9999px; left: -9999px;"></p>
  <p>
    <label for="name">Name (required)</label><br>
    <input type="text" name="name" id="name">
  </p>
  <p>
    <label for="email">Email (required)</label><br>
    <input type="text" name="email" id="email">
  </p>
  <p>
    <input type="submit" name="button" id="button" value="Submit">
  </p>
</form>
</body>
</html>
```

#### Failed Example 4

The message is not included in the accessibility tree.

```html
<html>
<head>
<meta charset="UTF-8">
<script src="http://code.jquery.com/jquery.js"></script>
<script>
$(document).ready(function(e) {
    $('#name').focusout(function () {
		$('#errors').html('');
        if ($('#name').val() === '') {
            $('#errors').append('<p>Please enter your name.</p>');
        }
    });

    $('#email').focusout(function () {
		$('#errors').html('');
        if ($('#email').val() === '') {
            $('#errors').append('<p>Please enter your email address.</p>');
        }
    });
});
</script>
</head>

<body>
<form name="signup" id="signup" method="post" action="">
  <p id="errors" aria-live="assertive" aria-atomic="true" aria-hidden="true"></p>
  <p>
    <label for="name">Name (required)</label><br>
    <input type="text" name="name" id="name">
  </p>
  <p>
    <label for="email">Email (required)</label><br>
    <input type="text" name="email" id="email">
  </p>
  <p>
    <input type="submit" name="button" id="button" value="Submit">
  </p>
</form>
</body>
</html>
```

#### Failed Example 5

The message does not identify the [input error](https://www.w3.org/TR/WCAG21/#dfn-input-error).

```html
<html>
<head>
<meta charset="UTF-8">
<script src="http://code.jquery.com/jquery.js"></script>
<script>
$(document).ready(function(e) {
    $('#name').focusout(function () {
		$('#errors').html('');
        if ($('#name').val() === '') {
            $('#errors').append('<p>Please fix the error.</p>');
        }
    });

    $('#email').focusout(function () {
		$('#errors').html('');
        if ($('#email').val() === '') {
            $('#errors').append('<p>Please fix the error.</p>');
        }
    });
});
</script>
</head>

<body>
<form name="signup" id="signup" method="post" action="">
  <p id="errors" aria-live="assertive" aria-atomic="true"></p>
  <p>
    <label for="name">Name (required)</label><br>
    <input type="text" name="name" id="name">
  </p>
  <p>
    <label for="email">Email (required)</label><br>
    <input type="text" name="email" id="email">
  </p>
  <p>
    <input type="submit" name="button" id="button" value="Submit">
  </p>
</form>
</body>
</html>
```


### Inapplicable

#### Inapplicable Example 1

No input element.

```html
<div></div>
```

#### Inapplicable Example 2

 [Document](https://www.w3.org/TR/dom/#concept-document) where the [document element](https://www.w3.org/TR/dom/#document-element) is not an HTML `html` element.

```html
<svg height="100" width="100">
    <circle cx="50" cy="50" r="40" stroke="black" stroke-width="3" fill="red" />
    Sorry, your browser does not support inline SVG.
</svg>
```