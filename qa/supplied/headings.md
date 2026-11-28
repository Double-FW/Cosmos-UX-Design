# Headings

## Rule Description

Headings provide a semantic and visual hierarchical structure to a document.

This rule tests that pages have a heading structure that meets the requirements of Headings.

## Rule Type

atomic

## Accessibility Requirements Mapping
- Headings
    - Required for conformance
    - Outcome mapping:
        - Any failed outcomes: success criterion is not satisfied
        - All passed outcomes: success criterion needs further testing
        - An inapplicable outcome: success criterion needs further testing

## Input Aspects

DOM Tree

## Applicability

This rule applies to any HTML page with a heading structure.

## Expectations

### Expectation 1

Each document has one &lt;h1&gt; element that has content that is unique to the current document and is a subset of the document's &lt;title&gt; element content and is the first semantic element within the document's &lt;main&gt; element (if one exists).

### Expectation 2

Subheadings must be the first element with content within the section they introduce, and the content of subheadings must describe the section they introduce.

### Expectation 3

Content that serves the purpose of heading within the visual document hierarchy must be marked up as an HTML heading (&lt;h1&gt; - &lt;h6&gt;). No other content should be marked up as a heading.

### Expectation 4

Subheading levels must be chosen based on their belonging within the document structure and not based on their appearance. In practice, this means that subheading levels must be either greater or equal to any previous subheading's level or one level higher.

### Expectation 5

Headings must be followed either by non-heading content or a subheading of one level higher.

## Accessibility Support

There are no major accessibility support issues known for this rule.

## Test Cases

### Passed

#### Passed Example 1

The document has one &lt;h1&gt; element, it is the first element with content inside the &lt;main&gt; element, and it has content that is a subset of the document's &lt;title&gt; element's content.

```html
<title>Search results for "weather": Example site</title>
```

```html
<!-- Other content -->
<main>
  <h1>Search results for "weather"</h1>
</main>
```

#### Passed Example 2

The &lt;h2&gt; subheading is followed by an &lt;h3&gt; subheading and content associated with the &lt;h3&gt; subheading follows it.

```html
<section>
  <h2>Top stories</h2>
  <section>
    <h3>
      <span class="breaking">Breaking:</span>
      Bid to get Labour to change Brexit stance
    </h3>
  </section>
</section>
```

### Failed

#### Failed Example 1

The document has multiple &lt;h1&gt; elements, neither of which is the first element with content inside the &lt;main&gt; element, and neither have content that is a subset of the document's &lt;title&gt; element's content.

```html
<title>Example site</title>
```

```html
<!-- Other content -->
<main>
  <p>Example site</p>
  <div>
    <h1>Search results</h1>
    <h1>"weather"</h1>
  </div>
</main>
```

#### Failed Example 2

The &lt;h2&gt; subheading is followed by an &lt;h4&gt; subheading, skipping a level. Content associated with the &lt;h4&gt; subheading precedes it.

```html
<section>
  <h2>Top stories</h2>
  <section>
    <div class="alert">Breaking:</div>
    <h4>Bid to get Labour to change Brexit stance</h4>
  </section>
</section>
```
