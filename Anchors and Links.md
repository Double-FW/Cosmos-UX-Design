---
title: Anchors and Links
description: Accessible web links prevent barriers for users who rely on assistive technologies such as screen readers to interact and navigate.
---

## Link content

When writing text content for a link, use concise but complete text describing the linked resource. This is particularly important for those who use a screen reader to scan the entire webpage and read out a list of only the links, where each link will have no context and must be understood on its own. For example, a series of article summaries, each containing a link with the text "Read more," will be presented as a list of many "Read more" snippets to the screen reader user, with no indication of what following any link will allow the user to read more about.

{% figure "A list of links presented by a screen reader. The text from each link is read without any context from where it appears on the webpage." %}
<img class="figure-img img-fluid" src="/static/img/links/vo-navigate-by-links.jpg" style="width:100%; max-width:32rem" alt="">
{% endfigure %}

Adding the missing context to the link text will remove barriers for screen reader users and will likely help sighted users keep track of what they are linking to. In exceptional cases, where it would be redundant or distracting for sighted users to read the full details in the link content, this can be managed by adding the context as visually hidden text available only to screen reader users.

```html
<a href="#values">
  Read more<span class="visually-hidden"> about our values</span>...
</a>
```

{% demoiframe title="Link with screen reader text." %}
<h1>Our Values</h1>
<p>
  We're committed to being more sustainable in everything we do.
</p>
<p>
  <a href="#values">
    Read more<span class="visually-hidden"> about our values</span>...
  </a>
</p>
{% enddemoiframe %}

> Screen reader: "link, Read more about our values ..."

Although the link text visually appears as only "Read more" in the context of the article entitled "Our Values," it will appear as "Read more about our values..." when a screen reader user opens the list of all links from the webpage.

{% aside "aside-info" %}
As a rule, you should keep the content intended for screen reader users the same as content intended for other users. Not all screen reader users are blind, and screen reader users that can see the text may be confused by hearing and seeing different content. In addition, having parallel but different content for different groups of users allows the possibility of differences in maintenance and quality to creep in: one version being localized while the other is not, or one lacking the same level of proofreading, for example.
{% endaside %}

## Icons in links

While having descriptive, visible text is the most accessible way to label a link, there are use cases where a well-understood icon would be more natural. In these cases, alternative text must be supplied for screen reader users who cannot see the icon.

A simple HTML `img` element will display the icon. Screen readers will announce that an "image" is present and read out any text in the element's `alt` attribute. Choose an alternative text that summarizes the _meaning_ of the image; there's no need to explain that it is an "image," "icon," or "picture" in the alternative text.

```html
Follow Us
<div class="social-links">
  <a href="#toot">
    <img class="icon icon-inline" src="/static/icons/mastodon.svg" alt="Mastodon">
  </a>
</div>
```

{% demoiframe
   title="Using an icon in a link.",
   head="<link rel=\"stylesheet\" href=\"/demos/icons.css\">"
%}
Follow Us
<div class="social-links">
  <a href="#toot">
    <img class="icon icon-inline" src="/static/icons/mastodon.svg" alt="Mastodon">
  </a>
</div>
{% enddemoiframe %}

> Screen reader: "Follow Us"
> Screen reader: "link, image, Mastodon"

## Translation services

Not every string of text on a webpage will be available to translation services, like Google Translate. When choosing a technique for adding alternative text to a webpage, it's essential to ensure that this text, which screen reader users will rely on, will also be translated. Test any translation tools or services your website plans to support to ensure that alternative text is supported.

{% figure "Originally in English, the service has translated the page into French. We can see that the text in the image element's `alt` attribute has also been translated into French." %}
<img class="figure-img img-fluid" src="/static/img/anchor-links/google-translate.jpg" style="width:100%; max-width:32rem" alt="">
{% endfigure %}

## External links

When linking to external websites, it is a good idea from a security and accountability perspective to inform the user that they are about to visit content you are not responsible for. This can be a concern if the user is logged into your site or has some other session that would be interrupted or dropped once they leave your site.

### New tabs

Opening links in new tabs or windows can make some users' experience more confusing and less accessible, so it should be used rarely. One exception may be links to external websites. [^1]

In the following examples, notice the `noreferrer` and `noopener` values in the link's `rel` attribute. This follows good security practices when opening a new browser tab by preventing information about, or access to, the current tab from inadvertently being made available to the new tab. While not normally a problem, if the website you are linking to were hacked, such information or access could be useful to anyone wanting to trick the user. [^2]

### Using an icon to signal new tabs

