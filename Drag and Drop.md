---
title: Drag and Drop
description: Help people move, reorder or drop items with controls they can use without dragging.
---

# Drag and Drop

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing notes](#testing-notes)
- [Sources](#sources)

## Introduction

Offer drag and drop when direct manipulation helps people move or arrange items. Give people another way to achieve the same result. Someone using a touchscreen, a pointer with limited precision or a screen reader may be unable to hold an item and move it accurately.

| User task | Control that works without dragging |
| --- | --- |
| Reorder a short list | Use ‘Move up’ and ‘Move down’ buttons. For a long list, offer a way to choose a position directly. |
| Move a card to another group | Offer ‘Move to’ with a choice of destinations and, where order matters, a position within the destination. |
| Place or resize an object | Offer directional controls, presets or labelled fields for coordinates or dimensions, as appropriate to the task. |
| Add files | Provide a labelled native file input. Keep the same file rules for the chooser and drop area. |

WCAG 2.2 [2.5.7 Dragging Movements](https://www.w3.org/TR/WCAG22/#dragging-movements) (Level AA) requires functionality that uses dragging to be achievable with a single pointer without dragging, except when dragging is essential or when the functionality is determined by the user agent and the author has left it unchanged. Check [2.1.1 Keyboard](https://www.w3.org/TR/WCAG22/#keyboard) (Level A) separately. A button can support both routes.

## Recommended markup

Use an ordered list for a sequence and give each group a descriptive heading. Use native buttons for relative moves and a labelled select or other standard form control for named destinations. Name each action for its item, such as ‘Move Accessibility review up’. Keep the visible action words in the accessible name.

Keep the document order in step with the visual order after a move. Move the item in the DOM or re-render the list from the updated order. Use normal list, heading and control semantics. See [Layout](Layout.md#visual-order-versus-source-order).

Keep a visible status region available for brief outcomes such as ‘Accessibility review moved to position 1 of 3’. Announce the result once after the change is accepted. See [Notifications and Status Messages](Notifications%20and%20Status%20Messages.md#persistent-inline-feedback).

## Recommended layout

Put move controls with the item or in an obvious action menu. Give the controls enough space to be operated by touch. Keep them available without hover and at narrow widths. See [Call to Action](Call%20to%20Action.md#give-pointer-users-room-to-activate-the-action).

Use text, a border or an insertion line to show where an item will go. Distinguish the proposed destination from keyboard focus and committed selection. Check those cues in forced colours and at high zoom. Keep scrolling and page zoom usable and avoid motion that obscures the result.

## Recommended behaviour

### Move or reorder

Offer the same valid destinations and results through drag, keyboard and click or tap. Make currently unavailable positions clear. For a long list or a distant destination, provide a direct move so people can reach it without repeating many steps.

Keep focus on the control associated with the moved item when it remains available. Restore it after moving or re-rendering the item if the browser loses it. If the original control disappears, choose a logical visible target, such as the moved item’s action control in the new group. See [Focus](Focus.md#focus-indication).

### Cancel or recover

Show a preview while dragging and commit on a valid drop. Cancel when the pointer operation is interrupted or released outside a valid destination. If there is a keyboard pickup mode, explain how to cancel it and restore the previous order on cancellation. Provide Undo where a mistaken move would be difficult to correct.

If the move saves remotely, distinguish pending, accepted and failed states. After a failure, preserve or restore a coherent order, explain what happened and allow retry. Keep the order, permissions and destination rules the same whichever input method started the move.

### Keep a usable baseline

Use native controls as the baseline and add drag only when the enhancement is ready. Where ordering is essential to the service, provide server-backed move actions or a working alternative if JavaScript is blocked. A file chooser should remain available when a drop area cannot initialise. See [Progressive Enhancement](Progressive%20Enhancement.md).

## Reference implementation

### Reorder a list without dragging

This vanilla HTML and JavaScript example shows the controls that make a reorder available without dragging. The order is local and resets on reload. The list stays readable when scripts are blocked. Use a server-backed equivalent for required editing. If you add a drag handle, route a valid drop through the same ordering and feedback logic.

```html
<!doctype html>
<html lang="en-GB">
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Reorder review tasks</title>
<style>
  #review-tasks button { margin: 0.25rem; }
  #review-tasks [aria-disabled="true"] { border-style: dashed; }
  #review-tasks button:focus-visible {
    outline: 2px solid currentColor;
    outline-offset: 2px;
  }
</style>

<h1>Review tasks</h1>
<ol id="review-tasks">
  <li><span>Check content</span></li>
  <li><span>Accessibility review</span></li>
  <li><span>Publish</span></li>
</ol>
<p id="move-result" role="status"></p>

<script>
  const list = document.getElementById('review-tasks');
  const result = document.getElementById('move-result');

  function updateButtons() {
    const items = [...list.children];
    items.forEach((item, index) => {
      item.querySelector('[data-direction="up"]')
        .setAttribute('aria-disabled', String(index === 0));
      item.querySelector('[data-direction="down"]')
        .setAttribute('aria-disabled', String(index === items.length - 1));
    });
  }

  for (const item of list.children) {
    const name = item.querySelector('span').textContent;
    for (const direction of ['up', 'down']) {
      const button = document.createElement('button');
      button.type = 'button';
      button.dataset.direction = direction;
      button.textContent = 'Move ' + name + ' ' + direction;

      button.addEventListener('click', () => {
        const neighbour = direction === 'up'
          ? item.previousElementSibling : item.nextElementSibling;
        if (!neighbour) return;

        const hadFocus = document.activeElement === button;
        if (direction === 'up') {
          list.insertBefore(item, neighbour);
        } else {
          list.insertBefore(item, neighbour.nextElementSibling);
        }

        updateButtons();
        if (hadFocus) button.focus();
        const position = [...list.children].indexOf(item) + 1;
        result.textContent = name + ' moved to position ' + position +
          ' of ' + list.children.length + '.';
      });

      item.append(button);
    }
  }
  updateButtons();
</script>
```

At a boundary, an unavailable button stays focusable and exposes `aria-disabled="true"`. The handler also guards against activation. This lets someone keep their place after a move and choose the opposite direction. A production design should give this state a clear visual treatment.

## Testing notes

Use a list with at least three items, plus an empty destination if items can move between groups. Record the browser, device, input and assistive technology used.

| Method and action | Expected result |
| --- | --- |
| Move one item twice using a mouse click, then repeat on touch using taps. Compare with dragging if offered. | Every movement, destination and position is available without dragging. Each route leaves the same result. |
| Tab to the item’s move control. Use Enter, then Space on a separate run. Move to each boundary and attempt another move. | Focus stays on a useful control. Unavailable actions are exposed and do nothing. Visual and reading order agree after each move. |
| Use a supported screen reader to find a move control, move the item and navigate the resulting list. Move to an empty group where supported. | The item, action, destination and position are understandable. One concise result is announced where needed. The new order remains available to read. |
| Start a drag and release outside a valid destination. Repeat with any keyboard pickup and cancel controls. Simulate a failed save where applicable. | An abandoned move leaves the accepted order intact. An unsuccessful save explains the outcome and the recovery route. |
| Enlarge text, then test narrow width and forced colours separately. Block scripts in another run. | Move actions and focus remain visible. The content stays available and essential editing has a working fallback. |
| For file upload, choose a file without dropping it. Repeat with an invalid file and remove it. | The chooser reaches the same outcome as dropping. File restrictions, errors and removal are usable without a gesture. |

## Sources

- [W3C: Understanding Dragging Movements](https://www.w3.org/WAI/WCAG22/Understanding/dragging-movements.html)
- [W3C: Understanding Keyboard](https://www.w3.org/WAI/WCAG22/Understanding/keyboard.html)
- [Atlassian Design: Pragmatic drag and drop accessibility guidelines](https://atlassian.design/components/pragmatic-drag-and-drop/accessibility-guidelines/)
- [GOV.UK Design System: File upload](https://design-system.service.gov.uk/components/file-upload/)
- [ACCESSIBILITY.md: Touch and pointer accessibility guidance](https://github.com/mgifford/ACCESSIBILITY.md/blob/main/examples/TOUCH_POINTER_ACCESSIBILITY_BEST_PRACTICES.md)
