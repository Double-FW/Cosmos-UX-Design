---
title: Page Structure
description: Organising a webpage to prevent barriers for users who scan and navigate content using assistive technologies.
---

# Page Structure

Assemble a page so people can identify it, find its main content and bypass repeated navigation. Use this guide for the document title, page layout in HTML, search landmarks and skip-link markup. For cross-cutting ARIA choices, see [ARIA and Screen Reader Usability](ARIA%20and%20Screen%20Reader%20Usability.md); for hidden content and skip-link visibility, see [Visually Hidden Content](Visually%20Hidden%20Content.md). For detailed guidance and checks on section labels and landmark roles, see [Headings](Headings.md) and [Landmarks](Landmarks.md).

## Document title

Give every page a unique, concise title that identifies its topic and, where useful, its service or site. Keep the wording consistent with the visible page heading where possible. Browsers and assistive technologies may announce the title when a page loads or navigation changes, and people can retrieve it later; do not assume it is always the first or only information announced.

```html
<head>
  <title>The Perfect Get-Together: What to drink | Recipes | Diageo</title>
</head>
```

### Dynamically updating the title

For client-side navigation, updating the document title is one part of introducing the new view. Keep the title, visible heading and current navigation state aligned, then follow [Client-side Routing](Client-side%20Routing.md#first-load-and-route-changes) for focus and loading feedback. Do not add a live announcement or move focus as a reflex for every in-page update; use [Dynamic Content](Dynamic%20Content.md) when the view itself has not changed.

## Headings

Screen readers commonly provide heading navigation, including a list of headings and commands to move between them. Use a logical hierarchy and concise, descriptive wording so the outline helps people understand and navigate the page. Some assistive technology views may truncate long headings; front-load the words that distinguish the section rather than relying on a fixed character limit.

Use [Headings](Headings.md) for heading levels, wording and tester checks.

## Landmarks

Use landmarks to identify the significant areas of a page. Keep the landmark list concise and check that people can find the main areas from it. See [Landmarks](Landmarks.md) for role, naming and navigation checks.

### Overview

Use page-level landmarks to identify the main areas of the page. Within those areas, headings label the content people can navigate to. See [Landmarks](Landmarks.md#guidance) for the role and naming rules.

### Nested regions

The various regions may be related to one another according to logical rules: some regions are not permitted to be contained within others, while some regions are often found within others. A `nav` element for example is frequently a child of the `header` element, creating a Banner landmark which contains a Navigation landmark.

Navigation links with a `nav` element create a `navigation` role within a `header` element that creates a `banner` role.

### Applying regions

This example shows a page title, a main heading, a named section and two navigation landmarks. The routes are illustrative. Use the [skip-link styles](#the-main-landmark) if the skip link is visually hidden until focused.

```html
<head>
  <title>Walking routes | Journey planner</title>
</head>
<body>
  <a href="#main-content">Skip to main content</a>
  <header>
    <nav aria-label="Main">
      <a href="/routes">Routes</a>
    </nav>
  </header>
  <main id="main-content" tabindex="-1">
    <h1>Walking routes</h1>
    <section aria-labelledby="collection-heading">
      <h2 id="collection-heading">Routes by area</h2>
      <p>Choose a route near your starting point.</p>
    </section>
  </main>
  <footer>
    <nav aria-label="Footer">
      <a href="/contact">Contact us</a>
    </nav>
  </footer>
</body>
```

#### Navigating by landmark

In a screen reader’s landmark list, look for the banner, Main navigation, main content, Routes by area region, content information and Footer navigation. The exact wording depends on the browser and screen reader. Use the [Landmarks checks](Landmarks.md#checks) to verify that each area is identifiable.

## Search

Use the native `<search>` element to identify a search or filtering region. It exposes a search landmark without adding a redundant `role="search"`. Give each search field its own associated label. When a page has more than one search region, give each a name that distinguishes its purpose. See the [Search guide](Search.md) for suggestions, query submission, results and interaction checks.

Use `role="search"` on a suitable container only where the host environment does not support the native element.

```html
<search aria-label="Site">
  <form action="/search" method="get">
    <label for="site-query">Search this site</label>
    <input id="site-query" name="q" type="search">
    <button type="submit">Search</button>
  </form>
</search>
```

If another search region appears on the page, give it a distinct name, such as `aria-label="Recipes"`. The landmark name identifies the search scope; the visible field label identifies what to enter.

## The main landmark

Use the native `<main>` element for the central content of the current page or application view. It exposes the `main` landmark without adding a redundant `role="main"`.

> The main content area consists of content that is directly related to or expands upon the central topic of a document, or the main function of an application. [^1]

Expose one `main` landmark for the document’s active view. If an application retains inactive views in the markup, keep their content out of the accessibility tree and keyboard navigation.

Give keyboard users an early skip link to the main content. Make it visible on focus, and check where focus and the next Tab stop land after activation.

```html
<a class="skip-link" href="#main-content">Skip to main content</a>
<header>Site header and navigation</header>
<main id="main-content" tabindex="-1">
  <h1>Page title</h1>
  <p>Main content.</p>
</main>

<style>
  .skip-link:not(:focus) {
    position: absolute;
    inline-size: 1px;
    block-size: 1px;
    overflow: hidden;
    clip-path: inset(50%);
    white-space: nowrap;
  }
</style>
```

## Section

Use a `section` for a related part of the page. It exposes a `region` landmark when it has an accessible name. Add a name when the section is worth finding through landmark navigation and a more specific landmark does not fit. Prefer to reference the visible heading.

```html
<section aria-labelledby="collection-heading">
  <h2 id="collection-heading">Collection options</h2>
  <p>Choose a collection location when you place your order.</p>
</section>
```

## Footer

The page-level `footer` exposes the `contentinfo` landmark. Use it for information about the site or page as a whole. An article, section or other part of a page can also have its own footer. That footer belongs to its local content and does not expose another `contentinfo` landmark by default.

```html
<body>
  <main>
    <article>
      <h1>Planning a woodland walk</h1>
      <p>Check the route and current conditions before setting out.</p>
      <footer><a href="/walks">Related walking guides</a></footer>
    </article>
  </main>
  <footer><a href="/contact">Contact us</a></footer>
</body>
```

[^1]: [_ARIA: main role_](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles/main_role), MDN web docs
