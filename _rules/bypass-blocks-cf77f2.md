---
id: cf77f2
name: Bypass Blocks
rule_type: composite
description: |
  This rule checks that each page has a mechanism to bypass blocks of content.
accessibility_requirements:
  wcag20:2.4.1: # Bypass Blocks (A)
    forConformance: true
    failed: not satisfied
    passed: satisfied
    inapplicable: further testing needed
input_rules:
  - b40fd1
authors:
- Jean-Yves Moyen
- Anne Thyme Nørregaard
---

## Applicability

This rule applies to any [document](#https://www.w3.org/TR/dom/#concept-document) where the [document element](#https://www.w3.org/TR/dom/#document-element) is an HTML `<html>` element.

## Expectation

For each test target, the outcome of at least one of the following rules is passed:

- [HTML page has a main landmark](https://act-rules.github.io/rules/b40fd1)


For each test target, the outcome of at least one of the following rules is passed:
- (G1: Adding a link at the top of each page that goes directly to the main content area)
- (G123: Adding a link at the beginning of a block of repeated content to go to the end of the block)
- (G124: Adding links at the top of the page to each area of the content)
- (H69: Providing heading elements at the beginning of each section of content)
- (H70: Using frame elements to group blocks of repeated material AND H64: Using the title attribute of the frame and iframe elements)
- (SCR28: Using an expandable and collapsible menu to bypass block of content)

## Assumptions

This rule assumes that the document has blocks of content that are repeated in multiple documents within the same website. If this is not the case, there is no requirement for the type of mechanism tested in this rule.

## Accessibility Support

Whereas the techniques [ARIA11: Using ARIA landmarks to identify regions of a page](), [H69: Providing heading elements at the beginning of each section of content]() and [H70: Using frame elements to group blocks of repeated material AND H64: Using the title attribute of the frame and iframe elements]() are prefectly valid ways of passing 2.4.1. Bypass Blocks, these solutions are directed towards screen reader users, and keyboard users will not benefit from these. The solutions with links for bypassing blocks will benefit all users.

## Background
- [Understanding Success Criterion 2.4.1: Bypass Blocks](https://www.w3.org/WAI/WCAG21/Understanding/bypass-blocks.html)
- [G1: Adding a link at the top of each page that goes directly to the main content area](https://www.w3.org/WAI/WCAG21/Techniques/general/G1)
- [G123: Adding a link at the beginning of a block of repeated content to go to the end of the block]()
- [G124: Adding links at the top of the page to each area of the content]()
- [ARIA11: Using ARIA landmarks to identify regions of a page]()
- [H69: Providing heading elements at the beginning of each section of content]()
- [H70: Using frame elements to group blocks of repeated material AND H64: Using the title attribute of the frame and iframe elements]()
- [SCR28: Using an expandable and collapsible menu to bypass block of content]()

## Test Cases

### Passed

### Failed

### Inapplicable