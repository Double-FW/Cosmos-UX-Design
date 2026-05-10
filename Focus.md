---
title: Focus
description: Presenting and managing focus in an accessible way.
---

## Keyboard navigation

Some users find navigating and interacting with a webpage via a keyboard easier than operating a mouse to control an onscreen pointer. Very experienced users, for example, may find keyboard navigation more efficient than a mouse, but for people with motor-control disabilities, such as hand tremors, joint injury or disease, or limb differences, using a mouse may be difficult or impossible.

People using alternative input devices such as a switch control [^1] or voice-controlled input may also act as "keyboard navigators" when those devices are configured to translate their input into keyboard combination equivalents.

Whereas a person controlling an onscreen cursor will move that cursor around a page freely and activate webpage controls by clicking the buttons on their mouse, a keyboard navigator uses key combinations or swipe gestures to advance a digital focus state (via a virtual cursor) along an ordered series of focusable elements encoded into the webpage. By default, focusable elements are links, buttons, details elements, audio and video controls, elements with a `contenteditable` attribute, and form controls: any element the user needs to interact with or provide input to. Applying these elements semantically is always the simplest way to ensure that focus works and works as expected.

## Tab order

A simple way to easily move focus with a keyboard is by enabling tab-key navigation. [^2] Use the <kbd>tab</kbd> key to move forward and the <kbd>shift</kbd>+<kbd>tab</kbd> combination to move backwards. A visible onscreen focus indicator should make it clear which element currently has focus; it should move in an expected direction, from left-to-right and top-to-bottom (RL-TB) for readers of RL-TB languages.

You can use a browser extension to visualize the focus order of elements on your webpages quickly. [^3] In the example below, it's apparent that a hidden element (the Close button of an unopened menu) is inadvertently in the focus order between the last navigation link and the search form.

{% figure "A keyboard navigator will need to press the TAB-key eight times to advance focus to the \"Search\" element." %}
<img class="figure-img img-fluid" src="/static/img/focus/taborder-numbered.jpg" alt="">
{% endfigure %}

### Changing the order

Changing the natural tab order of focusable elements is possible using the HTML `tabindex` attribute. Below are some types of values you can use:

- `tabindex="0"`: When applied to non-focusable elements, it allows them to become focusable as if they were a focusable element and part of the natural tab order of the webpage.
- `tabindex="-1"`: When applied to non-focusable elements, it allows them to become focusable via JavaScript's `.focus()` method.

Setting the `tabindex` to any number other than 0 or -1 sets that number as the explicit tab order of the element on the webpage. This creates confusion and barriers for users who can see the screen but navigate by the keyboard. Any element with a positive `tabindex` will receive focus _before_ other focusable elements with no `tabindex` value or a `tabindex` of 0. This likely results in a keyboard tab order different from what is visibly displayed.

### Keyboard traps

Keyboard traps occur when a user can move focus into an element on a webpage via the keyboard (or focus moves into that element automatically), but the user can not exit that element via the same mechanism.

> "If keyboard focus can be moved to a component of the page using a keyboard interface, then focus can be moved away from that component using only a keyboard interface... " [^8]

A typical example where focus movement should be intentionally limited is when presenting a modal dialog. Programmatically moving focus to the dialog and requiring the user to respond, for example, by activating an "I agree to the legal terms" button before continuing, is an appropriate pattern in some cases. However, it must be possible for the user to continue using only the keyboard.

## Focus indication

> "Any keyboard operable user interface has a mode of operation where the keyboard focus indicator is visible." [^4]

A visible indication of which element currently has focus is required to prevent barriers for users who can see the screen but rely on the keyboard to navigate. For this reason, you must not hide or disable the browser's ability to display a visible focus indication, typically based on the CSS `outline` property.

{% aside "aside-info" %}
The CSS `outline` property is separate from the `border` property. An element's outline is drawn _outside_ its border and is allowed to overlap nearby elements. For this reason, it is not computed as part of an element's height or width.
{% endaside %}

### Default styles

