---
title: "Share Tools"
description: "Help people share a page through a service or copy its link."
---

# Share Tools

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

Help people pass on a page using a service they choose or by copying its link. A long row of sharing controls can slow someone navigating by keyboard, and an icon alone can be hard to identify. Keep the options easy to find and easy to bypass. A selectable page link is enough if people only need the address.

## Recommended markup

Use a list of service links and a labelled read-only field for the address. Use a button to copy the address or open a panel.

## Recommended layout

<span id="presentation"></span>

- Give the group a visible heading, such as ‘Share this guide’. If the page has several groups, name each for the content it shares. Use a named `aside` if the tools are supporting content and a landmark would help people find them. See [Landmarks](Landmarks.md).
- Put service links in a list. Name each link for the action and service, such as ‘Share by email’. Prefer visible text. If you use an icon with a text label, keep the icon out of the accessible name and tab order. See [Icons](Icons.md) and [Anchors and Links](Anchors%20and%20Links.md#groups-of-external-destinations).
- Let the options wrap in source order. Leave space between controls and check that labels, borders and focus indicators remain distinguishable in forced colours. See [Colour, Contrast and Meaning](Colour%2C%20Contrast%20and%20Meaning.md#focus-and-icons-in-forced-colours).

## Recommended behaviour

<span id="behaviour"></span>

### Bypass the tools

When a long group interrupts the route to the next task, put a ‘Skip sharing’ link before its first control. If the link is visually hidden before focus, reveal it as soon as it receives focus. Target visible content immediately after the group, such as the next heading. Make the target programmatically focusable and check that the next Tab continues from there. Use unique target IDs when the page has several groups.

### Open a service or show more options

Use a link when activation opens a service. If the service opens in a new tab, explain that in the link text, such as ‘Share on Example Network (opens in a new tab)’. See [new tabs](Anchors%20and%20Links.md#new-tabs).

Use a button when activation reveals more sharing options on the current page. Keep each option in one place where practical. For a dismissible panel, follow [Information Panels](Information%20Panels.md#behaviour) for opening, closing and focus. When closing hides the focused control, return focus to the opener. Keep the page available while the panel is open.

### Copy the link

Provide a visible, labelled, read-only field containing the page address you intend people to share. Let people select and copy the full value with their usual keyboard or touch controls.

If you add a ‘Copy link’ button, copy only when someone activates it, and keep focus on the button. Confirm success in visible text only after the copy operation succeeds. If it fails, explain how to select and copy the link manually. Make both messages available without moving focus by following the [persistent inline feedback guidance](Notifications%20and%20Status%20Messages.md#persistent-inline-feedback).

## Reference implementation

<span id="implementation"></span>

### Markup example

This example shows an always-visible group and a skip destination. Replace the sample address with the page’s shareable URL and generate the email link with that address URL-encoded in the message body. The native link and selectable field work without scripts.

```html
<aside aria-labelledby="share-heading">
  <h2 id="share-heading">Share this guide</h2>
  <a href="#related-heading">Skip sharing</a>

  <ul>
    <li>
      <a href="mailto:?body=https%3A%2F%2Fexample.org%2Fguides%2Frepairs">
        Share by email
      </a>
    </li>
  </ul>

  <label for="share-address">Link to this guide</label>
  <input id="share-address" type="text"
    value="https://example.org/guides/repairs" readonly>
</aside>

<h2 id="related-heading" tabindex="-1">Related guidance</h2>
```

Keep the links and field available until any panel enhancement works. If you add a copy button, reveal it only after its action has been set up. Keep the read-only field available if clipboard access fails or scripts are blocked. See [Progressive Enhancement](Progressive%20Enhancement.md).

## Testing Approach and ACT Scripts

Test the supported variants. Record the method, browser and assistive technology where relevant, and compare the observed result with the expected behaviour.

<span id="checkpoints-for-the-tester"></span>

Use a page with a control after the share tools. Test the panel and copy button where present. Use a share address with query parameters if the service supports sharing one.

| Method and action | Expected result |
| --- | --- |
| Use a supported screen reader and keyboard to reach the group and move through each service link. Activate each link. | The group and links have clear names. Any new tab is identified before activation. The destination receives the intended page address. |
| Tab to ‘Skip sharing’, activate it and press Tab again. Repeat for each group if there are several. | The link is visible on focus. It moves focus past its own group, and the next Tab reaches the next control after it. |
| Open the sharing panel from its button. Close it with Close and Escape in separate runs. Reopen and Tab onward. | The expanded state and visible contents agree. Closing from inside restores focus to the opener. Closed controls are skipped and Tab can continue beyond the panel. |
| Tab into the address field, then select all and copy using the platform shortcut. Repeat using touch selection on a supported device. Paste into a text editor. | Focusing the field leaves the clipboard alone. Manual copying gives the full address from the field. |
| Activate ‘Copy link’ with the keyboard. Paste into a text editor. Then repeat with clipboard access blocked. | Focus stays on the button. The success message follows a successful copy. On failure, visible and announced instructions explain how to copy manually. |
| Block scripts and reload. Try the service link and copy the address manually. | The baseline sharing options are usable. Enhancement controls that cannot work are unavailable. |
| Enable forced colours. Tab through the tools. Increase text size, then separately narrow the viewport. | Labels, controls and focus remain visible. Tools wrap without hiding content or changing the tab order. |

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Link has non-empty accessible name (c487ae)](https://www.w3.org/WAI/standards-guidelines/act/rules/c487ae/proposed/)
- [Form field has non-empty accessible name (e086e5)](https://www.w3.org/WAI/standards-guidelines/act/rules/e086e5/proposed/)
- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)

<span id="related-references"></span>

## Sources

- [Use high contrast mode in Windows](https://support.microsoft.com/en-gb/help/13862/windows-use-high-contrast-mode)
- [Social Sharing Buttons: The First 10 Weeks](https://insidegovuk.blog.gov.uk/2014/02/20/gov-uk-social-sharing-buttons-the-first-10-weeks/)
- [Social Sharing Buttons Are Useless](https://www.tuliptree-studios.com/view-post/social-sharing-buttons-are-useless)
- [Removing Headaches From Focus Management](https://developers.google.com/web/updates/2016/03/focus-start-point)
- [Don't make every `<svg>` focusable](https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/8090208/)
- [Link has non-empty accessible name (c487ae)](https://www.w3.org/WAI/standards-guidelines/act/rules/c487ae/proposed/)
- [Form field has non-empty accessible name (e086e5)](https://www.w3.org/WAI/standards-guidelines/act/rules/e086e5/proposed/)
- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
