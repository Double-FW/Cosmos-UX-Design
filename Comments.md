---
title: "Comments"
description: "Help people read, navigate and take part in a discussion."
---

# Comments

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

Help people find the discussion, follow replies and post their own contribution. Repeated headings can make comments hard to distinguish in a screen reader’s headings list. Replacing a sorted list or hiding a focused control can also make someone using a keyboard lose their place.

## Recommended markup

Give the discussion and replies a meaningful structure. Label the reply field and identify which comment a reply action belongs to.

## Recommended layout

<span id="presentation"></span>

Give the discussion a visible heading that includes ‘Comments’ and identifies the topic when useful. When comments supplement an article, place them after it in an `aside` named by that heading. When the discussion is the main purpose of the page, put it in the main content.

Put the comment form or sign-in link first, followed by the total, sort control and comment stream. Explain what the total includes if it differs from the number of comments currently shown. Provide a ‘Leave a comment’ link after a long stream that takes people back to the main comment field or the sign-in link, whichever is available.

Use a list for the stream and nest replies within their parent’s list item. Give comments and replies headings at the same level within the discussion so readers can find either through heading navigation. Combine the contributor’s display name with an understandable time or other context that distinguishes repeated names. Use a `time` element with a machine-readable `datetime` value. Link each comment to its own stable fragment. Where useful, include a separate link from a reply to its parent.

Prefer visible action labels. If you need an overflow control, place it after the comment’s other actions and label it for the relevant comment, such as ‘More actions for Comment 12’. Use a border or other visible cue to distinguish replies when colours are overridden. Preserve the list structure and keep deep threads readable at narrow widths. If you limit nesting, explain where people can continue the discussion.

## Recommended behaviour

<span id="behaviour"></span>

### Posting and replying

