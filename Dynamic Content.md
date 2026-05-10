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

Changes in context necessitate moving focus away from the user's work. When changing content, you should ensure that [focus](../focus) does not shift. Ensure that the newly added content can be found by employing screen reader announcements or by inserting the content so that it will be encountered in the natural reading order of the page.

### Keeping order

One way to help users of assistive technology keep oriented as content changes is to be mindful of the reading order of that content.

For assistive technology such as screen readers, the reading order of content is based on its order in the webpage source HTML. This is also true for the tab order, [^1] (although this can be manipulated by using the `tabindex` attribute). Maintaining this order will help users understand where they are in the context of the rest of the page, web form, or content section. [^4]

For example, an error message added at the top of a web form may not be noticed by a user trying to fix input at the bottom. Adding new content near to and after the point of focus the user is at will make it easier for assistive technology users to find it.

### Maintaining focus

Changes in context, except when reloading the page, require that focus be moved to a new appropriate element in that context. The focus should not become hidden, trapped, or reset at the webpage's beginning. It should be possible for a screen reader to detect and announce the title, heading, or an accessible label to explain that new context.

If focus is moved to a notification dialog, ensure it returns to the same element that had focus once that dialog is closed.

Changes in context that require moving focus should not be initiated as a user interacts with a form (unless they have submitted that form). For example, changing the page or opening a new window in response to a user changing a checkbox could confuse or disorient the user unless the user is informed of that change. [^3]

### Notifications

Notifications are short, time-sensitive, or event-based messages that appear on the webpage to inform the user about some event or process or collect user input or confirmation. From a UX perspective, notifications can be broadly categorized based on user interaction:

- **Status Message**: A notification that does not include user interaction. For example, a "could not connect" or a "feedback sent" message.
- **Dialog**: A notification that includes user interaction. For example message with a button to confirm or dismiss, a message with a link to get more information, or a form with a field to enter a username.

### Status messages

Toasts, status updates, and loading indicators are examples of status messages. [^2] They visually pop up on the screen in an unobtrusive way so as not to interrupt the user and must be announced by assistive technologies such as screen readers (either as voice or Braille).

This notification style should be limited to "for your information" updates. Failing to notice a toast message shouldn't negatively affect your work or the subject of the message.

For example, if a user has earned a reward by interacting with a game on the website, a status message will notify the user of a change in status for the user's account, such as "You've got a new reward! Check your account page to learn more."

Status messages shouldn't include user-interactive elements; keyboard focus does not jump to a status message when it appears (otherwise, it would quickly become too disruptive for the user), and therefore keyboard-only users cannot easily interact with them.

If the content of the status message is redundant, for example, if it is to inform the user that a file was uploaded, but elsewhere on the webpage, the same information can also be found, then the status message can be temporary. Toasts, for example, are ephemeral, meaning they visually appear and may be read out by a screen reader. However, they will automatically disappear from view after a reasonably short period [^6] to avoid clutter or obscure the screen.

{% aside "aside-info" %}
People with vision-related disabilities or neurological/cognitive limitations are more likely to require more time to read status messages. Keep messages simple, clear, and concise. Give status messages that automatically disappear adequate time outs. A good rule of thumb is at least two seconds plus an additional second for every four words in the message (the average reading speed for adults in English). 
{% endaside %}

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

{% aside "aside-info" %}
Our demo uses random strings in its status messages to help ensure that each message is different. A screen reader may ignore a status message unless it differs from the previous message.
{% endaside %}

