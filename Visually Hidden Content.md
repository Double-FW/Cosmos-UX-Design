---
title: "Visually Hidden Content"
description: "Choose how content is hidden, keep visual and accessibility states consistent, and prevent focus, speech input and search barriers."
document_id: "cosmos-visually-hidden-content"
document_type: "design_practice"
schema_version: "1.0"
version: "1.0"
status: "draft"
language: "en-GB"
repository: "Double-FW/Cosmos-UX-Design"
repository_url: "https://github.com/Double-FW/Cosmos-UX-Design"
scope: {"platform":"web","applies_to":["content hidden from everyone","content visually hidden but exposed to assistive technology","content visible but hidden from the accessibility tree"],"excludes":["component-specific disclosure, tab, dialog, popover and tooltip interaction contracts; use the owning pattern guide"]}
license: "MIT"
audiences: ["designers","developers","design system managers","QA engineers","AI agents"]
related_guidance: ["ARIA and Screen Reader Usability.md","Anchors and Links.md","Focus.md","Labels.md","Icons.md","Disclosure.md","Accordion.md","Page_Structure.md"]
tags: ["hidden content","visually hidden","accessibility tree","aria-hidden","focus","voice control","screen readers","progressive enhancement"]
---

# Visually Hidden Content

Use this guide when a design or implementation hides content visually, from assistive technology, or from both. Decide which people need the content and why before choosing a technique. Hiding changes what people can see, find, hear, navigate to and operate.

For ARIA decisions more broadly, see [ARIA and Screen Reader Usability](ARIA%20and%20Screen%20Reader%20Usability.md). For a component’s open and closed behaviour, use its owning guide, such as [Disclosure](Disclosure.md), [Accordion](Accordion.md), [Tabs](Tabs.md), [Modal Dialogs](Modal%20Dialogs.md) or [Information Panels](Information%20Panels.md).

## Contents

