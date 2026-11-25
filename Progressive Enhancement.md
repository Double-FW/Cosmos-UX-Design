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
- At a viewport equivalent to 320 CSS pixels wide, can users read and operate vertically scrolling content without horizontal scrolling, apart from content that needs two dimensions? [Y/n]
- In DevTools, with the browser CPU and Network Throttle simulator set to their most degraded settings, is the core purpose of the page still delivered? [Y/n]
- In DevTools, when the browser has its Device Mode simulator set to the smallest available device screen, is the core purpose of the page still delivered? [Y/n]

## Additional Testing Methods and Strategies

Having an array of physical devices is ideal for testing, but you can also use tools like the Chrome Browser DevTools, and applying the [Device Mode](https://developer.chrome.com/docs/devtools/device-mode/) settings to approximate the web page under test on a range of devices: from mobiles, to tablets, and desktop. You can even simulate device inputs for touch, geolocation and device orientation. And in the Network tab you can simulate CPU and network throttling.

### Test text and layout when presentation changes

Use [Typography](Typography.md) for body-text size, line length and typeface guidance. Run its [resize and reflow checks](Typography.md#responsiveness) independently. Verify that the page keeps its core content and controls when enlarged and when the preferred font cannot load.

### Check touch targets on smaller screens

Use the design system’s documented target-size and spacing requirements. Increase targets where people need more room to operate adjacent controls. The [Rating Stars guide](Rating%20Stars.md#touch-target-size) uses a larger target for its closely spaced choices and identifies the source of that target.

### Don't rely on a `<noscript>` fallback

Some web pages use a `<noscript>` HTML tag to define alternative content for web browsers where JavaScript has been turned off. Research done by [Gov.UK](https://gds.blog.gov.uk/2013/10/21/how-many-people-are-missing-out-on-javascript-enhancement/) showed that a significant portion of users who could not run JavaScript did not have JavaScript turned off. In other words, JavaScript was turned on but didn't run for some _other reason_. This matters because a web browser will only show `<noscript>` content in the specific case that JavaScript was actually turned off. When JavaScript doesn't run for another reason, the user will not see the content that requires JavaScript, nor will they see any `<noscript>` alternative content.

By using [an intercepting proxy browser extension](https://chrome.google.com/webstore/detail/requestly-modify-headers/mdnleldcmiljblolnjhpnblkcekpdkpa?hl=en) a tester can block `.js` requests without disabling JavaScript in the browser. This is often a more realistic way to testing how a web page will hold up without JavaScript.
