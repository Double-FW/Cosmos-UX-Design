---
title: Page Metadata
description: Declare the page title, language, text direction, character encoding and viewport.
---

# Page Metadata

Give each web page a title that helps people recognise it when switching browser tabs or navigating with a screen reader. Declare the language so assistive technology can select suitable pronunciation. Set the viewport so people can enlarge the page when they need to.

This guide covers information declared on the HTML document. Use [Metadata Strips](Metadata%20Strips.md) for supporting details shown alongside an item, such as its duration or publication date.

## Guidance

### Declaring a web page language

Set `lang` on the `html` element to match the page’s default language. Use a valid language tag, such as `en-GB` for British English. WCAG 2.1 [3.1.1 Language of Page](https://www.w3.org/TR/WCAG21/#language-of-page) (Level A) requires the default language to be programmatically determinable.

Mark passages in another language on the element that contains them. WCAG 2.1 [3.1.2 Language of Parts](https://www.w3.org/TR/WCAG21/#language-of-parts) (Level AA) includes exceptions for proper names, technical terms, words of indeterminate language and expressions that have become part of the surrounding language.

Declare direction separately. For a page whose content runs right to left, use markup such as `<html lang="ar" dir="rtl">`. Set direction on a passage when it needs a different base direction. See [Typography](Typography.md#language-direction).

### Make titles specific enough to be distinct

Write a document title that describes the page’s topic or purpose. Place the specific description first so it remains useful when browser tabs truncate the title. Use [Page Structure](Page_Structure.md#document-title) for writing titles and [Client-side Routing](Client-side%20Routing.md) for keeping the title in sync with a view that changes within the same document.

### Don't disable pinch and spread to zoom

Configure the viewport to fit the device and allow people to zoom. Avoid settings that suppress scaling, such as `user-scalable=no` or `maximum-scale=1`. Check the result on a supported touch device. For the layout checks after zooming, see [Typography](Typography.md#responsiveness).

## Implementation

### Markup example

This example declares British English and UTF-8. It includes a French quotation with its own language tag. Replace the title, language and content to match the page.

```html
<!doctype html>
<html lang="en-GB">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Collection options | Journey planner</title>
</head>
<body>
  <main>
    <h1>Collection options</h1>
    <p>Your confirmation may include the phrase
       <q lang="fr">sur rendez-vous</q>, which means by appointment.</p>
  </main>
</body>
</html>
```

## Checks

### Checks which could be automated

Use an HTML validator or a browser inspection tool to check that the title, language declaration and viewport are present where expected and that their syntax is valid. Investigate validation findings that affect this markup. Check the rendered document as well as the initial HTML when a script can change the title or language.

### Checks which should be (at least partly) performed manually

| Method and action | Expected result |
| --- | --- |
| Open two pages using the same template. For client-side navigation, switch views and use Back. Inspect the browser tab title at each step. | The title describes the displayed page or view and distinguishes it from other pages. |
| Compare the page’s default language and any marked passage with the language tags. Read the passage and the text around it with a supported screen reader. | The tags match the actual languages. Where matching voices and language switching are enabled, the passage is read in the appropriate language and the following text returns to the surrounding language. |
| Where the page uses right-to-left or mixed-direction text, inspect the rendering in context. | Text, punctuation and numbers are in a meaningful order for the passage. |
| Pinch to zoom in and out on a supported touchscreen. Read a passage and operate a control while zoomed. | The page allows zoom and the content and control remain usable. |

Record the page or route, browser, device, assistive technology and result.

## Sources

- [HTML language codes](https://www.w3schools.com/tags/ref_language_codes.asp)
