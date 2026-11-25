---
title: Information Panels
description: "Opening contextual help."
---

# Information Panels

Use a labelled non-modal panel for supplementary information. Associate it with its trigger, expose the expanded state and provide a close button. Keep it inside the viewport and distinguish its boundary in forced colours. Retain an inline or linked route to the information if enhancement fails.

## Implementation

Position the panel near its trigger. Move or realign the pointing arrow at viewport edges and ensure neighbouring content cannot cover the panel. Use the [modal dialog pattern](Modal%20Dialogs.md) when the task actually requires the underlying page to be inactive.

## Checkpoints for the tester

1. Open the panel and check focus moves to its close control and the group name is available.
2. Close with Escape and with the button.
3. Check focus returns to the trigger and users can still move to the rest of the page while the panel is open.

## Related references

- [windows use high contrast mode](https://support.microsoft.com/en-gb/help/13862/windows-use-high-contrast-mode)
- [ARIA Authoring Practices: Menu button](https://www.w3.org/TR/wai-aria-practices-1.1/#menubutton)
- [ARIA Authoring Practices: Dialog](https://www.w3.org/TR/wai-aria-practices-1.1/#dialog_modal)
- [ARIA-label Is A Xenophobe](http://www.heydonworks.com/article/aria-label-is-a-xenophobe)
- [`@supports`](https://developer.mozilla.org/en-US/docs/Web/CSS/@supports)
- [CSS Triangle](https://css-tricks.com/snippets/css/css-triangle/)
- [WCAG2.1 2.1.2: Keyboard Trap](https://www.w3.org/TR/WCAG21/#no-keyboard-trap)
- [Offer choice](https://inclusivedesignprinciples.org/#offer-choice)
