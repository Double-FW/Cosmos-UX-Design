---
title: Drawer
description: Help people use a panel at the edge of a page while keeping or pausing access to the main task.
---

# Drawer

Use a drawer to give people room for a short related task or supporting content at the edge of the page. Choose whether people need to use the page while the drawer is open before deciding where to place the panel or how it appears.

| When to use it | Pattern |
| --- | --- |
| People need the page and panel together, for example to compare a list with details. | Use a non-modal drawer in the page layout. Place it alongside the page content or let it stack when space is limited. |
| People need to finish or dismiss a short, focused step before returning to the page. | Use a modal drawer. Follow [Modal Dialogs](Modal%20Dialogs.md) and position the dialog at the edge. |
| People need only a short explanation near a control. | Use [Disclosure](Disclosure.md) or [Information Panels](Information%20Panels.md). |
| The task needs a shareable destination, several steps or substantial reading. | Prefer a dedicated page with an ordinary link. |

Use the owning guide for the drawer’s content. For example, follow [Global Navigation](Global%20Navigation.md#more-destinations) for navigation links, [Filter and Sort](Filter%20and%20Sort.md) for applying filters and [Notifications and Status Messages](Notifications%20and%20Status%20Messages.md#notification-centre) for reviewable notifications.

## Presentation

Give the opener and the drawer a task-specific label or heading, such as ‘Filters’ or ‘Delivery details’. Put the visible title before the content. Provide a visible way to close the drawer. Keep any Apply, Save or Cancel action clearly named and explain the result where it matters.

Keep the title, controls and current focus visible at narrow widths and high zoom. If the drawer has its own scroll area, make the beginning, end and actions reachable with the keyboard. Check that fixed or sticky headers and footers leave focused controls visible. WCAG 2.2 [2.4.11 Focus Not Obscured (Minimum)](https://www.w3.org/TR/WCAG22/#focus-not-obscured-minimum) (Level AA) requires that author-created content does not entirely hide a component when it receives keyboard focus. Aim to keep the whole focused control visible.

Do not rely on a shadow or background colour alone to show the drawer’s boundary. Check the boundary, labels and focus indicator in [forced colours](Colour%2C%20Contrast%20and%20Meaning.md#focus-and-icons-in-forced-colours). Use logical positioning so the layout works in right-to-left languages. Keep all content available when text grows and check [reflow](Typography.md#responsiveness) separately.

## Behaviour

### Non-modal drawer

Use a disclosure control to reveal the panel. For a custom button, expose its expanded state and put its controlled content in a predictable reading and tab order. When closed, hide the panel and remove its controls from the tab order and accessibility tree. For an ordinary in-flow drawer, leave focus on the trigger when it opens and let people navigate into the panel in sequence.

Keep the rest of the page available by keyboard, pointer and screen reader. Let focus leave the panel freely and keep it open while people refer to the page. Prefer placing the panel in flow so it cannot cover controls that remain operable. If an open drawer overlaps the page, check that people can see and operate focusable controls behind or beside it. Change the layout if the overlay obscures them.

If closing would hide the focused element, return focus to the opener or another logical control. If focus has already moved to a usable element outside the drawer, leave it there.

### Modal drawer

Use a named modal dialog and make the underlying page inactive. Reveal the drawer and move focus to a useful starting point inside it. Choose a close control for simple content or the heading or first relevant control for structured content. Keep page focus inside until the drawer is dismissed. Close with a visible control and Escape, and return focus to the opener or the next logical place when the opener is gone or the task changes. Follow [Modal Dialogs](Modal%20Dialogs.md) for the implementation and focus rules.

Avoid opening a second modal from inside the drawer. Keep decisions in the current drawer or move longer tasks to a dedicated page. Keep dismissal distinct from applying changes. Give people a predictable way to retain or discard edits and avoid losing work on accidental dismissal. See [Refusal, Cancellation and Exit](Refusal%2C%20Cancellation%20and%20Exit.md) and the [Double-FW deceptive patterns catalogue](https://github.com/Double-FW/Deceptive_Patterns_and-Inclusive_Experience_Evaluation) for evaluating whether journeys make it difficult to leave or refuse.

### Resizing, motion and fallback

Prefer keeping the same interaction at different widths. A non-modal drawer can move below the related content. If the product deliberately changes to a modal at a breakpoint, switch the role, page availability and focus behaviour together. Preserve the drawer’s state and any entered information. Keep the current focus target if it remains available in the new layout. If a transition makes a focused element inactive, move focus to a usable target before interaction continues.

Make opening and closing work without a swipe, drag, hover or click on the backdrop. Treat sliding as optional motion. Use [reduced motion preferences](Motion%20and%20Animation.md#reduced-motion-preferences) and keep focus visible during any transition. If scripts or enhancement are unavailable, keep the task reachable through visible content, a native disclosure or a working link. See [Progressive Enhancement](Progressive%20Enhancement.md).

## Implementation

### Non-modal example

This standalone example uses a native disclosure. At wider widths it sits beside the order summary. At narrow widths it falls below the summary. The summary control remains available and also closes the drawer. Native disclosure needs no script and keeps focus on its summary when toggled.

```html
<div class="order-layout">
  <section aria-labelledby="order-heading">
    <h2 id="order-heading">Order summary</h2>
    <p>Your order has two items.</p>
  </section>

  <details class="order-drawer">
    <summary>Delivery details</summary>
    <p>Standard delivery takes three to five working days.</p>
  </details>
</div>

<style>
  .order-layout {
    display: flex;
    flex-wrap: wrap;
    align-items: start;
    gap: 1rem;
  }

  .order-layout > section {
    flex: 2 1 26rem;
    min-inline-size: 0;
  }

  .order-drawer {
    flex: 1 1 16rem;
    min-inline-size: 0;
    border: 1px solid currentColor;
    padding: 1rem;
  }
</style>
```

### Modal implementation

Start from the [native dialog example](Modal%20Dialogs.md#working-with-the-native-dialog-html-element). Use its working fallback and `showModal()`. Apply positioning and sizing to place the dialog at the edge and keep it within the viewport. Retain the dialog’s accessible name, focus handling and dismissal. Check scrolling and any sticky actions with real content. Use the [custom dialog guidance](Modal%20Dialogs.md#custom-modal-dialogs) only where the native dialog cannot meet the supported interaction needs.

## Checkpoints for the tester

Record the implemented variant, browser, device and assistive technology where relevant. Use a drawer with enough content to scroll and place a focusable control before and after it. For a drawer with editing, prepare an unsaved change as part of the test.

| Method and action | Expected result |
| --- | --- |
| Starting closed, find and activate the opener with Enter, Space and touch in separate runs. Inspect the accessibility tree before and after opening. | The opener identifies the task and any disclosure state matches visibility. Closed controls are absent from the tab order and reading order. Opening exposes the drawer name or visible heading. |
| For a non-modal drawer, open it and Tab and Shift+Tab between drawer and page. Browse both with a screen reader and use a page control with a pointer. | Focus follows the documented order. The page remains available and no focused control is obscured by the drawer. |
| For a modal drawer, open it and check initial focus and announcement. Tab and Shift+Tab through the drawer. Attempt to use the underlying page with a screen reader and pointer. | Focus starts at a useful place in the named dialog. Page content is inactive across input methods. |
| Close via the visible control. For a modal drawer, repeat with Escape and check the result when the opener is removed. For a custom non-modal drawer, test any additional dismissal it offers. | Dismissal is available without a gesture or backdrop click. Focus returns to an appropriate visible target when the focused content is removed. |
| If the drawer edits information, try Apply, Cancel and any other dismissal separately with unsaved changes. | Each action has the stated effect. No dismissal unexpectedly applies or loses changes. |
| Increase text size, then separately test reflow at a width equivalent to 320 CSS pixels for vertically scrolling content. Scroll to the last action with the keyboard and cross any layout breakpoint with the drawer open. | Drawer content and controls remain reachable. Fixed content does not obscure focus. The layout preserves entered information and uses the correct focus and page-availability rules. |
| Repeat with forced colours and reduced motion, then block scripts and reload. | Boundaries and focus remain identifiable, the drawer works without non-essential motion, and a usable baseline remains. |

## Sources

- [W3C ARIA Authoring Practices Guide: Disclosure](https://www.w3.org/WAI/ARIA/apg/patterns/disclosure/) and [Modal Dialog](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/) for the underlying interaction patterns.
- [W3C technique H102: Creating modal dialogs with the HTML dialog element](https://www.w3.org/WAI/WCAG22/Techniques/html/H102) for the native modal approach.
- [WCAG 2.2 Reflow](https://www.w3.org/TR/WCAG22/#reflow) and [Focus Not Obscured (Minimum)](https://www.w3.org/TR/WCAG22/#focus-not-obscured-minimum) for layout and focused controls.
- [Fluent 2 Drawer](https://fluent2.microsoft.design/components/web/react/core/drawer/usage) for inline and overlay layouts.
- [Singapore Government Design System Drawer](https://designsystem.tech.gov.sg/components/drawer) for modal drawer naming, focus and keeping the task short.
