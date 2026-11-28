---
title: "Disclosure"
description: "Show or hide supplementary content in place."
---

# Disclosure

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Build this pattern](#build-this-pattern)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

<span id="disclosure-show-and-hide-content"></span>

Use a disclosure when people can choose whether to read supplementary information in place. Give the control a label that identifies the content it reveals. Keep focus on the control when it opens or closes the panel. For grouped expandable sections, see [Accordion](Accordion.md); for switching among related panels, compare [Tabs](Tabs.md). For focus and announcements when content changes, see [Dynamic Content](Dynamic%20Content.md) and [Keyboard Navigation](Keyboard%20Navigation.md). For cross-cutting guidance on hidden states, focus and accessibility-tree exposure, see [Visually Hidden Content](Visually%20Hidden%20Content.md) and [ARIA and Screen Reader Usability](ARIA%20and%20Screen%20Reader%20Usability.md).

## Recommended markup

Use native `details` and `summary` where they fit. For a custom disclosure, use a button and keep its expanded state in step with the panel.

## Recommended layout

<span id="presentation"></span>

Keep the control visible in both states. Provide a visible expanded or collapsed indicator, such as the native marker or an icon whose shape changes with the state. Keep the panel next in source order so users reach the revealed content predictably.

## Recommended behaviour

Keep focus on the disclosure control when it opens or closes. After opening, let the next Tab reach the first interactive item in the panel if there is one.

## Build this pattern

<span id="reference-implementation"></span>
<span id="implementation"></span>

Use the [standalone disclosure build brief and Gherkin outcomes](build-patterns/Disclosure.md) to choose a variant, write a component semantics contract and verify the implementation. The companion includes a retrieval pack, source-check mapping and manual test links.

The [original reference implementation](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Disclosure.md#reference-implementation) remains available at its source revision.

<span id="native-html"></span>
- [Original native html example](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Disclosure.md#native-html)
<span id="custom-html"></span>
- [Original custom html example](https://github.com/Double-FW/Cosmos-UX-Design/blob/4925821776bbea3a5d837fac212581442a8c99e6/Disclosure.md#custom-html)

## Testing Approach and ACT Scripts

Test the supported variants. Record the method, browser and assistive technology where relevant, and compare the observed result with the expected behaviour.

<span id="checkpoints-for-the-tester"></span>

1. Tab to the control and use Enter and Space to open and close it.
2. Check that the control has a clear name and that its expanded or collapsed state is visible and announced by the screen reader.
3. Confirm the closed content is hidden visually and from assistive technology. After opening, check that any links or controls in the panel occur next in the keyboard sequence.
4. Block JavaScript and confirm the native control still works or the custom panel content is available directly. In a separate test, disable CSS and confirm that the control and content remain usable.

### Supplied QA scripts

- [Disclosure Test Criteria](qa/supplied/disclosure.md).

Read the [applicability and review notes](qa/Review.md) before using these scripts for release decisions.

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Summary element has non-empty accessible name (2t702h)](qa/w3c-act/2t702h.md)
- [Button has non-empty accessible name (97a4e1)](qa/w3c-act/97a4e1.md)
- [Element with aria-hidden has no content in sequential focus navigation (6cfa84)](qa/w3c-act/6cfa84.md)

## Sources

- [ARIA Authoring Practices Guide: Disclosure pattern](https://www.w3.org/WAI/ARIA/apg/patterns/disclosure/)
- [Summary element has non-empty accessible name (2t702h)](https://www.w3.org/WAI/standards-guidelines/act/rules/2t702h/proposed/)
- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
- [Element with aria-hidden has no content in sequential focus navigation (6cfa84)](https://www.w3.org/WAI/standards-guidelines/act/rules/6cfa84/proposed/)