Unless you actively disable the CSS `outline` property, the browser will display a default indicator to show which element currently has focus; each browser is free to use whatever color and style they choose.

{% figure "Here the navigation link \"News and Media\" has focus in three different web browsers. This shows the visual differences of the default focus indicator among those browsers: Microsoft Edge, Apple Safari, and Mozilla Firefox." %}
<img class="figure-img img-fluid" src="/static/img/focus/focus-indicator-default.jpg" alt="">
{% endfigure %}

 You might think the browser knows best when it comes to accessibility, but this responsibility belongs to the webpage designer. This is true for two reasons. Firstly the default style won't consistently match the brand and aesthetic of the rest of the page. Secondly, the usefulness of a focus indicator is in how perceivable it is: if it cannot be seen among the other elements on the page, it is effectively useless.

 The examples below show you linked images in a focused state: they both have visible focus indicators applied, but one is very difficult to see due to the color of the element it is applied to.

{% figure "Comparing the visibility of Microsoft's Edge default focus indicator, a thin black outline around the linked image, versus a custom styled outline styled in gold and offset slightly from the image." %}
<img class="figure-img img-fluid" src="/static/img/focus/focus-default-custom.jpg" alt="">
{% endfigure %}

The focus outline must have sufficient contrast with the surrounding elements to stand out and be easily found by the user. The standard for an accessible style is generally: [^5]

- It must display a solid outline that encloses the focused element, or the indicator must have an area equivalent to a 4px thick line along the shortest side of the focused element
- It must contrast at least 3:1 when changing from the focused to unfocused state, or the indicator must have a thickness of no less than 2px
- It must contrast at least 3:1 against adjacent colors

A thin black outline (the default in Microsoft Edge) around a dark image would be difficult for most people to spot. One way to use a more deliberate custom style with greater contrast is shown below:

```css
a:focus img {
  outline-color: goldenrod;
  outline-style: solid;
  outline-width: 3px;
  outline-offset: 2px;
}
```

## Visible when focused

Be aware that merely styling a focusable element so it is no longer visible, will not automatically make it unfocusable. For example, suppose an element is positioned off-screen and marked with `aria-hidden="true",` making it not visible on the screen and unavailable to the accessibility API. In that case, moving focus to that element or any focusable elements within it may still be possible. This will confuse sighted users, who will only perceive that the focus indicator suddenly disappears when it moves to an off-screen element.

