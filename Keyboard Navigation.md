---
title: Keyboard Navigation
description: "Ensure web pages are keyboard accessible."
---

## Summary

Although there are many different types of keyboards (or equivalent [switch devices](https://en.wikipedia.org/wiki/Switch_access)) that people can use to navigate web pages, from an implementation point of view, we mainly need to consider the following groups:

1. People who cannot see the screen and who use the keyboard to operate a screen reader — like NVDA, JAWS, or VoiceOver — which reads the content of the screen aloud.
2. All other keyboard users, meaning users who _can_ at least partly see the screen, who may or may not use a screen reader to augment their experience, but who depend upon visual cues displayed on the screen to navigate. 

The [Web Content Accessibility Guidelines 2.0](https://www.w3.org/WAI/WCAG21/Understanding/keyboard.html) requires web page providers "ensure that, wherever possible, content can be operated through a keyboard or keyboard interface."

## Purpose

Users will have a wide range visual and motor/coordination abilities which could make using a mouse or touchpad difficult or impossible. These users will be excluded from accessing your web page if keyboard navigation is not accessible. On the other hand some users just prefer the convenience and speed with which they can use keystrokes to move around a web page. Excluding these users will frustrate and annoy them.

A [WebAIM survey of screen reader users](https://webaim.org/projects/screenreadersurvey7/#problematic) found that a significant portion of users indicated that "a lack of keyboard accessibility" was a problem for them.  Access can includes using forms, navigational menus, interactive widgets, pop ups, and alerts, and any other type of component on a page. It is required too that keyboard access cannot not be timing restricted, meaning that the web page must allow the user to navigate and operate any content or interactive components via keyboard at whatever pace the user requires or prefers. 

## Checks

It is assumed that the following checks will be run in each of the modes described in the summary for this section: using a screen reader to navigate the web page under test via keyboard without seeing the screen, and using the keyboard without a screen reader and looking at the screen.

For each web page under test, perform the following checks. Record an answer, "yes" or "no" to each question. The passing result is indicated in uppercase. For each failing result, document it either as a defect or else provide an explanation of why it should be allowed as an exception or why it is not applicable.

### Checks which should be (at least partly) performed manually

- Is it possible to uses the Tab key to move forward and the Shift+Tab key combination to move the focus such that each link and control can be accessed and used without a mouse or touchscreen? [Y/n]
- As you move focus, it it clearly visible which element on the web page currently has focus, for example by a consistent, visibly contrasting outline? [Y/n]
- As you move focus, does each element that receives focus follow a logical, predictable order, moving from top to bottom and left to right (for web pages published in English and other left-to-right languages)? [Y/n]
- For interactive components, including form elements, is it possible to navigate and operate each element including setting any possible options, using the arrow keys, Enter key, Space key, etc? [Y/n]
- For complex components that automatically move focus when new content opens or appears, does focus move back to the original element once that content closes? [Y/n]
- For content that is visually hidden when the web page loads, does that content become visible when keyboard focus moves into the element?

## Additional Testing Methods and Strategies

### Activating on Safari

For the Mac Safari web browser, the ability to navigate a web page with the Tab key is not enabled by default. Use the menu: Settings → Advanced → Accessibility and enable the "Press Tab to highlight each item on a web page" option.

### Visible focus order

When navigating by Tab key, you should see an obvious indication of which element currently has focus, as you move across each of the focusable elements on the page in the order that they are defined in the source code for the web page which may not necessarily match the visual order as it appears on the page. If CSS or JavaScript is used to move or change the appearance of focusable elements this must be implemented so as not to confuse a user expecting focus to move as they would read the same content, from top to bottom and left to right as much as possible.

A common way to break the above expectation is my misapplying a `tabindex` attribute. Any element with a `tabindex` attribute set to any value higher than zero is very likely to break this test.

```html
<!-- DO NOT use a positive number for any tabindex attribute -->
<a href="#one">One</a>
<a href="#two">Two</a>
<a href="#three" tabindex=1>Three</a>
```

In the above example, when navigating by Tab key, the focus will move from: Three, to One, and then Two. This should be considered a defect since the visual layout (as it is here) would lead the user to expect focus to move from: One, Two, and then Three.

Finding elements with such `tabindex` setting is made obvious by using the Web Developer Browser Extension: enable the option: Information → Display Tab Index. Note any element with a `tabindex` attribute set to any value higher than zero.

### Focusable Control Elements

- Link: the Enter key should activate/follow the link	
- Button: the Enter or Space key should activate / press the button
- Checkbox / Radio Button: the Arrow keys should move the highlighted item, the Space key should check or uncheck the highlighted item
- Select (dropdown): the Arrow keys should move highlighted item, the Enter key should select the highlighted item
- Dialog: Esc key should close the dialog

### Keeping Focus Visible

Web browsers are already accessible when it comes to indicating which element on a web page has focus by adding a visible outline. By using CSS to style of that focus, the indicator can be customised or even turned off. Any missing or hard-to-see focus indicator should be considered a defect. (Note that using color-change alone to indicate focus would be inaccessible to users who can not perceive color.)

```css
/* DO NOT just disable features needed by keyboard navigators */
:focus { outline: none; }
```

### Visible on focus

Content that is visually hidden on the screen but which can receive focus via keyboard navigation, should be authored so that the content becomes visible once it has focus. An example of this would be a "Skip to Main Content" link, intended to allow keyboard users quickly hop over all the navigational content at the top of every page, and continue on with the main content.

For this technique to work the skip link must be on the web page and available in the usual focus order, but visually hidden until it receives focus (one example is shown below).

```css
.visually-hidden {
  position:absolute;
  left:-10000px;
  top:auto;
  width:1px;
  height:1px;
  overflow:hidden;
}
 
.visually-hidden.show-on-focus:focus-within {
  position:static;
  width:auto;
  height:auto;
}
```

```html
<p class="visually-hidden show-focused">
  <a href="#main">Skip to content</a>
</p>
```

### Keyboard shortcuts

While there are many well-known keyboard shortcuts defined in desktop applications, this isn't the case with the myriad different web pages that exist. While it is possible to use the `accesskey` attribute to define custom shortcut keys, this is not likely to increase accessibility if users can't predict or learn about them in an obvious way. And defining such shortcuts keys could even make things worse if they conflict with built-in standard shortcuts defined in the web browser or assistive technology.

### Avoiding Keyboard Traps

A "keyboard trap" exists whenever a feature on a webpage prevents the user from moving away from a focused element. This shouldn't happen of course, but poorly implemented JavaScript can sometimes alter the way keyboard focus normally works so as to be broken in this way if the web page author is not careful. Obviously this is considered a defect, since it breaks navigation entirely for a class of users. 

### Keyboard Usage on Mobile Devices

A [study conducted by WebAIM](https://webaim.org/projects/screenreadersurvey7/#mobilekeyboard) found that more than 40% of respondents who use a mobile screen reader use an external keyboard at least some of the time. This is certainly true with modern tablet computers that have soft keyboards built into their screen cover or available as a bluetooth add on. Additionally many disabled users connect [commercially available switch devices](https://www.atandme.com/?p=3758) to their mobiles or tablets that emulate a keyboard, allowing them access to the full functionality of a web page with only a press. The point is not to assume that just because a user may be on a device that has a touchscreen, that you do not need to support keyboard navigation.
