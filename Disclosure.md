---
title: Disclosure (show and hide content)
description: Show or hide supplementary content in place.
---

# Disclosure (show and hide content)

Use a disclosure when people can choose whether to read supplementary information in place. Give the control a label that identifies the content it reveals. Keep focus on the control when it opens or closes the panel.

## Presentation

Keep the control visible in both states. Provide a visible expanded or collapsed indicator, such as the native marker or an icon whose shape changes with the state. Keep the panel next in source order so users reach the revealed content predictably.

## Implementation

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

## Checkpoints for the Tester

1. Tab to the control and use Enter and Space to open and close it.
2. Check that the control has a clear name and that its expanded or collapsed state is visible and announced by the screen reader.
3. Confirm the closed content is hidden visually and from assistive technology. After opening, check that any links or controls in the panel occur next in the keyboard sequence.
4. Block JavaScript and confirm the native control still works or the custom panel content is available directly. In a separate test, disable CSS and confirm that the control and content remain usable.
