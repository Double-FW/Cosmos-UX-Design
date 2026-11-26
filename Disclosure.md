---
title: "Disclosure"
description: "Show or hide supplementary content in place."
---

# Disclosure

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

<a name="disclosure-show-and-hide-content"></a>

Use a disclosure when people can choose whether to read supplementary information in place. Give the control a label that identifies the content it reveals. Keep focus on the control when it opens or closes the panel.

## Recommended markup

Use native `details` and `summary` where they fit. For a custom disclosure, use a button and keep its expanded state in step with the panel.

## Recommended layout

<a name="presentation"></a>

Keep the control visible in both states. Provide a visible expanded or collapsed indicator, such as the native marker or an icon whose shape changes with the state. Keep the panel next in source order so users reach the revealed content predictably.

## Recommended behaviour

Keep focus on the disclosure control when it opens or closes. After opening, let the next Tab reach the first interactive item in the panel if there is one.

## Reference implementation

<a name="implementation"></a>

### Native HTML

Use `details` and `summary` for a simple disclosure. The browser handles keyboard interaction, state and visibility without JavaScript. Keep the native marker visible unless you supply and test an equivalent indicator.

```html
<details>
  <summary>Delivery and collection options</summary>
  <p>You can collect your order on weekdays or choose home delivery.</p>
</details>
```

### Custom HTML

Use a native button with `aria-expanded` to represent the panel state. This framework’s custom examples use `aria-controls` to identify the panel. A named region is optional. Use `role="region"` and `aria-labelledby` together when the extra landmark helps people navigate.

Start with the panel visible and the button hidden. Set up the button before collapsing the panel so the content remains available when scripts are blocked. Set visual indicators from the same expanded state and keep them decorative so the accessible name stays stable.

```html
<button type="button" id="delivery-toggle" aria-controls="delivery-options" hidden>
  Delivery and collection options
  <span class="disclosure-marker" aria-hidden="true"></span>
</button>
<div id="delivery-options">
  <p>You can collect your order on weekdays or choose home delivery.</p>
</div>

<style>
  .disclosure-marker::before { content: "+"; }
  [aria-expanded="true"] .disclosure-marker::before { content: "−"; }
</style>

<script>
  const deliveryToggle = document.getElementById('delivery-toggle');
  const deliveryOptions = document.getElementById('delivery-options');

  function setDeliveryOptionsExpanded(expanded) {
    deliveryToggle.setAttribute('aria-expanded', String(expanded));
    deliveryOptions.hidden = !expanded;
  }

  deliveryToggle.addEventListener('click', () => {
    setDeliveryOptionsExpanded(deliveryToggle.getAttribute('aria-expanded') !== 'true');
  });

  setDeliveryOptionsExpanded(false);
  deliveryToggle.hidden = false;
</script>
```

## Testing Approach and ACT Scripts

Test the supported variants. Record the method, browser and assistive technology where relevant, and compare the observed result with the expected behaviour.

<a name="checkpoints-for-the-tester"></a>

1. Tab to the control and use Enter and Space to open and close it.
2. Check that the control has a clear name and that its expanded or collapsed state is visible and announced by the screen reader.
3. Confirm the closed content is hidden visually and from assistive technology. After opening, check that any links or controls in the panel occur next in the keyboard sequence.
4. Block JavaScript and confirm the native control still works or the custom panel content is available directly. In a separate test, disable CSS and confirm that the control and content remain usable.

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Summary element has non-empty accessible name (2t702h)](https://www.w3.org/WAI/standards-guidelines/act/rules/2t702h/proposed/)
- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
- [Element with aria-hidden has no content in sequential focus navigation (6cfa84)](https://www.w3.org/WAI/standards-guidelines/act/rules/6cfa84/proposed/)

## Sources

- [ARIA Authoring Practices Guide: Disclosure pattern](https://www.w3.org/WAI/ARIA/apg/patterns/disclosure/)
