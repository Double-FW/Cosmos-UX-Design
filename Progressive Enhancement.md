---
title: "Progressive Enhancement"
description: "Use progressive enhancement and responsive design to support accessibility."
---

## Summary

Progressive enhancement ensures that users can use the widest possible array of devices and software, whilst the [core purpose](https://www.bbc.co.uk/accessibility/forproducts/guides/html/core-purpose/) of the web page is still usable and understandable. Additional nice-to-have features can then be added onto that core experience for the benefit of more capable devices and web browsers. 

## Purpose

Applying progressive enhancement practices will make your web page more resilient as well as inclusive; not every user will have the fastest connectivity, most powerful computer, or latest software installed. and sometimes a feature may be turned off by a user deliberately. You should assume they have good reason for this decision, but not every user has a choice. They may be using a computer that they don't maintain or control, at an office or library for example, and which blocks some types of content. Or they may be in an area or situation where they are forced to use very limited or degraded network access, like on a train, at an airport, or in a rural location.

Testing will require defining what the "core features" of the page under test are, and then testing those features on as wide a variety of devices and conditions as possible, including different assistive technologies. The purpose of building resilience into your web page is so that it will work in all the scenarios you _wouldn't normally expect_.

## Checks

For each web page under test, perform the following checks. Record an answer, "yes" or "no" to each question. The passing result is indicated in uppercase. For each failing result, document it either as a defect or else provide an explanation of why it should be allowed as an exception or why it is not applicable.

### Checks which could be automated

- Are all sizes specified in the design (CSS) of the webpage declared using relative units (`em`s or `rem`s) only, avoiding the use of _fixed_, pixel-count units like `px`? [Y/n]

### Checks which should be (at least partly) performed manually

- When the text size is modified to 200% of its default size in the browser settings, is all text on on the web page still visible and readable? [Y/n]
- When the page zoom is increased to 200% in the browser, does all text on on the web page increase proportionally? [Y/n]
- In DevTools, with the browser CPU and Network Throttle simulator set to their most degraded settings, is the core purpose of the page still delivered? [Y/n]
- In DevTools, when the browser has its Device Mode simulator set to the smallest available device screen, is the core purpose of the page still delivered? [Y/n]

## Additional Testing Methods and Strategies

Having an array of physical devices is ideal for testing, but you can also use tools like the Chrome Browser DevTools, and applying the [Device Mode](https://developer.chrome.com/docs/devtools/device-mode/) settings to approximate the web page under test on a range of devices: from mobiles, to tablets, and desktop. You can even simulate device inputs for touch, geolocation and device orientation. And in the Network tab you can simulate CPU and network throttling.

### Ensure text is resizable

No single text size is usable by everyone and it must be assumed that anyone may choose or need to resize the text of a web page to suit their own purposes. It is recommended that the default size for body text be the equivalent of at least 12px to 14px depending on how heavy the font is. Test that all web pages are usable and readable when the text resized to 200% this default size.

Text is considered usable and readable when it is not cropped and does not overlay other text or nearby elements. It should wrap as needed and maintain its typographical qualities, allowing sufficient line-height and letter and line spacing for example. Ensure that relative units (`em`s or `rem`s) are used to specify the sizes of web page elements like text and layout. This will enable the page to scale and adapt relative to the size of a device screen or its orientation (portrait or landscape).

### Prefer a line length of about 65 characters

Ideally lines of text should be between 50 to 70 characters (including spaces) long. Only if the layout does not permit, because of an overly narrow screen or large text, would it become necessary to use less characters per line.

### Ensure the web page is zoomable

Page zoom increases the apparent size of the entire page, while text resizing only increases the size of the text. Ensure all content is visible and usable when the web page is zoomed to 200% of its default level. This will likely result in the need for horizontal scrolling, which is acceptable in this circumstance.

### Use a readable typeface

Consider the color, size, and weight of text and how it may render on lower resolution screens, or when viewed with [color filters](https://support.microsoft.com/en-us/windows/use-color-filters-in-windows-43893e44-b8b3-2e27-1a29-b0c15ef0e5ce) applied. Light and ultra light typefaces are harder to read and will need to be displayed at larger sizes and higher contrast to remain readable to people with vision impairments. [Legibility research ](https://www.researchgate.net/publication/309593009_Legibility_of_Light_and_Ultra-Light_Fonts_Eyetracking_Study) shows that light and ultra light typefaces cause a higher cognitive load for all readers.

If the web page uses a specific typeface to maintain legibility, test the page with that typeface missing to check that it will work for users that can't load that specific typeface or who don't have it installed on their devices. 

### Adjust touch targets to be larger on smaller screens

On mobile devices with touchscreens the user must contend with both a smaller physical screen and the use of a finger to try to tap the targets (such as links and buttons) on that screen. Considering this, the minimum target size should be at least 7 x 7 mm, or larger if possible. This may require increasing the size of interactive elements on smaller screens.

### Don't rely on a `<noscript>` fallback

Some web pages use a `<noscript>` HTML tag to define alternative content for web browsers where JavaScript has been turned off. Research done by [Gov.UK](https://gds.blog.gov.uk/2013/10/21/how-many-people-are-missing-out-on-javascript-enhancement/) showed that a significant portion of users who could not run JavaScript did not have JavaScript turned off. In other words, JavaScript was turned on but didn't run for some _other reason_. This matters because a web browser will only show `<noscript>` content in the specific case that JavaScript was actually turned off. When JavaScript doesn't run for another reason, the user will not see the content that requires JavaScript, nor will they see any `<noscript>` alternative content.

By using [an intercepting proxy browser extension](https://chrome.google.com/webstore/detail/requestly-modify-headers/mdnleldcmiljblolnjhpnblkcekpdkpa?hl=en) a tester can block `.js` requests without disabling JavaScript in the browser. This is often a more realistic way to testing how a web page will hold up without JavaScript.
