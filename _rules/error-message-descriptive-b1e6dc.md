---
id: b1e6dc
name: Error message is descriptive
rule_type: composite
description: |
  This rule checks that when an input error is automatically detected the error is identified and described to the user in text

accessibility_requirements: # Remove whatever is not applicable
  wcag20:3.1.1: # Error Identification (A)
    forConformance: true
    failed: not satisfied
    passed: satisfied
    inapplicable: further testing needed

input_rules:
  - 334972
  - 36b590
  - 6f484a
  - 2045c3
  - 54621b

authors:
  -  Carlos Duarte
  -  João Vicente
---

## Applicability

This rule applies to any [document](#https://www.w3.org/TR/dom/#concept-document) where the [document element](#https://www.w3.org/TR/dom/#document-element) is an HTML `html` element that has at least one HTML or SVG element that has one of the following [semantic roles][semantic role]: `checkbox`, `combobox` (`select` elements), `listbox`, `menuitemcheckbox`, `menuitemradio`, `radio`, `searchbox`, `slider`, `spinbutton`, `switch` and `textbox`.

**Note**: The list of applicable [semantic roles][semantic role] is derived by taking all the [ARIA 1.1](https://www.w3.org/TR/wai-aria-1.1/) roles that:

- inherit from the [abstract](https://www.w3.org/TR/wai-aria/#abstract_roles) `input` or `select` role, and
- do not have a [required context](https://www.w3.org/TR/wai-aria/#scope) role that itself inherits from one of those roles.

## Expectation (1)

For each HTML or SVG element that has one of the following [semantic roles][semantic role]: `checkbox`, `combobox` (`select` elements), `listbox`, `menuitemcheckbox`, `menuitemradio`, `radio`, `searchbox`, `slider`, `spinbutton`, `switch` and `textbox` in the test target, the outcome of at least one of the following rules is passed:

- [Required form fields not completed](https://act-rules.github.io/rules/334972)
- [Invalid form field value](https://act-rules.github.io/rules/36b590)
- [aria-alertdialog Identifies Input Error](https://act-rules.github.io/rules/6f484a)
- [alert Role or Live Region Identify Input Error](https://act-rules.github.io/rules/2045c3)
- [aria-invalid Identifies Input Error](https://act-rules.github.io/rules/54621b)

## Assumptions

_There are currently no assumptions._

## Accessibility Support

_There are no major accessibility support issues known for this rule._

## Background

- [Understanding Success Criterion 3.3.1: Error Identification](https://www.w3.org/WAI/WCAG21/Understanding/error-identification)
- If the form contains required fields
    - [G83: Providing text descriptions to identify required fields that were not completed](https://www.w3.org/WAI/WCAG21/Techniques/general/G83)
    - [ARIA21: Using Aria-Invalid to Indicate An Error Field](https://www.w3.org/WAI/WCAG21/Techniques/aria/ARIA21)
    - [SCR18: Providing client-side validation and alert](https://www.w3.org/WAI/WCAG21/Techniques/client-side-script/SCR18)
- If the form requires the information to be in a specific format or of certain values
    - [G84: Providing a text description when the user provides information that is not in the list of allowed values](https://www.w3.org/WAI/WCAG21/Techniques/general/G84)
    - [G85: Providing a text description when user input falls outside the required format or values](https://www.w3.org/WAI/WCAG21/Techniques/general/G85)
    - [ARIA18: Using aria-alertdialog to Identify Errors](https://www.w3.org/WAI/WCAG21/Techniques/aria/ARIA18)
    - [ARIA19: Using ARIA role=alert or Live Regions to Identify Errors](https://www.w3.org/WAI/WCAG21/Techniques/aria/ARIA19)
    - [ARIA21: Using Aria-Invalid to Indicate An Error Field](https://www.w3.org/WAI/WCAG21/Techniques/aria/ARIA21)
    - [SCR18: Providing client-side validation and alert](https://www.w3.org/WAI/WCAG21/Techniques/client-side-script/SCR18)
    - [SCR32: Providing client-side validation and adding error text via the DOM](https://www.w3.org/WAI/WCAG21/Techniques/client-side-script/SCR32)

## Test Cases

### Passed

#### Passed Example 1

The error message that shows near the `input` element when it is not filled indicates the field must be filled. 

```html
<script>
    function processForm() {
        if (document.getElementById('text_field').value.length === 0) {
            document.getElementById('error').innerText = "You must fill the name field"
        }
    }
</script>

<form>
    <label for="text_field">Name (required)</label>
    <input type="text" id="text_field" required>
    <input type="button" value="Submit" onclick="processForm()">
    <div id="error"></div>
</form>
```

#### Passed Example 2

The error message identifies any required `input` element that has not been filled. The error message refers to the `input` elements by their labels. Completed fields retain their values.

```html
<script>
    function processForm() {
        document.getElementById('error').innerText = "";
        if (document.getElementById('name').value.length === 0) {
            document.getElementById('error').innerText += "You must fill the name field."
        }
        var color = document.forms[0].color.value;
        if (color.length === 0) {
            document.getElementById('error').innerText += "You must pick a color."
        }
    }
</script>

<form>
    <label for="name">Name (required)</label>
    <input type="text" id="name">
    <br>
    <label for="address">Address</label>
    <input type="text" id="address">
    <p>Pick a color (required)</p>
    <label><input type="radio" name="color" value="blue">Blue</label>
    <label><input type="radio" name="color" value="yellow">Yellow</label>
    <br>
    <input type="button" value="Submit" onclick="processForm()">
    <div id="error"></div>
</form>
```

#### Passed Example 3

The error message provided in the vicinity of the `input` element identifies the [input error](https://www.w3.org/TR/WCAG21/#dfn-input-error). Instructions are provided in the vicinity of the `input` element.

```html
<script>
    function processForm() {
        document.getElementById('error').innerText = "";
        var age = document.getElementById('age').value;
        console.log(isNaN(age));
        if (!age || isNaN(age)) {
            document.getElementById('error').innerText = "Age must be a number";
        } else if (age < 1) {
            document.getElementById('error').innerText = "Age must be positive";
        }
    }
</script>

<form>
    <label for="age">Age (years)</label>
    <input type="number" id="age" required>
    <span id="error"></span><br>
    <input type="button" value="Submit" onclick="processForm()">
</form>
```

#### Passed Example 4

The error message provided in the vicinity of the `input` element identifies the [input error](https://www.w3.org/TR/WCAG21/#dfn-input-error). Instructions are provided in help text and accessible description.

```html
<script>
    function processForm() {
        document.getElementById('error').innerText = "";
        var age = document.getElementById('age').value;
        console.log(isNaN(age));
        if (!age || isNaN(age)) {
            document.getElementById('error').innerText = "Age must be a number";
        } else if (age < 1) {
            document.getElementById('error').innerText = "Age must be positive";
        }
    }
</script>

<form>
    <label for="age">Age</label>
    <input type="number" id="age" aria-describedby="age_instructions">
    <span id="error"></span><br>
    <input type="button" value="Submit" onclick="processForm()">
</form>

<p>Form instructions:</p>
<p id="age_instructions">Enter age in years</p>
```

#### Passed Example 5

The error message provided in the vicinity of the `input` element identifies the [input error](https://www.w3.org/TR/WCAG21/#dfn-input-error). Instructions are provided in a tooltip.

```html
<html>

<head>

    <style>
        .form_tooltip:hover .tooltip {
            display: block;
        }

        .tooltip {
            display: none;
            background: #C8C8C8;
            margin-left: 28px;
            padding: 10px;
            position: absolute;
            z-index: 1000;
            width: 150px;
            height: 30px;
        }
    </style>

</head>

<body>

    <script>
        function processForm() {
            document.getElementById('error').innerText = "";
            var age = document.getElementById('age').value;
            console.log(isNaN(age));
            if (!age || isNaN(age)) {
                document.getElementById('error').innerText = "Age must be a number";
            } else if (age < 1) {
                document.getElementById('error').innerText = "Age must be positive";
            }
        }
    </script>

    <form>
        <div class="form_tooltip">
            <label for="age">Age</label>
            <input type="number" id="age">
            <p class="tooltip">Enter age in years</p>
        </div>
        <span id="error"></span><br>
        <input type="button" value="Submit" onclick="processForm()">
    </form>

</body>

</html>
```

#### Passed Example 6

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

#### Passed Example 7

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

#### Passed Example 8

alertdialog triggered on form submission presents message identifying the input error.

```html
<html>

<head>
    <meta charset="UTF-8">
    <title>Using aria-alertdialog to Identify Errors</title>
    <script src="http://code.jquery.com/jquery.js"></script>
    <script>
        $(document).ready(function (e) {
            $('#trigger-alertdialog').click(function () {
                var errors = false;
                var content = '<div id="firstElement" tabindex="0"></div><h1 id="alertHeading">Error</h1><div id="alertText">';
                if ($('#birth').val().length === 0) {
                    content += 'Please fill age.<br>';
                    errors = true;
                }
                if ($('#hire').val().length === 0) {
                    content += 'Please fill years on job.<br>';
                    errors = true;
                }
                if (!errors && Number($('#hire').val()) > Number($('#birth').val())) {
                    content += 'Years on job is larger than age. Please verify age and years on job values.'
                    errors = true;
                }
                content += '</div><button id="firstButton">Return to page and correct error</button><div id="lastElement" tabindex="0"></div>';
                if (errors) {
                    $('main').attr('aria-hidden', 'true');
                    var lastFocus = document.activeElement;
                    var modalOverlay = $('<div>').attr({ id: "modalOverlay", tabindex: "0" });
                    $(modalOverlay).appendTo('body');
                    var dialog = $('<div>').attr({ role: "alertdialog", "aria-labelledby": "alertHeading", "aria-describedby": "alertText", tabindex: "0" });
                    $(dialog).html(content).appendTo('body');
                    $('#firstButton').focus();

                    $('#lastElement').focusin(function (e) {
                        $('#firstButton').focus();
                    });
                    $('#firstElement').focusin(function (e) {
                        $('#firstButton').focus();
                    });

                    $('[role=alertdialog] button').click(function (e) {
                        $('main').attr('aria-hidden', 'false');
                        $(modalOverlay).remove();
                        $(dialog).remove();
                        lastFocus.focus();
                    });
                }
                return false;

            });

        });
    </script>
    <style type="text/css">
        #modalOverlay {
            width: 100%;
            height: 100%;
            z-index: 2;
            background-color: #000;
            opacity: 0.5;
            position: fixed;
            top: 0;
            left: 0;
            margin: 0;
            padding: 0;
        }

        [role=alertdialog] {
            width: 50%;
            margin-left: auto;
            margin-right: auto;
            padding: 5px;
            border: thin #000 solid;
            background-color: #fff;
            z-index: 3;
            position: fixed;
            top: 25%;
            left: 25%;
        }
    </style>
</head>

<body>
    <main>
        <form>
            <label for="birth">Age (years)</label>
            <input type="number" id="birth"><br>
            <label for="hire">Years on job</label>
            <input type="number" id="hire"><br>
            <button id="trigger-alertdialog">Submit</button>
        </form>
    </main>
</body>

</html>
```

#### Passed Example 9

alertdialog triggered on user completion presents message identifying the input error.

```html
<html>

<head>
    <meta charset="UTF-8">
    <title>Using aria-alertdialog to Identify Errors</title>
    <script src="http://code.jquery.com/jquery.js"></script>
    <script>
        $(document).ready(function (e) {
            function showDialog(message) {
                var content = '<div id="firstElement" tabindex="0"></div><h1 id="alertHeading">Error</h1><div id="alertText">';
                content += message;
                content += '</div><button id="firstButton">Return to page and correct error</button><div id="lastElement" tabindex="0"></div>';
                $('main').attr('aria-hidden', 'true');
                var lastFocus = document.activeElement;
                var modalOverlay = $('<div>').attr({ id: "modalOverlay", tabindex: "0" });
                $(modalOverlay).appendTo('body');
                var dialog = $('<div>').attr({ role: "alertdialog", "aria-labelledby": "alertHeading", "aria-describedby": "alertText", tabindex: "0" });
                $(dialog).html(content).appendTo('body');
                $('#firstButton').focus();

                $('#lastElement').focusin(function (e) {
                    $('#firstButton').focus();
                });
                $('#firstElement').focusin(function (e) {
                    $('#firstButton').focus();
                });

                $('[role=alertdialog] button').click(function (e) {
                    $('main').attr('aria-hidden', 'false');
                    $(modalOverlay).remove();
                    $(dialog).remove();
                    lastFocus.focus();
                });
                return false;

            }
            $('#birth').focusout(function () {
                if ($('#birth').val().length === 0) {
                    showDialog('Please fill age.<br>');
                }
            });
            $('#hire').focusout(function () {
                if ($('#hire').val().length === 0) {
                    showDialog('Please fill years on job.<br>');
                }
            });
        });
    </script>
    <style type="text/css">
        #modalOverlay {
            width: 100%;
            height: 100%;
            z-index: 2;
            background-color: #000;
            opacity: 0.5;
            position: fixed;
            top: 0;
            left: 0;
            margin: 0;
            padding: 0;
        }

        [role=alertdialog] {
            width: 50%;
            margin-left: auto;
            margin-right: auto;
            padding: 5px;
            border: thin #000 solid;
            background-color: #fff;
            z-index: 3;
            position: fixed;
            top: 25%;
            left: 25%;
        }
    </style>
</head>

<body>
    <main>
        <form>
            <label for="birth">Age (years)</label>
            <input type="number" id="birth"><br>
            <label for="hire">Years on job</label>
            <input type="number" id="hire"><br>
            <button id="trigger-alertdialog">Submit</button>
        </form>
    </main>
</body>

</html>
```

#### Passed Example 10

`aria-invalid` attribute set on target elements on form submission.

```html
<html lang="en">

<head>
    <meta charset="utf-8">
    <script src="http://code.jquery.com/jquery-1.10.2.js"></script>
    <script>
        $(document).ready(function (e) {
            $('#signup').submit(function () {
                $('#errText').html('');
                $('input').removeAttr("aria-invalid");
                $('label').removeAttr("class");
                var eFlag = 0;
                if ($('#first').val() === '') {
                    $('#first').attr("aria-invalid", "true");
                    $("label[for='first']").addClass('failed');
                    eFlag++;
                }
                if ($('#last').val() === '') {
                    $('#last').attr("aria-invalid", "true");
                    $("label[for='last']").addClass('failed');
                    eFlag++;
                }
                if ($('#email').val() === '') {
                    $('#email').attr("aria-invalid", "true");
                    $("label[for='email']").addClass('failed');
                    eFlag++;
                }
                if (eFlag > 0) {
                    $('#errText').html("Please complete all required fields (" + eFlag + ") and retry").focus();
                }
                return false;
            });
        });
    </script>
    <style type="text/css">
        label.failed {
            border: red thin solid;
        }
    </style>

</head>

<body>
    <h2 tabindex="-1" id="errText" aria-live="assertive"></h2>
    <form name="signup" id="signup" method="post" action="#">
        <p>
            <label for="first">First Name (required)</label><br>
            <input type="text" name="first" id="first">
        </p>
        <p>
            <label for="last">Last Name (required)</label><br>
            <input type="text" name="last" id="last">
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

#### Passed Example 11

`aria-invalid` attribute set on target elements on user completion.

```html
<html lang="en">

<head>
    <meta charset="utf-8">
    <script src="http://code.jquery.com/jquery-1.10.2.js"></script>
    <script>
        $(document).ready(function (e) {
            function updateMessage() {
                $('#errText').html('');
                var count = $('label[class="failed"]').length;

                if (count > 0) {
                    $('#errText').html("Please complete all required fields (" + count + ") and retry");
                }
            }
            $('#signup').submit(function () {
                updateMessage();
                return false;
            });

            $('#first').focusout(function () {
                $('#first').removeAttr("aria-invalid");
                $('label[for="first"]').removeAttr("class");
                if ($('#first').val() === '') {
                    $('#first').attr("aria-invalid", "true");
                    $("label[for='first']").addClass('failed');
                }
                updateMessage();
            });

            $('#last').focusout(function () {
                $('#last').removeAttr("aria-invalid");
                $('label[for="last"]').removeAttr("class");
                if ($('#last').val() === '') {
                    $('#last').attr("aria-invalid", "true");
                    $("label[for='last']").addClass('failed');
                }
                updateMessage();
            });

            $('#email').focusout(function () {
                $('#email').removeAttr("aria-invalid");
                $('label[for="email"]').removeAttr("class");
                if ($('#email').val() === '') {
                    $('#email').attr("aria-invalid", "true");
                    $("label[for='email']").addClass('failed');
                }
                updateMessage();
            });

        });
    </script>
    <style type="text/css">
        label.failed {
            border: red thin solid;
        }
    </style>

</head>

<body>
    <h2 tabindex="-1" id="errText" aria-live="assertive"></h2>
    <form name="signup" id="signup" method="post" action="#">
        <p>
            <label for="first">First Name (required)</label><br>
            <input type="text" name="first" id="first">
        </p>
        <p>
            <label for="last">Last Name (required)</label><br>
            <input type="text" name="last" id="last">
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

No error message is provided.

```html
<form>
    <label for="text_field">Name (required)</label>
    <input type="text" id="text_field" required>
    <input type="button" value="Submit" onclick="processForm()">
    <div id="error"></div>
</form>
```

#### Failed Example 2

The error message does not identify which form fields need to be filled.

```html
<script>
    function processForm() {
        document.getElementById('error').innerText = ""
        if (document.getElementById('name').value.length === 0) {
            document.getElementById('error').innerText = "You must fill all required fields."
        }
        var color = document.forms[0].color.value;
        if (color.length === 0) {
            document.getElementById('error').innerText = "You must fill all required fields."
        }
    }
</script>

<form>
    <label for="name">Name (required)</label>
    <input type="text" id="name" required>
    <br>
    <label for="address">Address</label>
    <input type="text" id="address">
    <p>Pick a color (required)</p>
    <label><input type="radio" name="color" value="blue" required>Blue</label>
    <label><input type="radio" name="color" value="yellow">Yellow</label>
    <br>
    <input type="button" value="Submit" onclick="processForm()">
    <div id="error"></div>
</form>
```

#### Failed Example 3

The error message is not visible.

```html
<script>
    function processForm() {
        if (document.getElementById('text_field').value.length === 0) {
            document.getElementById('error').innerText = "You must fill the name field"
        }
    }
</script>

<form>
    <label for="text_field">Name (required)</label>
    <input type="text" id="text_field" required>
    <input type="button" value="Submit" onclick="processForm()">
    <div id="error" style="position: absolute; top: -9999px; left: -9999px;"></div>
</form>
```

#### Failed Example 4

The error message is not included in the accessibility tree.

```html
<script>
    function processForm() {
        if (document.getElementById('text_field').value.length === 0) {
            document.getElementById('error').innerText = "You must fill the name field"
        }
    }
</script>

<form>
    <label for="text_field">Name (required)</label>
    <input type="text" id="text_field" required>
    <input type="button" value="Submit" onclick="processForm()">
    <div id="error" aria-hidden="true"></div>
</form>
```

#### Failed Example 5

Completed input fields do not retain their values.

```html
<script>
    function processForm() {
        document.getElementById('error').innerText = "";
        if (document.getElementById('name').value.length === 0) {
            document.getElementById('error').innerText += "You must fill the name field.";
            document.forms[0].reset();
        }
        var color = document.forms[0].color.value;
        if (color.length === 0) {
            document.getElementById('error').innerText += "You must pick a color."
            document.forms[0].reset();
        }
    }
</script>

<form>
    <label for="name">Name (required)</label>
    <input type="text" id="name">
    <br>
    <label for="address">Address</label>
    <input type="text" id="address">
    <p>Pick a color (required)</p>
    <label><input type="radio" name="color" value="blue">Blue</label>
    <label><input type="radio" name="color" value="yellow">Yellow</label>
    <br>
    <input type="button" value="Submit" onclick="processForm()">
    <div id="error"></div>
</form>
```

#### Failed Example 6

No error message is provided.


```html
<form>
    <label for="age">Age (years)</label>
    <input type="number" id="age" required>
    <span id="error"></span><br>
    <input type="button" value="Submit">
</form>

```

#### Failed Example 7

The error message does not identify the [input error](https://www.w3.org/TR/WCAG21/#dfn-input-error).

```html
<script>
    function processForm() {
        document.getElementById('error').innerText = "";
        var age = document.getElementById('age').value;
        console.log(isNaN(age));
        if (!age || isNaN(age) || age < 1) {
            document.getElementById('error').innerText = "Please fill the field correctly.";
        }
    }
</script>

<form>
    <label for="age">Age (years)</label>
    <input type="number" id="age" required>
    <span id="error"></span><br>
    <input type="button" value="Submit" onclick="processForm()">
</form>
```

#### Failed Example 8

The error message is not visible.

```html
<script>
    function processForm() {
        document.getElementById('error').innerText = "";
        var age = document.getElementById('age').value;
        console.log(isNaN(age));
        if (!age || isNaN(age)) {
            document.getElementById('error').innerText = "Age must be a number";
        } else if (age < 1) {
            document.getElementById('error').innerText = "Age must be positive";
        }
    }
</script>

<form>
    <label for="age">Age (years)</label>
    <input type="number" id="age" required>
    <span id="error" style="position: absolute; top: -9999px; left: -9999px;"></span><br>
    <input type="button" value="Submit" onclick="processForm()">
</form>
```

#### Failed Example 9

The error message is not included in the accessibility tree.

```html
<script>
    function processForm() {
        document.getElementById('error').innerText = "";
        var age = document.getElementById('age').value;
        console.log(isNaN(age));
        if (!age || isNaN(age)) {
            document.getElementById('error').innerText = "Age must be a number";
        } else if (age < 1) {
            document.getElementById('error').innerText = "Age must be positive";
        }
    }
</script>

<form>
    <label for="age">Age (years)</label>
    <input type="number" id="age" required>
    <span id="error" aria-hidden="true"></span><br>
    <input type="button" value="Submit" onclick="processForm()">
</form>
```

#### Failed Example 10

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

#### Failed Example 11

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

#### Failed Example 12

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

#### Failed Example 13

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

#### Failed Example 14

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

#### Failed Example 15

alertdialog without focusable elements.

```html
<html>

<head>
    <meta charset="UTF-8">
    <title>Using aria-alertdialog to Identify Errors</title>
    <script src="http://code.jquery.com/jquery.js"></script>
    <script>
        $(document).ready(function (e) {
            $('#trigger-alertdialog').click(function () {
                var errors = false;
                var content = '</div><h1 id="alertHeading">Error</h1><div id="alertText">';
                if ($('#birth').val().length === 0) {
                    content += 'Please fill age.<br>';
                    errors = true;
                }
                if ($('#hire').val().length === 0) {
                    content += 'Please fill years on job.<br>';
                    errors = true;
                }
                if (!errors && Number($('#hire').val()) > Number($('#birth').val())) {
                    content += 'Years on job is larger than age. Please verify age and years on job values.'
                    errors = true;
                }
                content += '</div><div id="firstButton">Click here to return to page and correct error</div>';
                if (errors) {
                    $('main').attr('aria-hidden', 'true');
                    var lastFocus = document.activeElement;
                    var modalOverlay = $('<div>').attr({ id: "modalOverlay", tabindex: "0" });
                    $(modalOverlay).appendTo('body');
                    var dialog = $('<div>').attr({ role: "alertdialog", "aria-labelledby": "alertHeading", "aria-describedby": "alertText", tabindex: "0" });
                    $(dialog).html(content).appendTo('body');

                    $('[role=alertdialog] #firstbutton').click(function (e) {
                        $('main').attr('aria-hidden', 'false');
                        $(modalOverlay).remove();
                        $(dialog).remove();
                        lastFocus.focus();
                    });
                }
                return false;

            });

        });
    </script>
    <style type="text/css">
        #modalOverlay {
            width: 100%;
            height: 100%;
            z-index: 2;
            background-color: #000;
            opacity: 0.5;
            position: fixed;
            top: 0;
            left: 0;
            margin: 0;
            padding: 0;
        }

        [role=alertdialog] {
            width: 50%;
            margin-left: auto;
            margin-right: auto;
            padding: 5px;
            border: thin #000 solid;
            background-color: #fff;
            z-index: 3;
            position: fixed;
            top: 25%;
            left: 25%;
        }
    </style>
</head>

<body>
    <main>
        <form>
            <label for="birth">Age (years)</label>
            <input type="number" id="birth"><br>
            <label for="hire">Years on job</label>
            <input type="number" id="hire"><br>
            <button id="trigger-alertdialog">Submit</button>
        </form>
    </main>
</body>

</html>
```

#### Failed Example 16

alertdialog opens without moving the focus to one of its focusable elements.

```html
<html>

<head>
    <meta charset="UTF-8">
    <title>Using aria-alertdialog to Identify Errors</title>
    <script src="http://code.jquery.com/jquery.js"></script>
    <script>
        $(document).ready(function (e) {
            $('#trigger-alertdialog').click(function () {
                var errors = false;
                var content = '<div id="firstElement" tabindex="0"></div><h1 id="alertHeading">Error</h1><div id="alertText">';
                if ($('#birth').val().length === 0) {
                    content += 'Please fill age.<br>';
                    errors = true;
                }
                if ($('#hire').val().length === 0) {
                    content += 'Please fill years on job.<br>';
                    errors = true;
                }
                if (!errors && Number($('#hire').val()) > Number($('#birth').val())) {
                    content += 'Years on job is larger than age. Please verify age and years on job values.'
                    errors = true;
                }
                content += '</div><button id="firstButton">Return to page and correct error</button><div id="lastElement" tabindex="0"></div>';
                if (errors) {
                    $('main').attr('aria-hidden', 'true');
                    var lastFocus = document.activeElement;
                    var modalOverlay = $('<div>').attr({ id: "modalOverlay", tabindex: "0" });
                    $(modalOverlay).appendTo('body');
                    var dialog = $('<div>').attr({ role: "alertdialog", "aria-labelledby": "alertHeading", "aria-describedby": "alertText", tabindex: "0" });
                    $(dialog).html(content).appendTo('body');

                    $('#lastElement').focusin(function (e) {
                        $('#firstButton').focus();
                    });
                    $('#firstElement').focusin(function (e) {
                        $('#firstButton').focus();
                    });

                    $('[role=alertdialog] button').click(function (e) {
                        $('main').attr('aria-hidden', 'false');
                        $(modalOverlay).remove();
                        $(dialog).remove();
                        lastFocus.focus();
                    });
                }
                return false;

            });

        });
    </script>
    <style type="text/css">
        #modalOverlay {
            width: 100%;
            height: 100%;
            z-index: 2;
            background-color: #000;
            opacity: 0.5;
            position: fixed;
            top: 0;
            left: 0;
            margin: 0;
            padding: 0;
        }

        [role=alertdialog] {
            width: 50%;
            margin-left: auto;
            margin-right: auto;
            padding: 5px;
            border: thin #000 solid;
            background-color: #fff;
            z-index: 3;
            position: fixed;
            top: 25%;
            left: 25%;
        }
    </style>
</head>

<body>
    <main>
        <form>
            <label for="birth">Age (years)</label>
            <input type="number" id="birth"><br>
            <label for="hire">Years on job</label>
            <input type="number" id="hire"><br>
            <button id="trigger-alertdialog">Submit</button>
        </form>
    </main>
</body>

</html>
```

#### Failed Example 17

alertdialog that lets the focus move away from it.

```html
<html>

<head>
    <meta charset="UTF-8">
    <title>Using aria-alertdialog to Identify Errors</title>
    <script src="http://code.jquery.com/jquery.js"></script>
    <script>
        $(document).ready(function (e) {
            $('#trigger-alertdialog').click(function () {
                var errors = false;
                var content = '<div id="firstElement" tabindex="0"></div><h1 id="alertHeading">Error</h1><div id="alertText">';
                if ($('#birth').val().length === 0) {
                    content += 'Please fill age.<br>';
                    errors = true;
                }
                if ($('#hire').val().length === 0) {
                    content += 'Please fill years on job.<br>';
                    errors = true;
                }
                if (!errors && Number($('#hire').val()) > Number($('#birth').val())) {
                    content += 'Years on job is larger than age. Please verify age and years on job values.'
                    errors = true;
                }
                content += '</div><button id="firstButton">Return to page and correct error</button><div id="lastElement" tabindex="0"></div>';
                if (errors) {
                    $('main').attr('aria-hidden', 'true');
                    var lastFocus = document.activeElement;
                    var modalOverlay = $('<div>').attr({ id: "modalOverlay", tabindex: "0" });
                    $(modalOverlay).appendTo('body');
                    var dialog = $('<div>').attr({ role: "alertdialog", "aria-labelledby": "alertHeading", "aria-describedby": "alertText", tabindex: "0" });
                    $(dialog).html(content).appendTo('body');
                    $('#firstButton').focus();

                    $('[role=alertdialog] button').click(function (e) {
                        $('main').attr('aria-hidden', 'false');
                        $(modalOverlay).remove();
                        $(dialog).remove();
                        lastFocus.focus();
                    });
                }
                return false;

            });

        });
    </script>
    <style type="text/css">
        #modalOverlay {
            width: 100%;
            height: 100%;
            z-index: 2;
            background-color: #000;
            opacity: 0.5;
            position: fixed;
            top: 0;
            left: 0;
            margin: 0;
            padding: 0;
        }

        [role=alertdialog] {
            width: 50%;
            margin-left: auto;
            margin-right: auto;
            padding: 5px;
            border: thin #000 solid;
            background-color: #fff;
            z-index: 3;
            position: fixed;
            top: 25%;
            left: 25%;
        }
    </style>
</head>

<body>
    <main>
        <form>
            <label for="birth">Age (years)</label>
            <input type="number" id="birth"><br>
            <label for="hire">Years on job</label>
            <input type="number" id="hire"><br>
            <button id="trigger-alertdialog">Submit</button>
        </form>
    </main>
</body>

</html>
```

#### Failed Example 18

After closing the alertdialog the focus does not return to the element that had it before opening the alertdialog.

```html
<html>

<head>
    <meta charset="UTF-8">
    <title>Using aria-alertdialog to Identify Errors</title>
    <script src="http://code.jquery.com/jquery.js"></script>
    <script>
        $(document).ready(function (e) {
            $('#trigger-alertdialog').click(function () {
                var errors = false;
                var content = '<div id="firstElement" tabindex="0"></div><h1 id="alertHeading">Error</h1><div id="alertText">';
                if ($('#birth').val().length === 0) {
                    content += 'Please fill age.<br>';
                    errors = true;
                }
                if ($('#hire').val().length === 0) {
                    content += 'Please fill years on job.<br>';
                    errors = true;
                }
                if (!errors && Number($('#hire').val()) > Number($('#birth').val())) {
                    content += 'Years on job is larger than age. Please verify age and years on job values.'
                    errors = true;
                }
                content += '</div><button id="firstButton">Return to page and correct error</button><div id="lastElement" tabindex="0"></div>';
                if (errors) {
                    $('main').attr('aria-hidden', 'true');
                    var modalOverlay = $('<div>').attr({ id: "modalOverlay", tabindex: "0" });
                    $(modalOverlay).appendTo('body');
                    var dialog = $('<div>').attr({ role: "alertdialog", "aria-labelledby": "alertHeading", "aria-describedby": "alertText", tabindex: "0" });
                    $(dialog).html(content).appendTo('body');
                    $('#firstButton').focus();

                    $('#lastElement').focusin(function (e) {
                        $('#firstButton').focus();
                    });
                    $('#firstElement').focusin(function (e) {
                        $('#firstButton').focus();
                    });

                    $('[role=alertdialog] button').click(function (e) {
                        $('main').attr('aria-hidden', 'false');
                        $(modalOverlay).remove();
                        $(dialog).remove();
                        $('#birth').focus();
                    });
                }
                return false;

            });

        });
    </script>
    <style type="text/css">
        #modalOverlay {
            width: 100%;
            height: 100%;
            z-index: 2;
            background-color: #000;
            opacity: 0.5;
            position: fixed;
            top: 0;
            left: 0;
            margin: 0;
            padding: 0;
        }

        [role=alertdialog] {
            width: 50%;
            margin-left: auto;
            margin-right: auto;
            padding: 5px;
            border: thin #000 solid;
            background-color: #fff;
            z-index: 3;
            position: fixed;
            top: 25%;
            left: 25%;
        }
    </style>
</head>

<body>
    <main>
        <form>
            <label for="birth">Age (years)</label>
            <input type="number" id="birth"><br>
            <label for="hire">Years on job</label>
            <input type="number" id="hire"><br>
            <button id="trigger-alertdialog">Submit</button>
        </form>
    </main>
</body>

</html>
```

#### Failed Example 19

The element with `role="alertdialog"` does not have an accessible name.

```html
<html>

<head>
    <meta charset="UTF-8">
    <title>Using aria-alertdialog to Identify Errors</title>
    <script src="http://code.jquery.com/jquery.js"></script>
    <script>
        $(document).ready(function (e) {
            $('#trigger-alertdialog').click(function () {
                var errors = false;
                var content = '<div id="firstElement" tabindex="0"></div><h1 id="alertHeading">Error</h1><div id="alertText">';
                if ($('#birth').val().length === 0) {
                    content += 'Please fill age.<br>';
                    errors = true;
                }
                if ($('#hire').val().length === 0) {
                    content += 'Please fill years on job.<br>';
                    errors = true;
                }
                if (!errors && Number($('#hire').val()) > Number($('#birth').val())) {
                    content += 'Years on job is larger than age. Please verify age and years on job values.'
                    errors = true;
                }
                content += '</div><button id="firstButton">Return to page and correct error</button><div id="lastElement" tabindex="0"></div>';
                if (errors) {
                    $('main').attr('aria-hidden', 'true');
                    var lastFocus = document.activeElement;
                    var modalOverlay = $('<div>').attr({ id: "modalOverlay", tabindex: "0" });
                    $(modalOverlay).appendTo('body');
                    var dialog = $('<div>').attr({ role: "alertdialog", "aria-describedby": "alertText", tabindex: "0" });
                    $(dialog).html(content).appendTo('body');
                    $('#firstButton').focus();

                    $('#lastElement').focusin(function (e) {
                        $('#firstButton').focus();
                    });
                    $('#firstElement').focusin(function (e) {
                        $('#firstButton').focus();
                    });

                    $('[role=alertdialog] button').click(function (e) {
                        $('main').attr('aria-hidden', 'false');
                        $(modalOverlay).remove();
                        $(dialog).remove();
                        lastFocus.focus();
                    });
                }
                return false;

            });

        });
    </script>
    <style type="text/css">
        #modalOverlay {
            width: 100%;
            height: 100%;
            z-index: 2;
            background-color: #000;
            opacity: 0.5;
            position: fixed;
            top: 0;
            left: 0;
            margin: 0;
            padding: 0;
        }

        [role=alertdialog] {
            width: 50%;
            margin-left: auto;
            margin-right: auto;
            padding: 5px;
            border: thin #000 solid;
            background-color: #fff;
            z-index: 3;
            position: fixed;
            top: 25%;
            left: 25%;
        }
    </style>
</head>

<body>
    <main>
        <form>
            <label for="birth">Age (years)</label>
            <input type="number" id="birth"><br>
            <label for="hire">Years on job</label>
            <input type="number" id="hire"><br>
            <button id="trigger-alertdialog">Submit</button>
        </form>
    </main>
</body>

</html>
```

#### Failed Example 20

The content of the alertdialog does not identify the error.

```html
<html>

<head>
    <meta charset="UTF-8">
    <title>Using aria-alertdialog to Identify Errors</title>
    <script src="http://code.jquery.com/jquery.js"></script>
    <script>
        $(document).ready(function (e) {
            $('#trigger-alertdialog').click(function () {
                var errors = false;
                var content = '<div id="firstElement" tabindex="0"></div><h1 id="alertHeading">Error</h1><div id="alertText">';
                if ($('#birth').val().length === 0) {
                    errors = true;
                }
                if ($('#hire').val().length === 0) {
                    errors = true;
                }
                if (!errors && Number($('#hire').val()) > Number($('#birth').val())) {
                    errors = true;
                }
                if (errors) {
                    content += 'Please fix the errors.<br>';
                    content += '</div><button id="firstButton">Return to page and correct error</button><div id="lastElement" tabindex="0"></div>';
                    $('main').attr('aria-hidden', 'true');
                    var lastFocus = document.activeElement;
                    var modalOverlay = $('<div>').attr({ id: "modalOverlay", tabindex: "0" });
                    $(modalOverlay).appendTo('body');
                    var dialog = $('<div>').attr({ role: "alertdialog", "aria-labelledby": "alertHeading", "aria-describedby": "alertText", tabindex: "0" });
                    $(dialog).html(content).appendTo('body');
                    $('#firstButton').focus();

                    $('#lastElement').focusin(function (e) {
                        $('#firstButton').focus();
                    });
                    $('#firstElement').focusin(function (e) {
                        $('#firstButton').focus();
                    });

                    $('[role=alertdialog] button').click(function (e) {
                        $('main').attr('aria-hidden', 'false');
                        $(modalOverlay).remove();
                        $(dialog).remove();
                        lastFocus.focus();
                    });
                }
                return false;

            });

        });
    </script>
    <style type="text/css">
        #modalOverlay {
            width: 100%;
            height: 100%;
            z-index: 2;
            background-color: #000;
            opacity: 0.5;
            position: fixed;
            top: 0;
            left: 0;
            margin: 0;
            padding: 0;
        }

        [role=alertdialog] {
            width: 50%;
            margin-left: auto;
            margin-right: auto;
            padding: 5px;
            border: thin #000 solid;
            background-color: #fff;
            z-index: 3;
            position: fixed;
            top: 25%;
            left: 25%;
        }
    </style>
</head>

<body>
    <main>
        <form>
            <label for="birth">Age (years)</label>
            <input type="number" id="birth"><br>
            <label for="hire">Years on job</label>
            <input type="number" id="hire"><br>
            <button id="trigger-alertdialog">Submit</button>
        </form>
    </main>
</body>

</html>
```

#### Failed Example 21

`aria-invalid` attribute set on target element that does not have an [input error](https://www.w3.org/TR/WCAG21/#dfn-input-error).

```html
<html lang="en">

<head>
    <meta charset="utf-8">
    <script src="http://code.jquery.com/jquery-1.10.2.js"></script>
    <script>
        $(document).ready(function (e) {
            $('#signup').submit(function () {
                $('#errText').html('');
                $('input').removeAttr("aria-invalid");
                $('label').removeAttr("class");
                var eFlag = 0;
                if ($('#first').val() !== '') {
                    $('#first').attr("aria-invalid", "true");
                    $("label[for='first']").addClass('failed');
                    eFlag++;
                }
                if ($('#last').val() !== '') {
                    $('#last').attr("aria-invalid", "true");
                    $("label[for='last']").addClass('failed');
                    eFlag++;
                }
                if ($('#email').val() !== '') {
                    $('#email').attr("aria-invalid", "true");
                    $("label[for='email']").addClass('failed');
                    eFlag++;
                }
                if (eFlag > 0) {
                    $('#errText').html("Please complete all required fields (" + eFlag + ") and retry").focus();
                }
                return false;
            });
        });
    </script>
    <style type="text/css">
        label.failed {
            border: red thin solid;
        }
    </style>

</head>

<body>
    <h2 tabindex="-1" id="errText" aria-live="assertive"></h2>
    <form name="signup" id="signup" method="post" action="#">
        <p>
            <label for="first">First Name (required)</label><br>
            <input type="text" name="first" id="first">
        </p>
        <p>
            <label for="last">Last Name (required)</label><br>
            <input type="text" name="last" id="last">
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

#### Failed Example 22

`aria-invalid` attribute not set on target element that has an [input error](https://www.w3.org/TR/WCAG21/#dfn-input-error).


```html
<html lang="en">

<head>
    <meta charset="utf-8">
    <script src="http://code.jquery.com/jquery-1.10.2.js"></script>
    <script>
        $(document).ready(function (e) {
            $('#signup').submit(function () {
                $('#errText').html('');
                $('input').removeAttr("aria-invalid");
                $('label').removeAttr("class");
                var eFlag = 0;
                if ($('#first').val() === '') {
                    $("label[for='first']").addClass('failed');
                    eFlag++;
                }
                if ($('#last').val() === '') {
                    $("label[for='last']").addClass('failed');
                    eFlag++;
                }
                if ($('#email').val() === '') {
                    $("label[for='email']").addClass('failed');
                    eFlag++;
                }
                if (eFlag > 0) {
                    $('#errText').html("Please complete all required fields (" + eFlag + ") and retry").focus();
                }
                return false;
            });
        });
    </script>
    <style type="text/css">
        label.failed {
            border: red thin solid;
        }
    </style>

</head>

<body>
    <h2 tabindex="-1" id="errText" aria-live="assertive"></h2>
    <form name="signup" id="signup" method="post" action="#">
        <p>
            <label for="first">First Name (required)</label><br>
            <input type="text" name="first" id="first">
        </p>
        <p>
            <label for="last">Last Name (required)</label><br>
            <input type="text" name="last" id="last">
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

#### Failed Example 23

[Programmatically determined](https://www.w3.org/TR/WCAG21/#dfn-programmatically-determinable) label or text instruction do not provide enough information to understand the [input error](https://www.w3.org/TR/WCAG21/#dfn-input-error).

```html
<html lang="en">

<head>
    <meta charset="utf-8">
    <script src="http://code.jquery.com/jquery-1.10.2.js"></script>
    <script>
        $(document).ready(function (e) {
            $('#signup').submit(function () {
                $('#errText').html('');
                $('input').removeAttr("aria-invalid");
                $('label').removeAttr("class");
                var eFlag = 0;
                if ($('#first').val() === '') {
                    $('#first').attr("aria-invalid", "true");
                    $("label[for='first']").addClass('failed');
                    eFlag++;
                }
                if ($('#last').val() === '') {
                    $('#last').attr("aria-invalid", "true");
                    $("label[for='last']").addClass('failed');
                    eFlag++;
                }
                if ($('#email').val() === '') {
                    $('#email').attr("aria-invalid", "true");
                    $("label[for='email']").addClass('failed');
                    eFlag++;
                }
                if (eFlag > 0) {
                    $('#errText').html("Please complete all required fields (" + eFlag + ") and retry").focus();
                }
                return false;
            });
        });
    </script>
    <style type="text/css">
        label.failed {
            border: red thin solid;
        }
    </style>

</head>

<body>
    <h2 tabindex="-1" id="errText" aria-live="assertive"></h2>
    <form name="signup" id="signup" method="post" action="#">
        <p>
            <label for="first">First Name</label><br>
            <input type="text" name="first" id="first">
        </p>
        <p>
            <label for="last">Last Name</label><br>
            <input type="text" name="last" id="last">
        </p>
        <p>
            <label for="email">Email</label><br>
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