<style>
  .status-control {
    border: 1px solid #666;
  }
  .status-content {
    position: fixed;
    top: 1rem;
    right: 1rem;
    border: 1px solid #666;
    background: white;
    margin-top: -1px;
    padding: 1rem;
    filter: drop-shadow(3px 3px 4px #44444488);
  }
  .status-content {
    opacity: 0;
    transition: opacity .5s ease-in-out;
  }
  .status-content.is-shown {
    opacity: 1;
    transition: opacity .5s ease-in-out;
  }
</style>
<div class="demo">
  <button
    class="status-control"
    onclick="showStatus('User ' + randomUser() + ' is online.')"
  >
    Launch Bot Army
  </button>
  <script>
  window.addEventListener('load', function () {
    window.showStatus = (function () {
      const statusContent = document.querySelector('.status-content');
      const statusQueue = [];
      const transitionTimeMs = 500; 
      const displayTimeMs = 3000;
      return function (/** string */ msg) {
        if (typeof msg === 'string') {
          statusQueue.push(msg);
        }
        if (statusContent.getAttribute('hidden') === null) {
          return;
        }
        if (statusQueue.length) {
          const msg = statusQueue.pop();
          statusContent.innerHTML = msg;
          statusContent.removeAttribute('hidden');
          const reflow = statusContent.offsetHeight;
          statusContent.classList.add('is-shown');
          setTimeout(function() {
            statusContent.classList.remove('is-shown');
            setTimeout(function() {
              statusContent.setAttribute('hidden', '');
              statusContent.innerHTML = '';
              showStatus();
            }, transitionTimeMs);
          }, displayTimeMs);
        }
      }
    })();
  });
  window.randomUser = function() {
    return 'bot-' + (Math.floor(Math.random() * 999) + 1);
  };
  </script>
</div>
<div
  class="status-content"
  role="status"
  hidden
></div>

### Dialog

A new set of native HTML APIs for creating popovers is planned, but no browser currently provides this functionality by default; it may be years before this API is widely available and proven compatible with assistive technology. A serviceable alternative is to use the `dialog` element instead.

The dialog's intended purpose is to present users with content they can interact with. This means the keyboard focus should automatically move into the dialog content when it opens. The user can then interact with this new content or close the dialog. When closing the dialog, the focus must move back to whatever element was used to open it.  

<div class="demo demo-morepanel">
<button class="morepanel-control" type="button" onclick="var content = document.getElementById('sharepanel1'); if (content.getAttribute('open') === null)content.show(); else content.close()">Share this</button>
<dialog id="sharepanel1" class="morepanel-options">
  <form method="dialog"><button class="button-close" aria-label="close"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" role="img" aria-hidden="true" focusable="false"><path fill="currentColor" d="M5.293 5.293a1 1 0 0 1 1.414 0L12 10.586l5.293-5.293a1 1 0 1 1 1.414 1.414L13.414 12l5.293 5.293a1 1 0 0 1-1.414 1.414L12 13.414l-5.293 5.293a1 1 0 0 1-1.414-1.414L10.586 12L5.293 6.707a1 1 0 0 1 0-1.414z"/></svg></button></form>
  <ul>
    <li><a href="javascript:void()">Email</a></li>
    <li><a href="javascript:void()">Toot</a></li>
    <li><a href="javascript:void()">Tweet</a></li>
    <li><a href="javascript:void()">Skeet</a></li>
    <li><a href="javascript:void()">Plop</a></li>
  </ul>
</dialog>
</div>
<style>
  .morepanel-control {
    border: 1px solid #666;
  }
  .button-close {
    position: absolute;
    width: 1.5rem;
    height:1.5rem;
    border-color: transparent;
    color: currentcolor;
    background-color: transparent;
    top: 0.5rem;
    right: 1rem;
  }
  .button-close svg {
    display: inline-block;
    margin: auto;
    width: 1.1rem;
    height: 1.1rem;
    vertical-align: text-top;
  }
  .morepanel-options {
    position: absolute;
    background-color: darkslategray;
    color: white;
    margin: 0;
    margin-top: 1rem;
    padding: 2.5rem 2rem 0.5rem;
    z-index: 999;
  }
  .morepanel-options ul {
    list-style-type: none;
    padding-left: 0;
  }
  .morepanel-options ul a {
    color: currentcolor;
  }
  /* pointer */
  .morepanel-options::before {
    content: '';
    clip-path: polygon(0 100%, 50% 0, 100% 100%);
    top: calc(-1rem + 1px);
    width: 1.3333rem;
    height: 1rem;
    display: inline-block;
    position: absolute;
    background-color: darkslategray;
    z-index: 999;
  }
</style>
<script>
window.addEventListener('load', function () {
  const dialogs = Array.from(document.querySelectorAll('.morepanel-options'));
  dialogs.forEach(function(dialog) {
    dialog.addEventListener('keydown', function (e) {
      if (e.keyCode === 27) {
        e.preventDefault();
        dialog.close();
      }
    });
  });
});
</script>

### Disclosure

The disclosure pattern comprises an interactive control element (typically a `button`) and an associated content region that can be expanded or collapsed. The control element is used to toggle the display of the related content.

The following ARIA attributes are needed to implement a disclosure widget:

- `aria-controls` - An attribute of the control element, the value is the `id` of the associated content region. This creates the programmatic relationship between the control and the content.
- `aria-expanded` - An attribute of the control element, the value is `true` if the associated content region is expanded; `false` if it is collapsed.
- `aria-labelledby` - An attribute of the associated content region; the value is the `id` of the control element.
- `role="region` - An attribute and value of the associated content region. This defines the content as a distinct region.

To illustrate, the following example uses a `button` as the control element and a `div` as the associated content region. In the initial state the state of the 

```html
<div class="disclosure">
  <button id="my-control" aria-controls="my-content" aria-expanded="false">
    Toggle Content
  </button>
  <div id="my-content" role="region" aria-labelledy="my-control">
    This is the content.
  </div>
</div>
```

When expanded, the value of the `aria-expanded` state should be updated via JavaScript to `true`.

<style>
  .disclosure-control {
    border: 1px solid #666;
  }
  .disclosure-control[aria-expanded="false"] ~ .disclosure-content {
    display: none;
  }
  .disclosure-control[aria-expanded="true"] ~ .disclosure-content {
    display: block;
  }
  .disclosure-control:after {
    font-weight: bold;
    display: inline-block;
    min-width: 1.5ch;
  }
  .disclosure-control[aria-expanded="false"]:after {
    content: "+";
  }
  .disclosure-control[aria-expanded="true"]:after {
    content: "-";
  }
  .disclosure-content {
    border: 1px solid #666;
    margin-top: -1px;
    padding: 1rem;
  }
</style>
<div class="demo">
  <div class="disclosure">
    <button
      class="disclosure-control"
      id="my-control"
      aria-controls="my-content"
      aria-expanded="true"
    >
      Toggle Content
    </button>
    <div
      class="disclosure-content"
      id="my-content"
      role="region"
      aria-labelledy="my-control"
    >
      This is the content.
    </div>
  </div>
  <script>
    const dc = Array.from(document.querySelectorAll(".demo .disclosure-control"));
    dc.forEach((item) => {
      item.setAttribute('aria-expanded', 'false');
      item.addEventListener('click', (event) => {
        const target = event.target;
      target.setAttribute('aria-expanded', target.getAttribute('aria-expanded')==='true'?'false':'true');
      });
    });
  </script>
</div>

## Accessibility notes for the tester

When evaluating a modal dialog for accessibility, add these checks to your usual testing process:

- **State changes:** Check that state changes are properly communicated to users, and that users can still access the content even if they cannot see the changes. The component that displays the state change should not include interactive elements and focus should not move to the component.
- **Dialogs:** Check that state changes are properly communicated to users, and that users can still access the content even if they cannot see the changes. Dialogs are designed to include interactive elements, so check that focus moves to the component, that keyboard-only users can interact successfully with the dialog, and that when the dialog is closes focus moves back to the same element that had focus before the dialog opened (if any).
- **Animations and transitions:** Check that animations and transitions are not too fast, cover too large an area, or contains flashing effects, and that users can still successfully use the content even if they have animations or transitions disabled.
- **WAI-ARIA:** Software testers may use an automated testing tool to check that dynamic content is properly marked up with WAI-ARIA, so that users with disabilities can use assistive technologies to access the content.

### Keyboard navigation

Navigation can be problematic when keyboard-only users encounter dynamically changing content on a webpage. Below is a table of general key-combinations to try when testing a dialog, for example, for keyboard accessibility.

- <kbd>tab</kbd> 
  - Moves focus to next focusable element inside the dialog.
  - When focus is on the last focusable element in the dialog, it must not move onto any element in the underlying webpage.
- <kbd>shift</kbd> + <kbd>tab</kbd>
  - Moves focus to previous focusable element inside the dialog.
  - When focus is on the first focusable element in the dialog, it must not move onto any element in the underlying webpage.
- <kbd>escape</kbd>
  - Closes the dialog.
  - Returns focus to the previously focused element in the underlying webpage (if any).

[^1]: [WCAG 2.1 SC 2.4.3: Focus Order (Level A)](https://www.w3.org/WAI/WCAG21/Understanding/focus-order)
[^2]: [WCAG 2.1 SC 4.1.3: Status Messages (Level AA)](https://www.w3.org/WAI/WCAG21/Understanding/status-messages.html)
[^3]: [WCAG 2.1 SC 3.2.2: On Input (Level A)](https://www.w3.org/WAI/WCAG21/Understanding/on-input)
[^4]: [WCAG 2.1 SC 1.3.2: Meaningful Sequence (Level A)](https://www.w3.org/WAI/WCAG21/Understanding/meaningful-sequence.html)
[^5]: [WCAG 2.1 SC 2.2.1: Timing Adjustable (Level A)](https://www.w3.org/WAI/WCAG21/Understanding/timing-adjustable.html)
[^6]: [How many words do we read per minute? A review and meta-analysis of reading rate by Marc Brysbaert](https://doi.org/10.1016/j.jml.2019.104047), Journal of Memory and Language, Volume 109, December 2019, 104047.
[^7]: Example derived from [WCAG 2.2 Technique H65: Using the title attribute to identify form controls when the label element cannot be used](https://www.w3.org/WAI/WCAG22/Techniques/html/H65)
[^8]: Elements with the `role="status"` attribute and value have an implicit `aria-live="polite"` attribute and value and an implicit `aria-atomic` value of `true`.
