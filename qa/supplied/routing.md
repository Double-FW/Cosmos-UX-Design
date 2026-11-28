# Routing

## Rule Description

Routing is the behaviour of single page applications to emulate the conventions of page loads.

## Rule Type

atomic

## Accessibility Requirements Mapping
- Routing
    - Required for conformance
    - Outcome mapping:
        - Any failed outcomes: success criterion is not satisfied
        - All passed outcomes: success criterion needs further testing
        - An inapplicable outcome: success criterion needs further testing

## Input Aspects
- DOM Tree

## Applicability

This rule applies to any HTML page that includes the Routing pattern.

## Expectations

### Expectation 1

The document &lt;title&gt; element content must reflect the current screen at all times. It must described the purpose of the screen and be unique within the screens of the website or application.

### Expectation 2

For sets of pages that share the same navigation block a skip link must be provided to allow keyboard users to bypass the block. It must precede the block and should be the first interactive element on the page.

### Expectation 3

aria-current with a value of page, step, location, true, or other appropriate value should be applied to links that correspond to the current page / step / location.

### Expectation 4

Focus must not be set when the application is first loaded; the browser default behaviour should apply.

### Expectation 5

When route changes take place focus must be set to the new content's &lt;h1&gt; element.
