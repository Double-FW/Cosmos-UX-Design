---
title: Headings
description: "Define a logical outline using headings."
---

## Summary

Within a web page, headings are used to communicate the overall organization and structure of the content on that page. The headings should act as a high-level outline of the page content, with a logical ranking of each topic and subtopic in hierarchical order.

## Purpose

A WebAIM survey of screen reader users revealed that 67.5% of respondents are most [likely to try headings first](https://webaim.org/projects/screenreadersurvey7/#finding) to find information on a lengthy web page. And 85.7% said that [having proper heading levels ](https://webaim.org/projects/screenreadersurvey9/#heading) was useful when when navigating a web page by headings. Failing to provide usable and well organized headings disables users from understanding your content and from being able to use assistive technologies to navigate within your page efficiently. While headings are useful for everyone, users of assistive technology are excluded more than others when headings are missing or improperly implemented.

## Checks

For each web page under test, perform the following checks. Record an answer, "yes" or "no" to each question. The passing result is indicated in uppercase. For each failing result, document it either as a defect or else provide an explanation of why it should be allowed as an exception or why it is not applicable.

### Checks which could be automated

- Do HTML heading tags like `<h1>` and `<h2>` appear in the HTML for the page? [Y/n]
- Are the headings in a hierarchical order without skipping heading levels (from `<h2>` to `<h4>` for example)? [Y/n]

### Checks which should be (at least partly) performed manually

- Are headings displayed in a consistent and recognizable style which makes them easy to identify versus other non-heading content? [Y/n]
- Does the text of each heading correctly and concisely describes the subject matter which it heads? [Y/n]
- Does the primary `<h1>` on the page correctly and concisely describe the subject matter of the total page? [Y/n]
- Are all examples of text that are visually styled to look like headings actually implemented with an HTML heading tag? [Y/n]

## Additional Testing Methods and Strategies

Use a tool like the Web Developer browser extension [see Tools] to reveal the heading structure of the page under test. Activate the option: Information → View Document Outline. Check if any part of the generated outline headings are flagged as "Missing heading". Scan from top to bottom and ensure that the outline correctly describes the topic and structure of the web page.

Screen reader devices rely on well ordered headings to describe and navigate web pages, however each requires slightly different steps to activate the "navigate by heading" feature. For example, on MacOS with the built-in [VoiceOver screen reader activated](https://dequeuniversity.com/tips/learn-voiceover), pressing the Control+Option+U then the Right Arrow key will pop-up and announce a menu of all the headings found in the HTML of the page. The [key combination for the NVDA screen reader](https://webaim.org/resources/shortcuts/nvda#links) is Insert+F7 to see a list of page links, headings, and landmarks. Navigate this list with the Up and Down Arrow keys to hear the screen reader speak the heading level and the text content of that heading. Activate a menu item by pressing the Enter Key to jump directly to that heading in the page.

## Examples and Additional Information

The `<h1>` heading should describe the subject of the web page as a whole and so would be similar or identical in content to the web page's `<title>` element.

```html
<h1>Recipe: Old Fashioned Butter Cookies</h1>
```

It is typical for the very first heading to be `<h1>` but it is not a defect for an `<h2>` to appear first, for example. The logical ranking and order of headings is more important than which is first, for example ensuring that no `<h2>` is followed next by an `<h4>` (skipping the expected `<h3>`).

To illustrate, if the navigation portion of the page appeared before the displayed title of the page, you might give the navigation heading a rank of `<h2>` but then further down use an `<h1>` heading to indicate the main subject matter of the entire page.

```html
<h2>Table of Contents</h2>
...
<h1>Favorite Recipes for the Holidays</h1>
...
```

Obviously screen reader devices are not able to recognize elements that are only _visually styled_ as headings as being true headings. Use a tool that can check the HTML source to find the actual headings (which is exactly how screen reader devices do it).

```html
<!-- DO NOT attempt to use visual style alone to indicate a heading level -->
<div class="looks-like-h1-heading">Making Mock Turtle Soup</div>
```

## Further Advice

When evaluating the usability of headings, prefer headings that are efficient and meaningful when presented as a list by a screen reader device. For example, having several headings with the same text content is not helpful when a user is deciding which to navigate to, and it is a strong indication that you've not organized the outline of the page ideally.

Additionally, by wording the headings so that the key descriptive terms are first, you allow a screen reader user to scan and find the section they are interested in more quickly, without having to listen to a large amount of filler words first.

```html
<!-- AVOID putting important text last in headings -->
<h2>Ingredients for Gardener's Pie: The Crust</h2>
<h2>Ingredients for Gardener's Pie: The Filling</h2>
<h2>Ingredients for Gardener's Pie: The Gravy</h2>
```

A better solution would be to add a level to the content by breaking the topic into further sub-topics, as shown below.

```html
<h2>Ingredients for Gardener's Pie</h2>
<h3>The Crust</h3>
<h3>The Filling</h3>
<h3>The Gravy</h3>
```

### Appearance of headings

A sighted user may want to visually quickly glance across a web page to find the heading that interests them. Headings should be visually identifiable, typically by being larger and more distinct than any surrounding text. Making headings more visually findable will be helpful for users with cognitive or visual disabilities.
