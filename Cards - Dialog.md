---
title: Cards - Dialog
description: A modal dialog that can be used to present content that is opened from the page's main content and requires the user's immediate attention.
---

While dialogs are not required to be modal, there are very few use cases for this, and non-modal dialogs are generally confusing for users, particularly keyboard users. Therefore, this component is only intended for modal dialogs.

> "For keyboard users, it can be very annoying to locate and dismiss the non-modal dialog - especially when the focus order does not match the visual order." [^4]

## Presentation

The dialog card is displayed overlaid atop the page's main content. Content on the host page may be obscured by the use of a backdrop as the intent is that, while displayed, only the dialog content can be interacted with; covering the host page can help users to focus on the dialog content.

### Requirements

It is common for dialogs components to be centered vertically and horizontally within the viewport and to be of fixed height. However, it is important to cater for browser zoom and mobile devices, which may have a smaller viewport. Therefore, dialogs should be responsive and adapt to the available space or be scrollable. The dialog must be positioned so that it is not obscured by the browser chrome or other elements on the page; for example, the top of the dialog must be positioned so that the top edge of the browser viewport does not crop it.

It must be possible to close all dialog cards. This can be achieved by providing a close button or, if the dialog requires the user to take action, by closing the dialog when the user takes that action. If the dialog is closed by the user, the focus must be returned to the element that opened the dialog unless the action taken in the dialog changes the context of the page or loads a new page.

### Guidelines

By convention, modal dialog content is treated as a separate page. This means that the dialog should start with a level one heading that describes the content of the dialog.

## Implementation

### Native HTML

The native HTML `dialog` element [^2] is well-supported by modern browsers and provides a simple way to create a dialog component. However, the Safari browser is tied to the user's macOS version. Therefore if your site or application has a significant number of users with a version of Safari earlier than 15.4 or a version of macOS earlier than Catalina (the oldest version of macOS that supports Safari 15.4), then a custom approach might be required.

``` html
<button type="button" data-ddp-component-dialog-open="ddp-component-dialog-1">Open Dialog</button>
<dialog id="ddp-component-dialog-1">
  [dialog content]
  <button type="button" data-ddp-component-dialog-close>Close</button>
</dialog>
```

{% demoinpage %}
<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Sagittis nisl rhoncus mattis rhoncus urna neque viverra justo.</p>

<button class="btn btn-primary" type="button" data-ddp-component-dialog-open="ddp-component-dialog-1">Open Dialog</button>
<dialog id="ddp-component-dialog-1">
  <h1>[dialog title]</h1>
  <p>[dialog content]</p>
  <p><button class="btn btn-secondary" type="button" data-ddp-component-dialog-close>Close</button></p>
</dialog>
{% enddemoinpage %}

Only content outside of the `dialog` will be obscured by the backdrop and made inaccessible to keyboard and screen reader users.

The dialog is displayed by calling the `showModal()` method on the dialog element. This method will display the dialog and prevent the user from interacting with the host page until the dialog is closed. 

``` javascript
let dialog = document.getElementById('ddp-component-dialog-1');
dialog.showModal();
```

Note: the `open()` method will also display the dialog but will not prevent the user from interacting with the host page, including content hidden by the dialog. This method is not recommended for use with dialog components.

The dialog is closed by calling the `close()` method on the dialog element. This method will close the dialog and automatically return the focus to the element that opened it if it still exists in the document.

``` javascript
let dialog = this.closest('dialog');
dialog.close();
```

If you need to set focus to a different element than the control that opened the dialog, you can listen for the `close` event and set focus to the desired element:

``` javascript
dialog.addEventListener("close", (event) => {
  document.getElementById('return-control').focus();
});
```

#### Managing focus

By default, the focus is set to the first [sequentially focusable element](https://html.spec.whatwg.org/multipage/interaction.html#sequential-focus-navigation-order) within the dialog when it is opened. This is not always desirable; for example, if the dialog contains long or complex content such as terms and conditions, it would not be a good user experience to set the focus to a "Close" at the end of the dialog. The default behavior can be overridden by setting the `autofocus` attribute on the element that should receive focus when the dialog is opened; this can include the `dialog` element itself:

``` html
<dialog autofocus>
  …
</dialog>
```

{% demoinpage %}
<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Sagittis nisl rhoncus mattis rhoncus urna neque viverra justo.</p>

<button  class="btn btn-primary" type="button" data-ddp-component-dialog-open="ddp-component-dialog-2">Open Dialog</button>
<dialog id="ddp-component-dialog-2" autofocus>
  <h1>[dialog title]</h1>
  <p>[dialog content]</p>
  <p><button class="btn btn-secondary" type="button" data-ddp-component-dialog-close>Close</button></p>
</dialog>
{% enddemoinpage %}

#### Styling the backdrop

The browser creates the backdrop, the element that obscures the host page. It can be styled using the `::backdrop` [^1] pseudo-element selector:

``` css
dialog::backdrop {
  background-color: rgba(0, 0, 0, 0.5);
}
```

### Custom HTML

The alternative to using the native `dialog` element is to create a custom dialog component using a combination of HTML, CSS, and JavaScript. We recommend following the approach documented by W3C ARIA Authoring Practices Guide to implement a custom dialog component. [^3]

## Checkpoints for the Tester

1. Moving focus to the control that opens the dialog using the <kbd>tab</kbd> key must be possible.
1. It must be possible to activate the control that opens the dialog using only a keyboard, for example, by pressing the <kbd>enter</kbd> or <kbd>space</kbd> keys.
1. When the dialog is opened, the focus must be set to a logical element within the dialog. This may be the dialog container or an element within the dialog, usually the first focusable element.
1. Content on the host page must not be accessible with a keyboard, screen reader, or other assistive technology while the dialog is open.
1. It must be possible to close the dialog. Usually, this will be with a "Close" button, which must be focusable using the <kbd>tab</kbd> key and activated using the <kbd>enter</kbd> or <kbd>space</kbd> keys.
1. When the dialog is closed, the focus must be returned to the element that opened the dialog unless the action taken in the dialog changes the context of the page or loads a new page.
1. The dialog container element must have the role of `dialog`.

[^1]: [_::backdrop_ mdn web docs](https://developer.mozilla.org/en-US/docs/Web/CSS/::backdrop)
[^2]: [_The Dialog element_ mdn web docs](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dialog)
[^3]: [W3C ARIA Authoring Practices Guide: Dialog (Modal) Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/)
[^4]: [_Non-modal dialogs_ Access & Use](https://accessuse.eu/en/non-modal-dialogs.html#problems)
