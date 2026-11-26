---
title: Notifications and Status Messages
description: Give people feedback they can read at their own pace, act on and find again.
---

# Notifications and Status Messages

Keep important feedback where people can find it. Someone may miss a short-lived toast while reading another part of the page, using magnification or listening to a screen reader. Put feedback about the current task near that task. Give people a notification centre for updates they may need to review later.

## Choose a pattern

| Pattern | Use it for | Announcement and focus | How long it stays available |
| --- | --- | --- | --- |
| **Persistent inline message** | A saved change, a failed request or a next step in the current task. | Use a polite status region for routine updates and keep focus where the person is working. For validation, use field errors and the [Forms recovery guidance](Forms.md#error-messages), including summary focus where appropriate. | Keep it available while it matters. Clear or replace it when the state changes, or let the person dismiss it if they can still find any information they need. |
| **Notification centre** | Background jobs, account updates and messages people may need across pages or visits. | Let the person open the destination. A dedicated page follows normal navigation. For an in-page panel, use its documented focus and dismissal behaviour. Let people read entries in order. | Keep entries according to a clear retention policy. Give people a way to revisit relevant notices and distinguish read, unread and actionable items. |
| **Modal dialog** | A decision or acknowledgement that must happen before the current task can continue safely. | Move focus into the named dialog, make the background inactive and provide a keyboard route to dismiss or decline it. | Keep it available until the person responds or dismisses it. Follow [Modal Dialogs](Modal%20Dialogs.md) for containment and focus return. |

A live region is an announcement mechanism. Pair it with visible inline feedback or a persistent record where people need to read the message again.

## Why avoid toasts

A toast appears briefly and may vanish before someone can find it, read it or reach an action. Keep errors, undo opportunities, deadlines, terms and other information that affects a decision in a persistent place. Where a product uses a toast for a low-priority update, make the same information available through the task or notification history and avoid duplicate announcements.

Review any time-limited prompt that pressures people to act or makes it harder to find a choice or material condition. See [Deceptive Patterns and Inclusive Experience Evaluation](https://github.com/Double-FW/Deceptive_Patterns_and-Inclusive_Experience_Evaluation).

## Implementation

### Persistent inline feedback

Place the message by the action or content it concerns. State the outcome and, if needed, explain how to continue. For an error tied to a field, associate the message with that field. Keep the draft available for correction. Use an error summary for several errors where it helps people find them.

For a routine update, put an empty, visible `role="status"` container in the document before writing to it. The role provides a polite live region. Update its text when the outcome is known. Keep any recovery link or button nearby in the ordinary tab order. Reserve assertive announcements for genuinely urgent information.

```html
<button type="submit">Save preferences</button>
<p id="preferences-status" role="status"></p>
```

Place this fragment in the service’s form. After a successful save, the host writes ‘Your preferences were saved’ into the status region. If saving fails, show the failure and its recovery action instead. See [Dynamic Content](Dynamic%20Content.md#announcing-status-updates) for the update technique and [WCAG 2.2 4.1.3 Status Messages](https://www.w3.org/WAI/WCAG22/Understanding/status-messages.html) (Level AA) for its scope.

### Notification centre

Provide a stable, clearly named link, such as ‘Notifications’, to a working page. If you also offer a header panel, keep the page available as the full history and follow [Information Panels](Information%20Panels.md) for opening, dismissal and focus. Use [Badge](Badge.md) to give any unread count meaning in context.

Use a heading and an ordinary list. State each item’s subject, time and any next action. Show unread status in text or through another cue that remains understandable without colour. Marking an item as read should leave it available in the history. If people can clear items, explain the effect and keep a usable focus position when an item is removed. Make important account or task information discoverable from its relevant page as well.

When new items arrive in the background, keep the person’s current focus and reading position. Announce one useful summary where needed. Avoid announcing the full feed on arrival or repeating the same update through an inline status and the centre. Make the empty state clear and provide a retry when the history cannot load.

## Checkpoints for the tester

| Method and action | Expected result |
| --- | --- |
| Trigger a routine action. Wait, then find its feedback visually and with a screen reader. | The result is announced without moving focus and remains readable while relevant. |
| Trigger several field errors and a separate failed request. Follow the recovery instructions. | Errors are associated with the relevant fields or task. The feedback identifies what to correct or how to retry, and the draft remains available. |
| Receive a background update while working elsewhere. Open Notifications with the keyboard and read the entries. | The update leaves the current task usable. The centre has a clear name and the relevant entry has understandable content and actions. |
| Mark an entry as read, clear an entry if supported, then reopen the centre. Test the empty and failed-loading states. | Read state and counts agree. A read entry remains available according to the retention policy. Clearing leaves a logical focus position and failure has a recovery path. |
| Test any modal used for urgent feedback. Open it, read its choices and dismiss it with the keyboard. | The modal identifies the decision, keeps the background inactive and can be dismissed. Focus returns to the opener or the next logical place. |
| Review any remaining toast while magnified and with a screen reader. Let it disappear. | The same information can still be found. Any action or material condition is available in a persistent place. |

## Sources

- [Accessible Rich Internet Applications (WAI-ARIA) 1.2, status role](https://www.w3.org/TR/wai-aria-1.2/#status)
- [Understanding status messages](https://www.w3.org/WAI/WCAG22/Understanding/status-messages.html)
