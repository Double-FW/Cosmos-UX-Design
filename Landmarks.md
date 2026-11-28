---
title: Landmarks
description: Help people find and move between the main areas of a web page.
---

# Landmarks

Landmarks let people using assistive technology find the main content, navigation and other important areas without reading the page from the start. Use a small set of meaningful landmarks so the list gives people a useful overview. A [survey of screen reader users](https://webaim.org/projects/screenreadersurvey9/#landmarks) describes how people use landmark navigation.

## Guidance

Use native HTML elements for the page’s main areas. Check the role the browser exposes, because some roles depend on where the element is used.

| Page area | Markup and exposed role |
| --- | --- |
| Site header | A page-level `header` exposes `banner`. A header within an article or section introduces that content. |
| Main content | Use one exposed `main` landmark for the document’s active view. |
| Major navigation | Use `nav` to expose a `navigation` landmark. When several navigation areas have different purposes, give them distinct names, such as ‘Main’ and ‘Footer’. |
| Supporting content | Use `aside` where the content supports the page and makes sense on its own. Check for the `complementary` role when it serves as a landmark. |
| Additional region | Use a named `section` when the content needs a landmark and a more specific role does not fit. Prefer `aria-labelledby` pointing to its visible heading. An `article` has article semantics of its own. |
| Site footer | A page-level `footer` exposes `contentinfo`. A footer within an article or section supplies information about that content. |

Keep significant page content easy to find from the landmark list. A finding that content sits outside a landmark is a prompt to check whether people can find it. For example, an early skip link can sit before the header.

## Implementation

Use the [Page Structure example](Page_Structure.md#applying-regions) to check how regions fit together and the [skip-link example](Page_Structure.md#the-main-landmark) to help keyboard users bypass repeated navigation.

## Checks

Inspect the browser’s accessibility tree or use a [landmark navigation extension](https://chrome.google.com/webstore/detail/landmark-navigation-via-k/ddpokpbjopmeeiiolheejjpkonlkklgp?hl=en-GB) to identify roles and names. Verify the result with a supported screen reader.

| Method and action | Expected result |
| --- | --- |
| Inspect the accessibility tree of the active page. If the application switches views, repeat after navigation. | The document exposes one main landmark and it contains the current view’s central content. |
| Open the screen reader’s landmark list and move to the header, main navigation and footer where present. | Page-level roles match their purpose. Local article headers and footers do not appear as page banners or content information landmarks. Landmarks with different purposes can be told apart by name. |
| Navigate to named sections and supporting content. Review any significant content flagged outside landmarks. | Each landmark is useful and clearly identified. The page’s main areas can be found through landmarks or the surrounding page structure. |
| From the top of the page, Tab to the skip link and activate it. Press Tab again. | The link is easy to reach and visible on focus. It moves focus to main content and the next Tab continues from there. |

### Supplied QA scripts

- [Skip Link Test Criteria](qa/supplied/skip-link.md) — Apply to a skip link where supplied. Keep landmark navigation checks separate.

Read the [applicability and review notes](qa/Review.md) before using these scripts for release decisions.

## Sources

- [survey of screen reader users](https://webaim.org/projects/screenreadersurvey9/#landmarks)
- [landmark navigation extension](https://chrome.google.com/webstore/detail/landmark-navigation-via-k/ddpokpbjopmeeiiolheejjpkonlkklgp?hl=en-GB)
