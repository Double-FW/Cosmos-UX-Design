---
title: "Site Menus"
description: "Help people find pages within the current site or service."
---

# Site Menus

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

Use a site menu to help people find pages within the current site or service and see where they are in its hierarchy. Use [Global Navigation](Global%20Navigation.md) for destinations and actions shared across the wider service. Use [Mega Menu](Mega%20Menu.md) when one top-level destination needs a panel with several groups of links.

## Recommended markup

Use a named `nav` with nested lists. Give a parent destination and its submenu toggle separate controls when both are available.

## Recommended layout

<span id="presentation"></span>

Give the `nav` landmark a name that tells people what it covers, such as ‘Help centre’. Make it distinguishable from any global navigation on the same page. Where a visible label is available, use `aria-labelledby` to reference it. For translated pages, include the landmark name when checking the translated interface.

Use a list for the top-level destinations. Nest each submenu list inside the list item for its parent so people using a screen reader can follow the hierarchy. If a parent has its own page, provide a link to it and a separate button with a name that identifies the submenu it opens.

Place the disclosed links beneath or beside their parent in the visual layout. Keep the list nested in HTML even if the panel spans the available width or uses columns. Align the panel’s content with the rest of the navigation where practical. On narrow screens, let links wrap or stack without changing the [visual and tab order](Layout.md#visual-order-versus-tab-order).

Give hover and keyboard focus comparable prominence. Make keyboard focus distinct from the current-page and expanded-state indicators, including in [forced colours](Colour%2C%20Contrast%20and%20Meaning.md#focus-and-icons-in-forced-colours). Use a visible indicator such as an underline for the current page, and use a separate outline for focus. If you use a decorative SVG on the toggle, hide it from assistive technology and keep it out of the tab order. See [Icons](Icons.md).

## Recommended behaviour

<span id="behaviour"></span>

### Open and close a submenu

Use native links for navigation and [disclosure buttons](Disclosure.md#custom-html) to show and hide submenus. Enter and Space activate a focused button. Opening leaves focus on the button and the next Tab reaches the first child link. Tab and Shift+Tab follow the ordinary order through the links and beyond the submenu.

Keep `aria-expanded` on the controlling button in sync with visibility. Use `hidden` for a collapsed submenu so its links leave both the tab order and the accessibility tree. Let people close an open submenu with its button or by pressing Escape from inside it. If closing would hide the focused link, return focus to the controlling button first.

### Show the current page

Put `aria-current="page"` on the link to the current page when it appears in the menu. Use it on the actual destination only. Initialise an enhanced submenu open when it contains that link. This lets people find their place without opening each section. Leave the branch available for people to close again.

### Adapt the whole menu at narrow widths

If you collapse the site menu at narrow widths, put a native ‘Menu’ button before the top-level list. Use the same disclosure behaviour and keep its state separate from any nested toggles. When people open the menu, keep focus on its button and let Tab reach the first visible destination. If a layout change would hide a focused link or disclosure button, keep it available until focus moves or move focus to the control that can reveal it again.

Start with all destinations available through ordinary links. Reveal any enhancement controls only after they work. If scripts are blocked or fail to initialise, keep the navigation and its nested links available. See [Progressive Enhancement](Progressive%20Enhancement.md).

## Reference implementation

<span id="implementation"></span>

This example shows one enhanced submenu with a separate parent link. The site menu itself stays visible at all widths. Replace the routes with working destinations and set the current-page attribute from the page being served. Use unique IDs if there is more than one menu.

```html
<nav class="site-menu" aria-label="Help centre">
  <ul>
    <li><a href="/help">Home</a></li>
    <li>
      <a href="/help/orders">Orders</a>
      <button type="button" id="orders-toggle" aria-controls="orders-links" hidden>
        Order topics <span class="site-menu-marker" aria-hidden="true"></span>
      </button>
      <ul id="orders-links">
        <li><a href="/help/orders/tracking" aria-current="page">Track an order</a></li>
        <li><a href="/help/orders/returns">Return an order</a></li>
      </ul>
    </li>
    <li><a href="/help/contact">Contact us</a></li>
  </ul>
</nav>

<style>
  .site-menu-marker::before { content: "+"; }
  .site-menu [aria-expanded="true"] .site-menu-marker::before { content: "−"; }
  .site-menu [aria-current="page"] { font-weight: bold; }
  .site-menu :focus-visible { outline: 2px solid currentColor; }
</style>

<script>
  const orderToggle = document.getElementById('orders-toggle');
  const orderLinks = document.getElementById('orders-links');

  function setOrderTopicsExpanded(expanded) {
    orderToggle.setAttribute('aria-expanded', String(expanded));
    orderLinks.hidden = !expanded;
  }

  orderToggle.addEventListener('click', () => {
    if (!orderLinks.hidden && orderLinks.contains(document.activeElement)) {
      orderToggle.focus();
    }
    setOrderTopicsExpanded(orderLinks.hidden);
  });

  orderLinks.addEventListener('keydown', event => {
    if (event.key !== 'Escape' || event.defaultPrevented) return;
    event.preventDefault();
    orderToggle.focus();
    setOrderTopicsExpanded(false);
  });

  setOrderTopicsExpanded(Boolean(orderLinks.querySelector('[aria-current="page"]')));
  orderToggle.hidden = false;
</script>
```

## Testing Approach and ACT Scripts

Test the supported variants. Record the method, browser and assistive technology where relevant, and compare the observed result with the expected behaviour.

<span id="checkpoints-for-the-tester"></span>

Use a page with both global and site navigation where available. Test a top-level current page and a current page inside a submenu. Include a focusable element after the menu.

| Method and action | Expected result |
| --- | --- |
| Use a screen reader to list the navigation landmarks and browse the site menu’s lists. | The site navigation has a distinct name and the child links are nested under the correct parent. The parent link and submenu button have distinct purposes. |
| Start with a collapsed submenu. Open with Enter and then Space in separate runs. Tab through its links and onward. | Focus stays on the button on opening. The state changes and the first child link is next. Focus continues through the menu and into the page. |
| From a child link, press Escape. Reopen and close with the button. | Focus returns to the controlling button after Escape. Closing updates the state and removes the links from keyboard and screen reader navigation. |
| Load a page linked inside a submenu, then load a top-level destination. | The current destination is indicated visually and programmatically. Its containing submenu starts open when applicable. A parent link does not take the child’s current-page state. |
| At a narrow width, test the whole-menu button if provided. Change width or zoom while a menu link has focus. | The menu remains operable. Focus stays on a visible target or moves to the control that can reveal it. Visual order matches tab order. |
| Enable forced colours and tab from the current link to another link and a submenu button. | Current page, keyboard focus and expanded state are distinguishable. |
| Block scripts and reload. In a separate run, restore scripts and disable CSS. | With scripts blocked, the destination links work and disclosure buttons remain hidden. With CSS disabled, the nesting and disclosure behaviour remain available. |

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
- [Link has non-empty accessible name (c487ae)](https://www.w3.org/WAI/standards-guidelines/act/rules/c487ae/proposed/)
- [Element with aria-hidden has no content in sequential focus navigation (6cfa84)](https://www.w3.org/WAI/standards-guidelines/act/rules/6cfa84/proposed/)

## Sources

<span id="related-references"></span>

- [Using Navigation Landmarks](https://accessibility.blog.gov.uk/2016/05/27/using-navigation-landmarks/)
- [Hiding Content Has No Effect on Accessible Name or Description Calculation](https://w3c.github.io/using-aria/#hiding-content-has-no-effect-on-accessible-name-or-description-calculation)
- [Don't make every `<svg>` focusable](https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/8090208/)
- [Using the `aria-current` attribute](https://tink.uk/using-the-aria-current-attribute/)
- [High Contrast-friendly dropshadow](https://codepen.io/dpersing/details/vJERyv)
