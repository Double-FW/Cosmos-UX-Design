---
title: "Global Navigation"
description: "Help people reach the main areas of a service, search and manage their account from a consistent header."
---

# Global Navigation

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

Use global navigation for destinations people need across a service. When the header changes at a narrow width or higher zoom, keep every destination available. A link that is visually clipped but still receives focus can leave keyboard users unable to see where they are.

## Recommended markup

Group destinations in a named navigation landmark. Use links for destinations and buttons for actions that reveal groups of links.

## Recommended layout

<span id="presentation"></span>

Place the navigation in the page header and give it a visible or accessible label that distinguishes it from other navigation landmarks. Group peer destinations in an unordered list. Keep their order consistent across pages and provide an early [skip link](Page_Structure.md#the-main-landmark).

- **Home.** Give a linked logo an accessible name that identifies its destination. If the link already contains suitable visible text, make the logo decorative. If a single-colour inline SVG logo should follow surrounding text, use `currentColor` for its fill or stroke. Check that it remains identifiable in forced colours. Use [Functional Alternative Text](Functional%20Alternative%20Text.md) and [Icons](Icons.md) for the image treatment.
- **Account.** Use ‘Sign in’ when signed out and ‘Your account’ or an identifiable account label when signed in. Keep the accessible name available at every width. If you add an account panel, its button exposes the expanded state. An account destination link needs only its link name.
- **Notifications.** Provide a working notifications link where the feature exists. If you show a count, say what it counts, such as ‘3 unread notifications’. See [Badge](Badge.md) for counts and [Notifications and Status Messages](Notifications%20and%20Status%20Messages.md#notification-centre) for notification history.
- **Search.** Provide a link to the search page or a working search form. Name the scope when it helps people choose, such as ‘Search this service’. Use [Search](Search.md) for the form, landmark and suggestions.

Keep text labels visible where practical. When a compact control uses an icon, make its purpose clear and preserve its accessible name. Check the logo, text, functional icons and control boundaries in forced colours and with user-defined colours. See [Colour, Contrast and Meaning](Colour%2C%20Contrast%20and%20Meaning.md) and [Focus](Focus.md#focus-indication).

## Recommended behaviour

<span id="behaviour"></span>

### Baseline navigation

Start with working links to all destinations, including any account, notification and search pages you provide. Keep those destinations accessible if scripts are blocked or fail. Use ordinary links for navigation and buttons for revealing panels. Preserve standard activation keys and the ability to open links in a new tab.

### More destinations

Use a [Disclosure](Disclosure.md) labelled ‘More destinations’ when links no longer fit. Put the panel immediately after its button in source order. Move each overflow item into the panel so it appears once. Keep hidden destinations out of visual, keyboard and screen reader navigation. If no priority links fit, hide or remove the empty list. Retain the existing order within each visible list. If the overflow drawer is a distinct navigation region, give it a `nav` landmark with a descriptive name such as ‘More destinations’.

For an ordinary disclosure, leave focus on the button when it opens and let the next Tab reach the first revealed link. If you use a [drawer](Drawer.md) that deliberately moves focus on open, reveal it first and use a predictable focus target. The first link is a useful target for keyboard activation. A named drawer container can be a target for pointer activation if it needs to introduce the panel. If Escape or another explicit close action would hide the focused item, return focus to the opener. If the panel closes because focus has already moved elsewhere, leave focus at its new destination. Use [Mega Menu](Mega%20Menu.md) and [Site Menus](Site%20Menus.md) for shared navigation behaviour.

If the drawer moves focus on opening, open and close it without animation when operated by keyboard. Make the focus target visible before moving focus. Follow [Motion and Animation](Motion%20and%20Animation.md) for reduced-motion handling. If text resizing or zoom changes which links fit, keep the focused link available until the user can move on.

## Reference implementation

<span id="implementation"></span>

This fragment shows a baseline with a skip link, a home destination, account and notification links, and a separate search destination. Replace the sample routes with working service routes. A script can group overflow links behind a native button once the enhancement is ready. Keep the baseline list available until then.

```html
<a href="#main">Skip to main content</a>
<header>
  <nav aria-label="Service navigation">
    <a href="/">Home</a>
    <ul>
      <li><a href="/services">Services</a></li>
      <li><a href="/support">Support</a></li>
    </ul>
    <ul>
      <li><a href="/sign-in">Sign in</a></li>
      <li><a href="/notifications">Notifications</a></li>
    </ul>
    <a href="/search">Search this service</a>
  </nav>
</header>
<main id="main" tabindex="-1">
  <h1>Service overview</h1>
</main>
```

If you enhance a destination link into a drawer, keep the link as a fallback until the replacement button and panel work. For an embedded search form, use the search landmark guidance in [Search](Search.md). For a notification drawer, provide access to the full notifications page when the drawer only shows recent items.

## Testing Approach and ACT Scripts

Test the supported variants. Record the method, browser and assistive technology where relevant, and compare the observed result with the expected behaviour.

<span id="checkpoints-for-the-tester"></span>

Run the checks on a wide and narrow layout and in the supported browsers and assistive technologies. Test account and notification states when those features are present.

| Method and action | Expected result |
| --- | --- |
| Tab from the beginning of the page and activate the skip link. Then navigate by landmarks with a screen reader. | The skip link reaches the main content. The page header and global navigation can be identified and distinguished from other regions. |
| Check Home, Search and the signed-in and signed-out account variants. Where notifications show a count, read the link or control with a screen reader. | Each destination or action has a clear name. The count includes what it counts. Icon-only presentations retain their accessible names. |
| At a narrow width, open More with Enter and Space in separate runs. Tab and Shift+Tab through its links. Repeat with a pointer if the drawer uses separate focus handling. | The expanded state is available. Focus follows the documented variant. Links are available once and in order. A distinct navigation drawer is identifiable by its landmark name. A drawer that moves focus opens and closes without animation when operated by keyboard. |
| Dismiss the drawer from inside it, then reopen it and Tab onward. | Explicit dismissal restores focus when the focused item would be hidden. Tabbing onward reaches the intended destination. Hidden links are skipped. |
| Block scripts and reload. Use the keyboard to follow each type of destination. Where switch access is supported, repeat a representative navigation task using switches. | The baseline destinations still work through ordinary links. |
| Increase text size up to 200%. In a separate run, test page zoom up to 400%. Resize while a priority link has focus and include a layout with no priority items visible. | Destinations remain reachable, focused links stay usable and the empty priority list adds no announcement. |
| Apply reduced motion, forced colours and supported user font and colour overrides. | The header, focus and controls remain distinguishable. Opening the panel remains usable without motion. |
| Measure target sizes and contrast in the enhanced header. | For the enhanced header’s design target, aim for 44 × 44 CSS pixels. Apply the text and meaningful non-text contrast criteria in the colour guide. |

### Supplied QA scripts

- [Navigation Menu Test Criteria](qa/supplied/navigation-menu.md) — Apply only to expandable navigation, with the guide’s screen reader and keyboard expectations.

Read the [applicability and review notes](qa/Review.md) before using these scripts for release decisions.

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Button has non-empty accessible name (97a4e1)](qa/w3c-act/97a4e1.md)
- [Link has non-empty accessible name (c487ae)](qa/w3c-act/c487ae.md)

## Sources

- [Shared disclosure behaviour](Disclosure.md)
- [Navigation lists and submenu controls](Site%20Menus.md)
- [Grouped navigation panels](Mega%20Menu.md)
- [Search forms and suggestions](Search.md)
- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
- [Link has non-empty accessible name (c487ae)](https://www.w3.org/WAI/standards-guidelines/act/rules/c487ae/proposed/)
