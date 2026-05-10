---
title: Color and Meaning
description: "Ensure that web pages don't require color perception."
--- 

## Summary

Each color (specifically hue) is not perceived the same by all users, and many people cannot differentiate some hues at all, or to varying degrees. Ensuring that web page designs do not require color perception means that you won't exclude anyone from having access to use and understand those features that rely upon color. Judicious use of color however, can _increase_ accessibility for some users. So rather than avoiding color-based meaning in a web page, ensure that when color is used as a visual cue there are alternative non-color visual cues provided too.

## Purpose

[Title III of the Americans with Disabilities Act](https://www.ada.gov/ada_title_III.htm) can be applied to businesses whose websites that are not accessible to users on the basis of their disability. That includes color vision deficiencies.

The red-green color vision deficiency (of which there are 4 distinct types and a multitude of varying degrees) is the most common form of the so-called color blindness disorders. It affects around 1 in 12 men and 1 in 200 women. There is also a less common blue-yellow color perception deficiency, and the relatively rare complete color blindness.

And keep in mind that the color-related issues affecting your users may actually be in their device. Consider a faulty phone screen or monitor for example, or over-bright lighting that can casts a glare on the screen.

## Checks

For each web page under test, perform the following checks. Record an answer, "yes" or "no" to each question. The passing result is indicated in uppercase. For each failing result, document it either as a defect or else provide an explanation of why it should be allowed as an exception or why it is not applicable.

### Checks which should be (at least partly) performed manually

- Considering each of the links on the page, are they all visually identifiable without relying on color? [Y/n]
- When interacting with each of the features on the page (like validation or progress indicators), is it possible to know what state each is in, even with color disabled? [Y/n]
- With regards to charts and graphs in particular, are categories or datasets displayed on the chart keyed by a visual treatment other than color? [Y/n]

## Additional Testing Methods and Strategies

If you are not colorblind yourself, you can test a web page under a colorblindness simulator. The [Sim Daltonism](https://michelf.ca/projects/mac/sim-daltonism/) app can be installed on your desktop and will open a viewer that will apply a colorblindness filter to anything below the app window. There are also [web browser extensions](https://chrome.google.com/webstore/detail/colorblindly/floniaahmccleoclneebhhmnjgdfijgg?hl=en) you can install that will perform the same transformation. Testing a web page with a monochromatic filter is a good way to check if it will be usable by people with every kind of color perception.

## Examples and Additional Information

Keep in mind that the relative contrast of foreground content and and its background can change when viewed with different color perception filters applied. What might be reasonably readable as green text on a blue background because of the different hues, may become completely unreadable when viewed by someone with a blue-yellow color deficiency for example. So consider that some contrast guidelines will also apply when testing color accessibility.

It must be possible for users to easily identify links in text. It is expected that links should be underlined, but your design may use other visual clues to make links stand out, such as color. Linked text must have at least 3:1 contrast when compared with the surrounding body text. In any case the linked text must display some indicator which is not purely color-based, such as an underline, when the link has keyboard focus or on cursor hover.
