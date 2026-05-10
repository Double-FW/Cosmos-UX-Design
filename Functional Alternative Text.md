---
title: Functional Alternative Text
description: "Provide appropriate text alternatives for images."
---

## Summary

Where the user does not have images available in their web browser or where the user cannot see images due to an impairment, text alternatives MUST be provided that act as a comparable source of information and utility so that the user can still access, understand and use the web page for its core intended purpose.

## Purpose

The Americans with Disabilities Act states that content must be available and usable by blind users and others who use assistive technologies to read and operate your web pages. It is grounds for an accessibility lawsuit when users are excluded from accessing a website or app due to a failure to provide appropriate alternative text-based content for those who cannot access image-based content.

## Checks

For each instance of an image on the web page, perform the following checks. Record an answer, "yes" or "no" to each question. The passing result is indicated in uppercase. For each failing result, document it either as a defect or else provide an explanation of why it should be allowed as an exception or why it is not applicable.

### Checks which could be automated

- Does each image have alternative text associated with it (even if that text is an empty value)? [Y/n]

### Checks which should be (at least partly) performed manually

- Is the alternative text concise, grammatically correct, and free of spelling errors? [Y/n]
- If the image purpose is purely decorative, is the alternative text an empty value, like `alt=""`? [Y/n]
- Does the alternative text describe the image in a way that communicates a comparable meaning to users who cannot see the image? [Y/n]
- If the image appears as the only label for a control, a link or button for example, is it possible to understand what the control does solely from the alternative text? [Y/n]
- Is the alternative text original / useful and not just repeating adjacent body text? [Y/n]
- Does the language used for the alternative text match the language declared for the rest of the document? [Y/n]

## Additional Testing Methods and Strategies

The primary test scenario represents a user who may or may not see the screen but who uses a screen reader device to listen to and interact with the web page content. Use a screen reader device to exercise each of the web page features under test. Check that it is possible to understand and interact with all features in a way that is comparable to that of sighted user who is not operating a screen reader.

A less common test scenario represents a user who can see the screen, but for whom the images did not load either because the user chose to disable images or the connectivity / bandwidth available made image loading slow or impossible. Use the Web Developer Extension to enable the  "Make Images Invisible", including "Make Background Images Invisible", and enable the "Display Alt Attributes" option, then exercise each of the web page features under test. Check that it is possible to understand and interact with all features in a way that is comparable to that of a user with images loaded.

## Examples and Additional Information

### Purely Decorative Images

These images add no meaning to the content of the web page and so should be silently ignored by screen reader devices. By definition, decorative images could be deleted without affecting the understandability and usability of the page.

```html
<img src="decor.jpg" alt="" />
```

Similarly, background images (defined in CSS) are automatically ignored by screen reader devices so must only be used for purely decorative purposes.

```html
<span style="background-image:url(decor.jpg); height:32px; width:32px"></span>
```

There is a frequently abused edge-case where the element displaying the background image is authored to contain text meant to be an alternative to the image, rather than defined in an HTML `alt` attribute.

```html
<!-- PREFER an img element with alt text over body text hidden with css-->
<a href="/blog" style="background-image:url(blog.jpg); height:32px; width:32px">
  <span class="visually-hidden">Our Blog</span>
</a>
```

Notice that the alternative text is in the body of the web page but is styled (via a developer-defined [`visually-hidden` class](https://webaim.org/techniques/css/invisiblecontent/) so as to only be accessible to users of a screen reader device. While this is _technically_ accessible to screen reader users, it is a worse approach than using an `img` element with an `alt` attribute because the visually-hidden text will never be revealed when images are disabled or for any reason fail to load, whereas the `alt` attribute text will.

### Content Images

These images are intrinsic to the content or utility of the page. Use the HTML `alt` attribute to provide a textual alternative for the image.

```html
<img src="content.jpg" alt="Text content conveying meaning goes here." />
```

Be careful not to provide _redundant_ content however. If the text used as an alternative for the image is already nearby in the regular body text, then the alternative text should be considered redundant and the image should therefore be treated as purely decorative. Otherwise the screen reader experience will be to hear the same text read aloud twice.

```html
<!-- DO NOT repeat adjacent body text in the alternative text, as shown here -->
<img src="photo.jpg" alt="Our new classroom is now open!">
<h3>Our new classroom is now open!</h3>
```

When evaluating the quality of alternative text consider the editorial meaning and purpose of the image in the context of the surrounding page. The alternative text should concisely impart what the image _means_, not just what it shows.

Don't assume that [other HTML features intended to provide alternative text](https://developer.mozilla.org/en-US/docs/Learn/Accessibility/HTML#other_text_alternative_mechanisms), such as the `longdesc`, `title` or `figcaption` attributes, will work for screen readers. It is not predictable whether any particular screen reader device (depending on how it is configured) will either ignore those attributes or if it will read any or even all of them.

If the amount of text information required is very large or complex, it is acceptable to use an ARIA attribute to mark another adjacent HTML element as the alternative text.

```html
<img src="chart.jpg" alt="Chart showing our results." aria-describedby="results" />

<div id="results">
    <h2>It's Getting Worse in the North</h2>
    <p>The latest data show an increasing disparity between the rates of...</p>
</div>
```

### Images in Controls

When images act as the label of a control – think of an image of a social media logo that is the only content in a link to the company social media feed – then the control must be accessible for users who cannot see the image. Use alternative text that explains the action of the control. A link should describe the destination, for example "Home Page", "About us", etc.  An image in a button should use alternative text that describes the purpose of the button, such as "Search", "Log In", or "Submit" for example. 

```html
<a href="/">
    <img src="company-logo.jpg" alt="Home" />
</a>
```

```html
<input type="image" src="search-icon.jpg" alt="Search" />
```

### Graphics is SVG Format

In some cases the image displayed may not be defined using a traditional `<img>` tag, but it must still be possible for screen reader devices to access some textual alternative. The example below shows a [technique documented by Léonie Watson](https://www.sitepoint.com/tips-accessible-svg/) which uses a combination of `role`, and `aria-labelledby` attributes referring to a `<title>` and `<desc>` to help assistive technology users access text that describes the graphic.

```html
<svg height="100" width="100" aria-labelledby="title desc" role="img">
  <title id="title">Red circle</title>
  <desc id="desc">A red circle.</desc>
  <circle cx="50" cy="50" r="40" fill="red" />  
</svg>
```