- [Choose the intended visibility](#choose-the-intended-visibility)
- [Design and handover](#design-and-handover)
- [Implementation guidance](#implementation-guidance)
- [Use visually hidden text carefully](#use-visually-hidden-text-carefully)
- [Focus, search and fragment navigation](#focus-search-and-fragment-navigation)
- [Native patterns for revealing content](#native-patterns-for-revealing-content)
- [Checks](#checks)
- [Sources](#sources)

## Choose the intended visibility

First identify whether the content should be available to everyone, only to people using assistive technology, or only while a particular state is active.

| Intended state | Visual presentation | Accessibility tree | Typical use |
| --- | --- | --- | --- |
| Hidden from everyone | Not rendered | Not exposed | Inactive content, such as a closed disclosure panel or an error message that does not currently apply |
| Visually hidden, available to assistive technology | Not visually presented, but remains in the document and accessibility tree | Exposed | A concise text equivalent or context that is useful when navigating non-visually and would otherwise be redundant |
| Visible, hidden from assistive technology | Rendered | Not exposed | A purely decorative icon or duplicate visual flourish whose meaning is already provided elsewhere |

Do not call content “inaccessible” when you mean only that it is visually hidden or absent from the accessibility tree. State which people, presentation modes and interaction states are affected.

The table describes intended outcomes, not interchangeable techniques. A CSS rule, the HTML <code>hidden</code> attribute, and <code>aria-hidden</code> have different effects. Choose the implementation that creates the required visual state, accessibility-tree state and focus behaviour together.

## Design and handover

Document the content and state, not just a CSS class or attribute. For each hidden item, specify:

- what content is hidden and the user need it serves;
- who should be able to perceive it: everyone, screen reader or other assistive technology users, or nobody in the current state;
- the state or action that reveals or hides it, and how a person can operate that change with supported input methods;
- whether it contains links, controls or other focusable elements;
- what happens to focus when the content opens, closes, or becomes unavailable;
- whether in-page search, a URL fragment, or a speech-input command should find the content;
- how the implementation behaves if JavaScript or CSS is unavailable, where relevant.

Keep the visual interface and programmatic presentation consistent. Content that is visible should normally be available to assistive technology. Hide visible content from the accessibility tree only when it is genuinely decorative or is a duplicate that would otherwise create confusing repetition. Do not hide meaningful text simply to shorten a screen reader’s output.

## Implementation guidance

### Hide content from everyone

For content that is not part of the current interface state, use a technique that removes it from visual presentation and the accessibility tree and prevents its descendants from receiving focus. The HTML <code>hidden</code> attribute or CSS <code>display: none</code> normally provides this outcome. Native patterns such as details and dialog manage their own states; use the relevant [component guide](README.md#design-patterns) for the full interaction contract.

When an interactive region closes, move focus before hiding it if focus is inside the region. Return focus to the control that opened it when that is appropriate to the pattern. Do not leave keyboard focus inside hidden content.

The HTML <code>hidden=&quot;until-found&quot;</code> state has different behaviour: browser find-in-page and fragment navigation can reveal its contents. Support and details vary, so test it in the product’s supported browsers. Do not add <code>aria-hidden=&quot;true&quot;</code> to such content: this can leave revealed content visually present but unavailable to assistive technology.

### Hide content visually but leave it in the accessibility tree

A visually hidden utility can keep concise text in the document and expose it to assistive technology without taking up visual layout space. It is project CSS, not an HTML feature or a standard class name. Use a tested, shared utility rather than inventing slightly different implementations in each component.

A common technique clips a small, absolutely positioned box and prevents wrapping. The following is an example only; check it against the project’s supported browsers, writing directions, zoom and text settings.

~~~css
.visually-hidden:not(:focus, :active, :focus-within) {
  position: absolute;
  inline-size: 1px;
  block-size: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip-path: inset(50%);
  white-space: nowrap;
  border: 0;
}
~~~

This variant becomes visible when it or a descendant receives focus. Use that behaviour for content such as a skip link that is deliberately hidden until keyboard focus. Do not apply it indiscriminately to every visually hidden text string; focus behaviour belongs to the component contract. See [Page Structure](Page_Structure.md#the-main-landmark) and [Focus](Focus.md#visible-when-focused).

Avoid positioning hidden content far outside the viewport. Large negative offsets can create horizontal scrolling, including in right-to-left layouts, and make focus behaviour harder to reason about.

### Hide content from assistive technology only

Use <code>aria-hidden=&quot;true&quot;</code> only when content is visible but should not be represented in the accessibility tree, typically because it is decorative or duplicates an equivalent name or description already available.

~~~html
<button type="button" aria-label="Close dialog">
  <span aria-hidden="true">×</span>
</button>
~~~

The button remains named “Close dialog”; the decorative symbol is omitted from its name. See [Icons](Icons.md) for decorative graphics and [Labels](Labels.md) for control names.

Do not put <code>aria-hidden=&quot;true&quot;</code> on a focusable element or on a container with visible focusable descendants. A keyboard user could reach something that assistive technology is told does not exist. <code>aria-hidden=&quot;false&quot;</code> on a child cannot undo <code>aria-hidden=&quot;true&quot;</code> on an ancestor. Remove or move the hidden ancestor, or change the ancestor’s state.

<code>aria-hidden</code> does not visually hide content, remove it from keyboard focus, close a component, or manage focus. Do not use it as a substitute for <code>hidden</code>, CSS visibility, or a component’s state mechanism.

## Use visually hidden text carefully

Extra text can provide context when a link is encountered outside its surrounding paragraph or card, such as in a screen reader’s links list. However, hidden text also changes the accessible name. If the visible words are removed from the accessibility tree, speech-input users may be unable to activate the control by saying what they can see.

Prefer concise visible link text that makes the destination clear. See [Anchors and Links](Anchors%20and%20Links.md). If hidden text is needed to add context, keep the visible label included in the accessible name and preferably at its start. Confirm the computed name and test the visible command with speech input. See [Labels](Labels.md) and WCAG [Label in Name](https://www.w3.org/WAI/WCAG22/Understanding/label-in-name.html).

For example, this pattern keeps the visible words “Read more” in the accessible name while adding visually hidden context. It still leaves a generic visual label, so a clearer visible link is usually preferable.

~~~html
<a href="/research/pony-study">
  <span class="visually-hidden">Read more about the study of wild ponies: </span>
  <span aria-hidden="true">Read more</span>
</a>
~~~

A better option is often to make the visible link itself descriptive:

~~~html
<a href="/research/pony-study">Read the study of the social habits of wild ponies</a>
~~~

Do not rely on <code>aria-label</code> or hidden text to compensate for an avoidably vague visual label. The visible label should remain useful to sighted people, speech-input users and people who use magnification.

### Avoid font-size zero as a hiding technique

Do not use <code>font-size: 0</code>, zero line-height, transparent text or similar “flattened text” techniques to hide meaningful content. User text-size overrides, browser settings and forced-colour modes can expose or distort the text, and these techniques do not provide a dependable visually hidden utility. Use a tested visually hidden class when the content genuinely needs to remain in the accessibility tree. Test enlarged text and user styles.

## Focus, search and fragment navigation

A visually hidden element can still be focusable. If a person tabs to an element that remains visually hidden, its focus indicator and location may be impossible to see. If an element or a parent is hidden with <code>aria-hidden=&quot;true&quot;</code>, it may still receive focus in some implementations while being absent from the accessibility tree.

When content is not available in the current state:

- remove it from the tab order and accessibility tree using the owning component’s state mechanism;
- before hiding a region that contains focus, move focus to a logical visible control or return it to the invoker according to the pattern;
- when content is hidden until focus, reveal the focused element and its focus indicator;
- do not add <code>tabindex=&quot;0&quot;</code> to ordinary text just to make a screen reader encounter it.

Visually hidden text remains in the document. Browser find-in-page may report a match that is not visually apparent, and a fragment link may target content that is not currently visible. Consider whether the hidden text is suitable for search and ensure in-page navigation does not strand people inside a closed panel. For expandable panels and fragment targets, follow the [Accordion](Accordion.md#links-to-a-section) or owning pattern guidance.

## Native patterns for revealing content

Use a native pattern when it matches the task:

- Use <code>details</code> and its first-child <code>summary</code> for a simple disclosure. The browser supplies the open/closed state and keyboard operation. See [Disclosure](Disclosure.md#native-html).
- Use <code>dialog</code> for a dialog that needs dialog semantics and modal or non-modal behaviour. See [Modal Dialogs](Modal%20Dialogs.md).
- The HTML Popover API can show and hide non-modal popover content. It is not a general replacement for a disclosure, tooltip or dialog, and it does not determine whether the content should receive focus. Select the pattern based on the content and task; see [Information Panels](Information%20Panels.md).

These patterns manage particular interaction states. They do not make an unsuitable content choice accessible, and they still need design and implementation checks in the supported browser and assistive technology combinations.

## Checks

Record the element or component, intended visibility state, browser, input method and assistive technology where relevant. Check each state, not only the initial page.

| Method and action | Expected result |
| --- | --- |
| Inspect each hidden item and compare its intended visual and accessibility-tree states. | The chosen technique matches the documented audience and state. Meaningful visible content is not omitted from assistive technology without a clear reason. |
| With a screen reader, navigate the page before and after the relevant action. | Content hidden from everyone is not announced while hidden. Visually hidden content intended for assistive technology is available in the expected reading or control context. Decorative content marked <code>aria-hidden</code> is not announced, while its meaningful equivalent remains available. |
| Use Tab and Shift+Tab around the hidden content, including controls inside it. | Hidden controls are not reachable. A focusable item that is visually hidden until focus becomes visible with its focus indicator. Focus does not remain inside content after that content closes. |
| For expandable content, operate the trigger by keyboard, pointer and touch where supported; open and close it. | The trigger’s visible and programmatic state agrees with the content’s actual visibility. The content is available when open and unavailable when closed. Focus follows the owning pattern. |
| Use speech input on controls whose accessible name includes hidden text. | The visible label can be used as a command and matches part of the accessible name. |
| Search for a phrase that exists only in visually hidden content and follow relevant fragment links. | A match or navigation target does not leave the person unable to locate or understand the revealed content. Hidden panels are revealed as required by the owning pattern. |
| Increase text size, zoom, change text settings and enable forced colours. | Content and focus indicators remain usable. No hidden text unexpectedly overlaps or clips the interface. |
| Where a no-script or no-style fallback is required, disable JavaScript and CSS in separate checks. | Essential information and actions remain available. The fallback does not expose controls that claim to hide content but cannot change its state. |

These checks are a manual design and implementation review, not a standalone conformance result. Apply the relevant WCAG success criteria and project test plan to the complete journey. Use the owning pattern’s test cases and the repository’s [QA guidance](README.md#shared-implementation-guidance) where applicable.

## Local W3C ACT rule text

- [Element with aria-hidden has no content in sequential focus navigation (6cfa84)](qa/w3c-act/6cfa84.md)

## Sources

- [HTML Standard: The hidden attribute](https://html.spec.whatwg.org/multipage/interaction.html#the-hidden-attribute)
- [ARIA in HTML: Use of aria-hidden](https://www.w3.org/TR/html-aria/#el-aria-hidden)
- [W3C: Using ARIA](https://www.w3.org/TR/using-aria/#fourth)
- [W3C ACT Rule: Element with <code>aria-hidden</code> has no content in sequential focus navigation](https://www.w3.org/WAI/standards-guidelines/act/rules/6cfa84/)
- [WCAG 2.2 Understanding: Label in Name](https://www.w3.org/WAI/WCAG22/Understanding/label-in-name.html)
- [HTML Standard: Popover API](https://html.spec.whatwg.org/multipage/popover.html)
- [Cosmos: Disclosure](Disclosure.md)
- [Cosmos: Information Panels](Information%20Panels.md)
