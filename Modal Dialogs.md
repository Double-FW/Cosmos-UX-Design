---
title: Modal Dialogs
description: "A guide to making accessible modal dialogs."
---

## Summary

The modal dialog pattern presents content that requires a user's full and immediate attention. The _modal_ characteristic means the user is temporarily blocked from reading or interacting with the underlying webpage until the dialog is closed. The dialog content is presented as an overlay in front of the underlying webpage. 

The use cases for modal dialogs include displaying a time-sensitive message for the user to acknowledge, collecting user input required to use the webpage, or confirming a critical action before proceeding.

## Caution

Because this pattern intentionally interrupts the user's workflow, it should be used sparingly, particularly if the modal dialog is opened automatically, for example, on page load. Deploying an inaccessible modal dialog can effectively exclude users from a website entirely. Consider alternatives such as an in-page presentation or a link to a separate, dedicated page.

## Semantic dialog roles

WAI-ARIA provides two possible semantic roles that can be used with modal dialogs based on the urgency of information they are meant to communicate:

- `dialog`: Use when the content contains necessary but non-urgent information.
- `alertdialog`: Use when the content contains urgent information or when immediate user input is required.

In practice, to the user, there is little to distinguish thse two roles, but you may choose to style them differently. Some assistive technologies may describe them differently, informing the user which role was used to define the dialog.

Setting `aria-modal="true"` on a dialog allows assistive technology to inform the user of its modal nature, and screen readers may restrict the user from browsing[^1] outside of the dialog content.

## Implementation options for dialogs

There are currently two options available for implementing modal dialogs:

- A bespoke technique builds on a generic HTML element like a `div`, adding the appropriate  `role` and `aria-modal` attributes. For example:
  
  ```html
  <div role="dialog" aria-modal="true">
    …
  </div>
  ```
  
  All presentation and behavior for the modal dialog must be added using a combination of HTML attributes, CSS rules, and JavaScript.
- More recently, browsers have started to support a new `dialog` HTML element. For example: 
  
  ```html
  <dialog aria-modal="true">
    …
  </dialog>
  ```
  
  The browser manages the dialog presentation and behavior directly, but you will likely still need to add _some_ CSS and JavaScript to customize the component to suit your specific needs.

It's possible to build nearly identical user experiences using either technique. Deciding which approach to take will depend on several considerations and how those factors might impact or exclude your particular users. We document both methods here and leave it to you to determine which will work better for your users.

### Consider browser compatibility

Using a generic HTML element with `role="dialog"` is long established and can be made to work with older browsers, at least back to Microsoft Internet Explorer 9.

