---
title: "Mega Menu"
description: "A navigation component used to display a large amount of content associated with a single menu item."
---

# Mega Menu

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

Use a mega menu when people need to browse several groups of destinations from a site’s main navigation. A panel can show multiple groups at once, provided people can follow their order with a keyboard or screen reader. For a simpler site hierarchy, see [Site Menus](Site%20Menus.md).

## Recommended markup

Use a named navigation landmark with links for destinations and buttons for disclosure panels. Put the controlled links after their trigger in source order.

## Recommended layout

<a name="presentation"></a>

Use clearly labelled buttons for panels and ordinary links for direct destinations. Give each panel a visible label or make its relationship to the trigger clear from the layout.

### Requirements

Each top-level menu control should expand a mega menu panel. If a mixture of links and mega menu controls are used, the mega menu controls should be visually distinct from other links.

Show which panel is open through a visible cue such as a changed marker. Distinguish the expanded state from the current destination. Follow [Site Menus](Site%20Menus.md) for separate parent links and current-page indicators.

For a multi-column panel, group related links under descriptive labels and arrange the columns in the same sequence as the content and focus order. Stack the groups as space narrows or text grows. Check that the open panel, its trigger and its links remain usable at zoom. Make the panel boundary, state marker and focus indicator clear in forced colours. See [Layout](Layout.md#visual-order-versus-tab-order) and [Focus](Focus.md#focus-indication).

## Recommended behaviour

### Behaviour

Only one panel may be open. Enter and Space activate the button. Tab and Shift+Tab follow the ordinary sequence of buttons and links. Escape closes the open panel. If focus is inside it, move focus to its button before hiding the links. If focus is already on a navigation control outside the panel, leave it there. Leaving the navigation or clicking outside closes the panel without stealing focus from the user’s next target. If an outside click would leave focus inside a panel that is closing, return it to the controlling button.

## Reference implementation

<a name="implementation"></a>

Build on the [custom disclosure behaviour](Disclosure.md#custom-html). Keep ordinary navigation links inside the panels, in source order after their buttons. This example shows two disclosure buttons and lists in a named navigation landmark. It demonstrates the interaction in source order. Use the layout guidance above when presenting the links in columns. The baseline group labels and links are available until JavaScript initialises the controls. Replace the sample destinations with working routes.

```html
<nav aria-label="Main" id="example-mega-menu">
  <ul>
    <li>
      <span>Products</span>
      <button type="button" aria-controls="menu-products" hidden>
        Products <span class="menu-marker" aria-hidden="true"></span>
      </button>
      <ul id="menu-products">
        <li><a href="/products">All products</a></li>
        <li><a href="/products/new">New products</a></li>
      </ul>
    </li>
    <li>
      <span>Help</span>
      <button type="button" aria-controls="menu-help" hidden>
        Help <span class="menu-marker" aria-hidden="true"></span>
      </button>
      <ul id="menu-help">
        <li><a href="/help/delivery">Delivery</a></li>
        <li><a href="/help/returns">Returns</a></li>
      </ul>
    </li>
  </ul>
</nav>

<style>
  .menu-marker::before { content: "+"; }
  #example-mega-menu [aria-expanded="true"] .menu-marker::before { content: "−"; }
</style>

<script>
  const megaMenu = document.getElementById('example-mega-menu');
  const menuButtons = [...megaMenu.querySelectorAll('button[aria-controls]')];
  let openMenuButton = null;

  function menuPanel(button) {
    return document.getElementById(button.getAttribute('aria-controls'));
  }

  function setMenuExpanded(button, expanded) {
    button.setAttribute('aria-expanded', String(expanded));
    menuPanel(button).hidden = !expanded;
  }

  function closeMenu(returnFocus) {
    if (!openMenuButton) return;

    const button = openMenuButton;
    openMenuButton = null;
    if (returnFocus) button.focus();
    setMenuExpanded(button, false);
  }

  menuButtons.forEach((button) => {
    button.addEventListener('click', () => {
      const previous = openMenuButton;
      if (previous && menuPanel(previous).contains(document.activeElement)) {
        button.focus();
      }
      closeMenu(false);
      if (button !== previous) {
        setMenuExpanded(button, true);
        openMenuButton = button;
      }
    });
    setMenuExpanded(button, false);
    button.hidden = false;
    button.previousElementSibling.hidden = true;
  });

  megaMenu.addEventListener('keydown', (event) => {
    if (event.key === 'Escape' && openMenuButton) {
      event.preventDefault();
      closeMenu(menuPanel(openMenuButton).contains(document.activeElement));
    }
  });

  megaMenu.addEventListener('focusout', (event) => {
    if (!megaMenu.contains(event.relatedTarget)) closeMenu(false);
  });

  document.addEventListener('click', (event) => {
    if (!megaMenu.contains(event.target) && openMenuButton) {
      closeMenu(menuPanel(openMenuButton).contains(document.activeElement));
    }
  });
</script>
```

## Testing Approach and ACT Scripts

Test the supported variants. Record the method, browser and assistive technology where relevant, and compare the observed result with the expected behaviour.

<a name="checkpoints-for-the-tester"></a>

Use the supported browsers and screen readers. Include a focusable control before and after the navigation.

| Method and action | Expected result |
| --- | --- |
| Tab to each trigger and use Enter and Space in separate runs. Open a second panel after the first. | The trigger exposes its expanded state and shows a visible cue. Only one panel stays open. Open links follow their trigger and closed links leave the tab order and accessibility tree. |
| Press Escape from a link inside the open panel. Then reopen the panel, move focus to a different navigation control without activating it and press Escape again. | The open panel closes in both cases. Focus returns to the opener in the first case and stays on the other control in the second. |
| Tab or Shift+Tab out of the navigation. Repeat with an outside click on a focusable control and on plain content. | The panel closes. A new focus destination stays usable. If closing would hide the focused link, focus returns to the opener. |
| For a multi-column panel, compare the visual, reading and focus sequences. Repeat at a narrow width and with increased text size and zoom. | The sequence remains predictable and every group label and link stays readable and reachable when the groups stack. |
| Enable forced colours and operate both panels with the keyboard. | The open state, panel boundary and focus indicator are distinguishable. |
| Block scripts and reload. In a separate run, restore scripts and disable CSS. | With scripts blocked, the group labels and all destinations are available. With CSS disabled, the triggers still operate and the screen reader can identify their states. |

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
- [Link has non-empty accessible name (c487ae)](https://www.w3.org/WAI/standards-guidelines/act/rules/c487ae/proposed/)
- [Element with aria-hidden has no content in sequential focus navigation (6cfa84)](https://www.w3.org/WAI/standards-guidelines/act/rules/6cfa84/proposed/)

## Sources

- [W3C APG: Disclosure Navigation Menu Example](https://www.w3.org/WAI/ARIA/apg/patterns/disclosure/examples/disclosure-navigation/)
- [Cosmos: Custom disclosure implementation](Disclosure.md#custom-html)
