# Promos

## Rule Description

A Promo component is used to advertise content. It contains a headline link to the target content and optionally an image, media indication icon, description, or metadata strip.

This rule tests that when a promo is included in an HTML page it meets the requirements of the Promo component.

## Rule Type

atomic

## Accessibility Requirements Mapping
- Promos
    - Required for conformance
    - Outcome mapping:
        - Any failed outcomes: success criterion is not satisfied
        - All passed outcomes: success criterion needs further testing
        - An inapplicable outcome: success criterion needs further testing

## Input Aspects
- DOM Tree

## Applicability

This rule applies to any HTML page that includes a Promos component.

## Expectations

### Expectation 1

Each promo component contains a headline link; the headline link must contain text, and the text should resemble that of the target page's &lt;title&gt; and &lt;h1&gt;.

### Expectation 3

If the promo contains an &lt;img&gt; element it must either:
- have an alt attribute with a null value (alt="") if the image is decorative and provides no additional content
- have an alt attribute with a value that describes the image's purpose

### Expectation 4

&lt;img&gt; elements used in the promo must not be contained within their own link.

### Expectation 5

No other content other than an optional &lt;img&gt; element may appear before the headline link in the source.

### Expectation 6

If a media indication icon is used, it may come before headline link in the source but must then:
- be hidden from assistive technologies using aria-hidden="true"
- have its content repeated as a readable text equivalent in the headline link

### Expectation 7

If a description is used it must appear after the headline link and before the metadata strip in the source.