One solution is to ensure that it becomes visible when the hidden element receives focus. So-called [skip links](https://webaim.org/techniques/skipnav/) are commonly implemented using this pattern. In the [TailWind CSS](https://tailwindcss.com/docs/screen-readers) framework, this can be achieved by adding two screen reader only classes:

```html
<div tabindex="0" class="sr-only focus:not-sr-only focus-within:not-sr-only">
  I become visible when I get focus.
  <a href="#content">Skip to Content</a>
</div>
```
{% demoiframe
   title="visible on focus.",
   head="<link href=
\"https://unpkg.com/tailwindcss@^2/dist/tailwind.min.css\"
          rel=\"stylesheet\">"
%}
<div style="min-height:10em">
  Use the <kbd>tab</kbd> key to navigate focus through this element to find content that only becomes visible when it has focus.
  <div tabindex="0" class="sr-only focus:not-sr-only focus-within:not-sr-only">
  I become visible when I get focus.
  <a href="#content">Skip to Content</a>
</div>
</div>
{% enddemoiframe %}

If you want to hide a focusable element from keyboard focus temporarily, you can add an HTML `tabindex="-1"` attribute to them when they become hidden and remove it when they become visible again.

## Not showing focus

Generally speaking, you shouldn't prevent a focus indicator from being shown; however, there is an exception where showing the focus indicator might be considered unexpected. Modern web browsers will only show their default focus indicator on a button or link when the user moves focus to it with the _keyboard_; they do not apply the indicator when a user clicks a button using a mouse, for example. If you set your own `outline` styles, however, you may be surprised to see the focus indicator now appears regardless of how the focus is moved to a button or link. You can imitate the browser's native convention by adding a CSS rule to hide your outline whenever the browser would not usually show an outline:

%source(src/demos/foundations/focus/focus.css)

```html
<button class="fcs">A button with styled focus</button>
<a class="fcs" href="#">A link with styled focus</a>
```

In the demonstration below, the button with default focus style and the button with custom focus styles should now behave the same when receiving focus by mouse or keyboard. This is accomplished with the somewhat convoluted `:focus:not(:focus-visible)` selector. To explain, the `:focus-visible` pseudo-class [^6] matches when an element has focus, and the browser determines that it will show a focus indicator. When combined with `:focus` and `:not` pseudo-classes, the entire selector will apply whenever an element has focus, but the browser determines it will _not_ show a focus indicator. In that case, we set the outline color to `transparent`. [^7]

{% demoiframe
   title="Button focus.",
   head="<link rel=\"stylesheet\" href=\"/demos/foundations/focus/focus.css\">"
%}
<p id="demo1"><button class="defaultfcs">Default focus button</button> <a class="defaultfcs" href="#demo1">Default focus link</a></p>
<p id="demo2"><button class="fcs">A button with styled focus</button> <a class="fcs" href="#demo2">A link with styled focus</a></p>
{% enddemoiframe %}

## Consider High Contrast Mode

The `outline` CSS property is the most reliable way to apply a focus indicator style. Other CSS properties like `background-color` or `box-shadow` should be avoided as ways to indicate focus because they may be removed if a user has [Windows High Contrast Mode](https://www.smashingmagazine.com/2022/06/guide-windows-high-contrast-mode/#about-windows-high-contrast-mode) activated.

## Checkpoints for the Tester

1. Unsure that all interactive elements on the page, including links, buttons, form fields, and menus, can all be accessed using only the keyboard. For example, use the <kbd>tab</kbd> key to skip forward to the next focusable element, the <kbd>shift</kbd>+<kbd>tab</kbd> keys to move backward, and the <kbd>enter</kbd> or <kbd>space</kbd> keys to activate them. You may need to use the arrow keys to move amongst groups of elements, like radio-button elements or menu items.
2. Ensure that each interactive element on the page shows a visible focus indicator (such as a colored outline ring) that highlights the element when it receives keyboard focus. Check that the focus state is clearly visible and has adequate contrast to be distinct from the surrounding content.
3. Ensure the keyboard navigation order follows a logical and intuitive structure. The keyboard navigation order should track the visual order of the content and not jump unexpectedly or skip any interactive elements.
4. Ensure that it is not possible to focus on an element that is not visible once it has focus.

[^1]: Video, Christopher Hill, [YouTube](https://www.youtube.com/watch?v=--Jakgk3tpA)
[^2]: [Use your keyboard like a mouse with Mac](https://support.apple.com/en-gb/guide/mac-help/mchlp1399/mac), macOS User Guide
[^3]: The example uses the [Taba11y](https://chrome.google.com/webstore/detail/taba11y/aocppmckdocdjkphmofnklcjhdidgmga?hl=en) web extension for Chrome.
[^4]: [_Understanding SC 2.4.7:Focus Visible (Level AA)_](https://www.w3.org/WAI/WCAG21/Understanding/focus-visible.html), WCAG 2.1
[^5]: [_Understanding Success Criterion 2.4.11: Focus Appearance (Minimum)_](https://www.w3.org/WAI/WCAG22/Understanding/focus-appearance-minimum), WCAG 2.2
[^6]: The `:focus-visible` pseudo-class has [very good browser support](https://developer.mozilla.org/en-US/docs/Web/CSS/:focus-visible#browser_compatibility).
[^7]: Using a `transparent` "color" for the outline instead of simply removing it means it will [still work in Windows High Contrast Mode](https://piccalil.li/quick-tip/use-transparent-borders-and-outlines-to-assist-with-high-contrast-mode/).
[^8]: [_No Keyboard Trap (Level A)_](https://www.w3.org/WAI/WCAG21/Understanding/no-keyboard-trap.html), WCAG 2.1
