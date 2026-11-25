---
title: Cards - Dialog
description: A modal dialog that can be used to present content that is opened from the page's main content and requires the user's immediate attention.
---

Use this card pattern when a task needs the user’s immediate attention and the underlying page needs to be inactive. If the card only reveals supplementary content, use [Cards – Reveal](Cards%20-%20Reveal.md).

## Presentation

The dialog card is displayed overlaid atop the page's main content. Content on the host page may be obscured by the use of a backdrop as the intent is that, while displayed, only the dialog content can be interacted with; covering the host page can help users to focus on the dialog content.

### Requirements

It is common for dialogs components to be centered vertically and horizontally within the viewport and to be of fixed height. However, it is important to cater for browser zoom and mobile devices, which may have a smaller viewport. Therefore, dialogs should be responsive and adapt to the available space or be scrollable. The dialog must be positioned so that it is not obscured by the browser chrome or other elements on the page; for example, the top of the dialog must be positioned so that the top edge of the browser viewport does not crop it.

It must be possible to close all dialog cards. This can be achieved by providing a close button or, if the dialog requires the user to take action, by closing the dialog when the user takes that action. If the dialog is closed by the user, the focus must be returned to the element that opened the dialog unless the action taken in the dialog changes the context of the page or loads a new page.

## Implementation

Use the [named native dialog example](Modal%20Dialogs.md#working-with-the-native-dialog-html-element). Open the card’s dialog with `showModal()`, label it from the visible heading and choose an initial focus target that matches the content. The heading level should fit the document structure.

For a long dialog, consider focusing the title or start of the content. Give users a visible way to close it and use Escape. If the opener is removed or the action moves the user into a different context, return focus to the next logical location. See [managing focus](Modal%20Dialogs.md#managing-focus) and [custom modal dialogs](Modal%20Dialogs.md#custom-modal-dialogs) for further implementation guidance.

## Checkpoints for the Tester

Run the [modal dialog checkpoints](Modal%20Dialogs.md#checkpoints-for-modal-dialogs), including the announcement of the dialog’s role and accessible name. With several cards on the page, confirm that each trigger opens the correct dialog, that its name identifies the content, and that closing returns focus to the same card unless the action changes context. Check that dialog content can be read and operated at narrow widths without clipping.
