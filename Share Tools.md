---
title: Share Tools
description: "Sharing a page."
---

# Share Tools

Give each service link a name that identifies the sharing action and destination. Present the links as a group and offer a focus-visible skip link past a long group. Provide a labelled, read-only URL field for manual copying. If a copy button is supplied, announce success only after copying succeeds.

## Implementation

Place the skip destination immediately after the group and make it programmatically focusable. The visible URL must remain selectable if clipboard access is unavailable. Avoid changing the clipboard just because the field receives focus.

## Checkpoints for the tester

1. Tab through the tools and try the skip link.
2. Open the sharing panel, copy the displayed address with the keyboard, then close it and check focus.
3. Verify that service icons and panel controls remain distinguishable in forced colours.

## Related references

- [windows use high contrast mode](https://support.microsoft.com/en-gb/help/13862/windows-use-high-contrast-mode)
- [Social Sharing Buttons: The First 10 Weeks](https://insidegovuk.blog.gov.uk/2014/02/20/gov-uk-social-sharing-buttons-the-first-10-weeks/)
- [Social Sharing Buttons Are Useless](https://www.tuliptree-studios.com/view-post/social-sharing-buttons-are-useless)
- [Removing Headaches From Focus Management](https://developers.google.com/web/updates/2016/03/focus-start-point)
- [Don't make every `<svg>` focusable](https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/8090208/)
