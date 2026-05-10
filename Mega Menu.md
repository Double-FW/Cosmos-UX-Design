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

The most straightforward approach to implementing a Mega Menu component is to use a series of custom disclosure components to reveal the content of each menu item. This is similar to the [Accordion component](/components/accordion), displayed as a horizontal navigation menu with additional behaviors.

``` html
<nav aria-label="Main" data-ddp-component-mega-menu>
  <ul>
    <li>
      <button type="button" id="mega-menu-control-1" aria-controls="mega-menu-content-1" aria-expanded="false">[Menu Item 1]</button></h5>
      <div role="region" id="mega-menu-content-1" aria-labelledby="mega-menu-control-1" hidden>
        <p>[Mega Menu 1 content]</p>
        <p><a href="#">Link 1</a></p>
      </div>
    </li>
    <li>
      <button type="button" id="mega-menu-control-2" aria-controls="mega-menu-content-2" aria-expanded="false">[Menu Item 2]</button></h5>
      <div role="region" id="mega-menu-content-2" aria-labelledby="mega-menu-control-2" hidden>
        <p>[Mega Menu 2 content]</p>
        <p><a href="#">Link 2</a></p>
      </div>
    </li>
    <li>
      <button type="button" id="mega-menu-control-3" aria-controls="mega-menu-content-3" aria-expanded="false">[Menu Item 3]</button></h5>
      <div role="region" id="mega-menu-content-3" aria-labelledby="mega-menu-control-3" hidden>
        <p>[Mega Menu 3 content]</p>
        <p><a href="#">Link 3</a></p>
      </div>
    </li>
  </ul>
</nav>
```

{% demoiframe
   title="Native HTML.",
   head="<link rel=\"stylesheet\" href=\"/demos/components/mega-menu/mega-menu.css\"><script src=\"/demos/components/mega-menu/mega-menu.js\"></script>"
%}
<nav aria-label="Main" data-ddp-component-mega-menu>
  <ul>
    <li>
      <button type="button" id="mega-menu-control-1" aria-controls="mega-menu-content-1" aria-expanded="false">[Menu Item 1]</button></h5>
      <div role="region" id="mega-menu-content-1" aria-labelledby="mega-menu-control-1" hidden>
        <p>[Mega Menu 1 content]</p>
        <p><a href="#">Link 1</a></p>
      </div>
    </li>
    <li>
      <button type="button" id="mega-menu-control-2" aria-controls="mega-menu-content-2" aria-expanded="false">[Menu Item 2]</button></h5>
      <div role="region" id="mega-menu-content-2" aria-labelledby="mega-menu-control-2" hidden>
        <p>[Mega Menu 2 content]</p>
        <p><a href="#">Link 2</a></p>
      </div>
    </li>
    <li>
      <button type="button" id="mega-menu-control-3" aria-controls="mega-menu-content-3" aria-expanded="false">[Menu Item 3]</button></h5>
      <div role="region" id="mega-menu-content-3" aria-labelledby="mega-menu-control-3" hidden>
        <p>[Mega Menu 3 content]</p>
        <p><a href="#">Link 3</a></p>
      </div>
    </li>
  </ul>
</nav>

<p><a href="#">Link</a></p>

<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Sagittis nisl rhoncus mattis rhoncus urna neque viverra justo. Viverra accumsan in nisl nisi scelerisque eu ultrices. Pulvinar sapien et ligula ullamcorper malesuada. Tellus in hac habitasse platea dictumst vestibulum rhoncus est pellentesque. Tellus cras adipiscing enim eu. Vel pretium lectus quam id leo in vitae. Leo vel fringilla est ullamcorper eget nulla. Vel eros donec ac odio tempor orci. Fames ac turpis egestas maecenas pharetra convallis posuere morbi. Volutpat diam ut venenatis tellus in metus vulputate eu.</p>
{% enddemoiframe %}

#### Behavior

Like the Accordion component, the Mega Menu component is controlled by a series of HTML `button` elements. When the button is activated, the associated content is revealed, and when the button is activated again, the related content is hidden. Only one mega menu panel may be visible at a time; opening one will close any other open panels.

The Mega Menu component has additional behaviors compared to the Accordion component.

* When the <kbd>escape</kbd> key is pressed while the mega menu has focus, any open panels are closed
* If keyboard focus leaves the mega menu while a panel is open, the panel is closed
* Clicking outside the mega menu will close any open panels

These behaviors ensure that the mega menu can be easily controlled using a keyboard or pointing device and that the mega menu panels cannot obscure other content on the page after the user has finished interacting with them.

#### Requirements

Each control in a mega menu that opens a panel must be an HTML `button type="button"` element. The HTML `button` element must have an `aria-controls` attribute that references the `id` of the content element associated with that mega menu control. When the value of `aria-expanded` is `true`, the associated mega menu panel must be visible. When the value of `aria-expanded` is `false`, the associated mega menu panel must be hidden visually and in the accessibility tree. [^1]

The mega menu panels must immediately follow their associated control in the HTML source code of the document; this ensures that the keyboard focus order is logical and that screen reader and keyboard users can easily navigate the mega menu.

#### Guidelines

The HTML `button` element may have an `aria-controls` attribute set to the `id` that references the `id` of the associated mega menu panel.

The mega menu panel may have a `role="region"` attribute and an `aria-labelledby` attribute that references the `id` of the HTML `button` element that controls the mega menu panel.

## Checkpoints for the Tester

1. Moving the keyboard focus to each mega menu control element must be possible using the <kbd>tab</kbd> key.
1. It must be possible to open a closed mega menu panel using only a keyboard, for example, by pressing the <kbd>enter</kbd> or <kbd>space</kbd> keys.
1. A screen reader should announce that each of the mega menu control elements is an element that can be interacted with and whether it is currently in a closed (collapsed) or open (expanded) state
1. The text of each mega menu control should indicate the content of the mega menu panel.
1. A visible indication must show which state each mega menu control is in, for example, a pointer symbol in a closed or open position.
1. The mega menu panel content must _not_ be perceivable, either visibly or to a screen reader when it is in a closed state.
1. The mega menu content must be readable with JavaScript disabled and with CSS disabled, 

[^1]: "The Accessibility Tree is a subset of the DOM tree, which contains elements from the DOM tree that are relevant and useful for displaying the contents of a page in a screen reader." [_Check the Accessibility Tree for keyboard and screen reader support_ Microsoft Edge Learn](https://learn.microsoft.com/en-us/microsoft-edge/devtools-guide-chromium/accessibility/test-accessibility-tree)
