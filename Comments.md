---
title: Comments
description: "Reading and joining a discussion."
---

# Comments

Group the labelled discussion after the article. Put the form or sign-in route first, followed by the total, sort controls and comment stream. Give each comment a heading with enough context to distinguish repeated display names, a permalink and an understandable time. Nest replies beneath their parent. Provide a way back to the form after the stream.

## Implementation

Put moderation rules before submission and associate the rules, character limit and errors with the textarea. Keep the draft available after a validation failure. Use a status message to confirm a successful post. Label a reply form with its parent comment. When voting is reversible, keep the selected state and total in sync. See [Forms](Forms.md#validation) and [Dynamic Content](Dynamic%20Content.md#status-messages).

### Markup example

This field fragment illustrates an example length limit. Put it inside the submission form and populate the status message after a successful post.

```html
<label for="comment-text">Your comment</label>
<p id="comment-hint">Keep your comment under 500 characters.</p>
<textarea id="comment-text" maxlength="499" aria-describedby="comment-hint"></textarea>
<p role="status" id="comment-status"></p>
```

## Checkpoints for the tester

1. Open Reply on a specific comment.
2. Focus should enter its labelled reply field.
3. Check that character limits, remaining characters and submission errors are available while editing.
4. On submission, confirm success and identify the posted reply.
5. Change the sort order and check the selected order and revised stream.
6. Operate each reaction and verify its name, selected state and updated count without duplicate announcements.

## Related references

- [Complementary Landmark](https://www.w3.org/TR/wai-aria-practices/examples/landmarks/complementary.html)
- [Using the `aria-describedby` attribute](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_aria-describedby_attribute)
- [ARIA Live Regions](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Live_Regions)
- [ARIA Authoring Practices: Menu button](https://www.w3.org/TR/wai-aria-practices-1.1/#menubutton)
- [WTFForms](http://wtfforms.com/)
- [NVDA Keyboard Shortcuts](https://dequeuniversity.com/screenreaders/nvda-keyboard-shortcuts)
- [Using the group role](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_group_role)
- [`appearance`](https://css-tricks.com/almanac/properties/a/appearance/)
- [WCAG2.1 1.4.1 Use Of Color](https://www.w3.org/TR/WCAG21/#use-of-color)
