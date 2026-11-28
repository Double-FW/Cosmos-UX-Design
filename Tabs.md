---
title: "Tabs"
description: "Help people switch between related sections of content in one place."
---

# Tabs

Choose a related section and read its content. For a new set assume horizontal manual activation. Confirm orientation, activation policy, fragment and history policy, first meaningful panel content and any maintained legacy keyboard model. Use ordinary links to headed sections until enhancement succeeds. Four or five labels is a design heuristic.

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Build this pattern](#build-this-pattern)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

Use tabs when people need to choose one of a few related sections in the same view. Keep labels short and make each panel useful on its own. Use four or five tabs as a design heuristic. If labels need more space or people need to compare sections or complete one task across them, consider [Accordion](Accordion.md) or visible sections with in-page links. For guidance on hidden panel states, focus and accessibility-tree exposure, see [Visually Hidden Content](Visually%20Hidden%20Content.md).

## Recommended markup

Use a named tablist, associated tabs and labelled panels after enhancement. Keep the baseline links and sections usable when scripts are blocked.

## Recommended layout

<span id="presentation"></span>

Give the tab set a name that explains what the tabs group, such as ‘Journey information’. Give each tab a visible text label that describes the content it reveals. Keep panel headings so people can find the same sections before and after enhancement.

Place the panel immediately after the tab list in source order. Show which tab is selected and make its relationship to the panel clear. Give keyboard focus its own visible indicator, including when it is on the selected tab. Keep the labels, selected state, focus and panel boundary distinguishable in [forced colours](Colour%2C%20Contrast%20and%20Meaning.md#focus-and-icons-in-forced-colours).

Keep the full labels readable at narrow widths and when people enlarge text. If the labels cannot fit in a horizontal row, consider vertical tabs or use an accordion or in-page navigation. If tabs become vertical, update their orientation and keyboard commands together. Avoid layouts that look like a series of disclosures but still behave like horizontal tabs.

## Recommended behaviour

<span id="behaviour"></span>

### Standard tabs

Use the [ARIA Authoring Practices Guide tabs pattern](https://www.w3.org/WAI/ARIA/apg/patterns/tabs/) for new tab sets. The baseline example uses manual activation so people can explore tab labels before opening a panel.

- Tab enters the tab list at the selected tab. Left and Right Arrow move between horizontal tabs, wrapping at the ends. For vertical tabs, set `aria-orientation="vertical"` on the tab list and use Up and Down Arrow. If implemented, Home and End move to the first and last tab.
- In manual activation, arrow keys move focus and leave the panel unchanged. Enter and Space select the focused tab. Click or tap also selects a tab. On selection, keep focus on the tab, expose its panel and hide the others. Change the selected styling and `aria-selected` together.
- Tab leaves the tab list for the active panel. If the panel starts with non-interactive content, make the panel a tab stop with `tabindex="0"` so people can reach and read its beginning. If its first meaningful content is already focusable, let Tab reach that content directly. Shift+Tab returns from the start of the panel to the tab list.
- Automatic activation is an option when the panel appears without noticeable delay and frequent changes will not interrupt use. In that variant, moving to a tab with an arrow key also selects it.

### Existing variant that moves focus into the panel

Some existing interfaces keep each tab in the Tab sequence and move focus into the chosen panel on activation. Small usability studies reported that some participants missed the arrow-key interaction or found it conflicted with screen reader browsing. Treat this as a reason to test discoverability with the people using the interface. The focus-moving variant uses a different keyboard model from the standard pattern.

If you maintain this variant, document it explicitly. Let people reach each tab with Tab or Shift+Tab and activate it with Enter, Space, click or tap. Reveal the panel, update selection, then move focus to the named panel using `tabindex="-1"`. The next Tab reaches the first interactive element inside or after the panel. Verify that Shift+Tab takes people back to the tab list. Keep the heading or tab label visible when moving focus, and preserve the screen reader’s own reading commands.

### Direct links and browser history

Start with in-page links to visible sections. Keep the same fragment IDs when enhancing those sections into panels. On first load, select the panel containing the fragment target, including a target inside a panel. Use the first panel when the URL has no matching target. Leave normal browser navigation intact for a fragment elsewhere on the page.

If selection updates the URL, record a history entry only when the selected destination changes. Restore the appropriate panel on Back and Forward, including the default panel when returning to the initial URL without a fragment. Reveal a hidden panel before moving to a fragment inside it. Keep focus where it is if it remains usable. When a history change would hide the focused element, move focus to a sensible visible target, such as the newly selected tab. See [Client-side Routing](Client-side%20Routing.md#back-and-forward) for history across views.

<span id="reference-implementation"></span>
<span id="implementation"></span>
<span id="markup-example"></span>
<span id="enhanced-markup"></span>

## Build this pattern

[Build Tabs](build-patterns/Tabs.md) using the prompt, retrieval pack and verification plan. [Original examples at the pilot baseline](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Tabs.md#reference-implementation).

## Testing Approach and ACT Scripts

The ACT-style checks in this section are pattern-specific checks maintained in this guide. The proposed W3C rules are listed separately.

<span id="checkpoints-for-the-tester"></span>

### ACT-style checks

Apply these checks to each enhanced tab set with two or more panels. Run the focus check for the implemented variant. Record pass, fail, not applicable or not run for each check.

| Method and action | Expected result |
| --- | --- |
| Inspect the DOM and accessibility tree. Compare the tab list, each tab and its panel. | There is one named `tablist` containing named `tab` controls. Each tab’s `aria-controls` resolves to one distinct `tabpanel`. Each panel’s `aria-labelledby` resolves to its own tab. The active panel follows the tab list in source order. |
| Activate each tab with the supported keyboard commands, touch and pointer in separate runs. | All controls are operable. Exactly one tab has `aria-selected="true"`. Every other tab has `aria-selected="false"`. The matching panel alone is visible and available to assistive technology. |
| Put a link or other control in a panel. Select another tab, then Tab through the interface and browse with a screen reader. | The selected panel’s content is readable and its controls are reachable. The hidden panel and its controls cannot be reached visually, by keyboard or through screen reader browsing. |
| Inspect the tabs at rest and with keyboard focus on an unselected tab where the variant allows it. Repeat in forced colours. | Each label identifies its panel. Tabs look operable and related to the visible panel. Selection and keyboard focus have distinct visible indicators. |
| For standard tabs, enter the tab list from outside, use the orientation’s arrow keys and activate a tab. Then Tab into the panel and Shift+Tab back from its start. | Entry is on the selected tab. Arrow keys move focus and wrap. Manual activation retains selection until Enter or Space. Automatic activation selects as focus moves. Activation by keyboard keeps focus on the tab. Tab reaches the panel or its first meaningful control. |
| For the existing focus-moving variant, Tab through the tabs and activate one. Inspect focus and press Tab, then repeat the activation and press Shift+Tab from the panel. | Every tab is reachable. Activation focuses the visible, named panel programmatically. The panel itself is outside the Tab sequence. The next Tab moves into or past its content and Shift+Tab from the panel returns to the tab list. |

### Fallback, layout and history checks

| Method and action | Expected result |
| --- | --- |
| Block scripts and reload. Activate each in-page link with the keyboard. | All sections are visible and have headings. Each link reaches its section and the user can continue from there. Tab roles and states are absent. |
| Load a direct link to a panel, then load a link to an element inside an initially hidden panel. Repeat with an unrelated fragment. | The matching panel is visible before its target is reached. A fragment outside the tabs keeps its normal destination. |
| If selection updates the URL, select two different tabs from the initial URL, then use Back and Forward. | The visible panel and selected tab follow the URL, including when returning to the initial URL. Focus remains visible and usable. |
| Enlarge text and then separately narrow the viewport. Use the controls again, including any vertical layout. | Labels stay readable, selection and focus stay clear, and the visible orientation matches the keyboard commands. |

### Supplied QA scripts

- [Tabs](qa/supplied/tabs.md) — Apply panel-focus and individual Tab-stop expectations only to the documented panel-focus variant. Use the standard tabs checks for roving focus and arrow keys.

Read the [applicability and review notes](qa/Review.md) before using these scripts for release decisions.

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [ARIA required owned elements (bc4a75)](qa/w3c-act/bc4a75.md)
- [Element with role attribute has required states and properties (4e8ab6)](qa/w3c-act/4e8ab6.md)
- [Element with aria-hidden has no content in sequential focus navigation (6cfa84)](qa/w3c-act/6cfa84.md)

<span id="related-references"></span>

## Sources

- [Tabs: ARIA Authoring Practices Guide](https://www.w3.org/WAI/ARIA/apg/patterns/tabs/)
- [Tabs, Used Right](https://www.nngroup.com/articles/tabs-used-right/)
- [Danger! ARIA Tabs](https://simplyaccessible.com/article/danger-aria-tabs/)
- [In-Page Links and Input Focus](http://accessibleculture.org/articles/2010/05/in-page-links/)
- [Removing Semantics Using The Presentation Role](https://www.accessibility-developer-guide.com/examples/sensible-aria-usage/presentation/)
- [Reading Commands and Cursors](https://doccenter.freedomscientific.com/doccenter/doccenter/rs11f929e9c511/2012-04-24_teachersandtrainers-l7/02_jawsandmagicreadingcommandsandcursors.htm)
- [Historical reference for the `-ms-high-contrast` media query](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/-ms-high-contrast)
- [WCAG 2.1 Focus Order](https://www.w3.org/TR/WCAG21/#focus-order)
- [ARIA required owned elements (bc4a75)](https://www.w3.org/WAI/standards-guidelines/act/rules/bc4a75/proposed/)
- [Element with role attribute has required states and properties (4e8ab6)](https://www.w3.org/WAI/standards-guidelines/act/rules/4e8ab6/proposed/)
- [Element with aria-hidden has no content in sequential focus navigation (6cfa84)](https://www.w3.org/WAI/standards-guidelines/act/rules/6cfa84/proposed/)