Older browsers do not support using the new `dialog` element.[^2] Importantly, no Internet Explorer (IE) version supports it. You can, however, find [polyfills for the `dialog` element](https://github.com/GoogleChrome/dialog-polyfill) designed to support recent versions of Internet Explorer.

Older browsers are an accessibility concern because [surveys show](https://webaim.org/projects/screenreadersurvey9/#browsers) that about 3% of people using screen reader software also use Internet Explorer; probably because older versions of the popular JAWS screen reader once recommended Internet Explorer 11. Poorer, older, less technically-savvy users are more likely to use older computers and software.

## A custom-built modal dialog implementation

When planning to build an accessible modal dialog from a generic HTML element, we give ourselves the following requirements:

- The dialog content must be hidden until the dialog opens, including from assistive technology.
- The component must be identifiable as a dialog to assistive technology and have an accessible label.
- Once opened, keyboard focus must move to an appropriate focusable element in the dialog.
- Interaction with the underlying webpage must be blocked while the dialog is open, including from assistive technology.
- The dialog must have an accessible way to be closed.
- Once closed, keyboard focus must return to the element that had focus before the dialog opened, if any.

### HTML and semantics

Our example implementation uses a generic `div` HTML element as its basis. Adding the `role="dialog"` HTML attribute gives it the semantic role of a `dialog`. The `aria-modal="true"` attribute tells assistive technology we will present this element modally.

We use the `hidden` HTML attribute to hide the dialog from users, including assistive technology users, before it is opened.

We will use an `h3` element, with an ID of `myModal-label-1` as our dialog's label. To tell assistive technology about this label, we add the `aria-labelledby="myModal-label-1"` attribute to the element with the role `dialog`.

Notice that we use a second inner `div` to hold the dialog's content, this one with `role="document"`. This is done partly for practical reasons: we need the outer element to look and function as the dialog backdrop, while the inner, content-containing `div` element is styled to look like a card centered on the screen. The `document` role is not strictly necessary, but, unless your dialog contains only a form, it is helpful to tell assistive technology to treat the dialog content as it would a document.

```html
<div id="myModal-1" hidden role="dialog" aria-labelledby="myModal-label-1" aria-modal="true">
  <div role="document" class="dialog-content centered-middle">
    
    <h3 id="myModal-label-1" tabindex="0">
      Registration Has Ended
    </h3>
    <p>
      You may continue viewing the registration website but can no longer register.
    </p>
    
    <!-- a button to close the modal dialog -->
    <button type="button" class="button-close" aria-label="Close" onclick="ModalDialog('myModal-1').close()">
      <span aria-hidden="true">×</span>
    </button>
    
  </div>
</div>
```

The `tabindex="0"` attribute on the `h3` element makes it the first focusable element in the dialog content. Our JavaScript implementation will automatically move keyboard focus to the first focusable element, meaning that for screen readers, it will move to the dialog's heading when it opens. (You should configure another focusable element in your content if it is more appropriate for the user experience.)

We've also added a close button; every modal needs to have an accessible way of being closed. Because we are building this implementation from scratch, it is up to us what JavaScript API we implement to close our dialog. In our example, the API for opening the modal dialog is `ModalDialog("myModal-1").open()`, and the API for closing it is `ModalDialog("myModal-1").close()`.

### Presentation style

A modal dialog has two presentation elements that must be styled: the **backdrop** and the **content**.

#### The modal dialog backdrop

Visual affordances should be carefully considered when implementing a modal interaction. For example, people using screen magnification software or settings may only see a portion of the entire webpage at any time. If the part of the page they can see suddenly becomes non-interactive when a modal dialog opens elsewhere on the page, and there is no accompanying visual indication, this will be very confusing, at least.

Modal dialogs are opened with a full-window "backdrop" behind them. This backdrop should be semi-transparent to visually indicate that while the underlying webpage is still there, it is temporarily blocked from any user interaction.

<figure>
  <img src="{{ baseurl }}/static/img/patterns/modal-backdrop.svg"
       alt="" style="max-width:28rem;margin-top:2rem">
  <figcaption>The modal dialog is presented as a card-like content area within a semi-transparent backdrop layer, sitting above and covering the rest of the underlying webpage.</figcaption>
</figure>

In our example implementation, the outermost HTML element, which has the `aria-modal="true"` attribute, acts as the backdrop. We use CSS to position this above and covering the underlying webpage. The backdrop is styled to have a semi-transparent black `background-color`, allowing the underlying webpage to remain partially visible. We use the `position: fixed;` declaration to keep the backdrop from moving when the user scrolls.

```css
[aria-modal="true"] {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 1000;
  background-color: rgba(0, 0, 0, 0.8);
}
```

Notice we set the `z-index` to 1000. This is an arbitrary value; it can be any value sufficient to place it atop all other visible layers on the webpage.

#### The modal dialog content

For our example implementation we want the dialog content to appear as a card centered in the middle of the browser window.

You must consider what will happen on narrow displays when the dialog content may wrap and not fit within its containing card. We use the `max-height: 100%` CSS declaration to prevent the dialog content from being clipped by the top and bottom boundaries of the web browser window. The `overflow: auto` declaration will allow the user to scroll the content (either horizontally or vertically) within the dialog in cases if it doesn't fit within the available space. This is especially important to accommodate users who may set their default font size to be "large" or "very large" in their OS or web browser settings.

```css
.centered-middle {
  position: fixed;
  top: 50%;
  bottom: auto;
  left: 50%;
  transform: translate(-50%, -50%);
}

.dialog-content {
  background-color: #ffffff;
  padding: 0.5rem 1.6rem 2rem 1.6rem;
  max-height: 100%;
  width: 80%;
  max-width: 32rem;
  overflow: auto;
  border: 1px solid currentColor;
}
```

#### The rest of the webpage

Another consideration is the underlying webpage while the dialog is open. Even though we've blocked any user interaction with the underlying webpage, it is still possible to scroll its content while the dialog is open. This looks odd and can be confusing if the user unintentionally scrolls the webpage content and then finds they are looking at a part of the page they don't recognize when the dialog eventually closes.

We can prevent the underlying webpage from scrolling by using JavaScript to add a `data-hasmodal` attribute to the `body` HTML element whenever a modal dialog opens, then remove that attribute when it closes. This attribute matches a CSS rule that uses `overflow: clip`, which prevents scrolling.

```css
[data-hasmodal] {
  overflow: clip;
}
```

Note that the `inert` HTML attribute can effectively be used to prevent user interaction with the underlying webpage, but because its browser support is [limited to modern browsers](https://caniuse.com/?search=inert), and the use of a custom modal dialog approach implies you are interested in supporting older browsers, we've chosen to not use it in this example implementation.

## Scripted behavior

There are two types of features that we need to implement with JavaScript: opening and closing the modal dialog and controlling keyboard focus.

### Opening and closing

The opening and closing task is relatively straightforward. Assuming the HTML element for the dialog was given a `hidden` HTML attribute, it will already be hidden to start. Our JavaScript must remove that attribute to "open" the modal dialog and then replace the `hidden` HTML attribute to "close" it later.

It's common practice to configure modal dialogs so the user can dismiss them by clicking on the backdrop or pressing the <kbd>escape</kbd> key. We handle both of these behaviors in our JavaScript. Still, we also recognize limited cases where you want a modal dialog to avoid being so easily dismissed. For example, if you _require_ the user to give some legal consent before proceeding, you might want to enforce a more explicit "I do" or "I don't" response before continuing. We've added support for a custom `data-undismissable` attribute for such cases. This attribute will exclude those dialogs from being dismissed using the <kbd>escape</kbd> key or by clicking the backdrop element. When using this attribute, it is up to you to make other means of closing the modal dialog available to the user.

### Controlling keyboard focus

We must implement the modal behavior for our dialog using JavaScript. To limit the movement of keyboard focus, we add an invisible focusable element at the top and bottom of the modal dialog content, creating a "focus fence." These fence elements are configured with JavaScript to bounce focus back from the bottom to the top and vice-versa whenever they receive focus. The effect is that when focus moves past the end or start of the dialog content, it is moved to wrap around to the opposite side: preventing keyboard focus from leaving the open dialog.

We also use JavaScript to remember which element, if any, was active when a modal dialog opens. We use this information to restore focus back to that element when the modal dialog closes. At the same time, we add a `data-hasmodal` attribute to the `body` HTML element of the webpage, which allows us to use a CSS style to turn off scrolling while the modal dialog is open.

To simplify managing keyboard focus, we've written a [small utility library]({{ baseurl }}/static/js/_focusHelper.js) to manage focus movement.

**Notice**: if the modal dialog contains an `iframe` HTML element displaying content from another website, web browser security policies may not let you use JavaScript to move keyboard focus into any part of the `iframe` content. In such cases, you should add a focusable element to the modal dialog content _before_ the `iframe` to act as a landing point for keyboard users.

### Multiple modals?

Modals are, by definition, singular and exclusive interaction patterns. If you need the user to be presented with more than one modal, rather than layering them together all at once, we prefer to take a chained approach, where each modal is opened individually but in series: only showing the next modal once the previous has closed. We've added a feature to our implementation that allows a callback function to be registered and run whenever a modal dialog closes.

So, for example, to open `myModal-1` and then have `myModal-2` open when `myModal-1` closes, you could use this API:

```javascript
ModalDialog("myModal-1").open(function onClose() {
  ModalDialog("myModal-2").open();
});
```

This chain could be extended of course, but for the user's sake, we hope you consider how annoying that could be!

If you find that you want to **nest modal dialogs**, that is, to have a modal dialog open from within the content of another modal dialog, we suggest that you are beyond the scope of a modal dialog use case. This complexity level indicates that the content in your first modal dialog probably warrants its own dedicated page.

## Demo: custom modal dialogs

This demonstration uses the implementation details described above.

- Clicking the "Open Custom Modal Dialog 1" button will open a modal dialog with a message informing the user that "registration has ended." This modal dialog can be dismissed by pressing the <kbd>escape</kbd> key, clicking anywhere on the backdrop, or clicking the close button.
- Clicking the "Open Custom Modal Dialog 2" button will open a modal dialog with a message requiring the user to give feedback on whether they agree to the terms and conditions. This demonstrates a modal dialog that **can not** be dismissed by pressing the <kbd>escape</kbd> key or clicking the backdrop.

<div class="demo">
  <button type="button" onclick="ModalDialog('myModal-1').open()">
    Open Custom Modal Dialog 1
  </button>
  <div id="myModal-1" hidden role="alertdialog" aria-labelledby="myModal-label-1" aria-modal="true">
    <div role="document" class="dialog-content centered-middle">
      <h3 id="myModal-label-1" tabindex="0">
        Registration Has Ended
      </h3>
      <p>
        You may continue viewing the registration website but can no longer register.
      </p>
      <button type="button" class="button-close" aria-label="Close" onclick="ModalDialog('myModal-1').close()">
        <span aria-hidden="true">×</span>
      </button>
    </div>
  </div>
  <button type="button" onclick="ModalDialog('myModal-2').open()">
    Open Custom Modal Dialog 2
  </button>
  <div id="myModal-2" hidden role="dialog" aria-labelledby="myModal-label-2" aria-modal="true" data-undismissable>
    <div role="document" class="dialog-content centered-middle" tabindex="0">
      <h3 id="myModal-label-2">
        Do You Agree To Our Terms &amp; Conditions?
      </h3>
      <p>
        Before using this website, you must agree to our terms and conditions.
      </p>
      <button type="button" aria-label="Do not agree" onclick="ModalDialog('myModal-2').close()">
        I do not agree.
      </button>
      <button type="button" aria-label="Agree" onclick="ModalDialog('myModal-2').close()">
        I agree.
      </button>
    </div>
  </div>
</div>

Clicking the "Open Both Custom Modal Dialogs" button demonstrates how to chain two modal dialogs together. It will open modal dialog 1 and modal dialog 2 once modal dialog 1 closes.

<div class="demo">
  <button type="button" onclick="ModalDialog('myModal-1').open(function onClose(){ModalDialog('myModal-2').open()})">
    Open Both Custom Modal Dialogs
  </button>
</div>

See the [CSS]({{ baseurl }}/static/css/modal-dialogs.css) and [JavaScript]({{ baseurl }}/static/js/modal-dialogs.js) used in the above demo.

***

## Working with the native `dialog` HTML element

Recent browsers support an alternative approach to creating modal dialogs, using a native `dialog` HTML element. This comes with built-in features, meaning you won't have to author and manage those yourself.

### Native `dialog` and keyboard focus

Managing keyboard focus is handled for you by the native `dialog` element. 

- Elements with an `autofocus` attribute within a `dialog` element will automatically get focused when the dialog is opened.
- If no  `autofocus` attribute is found when the dialog is opened, keyboard focus will automatically move to the first focusable element within the dialog.
- When a dialog is closed, keyboard focus will automatically be returned to the element that opened the dialog, if any.

### Native `dialog` backdrop

The native `dialog` element provides its backdrop as a pseudo-element automatically sized to fully cover the web browser window. It can be styled using the CSS `::backdrop` pseudo-element selector.

```css
dialog::backdrop {
  background-color: rgba(0, 0, 0, 0.8);
}
```

### Closing a native `dialog`

Native dialogs are implemented to close in any of the following ways:

- Pressing the <kbd>escape</kbd> key while the dialog is open. This default behavior can be modified or canceled by listening to the dialog's `cancel` event and responding accordingly.
- If the dialog contains a `form` element, then any action which submits that form will automatically close the dialog. This default behavior can be modified or canceled by listening to the form's `submit` event and responding accordingly.
- Calling the JavaScript `close()` method of the dialog. This behavior can be modified or canceled by listening to the dialog's `close` event and responding accordingly.

```html
<dialog id="dialog1" class="dialog-content" role="alert">
  <h3 tabindex=0>Alert: Logged Out</h3>
  <p>You have successfully logged out. You may safely close the browser now.</p>
  <button type="button" class="button-close" onclick="window.dialog1.close();" aria-label="close">
    <span aria-hidden="true">×</span>
  </button>
</dialog>
```

```html
<dialog id="dialog2" class="dialog-content" data-undismissable>
  <h3 tabindex=0>
    Are You Sure? 
  </h3>
  <p>
    Before continuing, please confirm that you understand that this is a big deal. 
  </p>
  <form
    method="dialog"
    onsubmit="if (!document.getElementById('check-agree').checked) { return false; } else { console.log('submitted'); }">
    <p>
      <label> 
        <input
          id="check-agree"
          name="check-agree"
          type="checkbox"
          autocomplete="off"
          onchange="document.getElementById('button-continue').disabled = !this.checked">
        Yes, I know what I'm doing.
      </label> 
    </p>
    <p>
      <button id="button-continue" type="submit" disabled>
        Do It
      </button> 
    </p>
  </form>
</dialog>
```

## Native dialog demo

This demo shows how to create a user experience similar to the ones we built using a custom modal dialog approach.

<div class="demo">
  <button type="button" onclick="window.dialog1.showModal();">
    Open Native Dialog 1
  </button>
  <dialog id="dialog1" class="dialog-content" role="alert">
    <h3 tabindex=0>Alert: Logged Out</h3>
    <p>You have successfully logged out. You may safely close the browser now.</p>
    <button type="button" class="button-close" onclick="window.dialog1.close();" aria-label="close">
      <span aria-hidden="true">×</span>
    </button>
  </dialog>
  <button type="button" onclick="window.dialog2.showModal();">
    Open Native Dialog 2
  </button>
  <dialog id="dialog2" class="dialog-content" data-undismissable>
    <h3 tabindex=0>Are You Sure?</h3>
    <p>Before continuing, please confirm that you understand that this is a big deal.</p>
    <form method="dialog" onsubmit="if (!document.getElementById('check-agree').checked) { return false; } else { console.log('submitted'); }">
      <p>
        <label><input id="check-agree" name="check-agree" type="checkbox" autocomplete="off" onchange="document.getElementById('button-continue').disabled = !this.checked"> Yes, I know what I'm doing.</label>
      </p>
      <p>
        <button id="button-continue" type="submit" disabled>
          Do It
        </button>
      </p>
    </form>
  </dialog>
</div>
<div class="demo">
  <button type="button" onclick="window.dialog1.showModal();window.dialog2.showModal();">
    Open Native Dialogs 1 and 2
  </button>
</div>

See the [CSS]({{ baseurl }}/static/css/native-dialogs.css) and [JavaScript]({{ baseurl }}/static/js/native-dialogs.js) used in the above demo.

## Checkpoints for modal dialogs

Regardless of the implementation, all modal dialogs must meet the following criteria.

- Before the modal dialog opens, the content must be hidden from users, including users of assistive technology.
- When the modal dialog opens:
  - The underlying webpage must become non-interactive to the user, including screen reader users.
  - The visual presentation of the underlying webpage must indicate that it is not currently interactive, for example, by becoming darkened or grayed-out.
  - Keyboard focus must move to an appropriate focusable element which is part of the modal dialog.
  - Screen readers must announce the role, either "dialog" or "alertdialog", and read the modal dialog label.
- While the modal dialog is open:
  - Keyboard focus and the virtual cursor for screen reader browsing must be restricted to not interact with the underlying webpage. (The native dialog element may allow the user to navigate out of the dialog and onto the browser's chrome; this should be considered acceptable.)
  - The non-interactive, underlying webpage should be obscured and have scrolling disabled while the modal dialog is open.
  - All content within the modal dialog must be accessible to users of assistive technology (follow the usual testing process).
  - All content within the modal dialog must be readable on narrow screens (width of 360px) when the browser settings for font size are set to "Very Large," and the page zoom is set to 200.
  - The modal dialog must have at least one focusable element that can close the modal dialog, possibly after providing some required input.
  - It is customary but not required that pressing the <kbd>escape</kbd> key will close an open modal dialog.
  - It is customary but not required that clicking on the backdrop (the non-interactive area outside of the modal dialog content) will close an open modal dialog.
  - Avoid closing a modal dialog automatically after a fixed time. Urgent and time-sensitive messages within a modal dialog must allow users to pause any "count down" for a reasonable period to process the information according to their needs.
- When the modal dialog closes
  - The modal dialog content must become hidden visually and from assistive technology.
  - The underlying webpage should return to being unobscured, interactive and scrollable again.
  - Keyboard focus should return to whatever element had focus before the modal dialog opened, if any.
- When showing multiple modal dialogs
  - We recommend that multiple modal dialogs be shown in series rather than stacked. (The native `dialog` element will automatically open multiple modal dialogs in stacked layers; this should be considered acceptable.)
  - Keyboard focus must follow all the rules outlined above, with focus moving from the closed modal dialog and appearing on the next opened modal dialog or the underlying webpage if there are no more open modal dialogs.

[^1]: Assistive technology, like screen readers, operate in different modes. When the user presses keys to operate form elements or navigate from one interactive element on a webpage to the next, screen readers pass those keypresses through to the web browser. For example, pressing the <kbd>tab</kbd> key in this mode will move the keyboard focus to the next interactive element. Another mode, called browse mode or reading mode, allows the assistive technology user to advance a virtual reading cursor over each element on a webpage, not just the interactive ones. The assistive technology then will try to describe each element using its label or read out any text or alternative text it finds.
[^2]: The [Can I Use website](https://caniuse.com/?search=dialog) reports that the `dialog` element is only available to about 94% of global web browser users. 

<script src="{{ baseurl }}/static/js/_focusHelper.js"></script>
<script src="{{ baseurl }}/static/js/modal-dialogs.js"></script>
<script src="{{ baseurl }}/static/js/native-dialogs.js"></script>
