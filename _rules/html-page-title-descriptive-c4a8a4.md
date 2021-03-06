---
id: c4a8a4
name: HTML page title is descriptive
rule_type: atomic
description: |
  This rule checks that the first title in an HTML page describes the topic or purpose of that page.
accessibility_requirements:
  wcag20:2.4.2: # Page Titled (A)
    forConformance: true
    failed: not satisfied
    passed: satisfied
    inapplicable: further testing needed
input_aspects:
  - DOM Tree
acknowledgements:
  authors:
    - Anne Thyme Nørregaard
    - Corbb O'Connor
htmlHintIgnore:
  # https://www.npmjs.com/package/htmlhint
  # (used with `npm test` to ensure validity of code snippets)
  - 'title-require'
---

## Applicability

This rule applies to the first HTML `title` element that

- is a [descendant](https://dom.spec.whatwg.org/#concept-tree-descendant) of the `html` element of a [web page](https://www.w3.org/TR/WCAG21/#dfn-web-page-s), and
- contains [children](https://dom.spec.whatwg.org/#concept-tree-child) that are [text nodes](https://dom.spec.whatwg.org/#text) that are not only [whitespace](#whitespace).

**Note**: The `title` elements of embedded documents, such as those in `iframe` or `object` elements, are not applicable because those are not web pages according to the definition in WCAG.

**Note**: The [HTML 5.2 specification](https://www.w3.org/TR/html52/document-metadata.html#the-title-element) requires that a document only has one `title` element, and that it is a child of the `head` element of a document. However, HTML 5.2 also describes what should happen in case of multiple titles, and titles outside the `head` element. Because of this, neither of these validation issues causes a conformance problem for WCAG.

## Expectation

The target element describes the topic or purpose of the overall content of the [document](https://dom.spec.whatwg.org/#concept-document).

## Assumptions

_There are no assumptions for this rule._

## Accessibility Support

- This rule assumes that browsers only recognize the first `title` element if multiple `title` elements are present in the [document](https://dom.spec.whatwg.org/#concept-document). Testing shows that this in general is the case. Therefore the scope of this rule is limited to only checking the first `title` element in a document.

## Background

- [Understanding Success Criterion 2.4.2: Page titled](https://www.w3.org/WAI/WCAG21/Understanding/page-titled.html)
- This rule is testing [Technique G88: Providing descriptive titles for Web pages](https://www.w3.org/WAI/WCAG21/Techniques/general/G88)
- A prerequisite for this rule is that [Technique H25: Providing a title using the title element](https://www.w3.org/WAI/WCAG21/Techniques/html/H25) is used for living up to 2.4.2 Page Titled

## Test Cases

### Passed

#### Passed Example 1

The `<title>` element describes the content of the document.

```html
<html>
	<head>
		<title>Clementine harvesting season</title>
	</head>
	<body>
		<p>
			Clementines will be ready to harvest from late October through February.
		</p>
	</body>
</html>
```

#### Passed Example 2

Two `<title>` elements where the first one describes the content of the document.

```html
<html>
	<head>
		<title>Clementine harvesting season</title>
		<title>Second title is ignored</title>
	</head>
	<body>
		<p>
			Clementines will be ready to harvest from late October through February.
		</p>
	</body>
</html>
```

#### Passed Example 3

Even though the descriptive `<title>` element is not placed within the `<head>` element that is the context the element can be used in [according to the HTML specification](https://html.spec.whatwg.org/#the-title-element), the rule still passes, since the browser fixes it, and it doesn't cause any known accessibility issues.

```html
<html>
	<head> </head>
	<body>
		<title>Clementine harvesting season</title>
		<p>
			Clementines will be ready to harvest from late October through February.
		</p>
	</body>
</html>
```

### Failed

#### Failed Example 1

`<title>` is not descriptive of the content of the document.

```html
<html>
	<head>
		<title>Apple harvesting season</title>
	</head>
	<body>
		<p>
			Clementines will be ready to harvest from late October through February.
		</p>
	</body>
</html>
```

#### Failed Example 2

Even though a correct `<title>` element is put in the `<head>` of the document, this rule only looks at the first `<title>` element.

```html
<html>
	<head>
		<title>First title is incorrect</title>
		<title>Clementine harvesting season</title>
	</head>
	<body>
		<p>
			Clementines will be ready to harvest from late October through February.
		</p>
	</body>
</html>
```

### Inapplicable

#### Inapplicable Example 1

This document has a `<title>` element but is inapplicable since the document element is an SVG `svg` element.

```svg
<svg xmlns="http://www.w3.org/2000/svg">
  <title>This is a circle</title>
  <circle cx="150" cy="75" r="50" fill="green"></circle>
</svg>
```
