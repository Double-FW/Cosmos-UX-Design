---
title: Structure
description: Organising a webpage to prevent barriers for users who scan and navigate content using assistive technologies.
---

## Document title

When using screen-reading software, the webpage title is always the first element announced. Having a unique, descriptive but concise title will make it easier for screen reader users to determine the content and purpose of your web page without having to scan the entire page.

Every page of a website should have its own unique title, capturing the overall content or purpose of that particular page. But consider that a user may have followed a link to arrive at the page, so appending appropriate context, for example the website title or the website section would be helpful. 

```html
<head>
  <title>The Perfect Get-Together: What to drink | Recipes | Diageo</title>
</head>
```

Avoid very short titles that don't provide an adequate overview of the content. But keep in mind that titles longer than about 60 characters are likely to be cropped in search engines listings.

### Dynamically updating the title

If you are following a "single page application" model and update a webpage content dynamically, be aware that changing a webpage `title` via a script method will not cause the new title to be announced by most screen readers. It may be necessary to use an ARIA Live Region to announce when new content is loading and then move focus to the webpage's `H1` element to make it obvious that the page content is different.

```html
<h1 tabindex="-1">The Perfect Get-Together: What to drink</h1>

```

## Headings

Screen readers will allow users to see a list of just the webpage's headings and to navigate the webpage by jumping directly to a specific heading. Keeping the total number of headings to a reasonable level, using descriptive but concise headings, and organising headings logically and hierarchically will make this much easier for users.

VoiceOver presents a menu that allows the user to scan and navigate the webpage by headings.

Notice that the VoiceOver menu crops any headings which are over about 50 characters in length, so it's useful to front-load your heading content so that the most specific and relevant text is near the beginning.

## Landmarks

Use landmarks to identify the significant areas of a page. Keep the landmark list concise and check that people can find the main areas from it. See [Landmarks](Landmarks.md) for role, naming and navigation checks.

### Overview

HTML tags that represent landmarks:

- `<aside>` - implies the `complementary` landmark role by default; used for content that relates to and supports the core content but can be understood separately.
- A page-level `<footer>` exposes `contentinfo`. A footer inside a sectioning element or `main` supplies information about that part of the page.
- A page-level `<header>` exposes `banner`. A header inside a sectioning element or `main` introduces that part of the page.
- `<main>` - implies the `main` role by default; used for content that expresses or relates directly to the core topic or purpose of a webpage.
- `<nav>` - implies the `navigation` role by default; intended to present navigational links to content within the webpage or the website.
- `<section>` implies the `region` role when the element has an accessible name; used for generic region of related content within a webpage, used when a more specific semantic element is not available.

Using landmarks can help users of screen reader assistive technology navigate your webpage. The VoiceOver screen reader for example will announce when the user enters a region, and offer options for easily navigating over, into, or out of that content.

VoiceOver announces that the user has moved the screen reader cursor into a footer region: "You are currently on a footer, inside web content. To exit this web area, press Control-Option-Shift-Up Arrow."

### Nested regions

The various regions may be related to one another according to logical rules: some regions are not permitted to be contained within others, while some regions are often found within others. A `nav` element for example is frequently a child of the `header` element, creating a Banner landmark which contains a Navigation landmark.

Navigation links with a `nav` element create a `navigation` role within a `header` element that creates a `banner` role.

### Applying regions

A typical webpage will be divided into landmarks using a general tag structure like:

```html
<body>
  <header>
    <nav aria-label="primary-links">
    </nav>
  </header>
  <main>
    <section aria-label="recipes">
    </section>
    <section aria-label="brands">
    </section>
  </main>
  <footer>
    <nav aria-label="footer-links">
    </nav>
  </footer>
</body>
```

Based on this markup the browser will identify each region based on its role.

* Banner
  - Navigation
* Main
  - Region (labelled)
  - Region (labelled)
* Content information
  - Navigation

A typical webpage is logically divided into several landmarks and regions. Here each region is overlaid and represented as a colored rectangle on the screengrab of the webpage.

#### Navigating by landmark

A user of the VoiceOver screen reader can hear a list of landmarks as an overview to use when quickly scanning or navigating the webpage content.

VoiceOver presents a list of landmarks on the webpage: "banner, navigation, main, recipes region, brands region, footer, navigation"

## Search

The `search` role is used to identify the form element (or elements) on a webpage that provide the ability to search the site, the page, or some other realated data source. The `search` role should be added the the `form` tag, or the element surrounding the form.

```html
<form role="search">
  <label for="search-input-nav">I'm interested in...</label>
  <input type="search" id="search-input-nav" name="q">
  <button type="submit">Search</button>
</form>
```

Multiple forms with the `search` role on a page can be distiguished by adding a descriptive `aria-label` that explains the purpose of each. Or you may use the same label if each search has the same purpose.

```html
<form role="search" aria-label="Site">
  ...
</form>

<form role="search" aria-label="Recipe">
  ...
</form>
```

Note that when screen readers encounter a landmark role, they will announce it after reading the label. In the example above: "Site search" and "Recipe search".

## The main landmark

The `main` role identifies central content that is the subject of that particular webpage; If the `title` describes the central content of the webpage, the element with the `main` role contains that content.

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
