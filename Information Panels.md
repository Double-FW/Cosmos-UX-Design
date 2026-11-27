---
title: "Information Panels"
description: "Let people open and dismiss supplementary information without leaving their current task."
---

# Information Panels

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

Use a non-modal information panel for short, contextual content people can choose to open, such as delivery details or [sharing tools](Share%20Tools.md). Keep essential instructions available in the main content. Use [Disclosure](Disclosure.md) when supplementary information can expand in place and focus can stay on its trigger. Use [Modal Dialogs](Modal%20Dialogs.md) when the underlying page needs to be inactive.

## Recommended markup

Use native buttons for the opener and Close action. Name the panel where that helps orientation.

### Native Popover API for overlay panels

For a web panel that needs to appear above other page content, consider the native [Popover API](https://html.spec.whatwg.org/multipage/popover.html). Use `popover="auto"` only when closing other auto popovers, light dismissal (clicking outside) and close requests are suitable; `popover="manual"` requires the interface to manage dismissal explicitly. Keep a visible Close control when people need a clear exit. The API manages popover visibility and top-layer presentation, but it does not make a panel modal or supply its accessible name, content semantics or all focus behaviour. Choose those from the interaction: simple content may leave focus on its trigger, while interactive content must remain reachable and operable. Keep the popover near its trigger in DOM order and verify the supported browser and assistive technology combinations.

## Recommended layout

<span id="presentation"></span>

Give the trigger a label that identifies the information, such as ‘Delivery details’. Give the panel a visible title and a Close button with an accessible name. Place the panel after its trigger in source order. It can contain ordinary text, images, links and controls.

Place the panel near its trigger and keep it within the visible area. Align it towards the available space if the trigger is near an edge. If an overlay would clip or obscure content at narrow widths or high zoom, show the panel in flow. Check that neighbouring content cannot cover its controls. A pointer arrow can reinforce the association. Keep the association understandable if the arrow is absent, and check the panel boundary and focus indicator in forced colours.

## Recommended behaviour

<span id="behaviour"></span>

On opening, reveal the panel, update the expanded state and move focus to its Close button. Keep the rest of the page available. Let people navigate the panel’s contents with the usual keyboard commands and Tab onward to the next destination.

Close the panel when the user activates Close, presses Escape inside it or activates the opener again. Return focus to the opener if closing would hide the focused element. When someone tabs out or clicks another control, let them keep their new focus. If a click outside dismisses the panel and leaves no usable focus target, return focus to the opener.

## Reference implementation

<span id="implementation"></span>

Build on [custom disclosure behaviour](Disclosure.md#custom-html). Use a native button with `aria-controls` referencing the panel and update `aria-expanded` with its visibility. A named group can expose the panel title when people enter it. Keep its descendants as ordinary links, buttons and other controls.

This standalone example shows an in-flow panel so it works without positioning code. The information is readable at first. The script connects the actions before it hides the panel and reveals its trigger. Replace the sample link with the relevant service route.

```html
<div id="delivery-info">
  <button type="button" id="delivery-trigger" aria-controls="delivery-panel" hidden>
    Delivery details
  </button>
  <div id="delivery-panel" role="group" aria-labelledby="delivery-title">
    <h2 id="delivery-title">Delivery details</h2>
    <button type="button" id="delivery-close" hidden>Close delivery details</button>
    <p>Standard delivery takes three to five working days.</p>
    <a href="/delivery">View delivery options</a>
  </div>
</div>

<script>
  const deliveryInfo = document.getElementById('delivery-info');
  const trigger = document.getElementById('delivery-trigger');
  const panel = document.getElementById('delivery-panel');
  const closeButton = document.getElementById('delivery-close');

  function setPanelOpen(open, returnFocus = false) {
    trigger.setAttribute('aria-expanded', String(open));
    if (returnFocus) trigger.focus();
    panel.hidden = !open;
    if (open) closeButton.focus();
  }

  trigger.addEventListener('click', () => {
    const opening = panel.hidden;
    setPanelOpen(opening, !opening);
  });
  closeButton.addEventListener('click', () => setPanelOpen(false, true));

  deliveryInfo.addEventListener('keydown', event => {
    if (event.key === 'Escape' && !panel.hidden) {
      event.preventDefault();
      setPanelOpen(false, true);
    }
  });

  document.addEventListener('focusin', event => {
    if (!panel.hidden && !deliveryInfo.contains(event.target)) {
      setPanelOpen(false);
    }
  });

  document.addEventListener('click', event => {
    if (panel.hidden || deliveryInfo.contains(event.target)) return;

    const active = document.activeElement;
    const returnFocus = panel.contains(active) || active === document.body;
    setPanelOpen(false, returnFocus);
  });

  setPanelOpen(false);
  closeButton.hidden = false;
  trigger.hidden = false;
</script>
```

If you position the panel as an overlay, set a maximum width that accounts for page padding. Recheck the position after zoom or resize. If you add an arrow, make it decorative and test the appearance with forced colours. Avoid relying on a solid background or shadow alone to show the boundary.

## Testing Approach and ACT Scripts

Test the supported variants. Record the method, browser and assistive technology where relevant, and compare the observed result with the expected behaviour.

<span id="checkpoints-for-the-tester"></span>

Run these checks in the supported browsers and screen readers. Include a focusable element after the panel to verify onward navigation.

| Method and action | Expected result |
| --- | --- |
| Start with the enhanced panel closed. Tab to the trigger, listen to its name and state, then open with Enter and Space in separate runs. | The trigger is named and collapsed initially. Opening exposes its expanded state, moves focus to Close and makes the group name and content available. |
| Close with the Close button, Escape and the opener in separate runs. | The panel is hidden, its contents leave tab and reading order, and focus is on the opener. |
| Open the panel, Tab past its last control, then repeat by clicking an unrelated control. Also try clicking outside on plain content. | Focus can leave the panel and the page remains operable. Dismissal preserves a new focus destination or restores focus to the opener if the click leaves no usable target. |
| If the panel overlays the page, test near both viewport edges. Increase text size, then narrow the viewport separately. Repeat in forced colours. | The panel and all controls remain visible and usable. The title, boundary and focus indication remain clear. |
| If implemented with the native Popover API, test its configured Escape and outside-click behaviour with keyboard, pointer and touch. | It dismisses only in the intended cases; focus remains usable, and dismissal does not interrupt or lose the user's task. |
| Block scripts and reload. | The supplementary content or its working destination is available, and enhancement controls do not appear in an unusable state. |

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
- [Element with aria-hidden has no content in sequential focus navigation (6cfa84)](https://www.w3.org/WAI/standards-guidelines/act/rules/6cfa84/proposed/)

## Sources

- [HTML Standard: popover attribute and light dismissal](https://html.spec.whatwg.org/multipage/popover.html)
- [Use high contrast mode in Windows](https://support.microsoft.com/en-gb/help/13862/windows-use-high-contrast-mode)
- [ARIA Authoring Practices: Menu button](https://www.w3.org/TR/wai-aria-practices-1.1/#menubutton)
- [ARIA Authoring Practices: Dialog](https://www.w3.org/TR/wai-aria-practices-1.1/#dialog_modal)
- [Translation of accessible labels](http://www.heydonworks.com/article/aria-label-is-a-xenophobe)
- [CSS feature queries with @supports](https://developer.mozilla.org/en-US/docs/Web/CSS/@supports)
- [CSS Triangle](https://css-tricks.com/snippets/css/css-triangle/)
- [WCAG2.1 2.1.2: Keyboard Trap](https://www.w3.org/TR/WCAG21/#no-keyboard-trap)
- [Offer choice](https://inclusivedesignprinciples.org/#offer-choice)
- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
- [Element with aria-hidden has no content in sequential focus navigation (6cfa84)](https://www.w3.org/WAI/standards-guidelines/act/rules/6cfa84/proposed/)
