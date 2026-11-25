---
title: Global Navigation
description: "Moving between services."
---

# Global Navigation

Provide a named navigation landmark, a home link with an accessible name, and clearly labelled account, notifications and search controls where supplied. Give notification counts a text equivalent. Keep priority links available across breakpoints. A More button can expose overflow links. Preserve ordinary link behaviour and provide an early [skip link](Structure.md#the-main-landmark).

## Implementation

Give the sign-in link text that identifies its purpose. When signed in, label account controls and expose their expanded state. When space runs out, move destinations into the overflow list and keep each link reachable once. For the overflow drawer variant, keyboard activation moves focus to the first link. Pointer activation can place focus on the labelled drawer container. Avoid animating keyboard-triggered opening and closing. Test this after text resizing as well as on initial page load.

## Checkpoints for the tester

1. Open More with Enter and Space and check that the first overflow destination is reachable.
2. Escape returns focus to the opener.
3. Tabbing onward should preserve the destination of focus.
4. Check the header with forced colours, reduced motion, user fonts, text resized to 200% and zoom up to 400%.
5. For the enhanced header design target, check 44 × 44 CSS pixel controls, 4.5:1 text contrast and 3:1 functional graphics and control boundaries.
