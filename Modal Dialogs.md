---
title: "Modal Dialogs"
description: "A guide to making accessible modal dialogs."
---

# Modal Dialogs

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

<span id="modal-dialog"></span>

Use a modal dialog when the user needs to address a specific task or message before returning to the page. Prefer inline content or a dedicated page when people can continue their work without interruption. Avoid opening a modal automatically unless the interruption is necessary. For a non-modal contextual panel, see [Information Panels](Information%20Panels.md); for supplementary content revealed in place, see [Disclosure](Disclosure.md).

## Recommended markup

### Roles and accessible names

- Use native `<dialog>` with `showModal()` for ordinary modal dialogs.
- Reserve `alertdialog` for a brief, urgent message that requires a response. Give it an accessible name and associate the message with `aria-describedby`.
- Give every dialog an accessible name. Use `aria-labelledby` to reference the visible title. For longer or structured content, let users read it in context rather than announcing all of it as a description.
- Use `show()` only for an explicitly non-modal dialog. The rest of the page remains available while it is open.

### Custom modal dialogs

Use the [W3C ARIA Authoring Practices Guide for modal dialogs](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/) if the native element cannot meet the supported environment or interaction requirements. A custom modal needs an appropriate role, an accessible name and `aria-modal="true"`. Implement initial focus, background inactivity, keyboard containment, dismissal and focus return. Markup alone cannot make the background inactive. Check both keyboard navigation and screen reader browse mode.

## Recommended layout

Keep the dialog title and dismissal controls easy to find. Let content reflow and check that fixed actions leave focused controls visible.

## Recommended behaviour

### Managing focus

For shared focus indication and keyboard-order guidance, see [Focus](Focus.md) and [Keyboard Navigation](Keyboard%20Navigation.md).

- Choose an initial focus target deliberately. For a short message this may be its close button. For long or structured content, consider a heading with `tabindex="-1"` and focus it when the modal opens. For a hard-to-reverse action, consider focusing the safer choice.
- The browser restores focus when the native modal closes if the previous focus target is still available. If the opener was removed or the action starts a new step in the journey, move focus to the next logical control or heading.
- Keep focus within the current modal when tabbing through page content. Reaching browser controls can be acceptable if users can return and the underlying page remains unavailable.

### Dismissal

Provide a visible, keyboard-operable close or cancel control. Usually allow Escape as a close request; backdrop dismissal is optional. For native `<dialog>`, the `closedby` attribute controls automatic dismissal: `closedby="closerequest"` allows close requests such as Escape; `closedby="any"` also allows light dismissal when the person clicks or taps outside; `closedby="none"` disables automatic closure from user actions, including Escape. The default is automatic behaviour based on whether the dialog is modal. Choose the least surprising behaviour for the task, and do not make outside dismissal the only way to close. If you choose `none`, retain a clear close or cancel control and provide a task-specific reason for suppressing Escape. If a step needs an explicit response, provide a clear keyboard-operable way to decline or cancel. Avoid automatic timeout dismissal. Use `method="dialog"` for controls that close a dialog without submitting data to a server, or call `close()` when the action has completed.

Prefer one modal at a time. If dialogs are opened in sequence, test focus at each transition and on return to the page. If a workflow genuinely needs stacked dialogs, test that only the active dialog can be used and that focus returns to the appropriate layer. For the wider task flow around declining, cancelling or leaving, see [Refusal, Cancellation and Exit](Refusal%2C%20Cancellation%20and%20Exit.md).

### Interrupting a task

Use an action dialog when someone needs to make a decision before continuing their task. Use a button for a choice that performs an action, such as cancelling and closing the dialog. Use a link for a choice whose purpose is navigation, such as opening a sign-in page.

Use a heading that identifies the decision and label each action with its consequence. Preserve access to the underlying task after cancellation. Keep the boundary of the dialog and its controls visible in forced colours.

## Reference implementation

### Working with the native dialog HTML element

The example is self-contained. The fallback link gives users an inline route to the same information when scripts or the native modal API are unavailable. In a real service, point the fallback at the information or task the user needs.

