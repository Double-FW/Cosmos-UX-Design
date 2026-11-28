---
title: "Carousels and Sliders"
description: "Help people browse slides or scrolling content and distinguish those patterns from a value slider."
---

# Carousels and Sliders

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing Approach and ACT Scripts](#testing-approach-and-act-scripts)
- [Sources](#sources)

## Introduction

Use a carousel when people benefit from browsing a short sequence of related content at their own pace. Slides add controls and can conceal information from people who scan, search the page or navigate by headings. Keep essential information and actions where people can find them directly.

### Choose the pattern

| User task | Pattern |
| --- | --- |
| Browse related items in a deliberate sequence, such as illustrations that explain successive steps. | Use a manually controlled carousel that displays one slide or a defined group of slides at a time. |
| Browse a row of peer items, with more available by scrolling. | Consider a horizontal scrolling list. Use a wrapping list or grid when people need to scan or compare the items together. |
| Choose an amount, such as volume or playback position. | Use a labelled range input. See [value sliders](#value-sliders). |

Prefer one carousel in a view, and use a list, grid or visible section for other collections. Each additional carousel adds its own controls, hidden content and state to keep track of. Multiple moving sequences can also compete for attention while someone is reading or acting elsewhere. If several carousels remain, give them distinct names, keep them independent and test the whole page with keyboard and screen reader navigation. See [Making Important Information Stand Out](Making%20Important%20Information%20Stand%20Out.md).

## Recommended markup

Name the carousel and its slides. Use native buttons to move through slides and a labelled range input to choose a value.

## Recommended layout

<span id="presentation"></span>

Give the collection a descriptive heading and each slide an identifiable title or label. Make Previous and Next easy to find beside the content they change. Match their visible and accessible names so people can operate them by voice. Put related controls together and keep their order consistent.

Show position in readable text, such as ‘Slide 2 of 5’ or ‘Items 4–6 of 12’. If you add dots, distinguish the current one without relying on colour or a background alone. Decorative dots need no focus stop. If dots act as controls, give each a name that identifies its destination and expose the current state. Prefer a small set of controls and use the relevant [carousel picker pattern](https://www.w3.org/WAI/ARIA/apg/patterns/carousel/) when offering direct selection.

Keep the controls, titles and focus visible in [forced colours](Colour%2C%20Contrast%20and%20Meaning.md#focus-and-icons-in-forced-colours) and as text grows. At narrow widths, show fewer items or use a vertical list where it makes the content easier to browse. Avoid clipping text and focus indicators. See [Focus](Focus.md#focus-indication) and [Layout](Layout.md#visual-order-versus-tab-order).

## Recommended behaviour

<span id="behaviour"></span>

### Paged carousel

Use native buttons for Previous and Next. Support Enter, Space, click and tap. Offer buttons even if you add swipe or drag. For author-defined dragging, WCAG 2.2 [2.5.7 Dragging Movements](https://www.w3.org/TR/WCAG22/#dragging-movements) (Level AA) requires a single-pointer alternative without dragging unless dragging is essential. Unmodified browser scrolling has a user-agent exception. For authored path-based gestures, see [2.5.1 Pointer Gestures](https://www.w3.org/TR/WCAG22/#pointer-gestures) (Level A).

Keep focus on Previous or Next when it changes the slide. For a finite sequence, expose when the preceding or following action is unavailable. If a focused button becomes unavailable and stays in the tab order, use `aria-disabled` and suppress activation in code. If you loop, make the wrap clear in the position feedback.

Hide inactive slides visually and from the accessibility tree and tab order, for example with `hidden`. Clipping or moving a slide off-screen does not remove its controls from keyboard navigation. If a gesture or layout change would hide focused content, move focus to a visible control that still makes sense. On a user-initiated change, update one concise polite status message with the new position. Keep the content available to read at the person’s pace. See [Notifications and Status Messages](Notifications%20and%20Status%20Messages.md#persistent-inline-feedback).

### Horizontal scrolling list

Use an ordinary list in a scrollable container. Keep its items in document order and available to assistive technology. When a link or control gains focus, make it fully visible. Make the scrolling area itself keyboard operable where its content has no focusable controls. Check how the viewport is named and how people reach it and leave it. Keep a usable scrollbar or another visible cue and provide simple buttons if the scroll interaction needs them. Let people scroll natively when scripts are blocked, or let items wrap into a list.

### Automatic advance

Keep automatic advance off by default. If people can start it, put a visible Start or Stop control before the changing content in the tab sequence. Stop rotation when focus enters the carousel or a pointer hovers over it. Resume only when the person requests it. Respect reduced motion for slide transitions and keep the example content usable without motion. Avoid announcing every automatic slide change.

WCAG 2.2 [2.2.2 Pause, Stop, Hide](https://www.w3.org/TR/WCAG22/#pause-stop-hide) (Level A) applies to non-essential automatic movement presented alongside other content when it lasts more than five seconds. For non-essential automatic updates presented alongside other content, provide a way to pause, stop, hide or control update frequency. That clause has no five-second threshold. Follow [Motion and Animation](Motion%20and%20Animation.md) for the related motion guidance.

## Reference implementation

<span id="implementation"></span>

### Manual carousel example

This example shows one slide at a time, with no rotation or animation. All content starts visible. The script sets up the buttons before hiding inactive slides. The enhanced set uses a named group. Use a landmark instead when the carousel is important enough to be an entry in the page’s landmark list. When using `aria-roledescription`, localise ‘carousel’ and ‘slide’, retain the underlying roles and test how the names and roles are presented.

```html
<div id="plant-steps">
  <h2 id="plant-steps-title">Planting steps</h2>
  <div data-controls hidden>
    <button type="button" data-previous>Previous slide</button>
    <button type="button" data-next>Next slide</button>
  </div>
  <p role="status" data-position></p>

  <div data-slide>
    <h3 id="prepare-title">Prepare the pot</h3>
    <p>Fill it with damp compost.</p>
  </div>
  <div data-slide>
    <h3 id="sow-title">Sow the seeds</h3>
    <p>Follow the spacing on the seed packet.</p>
  </div>
</div>

<script>
  const carousel = document.getElementById('plant-steps');
  const previous = carousel.querySelector('[data-previous]');
  const next = carousel.querySelector('[data-next]');
  const slides = Array.from(carousel.querySelectorAll('[data-slide]'));
  const position = carousel.querySelector('[data-position]');
  let current = 0;

  function showSlide(index) {
    current = index;
    slides.forEach((slide, i) => {
      slide.hidden = i !== current;
    });
    previous.setAttribute('aria-disabled', String(current === 0));
    next.setAttribute('aria-disabled', String(current === slides.length - 1));
    position.textContent = 'Slide ' + (current + 1) + ' of ' + slides.length;
  }

  previous.addEventListener('click', () => {
    if (current > 0) showSlide(current - 1);
  });
  next.addEventListener('click', () => {
    if (current < slides.length - 1) showSlide(current + 1);
  });

  carousel.setAttribute('role', 'group');
  carousel.setAttribute('aria-labelledby', 'plant-steps-title');
  carousel.setAttribute('aria-roledescription', 'carousel');
  slides.forEach(slide => {
    slide.setAttribute('role', 'group');
    slide.setAttribute('aria-labelledby', slide.querySelector('h3').id);
    slide.setAttribute('aria-roledescription', 'slide');
  });
  showSlide(0);
  carousel.querySelector('[data-controls]').hidden = false;
</script>
```

In production, style the unavailable buttons so their condition is visible alongside their focus indicator. If content depends on find-in-page or direct links, test whether people can find hidden items and consider providing a visible index or a ‘View all’ route. See [Progressive Enhancement](Progressive%20Enhancement.md).

### Value sliders

Use a native `input type="range"` when people choose a value from a range. Give it a visible label. Set the minimum, maximum, step and initial value to match the task. Show the current value and its unit where they help. Use `aria-valuetext` when a number alone would be unclear, and update it alongside the numeric input value. See [Video Controls](Video%20Controls.md#seek-through-the-video) for readable time on a media slider.

Let people change the value with the keyboard and by click or tap without dragging. Offer a labelled numeric input or step buttons where exact entry matters or the slider is difficult to operate with supported touch assistive technology. Use [Tabs](Tabs.md) if the interaction really is selecting a named content panel from a set of tabs.

## Testing Approach and ACT Scripts

Test the supported variants. Record the method, browser and assistive technology where relevant, and compare the observed result with the expected behaviour.

<span id="checkpoints-for-the-tester"></span>

Use a representative page with a focusable control before and after the collection. Include a slide with a link or button and test a multi-item layout where supported.

| Method and action | Expected result |
| --- | --- |
| Tab into the carousel. Activate both directions with Enter and Space in separate runs. Reach both ends, then Tab and Shift+Tab through the page. | Focus stays visible and moves predictably. Inactive slide controls are skipped. Unavailable actions are identified and do nothing. |
| With a supported screen reader, find the collection, change slides and browse the result. Try each button by its visible label using supported voice control. | Names identify the collection, slides and buttons. Position is available without duplicate announcements. Voice activation has the same effect as a click. |
| Repeat by touch. If a swipe or drag is available, also change slides by tapping the controls without a gesture. | Both directions work without dragging. Controls and visible slide content are usable. |
| If rotation is offered, start it, enter by keyboard and repeat by hovering. Leave, then restart explicitly. Repeat with reduced motion enabled. | Rotation is off initially and stops on focus entry or hover. It stays stopped until requested. The content remains usable without non-essential motion. |
| Enlarge text to 200%, then separately narrow the viewport. Enable forced colours. | Text and controls remain usable. Focus and current position are distinguishable. A scrolling list reveals focused items. |
| Block scripts and reload. In a separate run, restore scripts and disable CSS. Test find-in-page and any ‘View all’ route. | All content is reachable through the baseline or equivalent route. Behaviour remains understandable without styling and people have a reliable way to find the items. |
| Where several carousels are present, read and use the surrounding page, then operate each carousel. | The collections are distinguishable. Each control affects its own collection. Essential information stays available without traversing each sequence. |
| For a value slider, use the keyboard, tap without dragging and check the output with a supported screen reader, including touch where relevant. | Value, units and limits make sense. The displayed value follows the control, and any alternative input changes the same setting. |

### Supplied QA scripts

- [Carousel Test Criteria](qa/supplied/carousel.md).

Read the [applicability and review notes](qa/Review.md) before using these scripts for release decisions.

### Related W3C ACT rules

Apply these proposed rules where their applicability conditions are met. Use the pattern checks above to verify behaviour and outcomes beyond their scope.

- [Button has non-empty accessible name (97a4e1)](qa/w3c-act/97a4e1.md)
- [Scrollable content can be reached with sequential focus navigation (0ssw9k)](qa/w3c-act/0ssw9k.md)

## Sources

- [W3C ARIA Authoring Practices Guide: Carousel](https://www.w3.org/WAI/ARIA/apg/patterns/carousel/)
- [W3C ARIA Authoring Practices Guide: Slider](https://www.w3.org/WAI/ARIA/apg/patterns/slider/)
- [WCAG 2.2 Pause, Stop, Hide](https://www.w3.org/TR/WCAG22/#pause-stop-hide), [Pointer Gestures](https://www.w3.org/TR/WCAG22/#pointer-gestures) and [Dragging Movements](https://www.w3.org/TR/WCAG22/#dragging-movements)
- [Button has non-empty accessible name (97a4e1)](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/proposed/)
- [Scrollable content can be reached with sequential focus navigation (0ssw9k)](https://www.w3.org/WAI/standards-guidelines/act/rules/0ssw9k/proposed/)
