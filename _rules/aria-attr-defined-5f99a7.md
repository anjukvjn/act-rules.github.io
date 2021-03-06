---
id: 5f99a7
name: '`aria-*` attribute is defined in WAI-ARIA'
rule_type: atomic
description: |
  This rule checks that each `aria-` attribute specified is defined in ARIA 1.1.
accessibility_requirements:
input_aspects:
  - DOM Tree
acknowledgements:
  authors:
    - Jey Nandakumar
---

## Applicability

Any attribute that starts with `aria-`.

## Expectation

Each target attribute is defined in [WAI-ARIA Specifications](#wai-aria-specifications).

## Assumptions

_There are currently no assumptions_

## Accessibility Support

There are no major accessibility support issues known for this rule.

## Background

- [ARIA in HTML (working draft)](https://www.w3.org/TR/html-aria/#index-aria-global)
- [WAI ARIA Supported States and Properties](http://www.w3.org/TR/wai-aria/#states_and_properties)
- [G108: Using markup features to expose the name and role](https://www.w3.org/WAI/WCAG21/Techniques/general/G108)
- [Understanding Success Criterion 4.1.2: Name, Role, Value](https://www.w3.org/WAI/WCAG21/Understanding/name-role-value)
- [Semantics and ARIA](https://developers.google.com/web/fundamentals/accessibility/semantics-aria/)

## Test Cases

### Passed

#### Passed Example 1

A valid ARIA 1.1 attribute `aria-atomic` is used on element `article`.

```html
<article aria-atomic="true">This is a description of something cool...</article>
```

#### Passed Example 2

A valid ARIA 1.1 attribute `aria-modal` on element `div` with role `dialog`

```html
<div role="dialog" aria-modal="true">Contains modal content...</div>
```

#### Passed Example 3

A valid ARIA 1.1 attribute `aria-live` on element `div` with role `alert`

```html
<div role="alert" aria-live="assertive">
	Your session will expire in 60 seconds.
</div>
```

#### Passed Example 4

Multiple valid ARIA 1.1 attributes `aria-*` are specified on element `input` with role `spinbutton`

```html
<input role="spinbutton" aria-valuemax="100" aria-valuemin="0" aria-valuenow="25" type="number" value="25" />
```

### Failed

#### Failed Example 1

`aria-not-checked` is not a defined attribute in ARIA 1.1.

```html
<li role="menuitemcheckbox" aria-not-checked="true">List Item</li>
```

#### Failed Example 2

`aria-labelled` is not a defined attribute in ARIA 1.1.

```html
<span id="label">Birthday:</span>
<div contenteditable role="searchbox" aria-labelled="label" aria-placeholder="MM-DD-YYYY">
	01-01-2019
</div>
```

### Inapplicable

#### Inapplicable Example 1

Element without `aria-*` attribute.

```html
<canvas> </canvas>
```
