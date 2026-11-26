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

Place the message by the action or content it concerns. State the outcome and, if needed, explain how to continue. For an error tied to a field, associate the message with that field. Keep the draft available for correction. Use an error summary for several errors where it helps people find them. When a submitted form returns an error summary, link each summary item to the affected field and keep its wording consistent with the inline error. See the [error message](https://design-system.service.gov.uk/components/error-message/) and [error summary](https://design-system.service.gov.uk/components/error-summary/) examples.

For a routine update, put an empty, visible `role="status"` container in the document before writing to it. The role provides a polite live region. Update its text when the outcome is known. Keep any recovery link or button nearby in the ordinary tab order. Reserve assertive announcements for genuinely urgent information.

```html
<button type="submit">Save preferences</button>
<p id="preferences-status" role="status"></p>
```

Place this fragment in the service’s form. After a successful save, the host writes ‘Your preferences were saved’ into the status region. If saving fails, show the failure and its recovery action instead. See [Dynamic Content](Dynamic%20Content.md#announcing-status-updates) for the update technique and [WCAG 2.2 4.1.3 Status Messages](https://www.w3.org/WAI/WCAG22/Understanding/status-messages.html) (Level AA) for its scope.

A task that ends a journey may need a [confirmation page](https://design-system.service.gov.uk/patterns/confirmation-pages/) so people can revisit the outcome, reference number and next steps. For a wider notice unrelated to the current task, consider a [notification banner](https://design-system.service.gov.uk/components/notification-banner/).

### Notification centre

Provide a stable, clearly named link, such as ‘Notifications’, to a working page. If you also offer a header panel, keep the page available as the full history and follow [Information Panels](Information%20Panels.md) for opening, dismissal and focus. Use [Badge](Badge.md) to give any unread count meaning in context.

Use a heading and an ordinary list. State each item’s subject, time and any next action. Show unread status in text or through another cue that remains understandable without colour. Marking an item as read should leave it available in the history. If people can clear items, explain the effect and keep a usable focus position when an item is removed. Make important account or task information discoverable from its relevant page as well.

When new items arrive in the background, keep the person’s current focus and reading position. Announce one useful summary where needed. Avoid announcing the full feed on arrival or repeating the same update through an inline status and the centre. Make the empty state clear and provide a retry when the history cannot load.

### Worked example: a report is ready

1. A report finishes while someone is using another page. Add one unread entry to their notification history. If the count was zero, update the header link to ‘Notifications (1 unread)’. Leave focus and the reading position where they are.
2. If announcing the arrival helps, use an existing polite status region to announce ‘One new notification’. Keep the count in the link and the history in the list. Give the announcement to one region only.
3. When the person follows the Notifications link, load the dedicated page. Show the entry’s subject, time, unread state and a link to the report. If the service requires a decision or follow-up, state what to do and any deadline in the entry.
4. When the person chooses ‘Mark report notification as read’, update the count after the service accepts the change. Retain the entry, its report link and its place in the list. If the button is removed, put focus on the report link in the same entry for an in-place update. Leave a failed change retryable and keep the previous read state and count.

The first fragment belongs in the site header after the report has finished. The second belongs in the centre’s main content. The routes, identifier and time are illustrative. The host service provides the destinations and form handler, and uses the same stored read state to render the count and history. After a form submission without scripts, return to the centre with the updated entry.

```html
<a href="/notifications">Notifications (1 unread)</a>
```

```html
<main>
  <h1>Notifications</h1>
  <ul>
    <li id="notice-report-42">
      <h2>Your travel report is ready</h2>
      <p>Unread · <time datetime="2026-06-09T10:15:00+01:00">9 June, 10:15</time></p>
      <p><a href="/reports/42">Open your travel report</a></p>
      <form action="/notifications/report-42/read" method="post">
        <button type="submit">Mark report notification as read</button>
      </form>
    </li>
  </ul>
</main>
```

After the change, show ‘Read’ in the entry and update the header count. When there are no unread notices, the link can read ‘Notifications’. The [notification tray design reference](https://design.visa.com/patterns/notification-tray/) gives examples of counts, timestamps and read actions.

## Checkpoints for the tester

| Method and action | Expected result |
| --- | --- |
| Trigger a routine action. Wait, then find its feedback visually and with a screen reader. | The result is announced without moving focus and remains readable while relevant. |
| Trigger several field errors and a separate failed request. Follow the recovery instructions. | Errors are associated with the relevant fields or task. The feedback identifies what to correct or how to retry, and the draft remains available. |
| Receive a background update while working elsewhere. Open Notifications with the keyboard and read the entries. | The update leaves the current task usable. The centre has a clear name and the relevant entry has understandable content and actions. |
| Mark an entry as read, clear an entry if supported, then reopen the centre. Test the empty and failed-loading states. | Read state and counts agree. A read entry remains available according to the retention policy. Clearing leaves a logical focus position and failure has a recovery path. |
| Test any modal used for urgent feedback. Open it, read its choices and dismiss it with the keyboard. | The modal identifies the decision, keeps the background inactive and can be dismissed. Focus returns to the opener or the next logical place. |
| Review any remaining toast while magnified and with a screen reader. Let it disappear. | The same information can still be found. Any action or material condition is available in a persistent place. |
| Follow the worked example from a background completion to marking the notice as read. Repeat with scripts blocked and simulate a failed read update separately. | The count increases once when the report is ready. The centre opens from a working link. A successful read update decreases the count without removing the report entry. A failed update leaves the count and read state consistent and gives the person a way to retry. |

## Sources

- [Accessible Rich Internet Applications (WAI-ARIA) 1.2, status role](https://www.w3.org/TR/wai-aria-1.2/#status)
- [Understanding status messages](https://www.w3.org/WAI/WCAG22/Understanding/status-messages.html)
