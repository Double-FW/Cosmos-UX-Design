---
title: "ARIA and Screen Reader Usability"
description: "Design and review ARIA as part of a coherent screen reader experience, starting with native HTML semantics."
---

# ARIA and Screen Reader Usability

> “HTML is screen reader UX.”  
> — Ian Pouncey

The meaning and structure encoded in HTML shape what people can find, understand and do with a screen reader. Headings, lists, landmarks, labels, reading order, control behaviour and content relationships are part of the interface. ARIA contributes to this experience when native HTML cannot express the necessary semantics. It does not replace accessible structure or interaction.

## Contents

- [Scope](#scope)
- [Design the visual and screen reader experiences together](#design-the-visual-and-screen-reader-experiences-together)
- [What ARIA provides](#what-aria-provides)
- [Repair the cause before adding ARIA](#repair-the-cause-before-adding-aria)
- [Use ARIA with care](#use-aria-with-care)
- [Design and development handover](#design-and-development-handover)
- [Review the implemented experience](#review-the-implemented-experience)
- [Sources](#sources)

## Scope

Use this guide when planning a web interface, documenting a component, reviewing markup or deciding whether ARIA is needed. It covers cross-cutting design decisions. For decisions about content hidden visually, from assistive technology, or from both, see [Visually Hidden Content](Visually%20Hidden%20Content.md). Use the owning Cosmos pattern guide for the behaviour of a particular component, such as [Tabs](Tabs.md), [Disclosure](Disclosure.md), [Modal Dialogs](Modal%20Dialogs.md) or [Dynamic Content](Dynamic%20Content.md).

ARIA is a W3C specification for adding or refining semantics that browsers expose through accessibility APIs. It is commonly used by assistive technologies, including screen readers. A browser and assistive technology interpret and present those semantics together; ARIA does not dictate one exact spoken phrase. Support and output can vary by browser, assistive technology, platform, settings and interaction mode.

## Design the visual and screen reader experiences together

A screen reader experience is not a separate, hidden version of a visual interface. It is another way to perceive and navigate the same product. Design both experiences together:

- Use semantic structure to convey the hierarchy and relationships that the visual design shows. Headings, lists, tables, landmarks, form groups and source order help people understand and move through content.
- Keep visual and programmatic states aligned. If a control looks expanded, selected, checked, unavailable or invalid, its native state or accessibility state must reflect that condition.
- Give controls names that identify their visible purpose. People may navigate by control lists or use speech input, and many screen reader users can also see some of the interface. Do not replace visible wording with a different or conflicting accessible name.
- Keep task context available when a person reaches a control out of reading order, such as from a list of links, buttons or form fields. Follow [Labels](Labels.md) and [Designing the Screen Reader Journey](Designing%20the%20Screen%20Reader%20Journey.md).
- Keep announcements useful and concise. A screen reader user may be scanning headings, landmarks, links or controls, listening to content, using speech input, or reading braille. Avoid extra instructions, repeated labels, unnecessary landmarks and duplicate announcements that make the experience harder to navigate.

## What ARIA provides

ARIA roles identify the kind of interface object or region exposed to assistive technology. States and properties expose information such as whether a custom control is expanded, selected, required or invalid, or how content relates to another element. Accessible name and description attributes can supply text or reference existing text.

ARIA does not create visual styling, keyboard operation, focus management, event handling or content structure. A `role="button"` on a `div` does not make that element look, focus or behave like a native button. The implementation still needs the right interaction, focus behaviour, visible affordance and state changes.

Likewise, adding `aria-live` does not make every update helpful, and adding `aria-controls` does not show, hide or move focus to the referenced content. Use a relationship only where the selected pattern needs it, and implement the behaviour separately.

## Repair the cause before adding ARIA

Start with the element, content or interaction that caused the barrier. If the underlying code can be corrected, fix it rather than masking the problem with ARIA. ARIA may be a temporary mitigation when an immediate repair is not possible, but it must not be presented as the completed fix. Record the remaining barrier, owner, scope and condition for removing the workaround.

Use this decision order:

1. **Identify the user need and observed barrier.** State what the person needs to find, understand or do, and what currently prevents it.
2. **Check for a suitable native HTML element or attribute.** Use it when its built-in meaning and behaviour fit the task. A native link, button, label, heading, list, form control or `details` disclosure usually provides more than a role alone.
3. **Repair the structure or behaviour where it is wrong.** Correct an element used for the wrong purpose, a missing visible label, a broken reading order, an inaccessible focus path, or a state that does not match what is shown.
4. **Add only the ARIA needed to express a remaining semantic gap.** Check that the role and attributes are permitted for the element in [ARIA in HTML](https://www.w3.org/TR/html-aria/), and that the intended pattern is defined in the [ARIA Authoring Practices Guide](https://www.w3.org/WAI/ARIA/apg/).
5. **Implement and test the complete interaction.** A role is a promise about what the control is and how it can be used. Provide the expected keyboard interaction, focus handling, visible state, accessible state and recovery behaviour.
6. **Check the outcome with the supported browser and assistive technology combinations.** Do not infer success from the presence of ARIA attributes or from a validator alone.

| Barrier or tempting shortcut | Better starting point |
| --- | --- |
| A `span` or `div` styled as a link and activated with a click handler | Use an `a` element with a working `href` for navigation. |
| A generic element styled as a button | Use a native `button` for an action. If a custom widget is genuinely required, implement its complete keyboard and focus behaviour as well as its semantics. |
| A custom disclosure built from a button when native disclosure fits | Prefer `details` and `summary`. For a custom disclosure, keep the button’s `aria-expanded` value synchronised with the panel’s actual visibility. |
| A form control with no usable label | Add a visible, programmatically associated `label`. Use ARIA only where a native label cannot meet the real design requirement; do not use it to conceal a missing label. |
| A visible section that is hard to find in the code or reading order | Repair headings, source order or page structure. Do not add a landmark role merely to make the section appear in more navigation lists. |

## Use ARIA with care

### Preserve native semantics

Do not add a role that repeats an element’s native role. Do not change native semantics unless a documented interaction requires it and the complete replacement pattern is implemented. For example, changing a button’s role to `tab` changes what assistive technology understands about it; it does not create a tab interface. A tab pattern also needs the associated tab list and panels, selected state, focus model and expected keyboard operation. See [Tabs](Tabs.md).

Use the specific role and pattern that match the interaction. A site navigation list is not a menu widget just because it contains links. An alert is not a general way to make text noticeable. A screen reader announcement is not a substitute for leaving important content available to read again.

### Name and describe controls without contradiction

Prefer visible text and native naming mechanisms. For a form field, use an associated `label`; for a named region, refer to its visible heading where appropriate. Use `aria-label` only when a suitable visible name is not available and the element supports naming. An ARIA name can replace visible text in the accessibility tree, so check that it does not remove useful descendant content or conflict with the label a person sees.

Keep the visible label within the accessible name, with its wording first where additional context is needed. This supports speech input and helps avoid a mismatch between what someone sees and what they can say. Use `aria-describedby` for short, relevant supplementary information, not to attach a long block of unrelated content to every control. Refer to [Labels](Labels.md) and [Anchors and Links](Anchors%20and%20Links.md).

### Keep states true and synchronised

Use native state when it exists. For a custom control, define a single source of truth for the visible and programmatic state, then update them together. For example, when a custom disclosure opens, reveal its panel and change `aria-expanded` to `true`; when it closes, hide the panel and set the state to `false`. Do not use ARIA state to claim that an action succeeded before the interface has actually changed.

Keep ID references such as `aria-labelledby` and `aria-describedby` unique and valid. A broken reference can leave a control without the intended name or description. Use `aria-controls` only when it serves the chosen pattern; it expresses a relationship but does not implement the controlled behaviour.

### Avoid unnecessary output and conflicting information

Screen reader output is shaped by markup, content, browser and user preferences. Do not try to prescribe a fixed spoken script or add words to every control “just in case”. Repeated announcements, long descriptions, overly named regions, and live updates for changes that are already obvious can make navigation noisy and tiring. The 2026 WebAIM Million found that sampled home pages with ARIA had more detected errors on average than pages without ARIA. WebAIM also notes that these pages were more complex and that the correlation does not establish ARIA as the cause. Treat this as a reason to review complexity and implementation quality, not as evidence that ARIA itself causes barriers.

Before adding a role, name, state, description or live announcement, ask:

- What meaningful information is currently missing for the person?
- Is that information already conveyed by native HTML, visible text, position or an existing announcement?
- Will this addition remain concise, accurate and useful in the context where it is encountered?
- Can the person review important information after an announcement has finished?

Do not apply `aria-hidden="true"` to a focusable element or to a container that contains visible interactive controls. It hides content from assistive technology without necessarily removing it from keyboard focus, which can leave a person focusing on something they cannot perceive through their assistive technology. If content should be hidden from everyone, use an appropriate visual and interaction technique as well as managing its focus and state.

## Design and development handover

Document the intended user experience, not a guaranteed phrase that every screen reader must speak. For each component or journey state, specify:

| Record | What to describe |
| --- | --- |
| Purpose and structure | The user task, the suitable native elements, the content hierarchy and how the visual grouping is represented in the DOM and reading order. |
| Names and relationships | The visible label, accessible name source, relevant description, and any ID-based relationships. State which text is essential and where it appears. |
| Role and state | The intended role, current state, state changes and how they stay aligned with the visible interface. Use native semantics where they fit. |
| Input and focus | Pointer, touch, keyboard and speech input where applicable; activation keys; focus entry, movement, return and visibility. |
| Reading and announcements | What a person can find through headings, landmarks, links, controls and normal reading; which changes need an announcement, why, and how the information remains available. |
| Variants and fallback | Differences by platform or implementation, the supported browser and assistive technology combinations, and what remains usable if scripting or an enhancement is unavailable. |

Use the owning Cosmos component guide to define its detailed interaction. For task-level reading sequence, focus and recovery, see [Designing the Screen Reader Journey](Designing%20the%20Screen%20Reader%20Journey.md).

## Review the implemented experience

Review both the visual interface and what the browser exposes to assistive technology. Use an accessibility tree or browser inspection to check roles, names, states and relationships, then use the actual supported assistive technology combinations to observe how people can navigate and operate the interface.

- Check that the accessibility tree expresses the same content, grouping, labels and state that the visual interface communicates.
- Navigate by headings, landmarks, links, form controls and reading order. Confirm that important parts of the task are findable and make sense out of context.
- Operate each interactive pattern with a keyboard and other supported input methods. Check that focus remains visible and that no control is hidden from assistive technology while still focusable.
- Trigger opening, closing, selection, validation and dynamic updates. Confirm that state changes are communicated once, accurately, and without unexpected focus movement.
- Compare results across the product’s supported browser and assistive technology combinations. Record the environment and observed output; do not fail a test solely because a screen reader uses a different phrase or order than an illustrative example.
- Use automated checks to find detectable markup problems, but review meaning, usability and task completion with people and assistive technologies. An ARIA validator or automated scan cannot establish that the screen reader experience is coherent.

For patterns with complex keyboard interaction, follow the corresponding guide and the relevant APG pattern rather than relying on this cross-cutting checklist.

## Sources

- [WAI-ARIA 1.2 specification](https://www.w3.org/TR/wai-aria-1.2/)
- [ARIA in HTML](https://www.w3.org/TR/html-aria/)
- [Using ARIA](https://www.w3.org/TR/using-aria/)
- [ARIA Authoring Practices Guide: names and descriptions](https://www.w3.org/WAI/ARIA/apg/practices/names-and-descriptions/)
- [ARIA Authoring Practices Guide: design patterns](https://www.w3.org/WAI/ARIA/apg/patterns/)
- [WebAIM: Introduction to ARIA](https://webaim.org/techniques/aria/)\n- [WebAIM: The 2026 Million report](https://webaim.org/projects/million/)
- [TetraLogical: Foundations — introduction to WAI-ARIA](https://tetralogical.com/blog/2023/02/10/foundations-wai-aria/)
- [TetraLogical: Foundations — HTML semantics](https://tetralogical.com/blog/2022/10/05/foundations-html-semantics/)
- [TetraLogical: Browsing with a desktop screen reader](https://tetralogical.com/blog/2021/09/29/browsing-with-a-desktop-screen-reader/)
- [TetraLogical: Common misconceptions about screen readers](https://tetralogical.com/blog/2025/10/14/common-misconceptions-about-screen-readers/)
- [BBC Accessibility, Your Team and You: How to document the screen reader user experience](https://bbc.github.io/accessibility-news-and-you/guides/screen-reader-ux.html)
- [BBC Accessibility, Your Team and You: Accessibility swarm checklist](https://bbc.github.io/accessibility-news-and-you/checklist/accessibility-swarm.html)