Give the comment field a persistent label. Show the moderation rules and any character limit before people submit. Associate the limit and any validation error with the field. Keep remaining characters available during editing and avoid unsolicited announcements on every keystroke. Follow [Forms](Forms.md#validation) for validation and recovery.

Keep the submit button available so someone who submits an empty comment can find out what to correct. Preserve the draft after validation or a failed request. Clear the draft after the service confirms it has accepted the contribution. Say ‘Your comment was posted’ when it is public, or ‘Your comment is awaiting moderation’ when it has only been accepted for review. Update the public stream and count when the comment becomes public. Use a [status message](Dynamic%20Content.md#status-messages) for confirmation and leave it available long enough to read. Keep focus on a usable control after submission.

Open a reply form directly after its control and move focus to its labelled field. Identify the parent comment in the form. Give replies the same validation, character limit and confirmation behaviour as top-level comments. If the person cancels, return focus to the Reply button. When sign-in is needed, explain how to return to the discussion.

### Sorting and loading

Use a labelled native select for simple sorting. Offer ‘Latest first’ and ‘Oldest first’ where chronological ordering is supported. Offer popularity sorting only when people have a meaningful basis for interpreting it. Keep the control’s label stable and expose the selected option separately. When applying the sort, retain focus on the initiating control, keep replies with their parents and preserve the selected order. If results take time to load, indicate the wait and explain any failure with a retry path. See [Filter and Sort](Filter%20and%20Sort.md) and [Load More](Load%20More.md).

### Reactions and other actions

For a reversible reaction, use a button with a stable visible label and update `aria-pressed` with its visible selected state. Use a cue such as a changed shape or border alongside any change in colour. Place the reaction total beside the button and associate it as a description when it helps people decide. If Like and Dislike are mutually exclusive, choosing one clears the other and updates both totals. Activating the selected button again withdraws that reaction.

For overflow actions, use a [Disclosure](Disclosure.md) with the actions immediately after the trigger. Follow [Share Tools](Share%20Tools.md) for sharing links. Keep buttons, sorting controls and focus indicators visible in forced colours.

## Reference implementation

<span id="implementation"></span>

### Markup example

This fragment shows one public comment with a reply. The host service supplies the discussion URL, form submission, validation, character count updates and announcements. Use real comment IDs and timestamps from the service.

```html
<aside aria-labelledby="discussion-title">
  <h2 id="discussion-title">Comments on the proposed walking route</h2>

  <form action="/discussion/comments" method="post">
    <p>Keep comments on the route and avoid sharing personal details.</p>
    <label for="comment-text">Your comment</label>
    <p id="comment-limit">Maximum 500 characters.</p>
    <textarea id="comment-text" name="comment" required maxlength="500"
              aria-describedby="comment-limit"></textarea>
    <button type="submit">Post comment</button>
  </form>
  <p id="comment-status" role="status"></p>

  <p>2 comments, including replies</p>
  <form action="/discussion" method="get">
    <label for="comment-sort">Sort comments</label>
    <select id="comment-sort" name="sort">
      <option value="latest">Latest first</option>
      <option value="oldest">Oldest first</option>
    </select>
    <button type="submit">Apply sort</button>
  </form>

  <ul>
    <li id="comment-12" tabindex="-1">
      <h3>
        <a href="#comment-12">
          Walker 1, <time datetime="2026-06-09T10:15:00Z">10:15 on 9 June</time>
        </a>
      </h3>
      <p>The path by the river has several places to rest.</p>
      <ul>
        <li id="comment-13" tabindex="-1">
          <h3>
            <a href="#comment-13">
              Walker 2, <time datetime="2026-06-09T10:22:00Z">10:22 on 9 June</time>
            </a>
          </h3>
          <p>Are any of the benches near the entrance?</p>
        </li>
      </ul>
    </li>
  </ul>

  <a href="#comment-text">Leave a comment</a>
</aside>
```

Keep fragment IDs stable through sorting and refreshes. If comments load asynchronously, make the linked comment available before directing the reader to it. Check that the target can be read and that the next Tab continues from the comment. Where enhanced posting and sorting are provided, prefer working server routes as the baseline. If a feature depends on scripts, explain what is unavailable and give an alternative where the service provides one.

## Testing Approach and ACT Scripts

Test the supported variants. Record the method, browser and assistive technology where relevant, and compare the observed result with the expected behaviour.

<span id="checkpoints-for-the-tester"></span>

| Method and action | Expected result |
| --- | --- |
| Read the discussion heading, sort control and list with a screen reader. Navigate by headings and list items, including replies. | The discussion has a clear name. The total’s scope is understandable. Individual comments are distinguishable and replies are nested with their parent. |
| Activate a comment permalink with the keyboard, open the copied URL directly, then try ‘Leave a comment’ after the stream. | The intended comment is available and keyboard navigation can continue from it. The link at the end takes the reader to the available comment field or sign-in control. |
| Submit an empty comment, edit a draft near its limit and simulate a failed submission. Then try successful public posting and moderation where supported. | The limit, remaining count and recovery instructions are available. Failed submissions retain the draft. Successful submissions clear it and communicate the actual outcome without losing focus. The public list and total reflect published comments. |
| Open Reply on a specific comment, cancel, then reopen it and submit a reply. | The labelled field receives focus and identifies its parent. Cancelling returns focus to Reply. A published reply appears under the correct parent. |
| Change the sort order with the keyboard. Repeat with a slow or failed response. | The selection and displayed order agree after success. Replies stay with their parents. Focus stays on a usable control and any wait or failure is communicated. |
| Select, switch and withdraw reactions where supported. Open and close any overflow actions. | Reaction labels stay stable, pressed states and totals agree, and selected states are visible without colour. Overflow actions are reachable when open and skipped when collapsed. |
| Block scripts and reload. In separate runs, narrow the viewport, increase text size and enable forced colours. | The documented baseline is available. The stream, reply structure, controls and keyboard focus remain visible and usable in the supported layouts. |

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
- [Form field has non-empty accessible name (e086e5)](https://www.w3.org/WAI/standards-guidelines/act/rules/e086e5/proposed/)

## Sources

- [W3C APG: complementary landmark example](https://www.w3.org/TR/wai-aria-practices/examples/landmarks/complementary.html)
- [Using the `aria-describedby` attribute](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_aria-describedby_attribute)
- [ARIA Live Regions](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Live_Regions)
- [ARIA Authoring Practices: Menu button](https://www.w3.org/TR/wai-aria-practices-1.1/#menubutton)
- [Form styling examples](http://wtfforms.com/)
- [Screen reader keyboard shortcuts](https://dequeuniversity.com/screenreaders/nvda-keyboard-shortcuts)
- [MDN: Using the ARIA group role](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_group_role)
- [CSS appearance property](https://css-tricks.com/almanac/properties/a/appearance/)
- [WCAG2.1 1.4.1 Use Of Color](https://www.w3.org/TR/WCAG21/#use-of-color)
- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
- [Form field has non-empty accessible name (e086e5)](https://www.w3.org/WAI/standards-guidelines/act/rules/e086e5/proposed/)