%source(src/demos/foundations/anchor-links/anchor-links.css#1-17)

```html
<p>
  Our suppliers use
  <a href="#sustainable" class="is-external" target="_blank" rel="noopener noreferrer">sustainable practices</a> to grow our spices and flavorings.
</p>
```

{% demoiframe
  title="Using an icon to indicate a link opens in a new tab.",
  head="<link rel=\"stylesheet\" href=\"/demos/foundations/anchor-links/anchor-links.css\">"
%}
<p>
  Our suppliers use
  <a href="#sustainable" class="is-external" target="_blank" rel="noopener noreferrer">sustainable practices</a> to grow our spices and flavorings.
</p>
{% enddemoiframe %}

> Screen reader: "link, sustainable practices, (external website, opens in new tab)"

## Links to phone numbers

Using the most appropriate URI scheme in the `href` attribute for web links can make your contact information more usable, especially for users with disabilities. [^5] Some examples are shown below.

```css
[itemprop="telephone"]:before {
  content: "\260e";
  content: "\260e" / "telephone";
  margin-right: 1em;
}
```

Notice that in the example HTML below, we use the "LocalBusiness" micro-data scheme to encourage search engines to display the business contact data in the most usable format. [^6] This can make searching for and dialing a business phone number a much easier task, especially for users of assistive technologies. 

```html
<div itemscope itemtype="https://schema.org/LocalBusiness">
  Get in touch:
  <span itemprop="telephone">
    <a href="tel:+441234567890">+44 1-2345-67890</a>
  </span>
</div>
```

{% demoiframe
  title="Using device URI schemes.",
  head="<style>[itemprop=\"telephone\"]:before{content:\"\\260e\";content:\"\\260e\"/\"telephone\";margin-right:0.4em;}</style>"
%}
<div itemscope itemtype="https://schema.org/LocalBusiness">
Get in touch:
<span itemprop="telephone">
<a href="tel:+441234567890">+44 1-2345-67890</a>
</span>
</div>
{% enddemoiframe %}

> Screen reader: "Get in touch: telephone"
> Screen reader: "link, +44 1-2345-67890"

While a web link that automatically opens a telephone application would mainly be helpful on mobile phone devices, don't try to disable the default telephone link functionality on desktop devices. Most modern desktop operating systems allow you to call via a telephone link. Desktop VoIP apps, like FaceTime, Microsoft Teams Voice, or Skype, can be configured to handle the `tel:` scheme in web links. MacOS, for example, supports making phone calls from the desktop using an iPhone. [^4] This feature type can be beneficial for individuals with disabilities.

## Adding anchors to headings

For longer articles, encourage users to copy and share links to specific parts of the web page by adding visible clues that the heading has an anchor link.

%source(src/demos/foundations/anchor-links/anchor-links.css#18-35)

```html
<div style="padding-left:1.5em">
  <h3 id="fresh-idea" class="is-anchor">
    <a href="#fresh-idea">A Fresh Idea</a>
  </h3>
  <p>
    Try a new twist on a favorite drink for summer. It's cool, refreshing, and filled with delicious fruit flavors.
  </p>
</div>
```

{% demoiframe
  title="Add an in-page link to a heading.",
  head="<link rel=\"stylesheet\" href=\"/demos/foundations/anchor-links/anchor-links.css\">"
%}
<div style="padding-left:1.5em">
  <h3 id="fresh-idea" class="is-anchor">
    <a href="#fresh-idea">A Fresh Idea</a>
  </h3>
  <p>
    Try a new twist on a favorite drink for summer. It's cool, refreshing, and filled with delicious fruit flavors.
  </p>
</div>
{% enddemoiframe %}

> Screen reader: "heading level 3, link, A Fresh Idea"

The icon that appears on hover (or focus) is implemented as a background image to keep it out of the way of screen readers: the link symbol is a visible but only decorative way of indicating the presence of the anchor link.

## Checkpoints for the Tester

1. Ensure each link contains visible text or a well-understood, conventional icon.
2. For links containing only an icon, ensure that equivalent text is assigned to the graphic so that a screen reader can speak.
3. Ensure that all links are consistently and visually distinct from the surrounding text so the user can identify them. Do not use color alone to identify links; this is inaccessible to users with low color perception.
4. Ensure that when navigating the web page using the keyboard, it is visually apparent when a link has keyboard focus.
5. Ensure that when the links are presented as a list, separate from their surrounding context, they have text or alternative text that is concise but distinctly describes the resource being linked to. (Link text such as "click here," for example, is inadequate.)
6. Prefer that when a link contains text along with an icon, where the icon is decorative-only, the screen reader ignores the icon.
7. Ensure that examples of multiple links to the same resource are not adjacent or wrapping each other; combine these into a single link instead.
8. Ensure that any cases of hidden or alternative text available only to screen readers are not different than the visible text or different from the icon's apparent meaning.

[^1]: _G200: Opening new windows and tabs from a link only when necessary_ [Techniques for WCAG 2.0](https://www.w3.org/TR/WCAG20-TECHS/G200.html)
[^2]: "Tabnabbing is a computer exploit and phishing attack, which persuades users to submit their login details and passwords to popular websites by impersonating those sites and convincing the user that the site is genuine." [_Tabnabbing_, Wikipedia](https://en.wikipedia.org/wiki/Tabnabbing)
[^3]: [_title - the SVG accessible name element_, mdn web docs](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/title)
[^4]: "FaceTime in macOS 10.10 and later can also use the tel URL scheme to launch the Phone app on an iOS device using Handoff. This scenario works when FaceTime is configured to dial phone numbers (the default configuration) and the iOS device is connected to the same iCloud account as the Mac." [_Apple URL Scheme Reference_](https://developer.apple.com/library/archive/featuredarticles/iPhoneURLScheme_Reference/PhoneLinks/PhoneLinks.html)
[^5]: "Standardized URI schemes have been defined for some common device functions, e.g. making phone calls, sending an SMS, and managing address books. These URI schemes, if supported, can enable users to easily use these functions from Web applications." [_Make Telephone Numbers "Click-to-Call"_, W3C Mobile Web Application Best Practices](https://www.w3.org/TR/mwabp/#bp-interaction-uri-schemes)
[^6]: [telephone, A Schema.org Property](https://schema.org/telephone)
