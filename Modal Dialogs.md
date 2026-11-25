---
title: Modal Dialogs
description: "A guide to making accessible modal dialogs."
---

## Summary

Use a modal dialog when the user needs to address a specific task or message before returning to the page. Prefer inline content or a dedicated page when people can continue their work without interruption. Avoid opening a modal automatically unless the interruption is necessary.

## Roles and accessible names

- Use native `<dialog>` with `showModal()` for ordinary modal dialogs.
- Reserve `alertdialog` for a brief, urgent message that requires a response. Give it an accessible name and associate the message with `aria-describedby`.
- Give every dialog an accessible name. Use `aria-labelledby` to reference the visible title. For longer or structured content, let users read it in context rather than announcing all of it as a description.
- Use `show()` only for an explicitly non-modal dialog. The rest of the page remains available while it is open.

## Working with the native dialog HTML element

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

### Managing focus

- Choose an initial focus target deliberately. For a short message this may be its close button. For long or structured content, consider a heading with `tabindex="-1"` and focus it when the modal opens. For a hard-to-reverse action, consider focusing the safer choice.
- The browser restores focus when the native modal closes if the previous focus target is still available. If the opener was removed or the action starts a new step in the journey, move focus to the next logical control or heading.
- Keep focus within the current modal when tabbing through page content. Reaching browser controls can be acceptable if users can return and the underlying page remains unavailable.

### Dismissal

Provide a visible, keyboard-operable close or cancel control and support Escape. Backdrop dismissal is optional. If a step needs an explicit response, provide a clear keyboard-operable way to decline or cancel. Avoid automatic dismissal. Use `method="dialog"` for controls that close a dialog without submitting data to a server, or call `close()` when the action has completed.

Prefer one modal at a time. If dialogs are opened in sequence, test focus at each transition and on return to the page. If a workflow genuinely needs stacked dialogs, test that only the active dialog can be used and that focus returns to the appropriate layer.

## Custom modal dialogs

Use the [W3C ARIA Authoring Practices Guide for modal dialogs][modal-pattern] if the native element cannot meet the supported environment or interaction requirements. A custom modal needs an appropriate role, an accessible name and `aria-modal="true"`. Implement initial focus, background inactivity, keyboard containment, dismissal and focus return. Markup alone cannot make the background inactive. Check both keyboard navigation and screen reader browse mode.

## Checkpoints for modal dialogs

1. Before opening, the dialog is hidden visually and from assistive technology.
2. The trigger has a clear name and works with the keyboard. When the dialog opens, focus moves to a useful element inside it, and the screen reader announces its role and name.
3. Tab and Shift+Tab can reach all relevant modal controls without moving focus onto the underlying page. The underlying page is unavailable to pointer interaction and screen reader browsing.
4. A visible backdrop or equivalent treatment indicates that the underlying page is inactive. Check that page scrolling does not interfere with reading or operating the modal.
5. The content and controls remain usable when text is resized and at narrow widths. Use the [resize and reflow checks](Typography.md#responsiveness).
6. The visible close or cancel control and Escape close the modal. If backdrop dismissal is implemented, verify that a click within the dialog does not dismiss it accidentally.
7. After closing, the underlying page is usable again and focus returns to the trigger or the next logical location.
8. With the enhancement script blocked, verify that the user can reach any essential information or action by the documented fallback.

[modal-pattern]: https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/
