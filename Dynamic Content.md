---
title: Dynamic Content
description: Modify web page content dynamically.
---

## Summary

Dynamic content appears or is updated due to user interaction, timing, or in response to other parameters. Here are some examples of dynamic content:

- An error message is added to a web form when the user tries to submit the form with invalid input
- A counter increments in a sidebar as an indication that a new message was received
- A progress bar shows that a process is ongoing when a user submits a request

Dynamic content must be able to be perceived and operated upon by all users, including those who rely on assistive technologies.

## Accessibility notes for the designer

### Changing context (versus changing content)

Considering the type of change is important when deciding how to present that change to the user. While a short status message appearing in a web component may be considered a change to the _content_ of the page, more disruptive kinds of changes are considered a change in _context_.

If the user could consider themselves somewhere new, that would be perceived as a change in context; if they could consider that something was added to where they already were, that would be perceived as a change in content.

Examples of a change of context include:

- Opening a new window or navigating to a new page
- Opening a dialog that can receive user input
- Programmatically moving focus to a different part of the page
- Changing the current page to appear as if the user had navigated to a new page
- Making changes to the layout and content of a page that could cause a user to become lost

Changes in context necessitate moving focus away from the user's work. When changing content, you should ensure that [focus](Focus.md) does not shift. Ensure that the newly added content can be found by employing screen reader announcements or by inserting the content so that it will be encountered in the natural reading order of the page.

### Keeping order

One way to help users of assistive technology keep oriented as content changes is to be mindful of the reading order of that content.

For assistive technology such as screen readers, the reading order of content is based on its order in the webpage source HTML. This is also true for the tab order, [^1] (although this can be manipulated by using the `tabindex` attribute). Maintaining this order will help users understand where they are in the context of the rest of the page, web form, or content section. [^4]

For example, an error message added at the top of a web form may not be noticed by a user trying to fix input at the bottom. Adding new content near to and after the point of focus the user is at will make it easier for assistive technology users to find it.

### Maintaining focus

Changes in context, except when reloading the page, require that focus be moved to a new appropriate element in that context. The focus should not become hidden, trapped, or reset at the webpage's beginning. It should be possible for a screen reader to detect and announce the title, heading, or an accessible label to explain that new context.

If focus is moved to a notification dialog, ensure it returns to the same element that had focus once that dialog is closed.

Changes in context that require moving focus should not be initiated as a user interacts with a form (unless they have submitted that form). For example, changing the page or opening a new window in response to a user changing a checkbox could confuse or disorient the user unless the user is informed of that change. [^3]

### Notifications

Use [Notifications and Status Messages](Notifications%20and%20Status%20Messages.md) to choose between inline feedback, a notification centre and a modal dialog.

### Status messages

A status message describes a result, a waiting state or another change without taking focus. Make the update available to assistive technology. [^2] Keep important messages near the task so people can find them again. Put associated actions next to the message in the normal reading and focus order.

People read at different speeds and may need time to locate an update. [^6] Keep the information available after any temporary presentation ends. See [why to avoid toasts](Notifications%20and%20Status%20Messages.md#why-avoid-toasts).

## Accessibility notes for the developer

### Before you start

As part of the requirement for a designed, accessible experience, you should confirm the UX&D documentation for a dynamic content has defined:

- How changes to content will be presented visually (including to users of screen magnifiers).
- How assistive technology will perceive the changes to content.
- If the new content includes elements that the user may interact with, how will keyboard-only users access the content, and how will they ignore or return to their original focus after accessing it?

### Announcing status updates

Content that is presented to users visually must also be available to assistive technologies. The following example uses the `role="status"` attribute to create a "live region", [^8] which allows screen readers to announce changes to the content of the element.

```html
<div role="status">
  Your document was saved.
</div>
```

```html
<div role="alert">
  Your document could not be saved.
</div>
```

```html
<div aria-live="polite">
  A document was shared with you.
</div>
```

The `status` role is appropriate for messages that present informative updates, but you can also use the `alert` role. The `alert` role is more assertive, meaning it will interrupt any other announcements that a screen reader might make. You should decide which role is more appropriate for your content. Avoid spamming the user with too many assertive announcements.

```html
<button id="save-settings" type="button">Save settings</button>
<p id="save-status" role="status"></p>

<script>
  const saveStatus = document.getElementById('save-status');

  document.getElementById('save-settings').addEventListener('click', () => {
    saveStatus.textContent = 'Settings saved.';
  });
</script>
```

Keep the live region in the document before updating its message. In a real form, update it after the save succeeds. Keep essential messages available and test what happens if a user repeats an action or receives several updates.

### Dialog

Use a dialog when a user needs to interact with a new context. Use the [modal dialog guide](Modal%20Dialogs.md) when the underlying page must be inactive. A non-modal dialog opened with `show()` leaves the underlying page available and needs an intentional keyboard route into and out of the dialog. For ordinary secondary actions or information that can remain next in the page’s reading order, consider disclosure.

### Disclosure

Use the [disclosure implementation](Disclosure.md#implementation) when a control expands content in place. Keep focus on the control when toggling, and keep the expanded state and panel visibility in sync. Follow the [accordion](Accordion.md) or [mega menu](Mega%20Menu.md) guide for grouped or navigation-specific behaviour.

## Accessibility notes for the tester

- **Status changes:** Check that visible status messages are announced without moving keyboard focus. Test successive updates and check that essential messages remain available long enough to act on.
- **Dialogs and disclosures:** Use the [modal dialog checkpoints](Modal%20Dialogs.md#checkpoints-for-modal-dialogs) or [disclosure checkpoints](Disclosure.md#checkpoints-for-the-tester) as applicable.
- **Animations and transitions:** Check that users can still operate the content with animation disabled. Use the [motion and animation checks](Motion%20and%20Animation.md#checkpoints-for-the-tester).

[^1]: [WCAG 2.1 SC 2.4.3: Focus Order (Level A)](https://www.w3.org/WAI/WCAG21/Understanding/focus-order)
[^2]: [WCAG 2.1 SC 4.1.3: Status Messages (Level AA)](https://www.w3.org/WAI/WCAG21/Understanding/status-messages.html)
[^3]: [WCAG 2.1 SC 3.2.2: On Input (Level A)](https://www.w3.org/WAI/WCAG21/Understanding/on-input)
[^4]: [WCAG 2.1 SC 1.3.2: Meaningful Sequence (Level A)](https://www.w3.org/WAI/WCAG21/Understanding/meaningful-sequence.html)
[^6]: [How many words do we read per minute? A review and meta-analysis of reading rate](https://doi.org/10.1016/j.jml.2019.104047), Journal of Memory and Language, Volume 109, December 2019, 104047.
[^8]: Elements with the `role="status"` attribute and value have an implicit `aria-live="polite"` attribute and value and an implicit `aria-atomic` value of `true`.
