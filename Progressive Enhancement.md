---
title: Progressive Enhancement
description: Keep essential content and actions usable when an enhancement is unavailable or fails.
---

# Progressive Enhancement

Keep the main task usable when a script, stylesheet or other resource loads late or fails. For example, people should still be able to find the next page of results if the Load more button cannot initialise. Start by identifying what people need to read, do and recover from.

## Purpose

People may use an older browser, a restricted device or an unreliable connection. The baseline is the content and behaviour available before optional enhancements run. Define which tasks the baseline supports and how people can continue if an enhancement is interrupted.

## Implementation

Use HTML for the content, links and controls people need. Use working destinations and standard form submission where the service can support them. Add styling and scripted behaviour once the required resources and handlers are ready. If a feature genuinely depends on a capability, explain what is unavailable and provide a working alternative route where the service offers one.

Check for the specific capability the enhancement needs. Keep baseline links and content available until their replacements work. If initialisation fails, leave a usable route to the task. While a request is pending, keep existing content available where possible. If it fails, explain what happened, retain entered information and offer a retry or a way to continue. See [Dynamic Content](Dynamic%20Content.md#status-messages) for status messages.

| Pattern | Baseline and enhancement |
| --- | --- |
| [Disclosure](Disclosure.md#custom-html) | Make the supplementary content readable before activating the custom toggle. The example connects the handler before collapsing the content. |
| [Load More](Load%20More.md#pagination) | Provide working page links. Once the enhancement is ready, let people request another batch and retain access to pagination for recovery. |
| [Filter and Sort](Filter%20and%20Sort.md#enhancing-a-narrow-layout) | Keep every choice available. Add a disclosure for overflow only after its trigger and links work. |
| [Forms](Forms.md#validation) | Validate submitted values on the server. Use browser-side feedback to help people correct entries and preserve their values when submission fails. |

## Checks

Choose a representative task and record what people need to complete it. Test the normal journey first. Repeat with one failure or presentation change at a time, then combine conditions where the project needs to support them.

### Checks which could be automated

Check that essential routes and fallback controls exist. Where test tooling allows it, block an optional resource and check that the baseline still responds. Use automated findings about fixed sizes to guide inspection. The unit alone cannot establish whether content resizes and reflows.

### Checks which should be (at least partly) performed manually

| Method and action | Expected result |
| --- | --- |
| Block an enhancement script request while leaving JavaScript enabled. Reload and carry out the core task. Then disable JavaScript and repeat as a separate check. | The documented baseline remains available. Optional controls appear only when they work. |
| Interrupt or delay an enhancement request after the page has loaded, such as a request for another results batch. | Existing content and edits stay available. The interface communicates the wait or failure and offers the documented recovery route. |
| Disable CSS and repeat a representative task with a keyboard. Block the preferred font separately. | Content follows a meaningful order. Labels, instructions and essential actions remain available. Text stays legible in the fallback font. |
| Increase text size to 200%. Separately test vertically scrolling content at a width equivalent to 320 CSS pixels. | Text remains readable and controls work. Vertically scrolling content reflows without horizontal scrolling except where a two-dimensional layout is needed for meaning or use. See [WCAG 2.1 1.4.4 Resize Text](https://www.w3.org/TR/WCAG21/#resize-text) and [1.4.10 Reflow](https://www.w3.org/TR/WCAG21/#reflow), both Level AA. |
| Repeat the core task with throttled network and CPU settings. Use the supported keyboard, screen reader and touch inputs where relevant. | The baseline is usable when it becomes available. Delayed enhancements show understandable feedback where needed and leave focus usable. |

Record the tested task, browser, device, input method, settings, failed resource and observed result.

## Additional Testing Methods and Strategies

Use a supported device for representative checks. Use [device mode](https://developer.chrome.com/docs/devtools/device-mode/) and throttling to reproduce specific widths, input conditions and delays.

### Test text and layout when presentation changes

Use [Typography](Typography.md#responsiveness) for resize and reflow checks and [Layout](Layout.md) for reading and focus order across layouts. Confirm that people can use the core task after each change.

### Check touch targets on smaller screens

Use the design system’s target-size and spacing requirements. Increase targets where people need more room between adjacent controls. The [Rating Stars guide](Rating%20Stars.md#touch-target-size) gives a specific example.

### Don't rely on a `<noscript>` fallback

A browser with scripting enabled can still fail to download or execute a script. A `noscript` fallback does not cover that condition. Keep essential content and working routes available in the initial page. In addition to disabling scripts, block the relevant request with developer tools or an [extension that can block requests](https://chrome.google.com/webstore/detail/requestly-modify-headers/mdnleldcmiljblolnjhpnblkcekpdkpa?hl=en).

## Sources

- [Research on missing JavaScript enhancements](https://gds.blog.gov.uk/2013/10/21/how-many-people-are-missing-out-on-javascript-enhancement/)
- [WCAG 2.1 1.4.4 Resize Text](https://www.w3.org/TR/WCAG21/#resize-text)
- [1.4.10 Reflow](https://www.w3.org/TR/WCAG21/#reflow)
- [device mode](https://developer.chrome.com/docs/devtools/device-mode/)
- [extension that can block requests](https://chrome.google.com/webstore/detail/requestly-modify-headers/mdnleldcmiljblolnjhpnblkcekpdkpa?hl=en)
