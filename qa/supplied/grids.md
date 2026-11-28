# Grids

## Rule Description

Grids enable consistent and efficient visual layouts.

## Rule Type

atomic

## Accessibility Requirements Mapping
- Grids
    - Required for conformance
    - Outcome mapping:
        - Any failed outcomes: success criterion is not satisfied
        - All passed outcomes: success criterion needs further testing
        - An inapplicable outcome: success criterion needs further testing

## Input Aspects
- DOM Tree

## Applicability

This rule applies to any HTML page that include Grids.

## Expectations

### Expectation 1

Table elements must not be allowed to wrap and find themselves falling under different headers.

### Expectation 2

Do not use &lt;table&gt; elements for non-tabular content.

### Expectation 3

Semantic elements must be nested according to their specification. Content should be validated against the latest HTML version at the time the document is authored.
