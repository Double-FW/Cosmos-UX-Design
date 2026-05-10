---
title: Landmarks
description: "Organize web pages with HTML landmarks."
---

## Summary

By adding HTML landmarks to your web pages you enable assistive technologies to identify the regions which a user might want to quickly skip to (or skip over), for example by using the `<header>`, `<nav>`, `<main>`, and `<footer>` tags.

## Purpose

[A WebAIM survey of screen reader users](https://webaim.org/projects/screenreadersurvey9/#landmarks) revealed that more than half of respondents will at least sometimes use HTML landmarks to navigate within a web page. While other features, such as headings and links, may be used more frequently according to the survey, adding HTML landmarks into the array of virtual signposts can only be a help, particularly to users who cannot visually scan a document to decide where to start. 

## Checks

For each web page under test, perform the following checks. Record an answer, "yes" or "no" to each question. The passing result is indicated in uppercase. For each failing result, document it either as a defect or else provide an explanation of why it should be allowed as an exception or why it is not applicable.

### Checks which could be automated

- Does the web page have exactly one region identified with the `main` landmark? [Y/n]
- Is all content on the web page contained within a landmark region? [Y/n]

### Checks which should be (at least partly) performed manually

- Are areas of the web page that are primarily meant to display navigational links identified using the `nav` landmark? [Y/n]
- Is the web page header content identified using the `header` landmark? [Y/n]
- Is the web page footer content identified using the `footer` landmark? [Y/n]
- Considering other landmarks, if they are used, does each correctly communicate the content it contains: `section`, `article`, `aside`?
- If a skip link exists, is that link at or near the top of the web page and can it be used when navigating by keyboard to junp to the main content? [Y/n]

## Additional Testing Methods and Strategies

Navigating by HTML landmarks is a technique that can be used by both sighted and blind users, even without the aid of a screen reader device. For example [browser plug-ins are available](https://chrome.google.com/webstore/detail/landmark-navigation-via-k/ddpokpbjopmeeiiolheejjpkonlkklgp?hl=en-GB) to allow the user to easily display a navigation menu based on the HTML landmarks. Use the browser plugins to test that each landmark is correct.

## Examples and Additional Information

For users who do not have access to navigate-by-landmark features built into their web browser or assistive technology, you may provide what are know as "skip links" as a feature of the web page itself. The skip links should be the first content in the body of the web page and should link to the content identified using the `main` landmark.

```html
<body>
    <nav>
        <a class="show-on-focus" href="#main">Skip to main content.</a>
        <!-- additional navigation and header content -->
    </nav>

    <main id="main">
        The main content...
    </main>
    
    ...
```

This is most helpful for users that would otherwise have to scan through lots of navigation links and header content with a keyboard for example before getting to the content they actually want to access.

It is acceptable to hide these skip link to avoid cluttering the web page for users who navigate by pointer (mouse or touch for example) [by applying CSS](https://css-tricks.com/how-to-create-a-skip-to-content-link/) that causes the link to be visually hidden until it receives keyboard focus.
