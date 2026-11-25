---
title: Mega Menu
description: A navigation component used to display a large amount of content associated with a single menu item.
---

The Mega Menu is typically used to display lists of links to other pages or sections of a website.

## Presentation

The Mega Menu component appears like a traditional horizontal navigation menu, but instead of containing links, each menu item expands to reveal a panel containing additional content.

### Requirements

Each top-level menu control should expand a mega menu panel. If a mixture of links and mega menu controls are used, the mega menu controls should be visually distinct from other links.

When the mega menu panel is expanded, the menu control should be visually indicated as active, and the panel should be visually associated with the menu control that expanded it.

## Implementation

Build on the [custom disclosure behaviour](Cards%20-%20Reveal.md#custom-html). Keep ordinary navigation links inside the panels, in source order after their buttons. The example keeps the links available until JavaScript can initialise the controls. Replace the sample destinations with working routes.

```html
<nav aria-label="Main" id="example-mega-menu">
  <ul>
    <li>
      <button type="button" aria-controls="menu-products" hidden>
        Products <span class="menu-marker" aria-hidden="true"></span>
      </button>
      <ul id="menu-products">
        <li><a href="/products">All products</a></li>
        <li><a href="/products/new">New products</a></li>
      </ul>
    </li>
    <li>
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
  });

  megaMenu.addEventListener('keydown', (event) => {
    if (event.key === 'Escape' && openMenuButton) {
      event.preventDefault();
      closeMenu(true);
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

### Behaviour

Only one panel may be open. Enter and Space activate the button. Tab and Shift+Tab follow the ordinary sequence of buttons and links. Escape closes the open panel and returns focus to its button. Leaving the navigation or clicking outside closes the panel without stealing focus from the user’s next target. If an outside click would leave focus inside a panel that is closing, return it to the controlling button.

## Checkpoints for the Tester

1. Tab to each menu button and operate it with Enter and Space. Verify that its expanded state is visible and announced by a screen reader.
2. Confirm that only one panel is open, closed links are outside the tab order and accessibility tree, and open links follow their button in the tab order.
3. Press Escape from a link and verify that focus returns to its button. Then test tabbing out and clicking outside, including whether the next focused element remains usable.
4. Block JavaScript and verify that the links remain reachable. Disable CSS in a separate test and verify that the buttons still operate and the panel state is announced.
