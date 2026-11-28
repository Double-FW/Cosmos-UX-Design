---
title: Keyboard Navigation
description: Help people complete tasks using a keyboard or equivalent input.
---

# Keyboard Navigation

Make it possible to find, operate and leave each part of a web journey using a keyboard interface. People may use a physical keyboard, a switch device or a screen reader that provides its own navigation commands. Test complete tasks so you can see what happens when people move between controls, open a panel or continue after an error.

## Guidance

### Reach and operate each function

Use native links, buttons and form controls where they meet the need. Under [WCAG 2.1 criterion 2.1.1 Keyboard](https://www.w3.org/WAI/WCAG21/Understanding/keyboard.html) (Level A), functionality must work through a keyboard interface without requiring specific timing for individual keystrokes. The exception applies when the underlying function depends on the path of a movement, rather than only where it starts and ends.

Tab and Shift+Tab move through controls in a sequence that preserves meaning and lets people complete the task. Keep the source and visual sequences aligned where changing their relationship would confuse someone. Avoid positive `tabindex` values, which put controls ahead of the ordinary sequence. Use [Focus](Focus.md#tab-order) for indicators, tab stops and managing focus.

Use the commands people expect for each control. A composite control can have one ordinary Tab stop and use arrow keys to reach its items.

| Control | What to try |
| --- | --- |
| Link or button | Use Enter for a link. Test both Enter and Space on a button. |
| Form control | Use Space to toggle a checkbox and arrow keys within a native radio group. For a native select, use the browser and platform’s keyboard commands to open it, move through choices and commit a value. See [Forms](Forms.md#checks). |
| Tabs | Tab into the tab list, then use that pattern’s arrow-key and activation behaviour. See [Tabs](Tabs.md). |
| Modal dialog | Use the available close or cancel control and test Escape. Follow the [modal dialog checkpoints](Modal%20Dialogs.md#checkpoints-for-modal-dialogs). |

### Continue after a change

Provide a [skip link](Page_Structure.md#the-main-landmark) so people can bypass repeated navigation. If the link is visually hidden initially, reveal it on focus. Check that activation takes the user to the main content and that their next Tab starts from there.

For keyboard behaviour after an in-app route change, see [Client-side Routing](Client-side%20Routing.md). For in-place content updates, focus and status messages, see [Dynamic Content](Dynamic%20Content.md).

When a component opens, manage focus according to its pattern. A simple [Disclosure](Disclosure.md) leaves focus on its trigger. A dialog moves focus inside. On dismissal, return focus to the opener when the person is resuming the same task and the opener is still available. If the opener is gone or the action starts a new step, move focus to the next logical place. If a non-modal panel closes after focus has already moved onward, preserve the new destination.

Every component that receives keyboard focus needs a keyboard route out. If leaving requires an unfamiliar key or procedure, explain it before the person needs it. For a modal, keep the background unavailable while it is open and provide a keyboard-operable dismissal path. See [Keyboard traps](Focus.md#keyboard-traps).

### Shortcut keys

Use the standard keyboard interaction for the feature as the baseline. Where extra shortcuts are offered, make them discoverable and check for conflicts with the browser, operating system and assistive technology. Keep visible controls available for the same actions.

## Checks

Choose a representative task that crosses page sections or components, such as starting a search, completing a form, correcting an error and returning to the results. Run it with the keyboard while looking at the page, then with a supported screen reader using both reading and interaction commands. Where supported, include an external keyboard or keyboard-emulating switch on a touch device. Check that the test browser is configured to Tab to links and controls before reporting a failure. In Safari on macOS, check the Advanced settings for the option to highlight each item using Tab.

### Checks which should be (at least partly) performed manually

| Method and action | Expected result |
| --- | --- |
| Start at the top of the page. Tab to the skip link, activate it and press Tab again. | The skip link is visible when focused. It bypasses repeated navigation, and the next Tab continues from the main content. |
| Move forwards and backwards through the task. Use each component’s expected keyboard commands to reach its items. | Focus remains visible and the order preserves meaning and operability. Every interactive function is reachable, including items reached with component-specific keys. |
| Complete and submit the form using the keyboard. Trigger a validation error and correct it. | Values can be entered and changed, the error can be found and corrected, and the person can complete the task without a pointer or timed keystrokes. |
| Open, operate and dismiss a component. Tab onward. Where the opener can disappear, repeat that path. | Focus moves according to the pattern, hidden controls are skipped and the person can leave the component. Focus returns to the opener or continues at the next logical destination. |
| Repeat a representative task at increased zoom and in forced colours. | The focused item and its indicator remain visible, and all actions stay operable. Use [Focus checks](Focus.md#act-style-checks) for detailed scenarios. |

Record the browser, device, assistive technology, task, first failing step and what happened to focus. Use the component guide to verify specific keys and states.

### Supplied QA scripts

- [Skip Link Test Criteria](qa/supplied/skip-link.md) — Apply to the skip-link route through repeated navigation.

Read the [applicability and review notes](qa/Review.md) before using these scripts for release decisions.

## Sources

- [Switch access](https://en.wikipedia.org/wiki/Switch_access)
- [Screen reader survey findings on access barriers](https://webaim.org/projects/screenreadersurvey7/#problematic)
- [Screen reader survey findings on mobile keyboard use](https://webaim.org/projects/screenreadersurvey7/#mobilekeyboard)
- [Examples of switches used with mobile devices](https://www.atandme.com/?p=3758)
- [WCAG 2.1 criterion 2.1.1 Keyboard](https://www.w3.org/WAI/WCAG21/Understanding/keyboard.html)