```html
<div>
  <a id="help-fallback" href="#delivery-help">Delivery help</a>
  <button id="open-delivery-dialog" type="button" hidden>Delivery help</button>

  <dialog id="delivery-dialog" aria-labelledby="delivery-dialog-title" hidden>
    <h2 id="delivery-dialog-title">Delivery help</h2>
    <p>You can update delivery instructions before submitting your order.</p>
    <form method="dialog">
      <button type="submit" autofocus>Close</button>
    </form>
  </dialog>
</div>

<section id="delivery-help-inline" aria-labelledby="delivery-help">
  <h2 id="delivery-help" tabindex="-1">Delivery help</h2>
  <p>You can update delivery instructions before submitting your order.</p>
</section>

<style>
  #delivery-dialog {
    box-sizing: border-box;
    max-inline-size: min(36rem, calc(100% - 2rem));
    max-block-size: calc(100% - 2rem);
    overflow: auto;
    overflow-wrap: anywhere;
  }

  #delivery-dialog::backdrop {
    background: rgb(0 0 0 / 65%);
  }
</style>

<script>
  const deliveryDialog = document.getElementById('delivery-dialog');
  const openDeliveryDialog = document.getElementById('open-delivery-dialog');

  if (typeof deliveryDialog.showModal === 'function') {
    openDeliveryDialog.addEventListener('click', () => {
      deliveryDialog.showModal();
    });

    deliveryDialog.hidden = false;
    document.getElementById('delivery-help-inline').hidden = true;
    document.getElementById('help-fallback').hidden = true;
    openDeliveryDialog.hidden = false;
  }
</script>
```

## Testing Approach and ACT Scripts

Test the supported variants. Record the method, browser and assistive technology where relevant, and compare the observed result with the expected behaviour.

### Checkpoints for modal dialogs

1. Before opening, the dialog is hidden visually and from assistive technology.
2. The trigger has a clear name and works with the keyboard. When the dialog opens, focus moves to a useful element inside it, and the screen reader announces its role and name.
3. Tab and Shift+Tab can reach all relevant modal controls without moving focus onto the underlying page. The underlying page is unavailable to pointer interaction and screen reader browsing.
4. A visible backdrop or equivalent treatment indicates that the underlying page is inactive. Check that page scrolling does not interfere with reading or operating the modal.
5. The content and controls remain usable when text is resized and at narrow widths. Use the [resize and reflow checks](Typography.md#responsiveness).
6. The visible close or cancel control closes the modal. Verify Escape and outside-click behaviour against the native `closedby` value (or the custom implementation's equivalent); an interaction inside the dialog must not trigger light dismissal.
7. After closing, the underlying page is usable again and focus returns to the trigger or the next logical location.
8. With the enhancement script blocked, verify that the user can reach any essential information or action by the documented fallback.

## Interrupting a task

Use an action dialog when continuing the current task depends on a decision. Use a button for an action that changes the current page and a link for a choice that takes the user to another page. For example, a sign-in link can lead to authentication while a Cancel button closes the dialog and returns the user to the original task.

Use a heading that identifies the decision and label each action with its consequence. Preserve access to the underlying task after cancellation. Keep the boundary of the dialog and its controls visible in forced colours.

### Checkpoints for the tester

1. Open the dialog from a control midway through a page.
2. Check initial focus, read the heading and choices, cancel, and resume at the trigger.
3. If a choice leads to another page, check that it is a link and opens the intended destination. If a choice performs an action, check that it is a button and that the expected action completes.

### Supplied QA scripts

- [Modal Dialog Test Criteria](qa/supplied/modal-dialog.md) — Run the multiple-dialog case only for an existing multi-dialog workflow. Prefer one modal at a time and test the actual return-focus path.

Read the [applicability and review notes](qa/Review.md) before using these scripts for release decisions.

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Button has non-empty accessible name (97a4e1)](qa/w3c-act/97a4e1.md)
- [Element with aria-hidden has no content in sequential focus navigation (6cfa84)](qa/w3c-act/6cfa84.md)

<span id="related-references"></span>

## Sources

- [HTML Standard: dialog element and `closedby`](https://html.spec.whatwg.org/multipage/interactive-elements.html#the-dialog-element)
- [Use high contrast mode in Windows](https://support.microsoft.com/en-gb/help/13862/windows-use-high-contrast-mode)
- [Modal window (Wikipedia)](https://en.wikipedia.org/wiki/Modal_window)
- [Visually hidden text example](https://gist.github.com/Heydon/c8d46c0dd18ce96b5833b3b564e9f472)
- [Notifications (Inclusive Components blog)](https://inclusive-components.design/notifications/)
- [The current state of modal dialog accessibility](https://developer.paciellogroup.com/blog/2018/06/the-current-state-of-modal-dialog-accessibility/)
- [W3C APG: Modal Dialog Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/)
- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
- [Element with aria-hidden has no content in sequential focus navigation (6cfa84)](https://www.w3.org/WAI/standards-guidelines/act/rules/6cfa84/proposed/)
