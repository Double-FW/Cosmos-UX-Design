# Targeted accessibility QA

Use the owning Cosmos guide to identify the implemented variant and expected behaviour. Use the supplied scripts for matching scenarios, and W3C rules for their defined targets. Read the [coverage and review notes](Review.md) before making release decisions.

The [supplied suites](#supplied-suites) reproduce the wording, numbering and code in the supplied ACT Tests document. Links in the mapping identify where to use each suite. Notes about scope, conflicts and proposed fixes are kept in the review.

## Supplied suites

| Suite | Cosmos guides and applicability |
| --- | --- |
| [Accordions](supplied/accordion.md) | [Accordion](../Accordion.md). |
| [Cards](supplied/cards.md) | [Cards](../Cards.md). |
| [Data tables](supplied/data-tables.md) | [Data Tables](../Data%20Tables.md). |
| [Focus](supplied/focus.md) | [Focus](../Focus.md). |
| [Form fields and validation](supplied/form-validation.md) | [Forms](../Forms.md). [HTML Form Elements](../HTML%20Form%20Elements.md) — Apply the field naming, instructions and state checks to the controls in scope. |
| [Grids](supplied/grids.md) | [Layout](../Layout.md). |
| [Headings](supplied/headings.md) | [Headings](../Headings.md). |
| [Iconography](supplied/iconography.md) | [Icons](../Icons.md). |
| [Promos](supplied/promos.md) | [Promotional Cards](../Promotional%20Cards.md). |
| [Routing](supplied/routing.md) | [Client-side Routing](../Client-side%20Routing.md). |
| [Tabs](supplied/tabs.md) | [Tabs](../Tabs.md). |
| [Call to Action Test Criteria](supplied/call-to-action.md) | [Call to Action](../Call%20to%20Action.md). |
| [Carousel Test Criteria](supplied/carousel.md) | [Carousels and Sliders](../Carousels%20and%20Sliders.md). |
| [Checkbox and Radio Button Test Criteria](supplied/checkbox-radio.md) | [HTML Form Elements](../HTML%20Form%20Elements.md). |
| [Dark Mode Switcher Test Criteria](supplied/dark-mode-switcher.md) | [Colour, Contrast and Meaning](../Colour%2C%20Contrast%20and%20Meaning.md). |
| [Disclosure Test Criteria](supplied/disclosure.md) | [Disclosure](../Disclosure.md). [Expandable Content Previews](../Expandable%20Content%20Previews.md) — Apply shared disclosure checks. Use this guide’s continuation-focus procedure when the preview moves focus. |
| [Links and Anchors Test Criteria](supplied/links-and-anchors.md) | [Anchors and Links](../Anchors%20and%20Links.md). |
| [Modal Dialog Test Criteria](supplied/modal-dialog.md) | [Modal Dialogs](../Modal%20Dialogs.md). [Drawer](../Drawer.md) — Apply to the modal drawer variant only. [Information Panels](../Information%20Panels.md) — Apply only when the panel is implemented as a modal dialog. |
| [Navigation Menu Test Criteria](supplied/navigation-menu.md) | [Site Menus](../Site%20Menus.md) — Apply disclosure menu checks where the site menu actually collapses. [Mega Menu](../Mega%20Menu.md) — Use the guide’s Escape and focus contract for the expanded panel. [Global Navigation](../Global%20Navigation.md) — Apply only to expandable navigation, with the guide’s screen reader and keyboard expectations. |
| [Paged Navigation Test Criteria](supplied/paged-navigation.md) | [Search](../Search.md) — Apply only if results use numbered pagination. [Filter and Sort](../Filter%20and%20Sort.md) — Apply only if results use numbered pagination. [Load More](../Load%20More.md) — Apply only to a separate pagination fallback, where provided. |
| [Progress Indicator Test Criteria](supplied/progress-indicator.md) | [Notifications and Status Messages](../Notifications%20and%20Status%20Messages.md) — Apply only when there is a progress indicator. Use the guide’s live feedback checks for messages. [Dynamic Content](../Dynamic%20Content.md) — Apply only when updates include a progress indicator. [Load More](../Load%20More.md) — Apply only if the loading UI exposes a progress indicator. |
| [Skip Link Test Criteria](supplied/skip-link.md) | [Page Structure](../Page_Structure.md). [Keyboard Navigation](../Keyboard%20Navigation.md) — Apply to the skip-link route through repeated navigation. [Landmarks](../Landmarks.md) — Apply to a skip link where supplied. Keep landmark navigation checks separate. |
| [Toggle Button Test Criteria](supplied/toggle-button.md) | [Video Controls](../Video%20Controls.md) — Apply only to a stable-label toggle. Use the guide’s own checks for changing Play/Pause or Mute/Unmute action labels. [Colour, Contrast and Meaning](../Colour%2C%20Contrast%20and%20Meaning.md) — Apply only to a two-state switch or toggle. Use the theme-specific suite for a System, Light and Dark selector. |
| [Alternative Text Test Criteria](supplied/alternative-text.md) | [Functional Alternative Text](../Functional%20Alternative%20Text.md). [Infographics](../Infographics.md) — Apply to the image and its detailed alternative. Test access to the underlying information separately. [Charts, Graphs and Maps](../Charts%2C%20Graphs%20and%20Maps.md) — Apply to image alternatives where relevant. Test interactive exploration separately. |

## Tooltip checks supplied separately

Use the [Tooltips checklist and six ACT-style cases](../Tooltips.md#testing-approach-and-act-scripts) for hidden content, trigger descriptions, pointer and keyboard dismissal, competing triggers, touch and readable layout.

## W3C ACT rules

The files below reproduce core rule text and examples from the licensed upstream source. Each file links to the exact W3C rule. Consult that page for formal mappings, definitions, implementation reports and runnable fixtures. The W3C material carries its [own notice and licence](w3c-act/LICENSE.md).

- [Scrollable content can be reached with sequential focus navigation (0ssw9k)](w3c-act/0ssw9k.md)
- [Image has non-empty accessible name (23a2a8)](w3c-act/23a2a8.md)
- [Visible label is part of accessible name (2ee8b8)](w3c-act/2ee8b8.md)
- [Summary element has non-empty accessible name (2t702h)](w3c-act/2t702h.md)
- [Element with role attribute has required states and properties (4e8ab6)](w3c-act/4e8ab6.md)
- [Element with aria-hidden has no content in sequential focus navigation (6cfa84)](w3c-act/6cfa84.md)
- [Autocomplete attribute has valid value (73f2c2)](w3c-act/73f2c2.md)
- [Button has non-empty accessible name (97a4e1)](w3c-act/97a4e1.md)
- [ARIA required owned elements (bc4a75)](w3c-act/bc4a75.md)
- [Link has non-empty accessible name (c487ae)](w3c-act/c487ae.md)
- [Table header cell has assigned cells (d0f69e)](w3c-act/d0f69e.md)
- [Form field has non-empty accessible name (e086e5)](w3c-act/e086e5.md)
- [Video element auditory content has captions (f51b46)](w3c-act/f51b46.md)
