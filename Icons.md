---
title: Icons
description: When used judiciously, icons can improve usability and accessibility in webpages.
---

# Icons

Icons are an effective tool to emphasize and clarify your content. Used judiciously they can improve usability and accessibility for those with reading difficulties, visual impairments, neuro-cognitive differences or people who may not be adept with the language. However these benefits must be delivered in such a way so as to avoid creating barriers for blind users and others who rely upon screen reader (or braille display) devices. For the use of ARIA with decorative graphics and accessible names, see [ARIA and Screen Reader Usability](ARIA%20and%20Screen%20Reader%20Usability.md) and [Visually Hidden Content](Visually%20Hidden%20Content.md).

## Use the SVG format

The options for which format to use when including icons really comes down to either an icon font or using SVG graphics. Other possibilities, like using rasterized images, are always a worse option since they are less flexible and do not scale without looking janky. Both SVGs and icon fonts allow for sharp scaling but of the two there are more downsides to using icon fonts.

Users may prefer or need to override the fonts specified in a webpage. This is common enough (e.g. it will happen whenever a page is viewed in "reader mode") that it should be expected and the resulting confusion avoided. Also, since icon fonts are in fact _fonts_, browsers will apply the same anti-aliasing and font-smoothing algorithm to them as they do to any other text. SVGs however are understood to be graphical elements.

Depending on how you include the SVG, the options for animation and positioning are also much greater with SVGs than icon fonts.

### SVG as `img`

Browsers have good support for using SVG graphics as `src` documents for our old friend the HTML `img` element. 

```html
<a href="#accessibility" style="font-size:120%">
  <img class="icon icon-inline icon-dark" src="icon-accessibility-symbol.svg" alt="">Accessibility settings
</a>
```

> Screen reader: "link, Accessibility settings"

The HTML `img` element is a very straightforward method of including an SVG. Including SVG data in the HTML itseld offers more options, for example an `img`-based icon cannot be styled to match the `currentColor`. Images however can match the relative size of the surrounding text, based on the rules in our `.icon` class (see below).

Notice that the image element has an `alt=""` attribute. This is because we are treating the icon as purely decorative since the adjacent text already provides the content and meaning of the link.

While this is a usable solution, if you want to have more flexibility for changing the color of your icons or applying animations, use the `svg` element for icons.

### Controlling the color of SVGs

Using CSS to control the color of individual components in an `svg` element is very flexible and relatively straightforward. It is possible however to affect the color of an `img` element that uses an SVG graphic as its source by using a CSS filter. The style rule below for example, when added to an `img` element, will modify the color of a black SVG to green.

```html
<img class="icon icon-inline" src="icon-accessibility-symbol.svg" alt="Accessibility">
<img class="icon icon-inline filter-green" src="icon-accessibility-symbol.svg" alt="Accessibility">
```

```css
.icon-inline {
  inline-size: 1em;
  block-size: 1em;
  vertical-align: text-top;
  fill: currentColor;
}
```

Use this rule for an inline, filled icon. For a line icon, use `stroke: currentColor` and preserve its intended fill. Check the result after text resizing and in forced colours.

### SVGs that adapt to colour changes

Use `currentColor` for the intended fills or strokes of a single-colour inline SVG. Where an illustration or chart uses several colours, assign them by purpose and test the combinations in each supported theme. If colours become similar under an override, direct labels, line patterns and marker shapes should still distinguish the data. Keep any necessary values available in text or a [data table](Data%20Tables.md).

This simplified line chart uses two line styles and two marker shapes. The graphic keeps those distinctions when both series use the same colour. The caption gives the comparison in text.

```html
<figure>
  <svg role="img" aria-labelledby="travel-trend-title"
       width="160" height="80" viewBox="0 0 160 80">
    <title id="travel-trend-title">Travel trends, May to June</title>
    <g fill="none" stroke="currentColor" stroke-width="3">
      <path d="M20 55 L140 20"></path>
      <path d="M20 25 L140 60" stroke-dasharray="6 5"></path>
    </g>
    <g fill="currentColor">
      <circle cx="20" cy="55" r="4"></circle>
      <circle cx="140" cy="20" r="4"></circle>
      <rect x="16" y="21" width="8" height="8"></rect>
      <rect x="136" y="56" width="8" height="8"></rect>
    </g>
  </svg>
  <figcaption>
    From May to June, walking trips rose (solid line, round markers)
    and cycling trips fell (dashed line, square markers).
  </figcaption>
</figure>
```

