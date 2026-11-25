---
title: Site Menus
description: "Navigating a site hierarchy."
---

# Site Menus

Use a named navigation landmark containing nested lists of links. Keep a parent destination link separate from its submenu toggle. Mark the current page with `aria-current="page"`. Align hover and keyboard focus treatments. Use the [mega menu guide](Mega%20Menu.md) for shared disclosure behaviour.

## Implementation

Preserve the nested list structure even if CSS displays several columns. A regular site navigation list uses links and disclosure buttons. Its users can move through destinations with Tab without learning application-menu commands.

## Checkpoints for the tester

1. Open a submenu and press Tab.
2. The first child link should follow its button in focus order.
3. Close the submenu and confirm hidden links are skipped.
4. Block scripts and check the nested destinations can still be followed.

## Related references

- [Using Navigation Landmarks](https://accessibility.blog.gov.uk/2016/05/27/using-navigation-landmarks/)
- [Hiding Content Has No Effect on Accessible Name or Description Calculation](https://w3c.github.io/using-aria/#hiding-content-has-no-effect-on-accessible-name-or-description-calculation)
- [Don't make every `<svg>` focusable](https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/8090208/)
- [Using the `aria-current` attribute](https://tink.uk/using-the-aria-current-attribute/)
- [High Contrast-friendly dropshadow](https://codepen.io/dpersing/details/vJERyv)
