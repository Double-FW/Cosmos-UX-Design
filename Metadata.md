---
title: Metadata
description: "Use correct metadata to enable web clients to work accessibly."
---

## Summary

Metadata, literally "data about data", is vital for an accessible web page. The HTML standard defines multiple ways for web page authors to provide this metadata, including a series of aptly named "meta tags", and various other tags and attributes that declare things like the language, character set, title, and preferred viewport scale, all of which relate to the web page document. While web page metadata can be provided using a variety of tags and attributes, what they all have in common is that they follow well-established industry standards and so are machine-readable.

## Purpose

The benefits of accurate and well-written metadata are known to apply to two types of machine-readability: search engine spiders and assistive technologies. For example, having a meaningful and well-written `<title>` for a web page will allow Google to display that title as the clickable headline on its search results page, for the title to be included when sharing the web page on social media, and for the title to be used as the name of the bookmark in a user's web browser. In addition that same title will be used by screen reader devices to announce the name of the web page when it loads, so that users who may not be able to see the screen can hear an accurate and concise description of the web page's subject matter. 

## Checks

For each web page under test, perform the following checks. Record an answer, "yes" or "no" to each question. The passing result is indicated in uppercase. For each failing result, document it either as a defect or else provide an explanation of why it should be allowed as an exception or why it is not applicable.

### Checks which could be automated

- Does the web page result in no HTML validation errors when checked by a W3C HTML validation tool? [Y/n]
- Does the web page have at least an overall language attribute set which follows the [standard laguage codes](https://www.w3schools.com/tags/ref_language_codes.asp) for HTML? [Y/n]

### Checks which should be (at least partly) performed manually

- Does the web page have a `<title>` that accurately and concisely describes the subject of the entire page? [Y/n]
- If the web page is viewed on a touch screen that supports pinch and spread to zoom, is it possible to use these gestures to zoom in or out of the page? [Y/n]

## Additional Testing Methods and Strategies

Validating the HTML is always a good start to checking the underlying metadata. The Web Developer Browser Extension [see Tools] makes this simple to do, use the menu item: Tools → Validate HTML. Evaluating the meta tags is also simplified by using this tool. Activate the menu option: Information → View Meta Tag Information for the web page under test.

Eventually, due to the nature of metadata being optimised for machines, you may need to use the `view source` option of your browser's developer tools to access the HTML for the web page under test directly. Look for any metadata set on the `<html>` tag or defined in the document's `<head>` section. 

## Examples and Additional Information

### Declaring a web page language

The `lang` attribute can be added to many HTML elements to indicate what language the text in that element is written in. At the very least, the outermost `<html>` tag must have a language declared. If particular parts of the page are written in a different language (for example where a foreign phrase is intentionally quoted) that element must have a correct `lang` attribute value depending on the language used.

If the language used does not follow the default left-to-right order for reading text, ensure that the `dir` attribute is also included to specify "rtl" for example, when citing a right-to-left display pattern.

```html
<html dir="rtl" lang="ar">
```

Failing to set the language will likely harm your web page search engine ranking, make it more difficult for tools to render automatic translations of your content, and prevent screen reader devices from accurately using the most appropriate pronunciation engine when reading out your text content.

### Make titles specific enough to be distinct

The title of a web page should be specific enough that it can be used to distinguish that particular web page from any other page across the entire website. By following this rule you should end up with each web page having a unique title; otherwise the implication is that the chosen title was either not specific enough in its description or else you have multiple web pages with the same content.

### Don't disable pinch and spread to zoom

Touchscreen devices often support gestures know as pinch and spread to allow the user to zoom in and out of specific visible areas of a web page. It is technically possible to define metadata that will disable this (by setting the `maximum-scale=1` attribute for example). If this is found to be the case it should be considered a defect. A correct meta tag which provides a more accessible viewport metadata value is shown below.

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0"> 
```