Check inline SVGs and any SVGs loaded as images in the default theme, dark theme and forced colours. An SVG loaded as an image needs its own suitable artwork or asset variant. Keep `forced-color-adjust: none` for cases where preserving the colour itself is essential, such as a swatch. Apply it narrowly and follow the [swatch guidance](Colour%2C%20Contrast%20and%20Meaning.md#when-the-colour-is-the-content).

## Managing SVG icons

SVG icon definitions, the mathematical points, paths, strokes, and fill data that tell the browser how to draw the icon, must be made available in your HTML document. You can inline this data _in situ_ each time you wish an icon to be shown.  I personally prefer to define all the icon definitions I intend to use together in one place, and then refer to those definitions whenever I want one to be shown. For example, this SVG icon set includes only two icon symbols representing the accessibility icon and a popular social media brand, but it could logically have any number of named symbols.

```html
<a href="https://example.com/accessibility" style="color:rebeccapurple">
  <svg aria-hidden="true" focusable="false" role="img" class="icon icon-inline">
    <use href="#icon-accessibility" xlink:href="#icon-accessibility"></use>
  </svg>Accessibility Settings
</a>
```

```html
<span class="visually-hidden" aria-hidden="true">
  <svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
    <defs>
        <symbol id="icon-accessibility" viewBox="0 0 32 32">
            <title>Accessibility</title>
            <path d="M13 3a3 3 0 1 1 6 0 3 3 0 0 1-6 0z" />
            <path d="m20 10 10.3-4.443-.743-1.857L17 8h-2L2.443 3.7 1.7 5.557 12 10v8L7.898 31.268l1.87.709L15.572 19h.857l5.804 12.977 1.87-.709L20.001 18z" />
        </symbol>
    </defs>
  </svg>
</span>
<a href="https://example.com/accessibility" style="color:rebeccapurple">
  <svg aria-hidden="true" focusable="false" role="img" class="icon icon-inline">
    <use href="#icon-accessibility" xlink:href="#icon-accessibility"></use>
  </svg>Accessibility Settings
</a>
```

> Screen reader: "link, Accessibility settings"

SVG data is readily compatible with web component-based frameworks like React. For a small number of icons, you could export each icon individually like so:

```js
export const AccessibilityIcon = () => {
  return (
    <svg
      aria-hidden="true"
      role="img"
      focusable="false"
      viewBox="0 0 32 32"
      xmlns="http://www.w3.org/2000/svg"
    >
      <path d="M13 3c0-1.657 1.343-3 3-3s3 1.343 3 3c0 1.657-1.343 3-3 3s-3-1.343-3-3z"></path>
      <path d="M20 10l10.3-4.443-0.743-1.857-12.557 4.3h-2l-12.557-4.3-0.743 1.857 10.3 4.443v8l-4.102 13.268 1.87 0.709 5.804-12.977h0.857l5.804 12.977 1.87-0.709-4.102-13.268z"></path>
    </svg>
  )
}
```

```html
<AccessibilityIcon />
```

Open source tools like [SVGR](https://github.com/gregberge/svgr) or [unplugin-icons](https://github.com/antfu/unplugin-icons) make it convenient to automatically transform SVG formatted graphics into React icon components.

## Icons and accessible names

HTML elements having the role of link, button, label, columnheader, and others  must be provided text content to act as the "accessible name" of the element.

A common example where this becomes important is when an icon is used as the only content of a link. A screen reader encountering such a link will, by default, only announce "link" followed by whatever the `href` attribute value is: hardly useful!

```html
<a href="/">Home</a>
<a href="/"><img src="icon-home.png" alt="Home"></a>
```

By default the accessible name of a link is the text content of the element. In the first link shown the name is the text \"Home\". Since the second link shown uses an `img` icon the `alt="Home"` attribute provides the accessible name instead

## Providing text equivalents

Icons are graphical, so their meaning can't be perceived by blind or visually impaired users who rely upon assistive technology like text-to-speech or braille displays, unless equivalent text is also provided.

The best supported and most straightforward technique for providing equivalent text to an icon is to treat the icon as if it were purely decorative and use visible text to supply meaning. The visible text will naturally be supported by every user-agent, consistently available to every screen reader and to sighted users as well. In particular this has the benefit of increasing understandability for sighted users who may not recognise the meaning of an icon alone: users with cognitive, nuerodivergent, or socio-cultural differences for example.

Examples of ISO-7000 icons that may be confusing to you initially if there were no accompanying text to explain what these graphical symbols represent (from left to right): Washing solution, Automatic (engine) transmission, and Disinfectant for floor and walls.

The following CSS visually styles the icons used in the demonstrations below, but this can be adjusted to suit your own designs. The `svg` elements are styled to adapt fully with their surrounding text: the `fill` and `stroke` properties are set to match whatever the `currentColor` is, and the `width` and `height` are measured in the relative unit `em` which will allow the icon to naturally resize along with the surrounding text.

In the example below the icon is assigned the `aria-hidden="true"` attribute because while we want the icon to be visible, it is not necessary for screen reader users to understand the meaning of the link: the text already provides that meaning. Similarly no SVG `title` element or other alternative text accommodation is needed either.

```html
We're on the socials! 
<a href="#toot">
  <svg aria-hidden="true" focusable="false" role="img" class="icon icon-inline">
    <use href="#icon-mastodon" xlink:href="#icon-mastodon"></use>
  </svg>Toot us on Mastodon
</a>
```

```html
<span class="visually-hidden" aria-hidden="true">
<svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
<defs>
<symbol id="icon-mastodon" viewBox="0 0 32 32">
<title>Mastodon</title>
<path d="M30.924 10.505c0-6.941-4.548-8.976-4.548-8.976C24.083.476 20.144.033 16.055 0h-.101c-4.091.033-8.027.476-10.32 1.529 0 0-4.548 2.035-4.548 8.976 0 1.589-.031 3.491.02 5.505.165 6.789 1.245 13.479 7.521 15.14 2.893.765 5.379.927 7.38.816 3.629-.2 5.667-1.296 5.667-1.296l-.12-2.633s-2.593.817-5.505.719c-2.887-.099-5.932-.311-6.399-3.855a7.069 7.069 0 0 1-.064-.967v-.028.001s2.833.693 6.423.857c2.195.1 4.253-.129 6.344-.377 4.009-.479 7.5-2.949 7.939-5.207.689-3.553.633-8.676.633-8.676zm-5.365 8.946H22.23v-8.159c0-1.72-.724-2.592-2.171-2.592-1.6 0-2.403 1.035-2.403 3.083v4.465h-3.311v-4.467c0-2.048-.803-3.083-2.403-3.083-1.447 0-2.171.873-2.171 2.592v8.159H6.442v-8.404c0-1.719.437-3.084 1.316-4.093.907-1.011 2.092-1.528 3.565-1.528 1.704 0 2.995.655 3.848 1.965l.828 1.391.829-1.391c.853-1.311 2.144-1.965 3.848-1.965 1.472 0 2.659.517 3.565 1.528.877 1.009 1.315 2.375 1.315 4.093z"></path>
</symbol>
</defs>
</svg>
</span>
We're on the socials! 
<a href="#toot">
<svg aria-hidden="true" focusable="false" role="img" class="icon icon-inline">
<use href="#icon-mastodon" xlink:href="#icon-mastodon"></use>
</svg>Toot us on Mastodon
</a>
```

> Screen reader: "We're on the socials!"
> Screen reader: "link, Toot us on Mastodon"

Be careful not to provide regular text _and_ alternative text. In the previous example the `aria-hidden="true"` and `focusable="false"`  attributes are used to keep the icon graphic out of the way of screen readers and keyboard navigation, and ensures that screen reader users don't hear confusing and redundant information like "link, Mastodon Toot us on Mastodon".

## Using hidden text

In some cases the design requires that no visible text appear with the icon: a simple row of social media logos that link to different accounts in the footer for example. Understand that removing the visible text will always make an icon less accessible, but for users of assistive technology like screen readers, the text equivalent **must** still be supplied.

The range of techniques for supplying alternative text for an SVG icon includes using one of several ARIA attributes and combines SVG `title` and `desc` elements. None of these has perfect support in every combination of device, web browser, and assistive technology however.

A simpler approach  is to continue to supply equivalent text alongside the icon but to style that text so that it is visually hidden (whilst still available to screen readers). This has the advantage that if the browser is in a state where it cannot apply CSS style sheets, the visually-hidden text will very helpfully become visually visible again. In these examples the `visually-hidden` class is used to accomplish this. There are accessible definitions of this class in many CSS frameworks, like [Tailwind](https://tailwindcss.com/docs/screen-readers) and [Bootstrap](https://getbootstrap.com/docs/5.2/getting-started/accessibility/#visually-hidden-content).

```html
<a href="https://example.com/toot">
  <svg aria-hidden="true" focusable="false" role="img" class="icon">
    <use href="#icon-mastodon" xlink:href="#icon-mastodon"></use>
  </svg><span class="visually-hidden">Toot us on Mastodon</span>
</a>
```

```html
<span class="visually-hidden" aria-hidden="true">
<svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
<defs>
<symbol id="icon-mastodon" viewBox="0 0 32 32">
<title>Mastodon</title>
<path d="M30.924 10.505c0-6.941-4.548-8.976-4.548-8.976C24.083.476 20.144.033 16.055 0h-.101c-4.091.033-8.027.476-10.32 1.529 0 0-4.548 2.035-4.548 8.976 0 1.589-.031 3.491.02 5.505.165 6.789 1.245 13.479 7.521 15.14 2.893.765 5.379.927 7.38.816 3.629-.2 5.667-1.296 5.667-1.296l-.12-2.633s-2.593.817-5.505.719c-2.887-.099-5.932-.311-6.399-3.855a7.069 7.069 0 0 1-.064-.967v-.028.001s2.833.693 6.423.857c2.195.1 4.253-.129 6.344-.377 4.009-.479 7.5-2.949 7.939-5.207.689-3.553.633-8.676.633-8.676zm-5.365 8.946H22.23v-8.159c0-1.72-.724-2.592-2.171-2.592-1.6 0-2.403 1.035-2.403 3.083v4.465h-3.311v-4.467c0-2.048-.803-3.083-2.403-3.083-1.447 0-2.171.873-2.171 2.592v8.159H6.442v-8.404c0-1.719.437-3.084 1.316-4.093.907-1.011 2.092-1.528 3.565-1.528 1.704 0 2.995.655 3.848 1.965l.828 1.391.829-1.391c.853-1.311 2.144-1.965 3.848-1.965 1.472 0 2.659.517 3.565 1.528.877 1.009 1.315 2.375 1.315 4.093z"></path>
</symbol>
</defs>
</svg>
</span>
<a href="https://example.com/toot">
  <svg aria-hidden="true" focusable="false" role="img" class="icon">
    <use href="#icon-mastodon" xlink:href="#icon-mastodon"></use>
  </svg><span class="visually-hidden">Toot us on Mastodon</span>
</a>
```

> Screen reader: "link, Toot us on Mastodon"

## Icons in buttons

The recommendations and caveats above can be applied to icons used as button content too: treat the icon as decorative only and supply equivalent text alongside the SVG. The text may be visually hidden, whilst kept available to screen readers, if required by the design.

```html
<button class="btn btn-primary">
  <svg aria-hidden="true" focusable="false" role="img" class="icon">
    <use href="#icon-mastodon" xlink:href="#icon-mastodon"></use>
  </svg><span class="visually-hidden">Load more toots from Mastodon</span>
</button>

<button class="btn btn-light border">
  <svg aria-hidden="true" focusable="false" role="img" class="icon icon-inline">
    <use href="#icon-mastodon" xlink:href="#icon-mastodon"></use>
  </svg><span class="visually-hidden">Mastodon </span>Send toot
</button>
```

```html
<span class="visually-hidden" aria-hidden="true">
<svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
<defs>
<symbol id="icon-mastodon" viewBox="0 0 32 32">
<title>Mastodon</title>
<path d="M30.924 10.505c0-6.941-4.548-8.976-4.548-8.976C24.083.476 20.144.033 16.055 0h-.101c-4.091.033-8.027.476-10.32 1.529 0 0-4.548 2.035-4.548 8.976 0 1.589-.031 3.491.02 5.505.165 6.789 1.245 13.479 7.521 15.14 2.893.765 5.379.927 7.38.816 3.629-.2 5.667-1.296 5.667-1.296l-.12-2.633s-2.593.817-5.505.719c-2.887-.099-5.932-.311-6.399-3.855a7.069 7.069 0 0 1-.064-.967v-.028.001s2.833.693 6.423.857c2.195.1 4.253-.129 6.344-.377 4.009-.479 7.5-2.949 7.939-5.207.689-3.553.633-8.676.633-8.676zm-5.365 8.946H22.23v-8.159c0-1.72-.724-2.592-2.171-2.592-1.6 0-2.403 1.035-2.403 3.083v4.465h-3.311v-4.467c0-2.048-.803-3.083-2.403-3.083-1.447 0-2.171.873-2.171 2.592v8.159H6.442v-8.404c0-1.719.437-3.084 1.316-4.093.907-1.011 2.092-1.528 3.565-1.528 1.704 0 2.995.655 3.848 1.965l.828 1.391.829-1.391c.853-1.311 2.144-1.965 3.848-1.965 1.472 0 2.659.517 3.565 1.528.877 1.009 1.315 2.375 1.315 4.093z"></path>
</symbol>
</defs>
</svg>
</span>
<button>
  <svg aria-hidden="true" focusable="false" role="img" class="icon">
    <use href="#icon-mastodon" xlink:href="#icon-mastodon"></use>
  </svg><span class="visually-hidden">Load more toots from Mastodon</span>
</button>

<button>
  <svg aria-hidden="true" focusable="false" role="img" class="icon icon-inline">
    <use href="#icon-mastodon" xlink:href="#icon-mastodon"></use>
  </svg><span class="visually-hidden">Mastodon </span> Send toot
</button>
```

> Screen reader: "Load more toots from Mastodon, button.
> Mastodon Send toot, button, group."

### Tooltips

A tooltip may provide brief supplemental information, but it must not supply the only accessible name for an icon-only control. Prefer a visible label; otherwise provide the control’s accessible name before the tooltip opens. Follow [Tooltips](Tooltips.md) for the tooltip’s markup, behaviour and testing requirements, and [Labels](Labels.md#icon-only-controls) for naming controls.

## Spinner icon

Use visible status text for an operation that takes time. A spinner can reinforce the message. Keep the control’s name stable and put routine updates in a separate [status region](Dynamic%20Content.md#status-messages) that exists before the first update. The operation’s actual state determines the message. Reduced motion controls whether the spinner animates.

This fragment illustrates the pending, successful and failed states. The host application starts the upload and calls `setUploadStatus` as it progresses. The fragment does not start a network request.

```html
<button type="button">Upload</button>
<p role="status">
  <span id="upload-spinner" class="upload-spinner" aria-hidden="true" hidden></span>
  <span id="upload-status"></span>
</p>

<style>
  .upload-spinner:not([hidden]) {
    display: inline-block;
    inline-size: 1em;
    block-size: 1em;
    border: 0.15em solid currentColor;
    border-inline-end-color: transparent;
    border-radius: 50%;
  }

  @media (prefers-reduced-motion: no-preference) {
    .upload-spinner:not([hidden]) {
      animation: upload-spin 1s linear infinite;
    }
  }

  @keyframes upload-spin {
    to { transform: rotate(1turn); }
  }
</style>

<script>
  function setUploadStatus(state) {
    const messages = {
      pending: 'Uploading file…',
      success: 'File uploaded.',
      failure: 'Upload failed. Try again.'
    };

    document.getElementById('upload-spinner').hidden = state !== 'pending';
    document.getElementById('upload-status').textContent = messages[state];
  }
</script>
```

## Checking icon states

Keep the visual icon and the control’s exposed information in sync. For a Play/Pause action, update the icon and the label together to describe the next action. For a toggle with a stable label, use its pressed state and change the icon’s shape or another visible cue. For a disclosure, update the icon and `aria-expanded` from the same state. See [Video Controls](Video%20Controls.md), [Disclosure](Disclosure.md) and [Colour, Contrast and Meaning](Colour%2C%20Contrast%20and%20Meaning.md).

### ACT-style checks

Rule identifier: `gel-components-iconography`

Test decorative icons, meaningful icons and controls whose icons change with state. Apply each check to the relevant variant.

| Method and action | Expected result |
| --- | --- |
| Tab through links and buttons containing SVGs. Where the project supports browsers that expose SVGs to focus by default, check the effect of `focusable="false"`. | Focus reaches the control. Its decorative SVG adds no tab stop. |
| Measure icons and state indicators needed to understand content or identify a control against the relevant adjacent colours. | Where [WCAG 2.1 1.4.11 Non-text Contrast](https://www.w3.org/TR/WCAG21/#non-text-contrast) applies, the required visual information has at least 3:1 contrast. |
| Change a control’s state. Look for a cue independent of hue and repeat in grayscale or forced colours. Compare the result with the name or state exposed to a screen reader. | A change in shape, orientation, border, text or another non-colour cue makes the state distinguishable. The exposed action or state agrees with the visible one. |

### Spinner checks

Trigger a pending operation, then test success and failure. Repeat with reduced motion enabled. The same status text appears and is announced in both settings. The spinner remains still in reduced motion and disappears when the operation ends. Focus stays with the user.

## Sources

- [Private Use Areas](https://en.wikipedia.org/wiki/Private_Use_Areas)
- [Tooltip](https://en.wikipedia.org/wiki/Tooltip)
- [html5 accessibility chops title attribute use and abuse](https://developer.paciellogroup.com/blog/2012/01/html5-accessibility-chops-title-attribute-use-and-abuse/)
- [How Users Kill Your Icon Fonts](https://timteeling.com/how-users-break-your-icon-fonts/)
- [Don't make every `<svg>` focusable](https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/8090208/)
- [ARIA-label Is A Xenophobe](http://www.heydonworks.com/article/aria-label-is-a-xenophobe)
- [WCAG2.1 1.4.3 Contrast (Minimum)](https://www.w3.org/TR/WCAG21/#contrast-minimum)
- [WCAG2.1 1.4.1 Use Of Color](https://www.w3.org/TR/WCAG21/#use-of-color)
- [non text contrast](https://www.w3.org/TR/WCAG21/#non-text-contrast)
- [Live region guidance](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Live_Regions)
- [Reduced-motion media feature](https://drafts.csswg.org/mediaqueries-5/#prefers-reduced-motion)
- [SVGR](https://github.com/gregberge/svgr)
- [unplugin-icons](https://github.com/antfu/unplugin-icons)
- [Tailwind](https://tailwindcss.com/docs/screen-readers)
- [Bootstrap](https://getbootstrap.com/docs/5.2/getting-started/accessibility/#visually-hidden-content)
- [SVG image support](https://caniuse.com/svg-img)
- [Require `svg` elements to have focusable attribute (svg-focusable)](https://html-validate.org/rules/svg-focusable.html)
- [Inline SVG support](https://caniuse.com/svg-html5)
- [_Providing Accessible Names and Descriptions_](https://www.w3.org/WAI/ARIA/apg/practices/names-and-descriptions/#x5-4-naming-techniques)
