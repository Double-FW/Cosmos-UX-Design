# FWFW Atomic UX Design Framework

A collection of accessibility techniques for people designing, building, testing and improving web design systems. The guides cover page foundations, interaction, form controls and reusable components.

## Guide index

### Page foundations

- [Structure](Structure.md)
- [Headings](Headings.md)
- [Landmarks](Landmarks.md)
- [Metadata](Metadata.md)
- [Layout](Layout.md)
- [Typography](Typography.md)
- [Color and Meaning](Color%20and%20Meaning.md)
- [Motion and Animation](Motion%20and%20Animation.md)
- [Progressive Enhancement](Progressive%20Enhancement.md)

### Interaction and content

- [Anchors and Links](Anchors%20and%20Links.md)
- [Focus](Focus.md)
- [Keyboard Navigation](Keyboard%20Navigation.md)
- [Functional Alternative Text](Functional%20Alternative%20Text.md)
- [Icons](Icons.md)
- [Dynamic Content](Dynamic%20Content.md)

### Forms

- [HTML Form Elements](HTML%20Form%20Elements.md)
- [Forms](Forms.md)
- [Labels](Labels.md)
- [Rating Stars](Rating%20Stars.md)

### Components

- [Accordion](Accordion.md)
- [Breadcrumbs](Breadcrumbs.md)
- [Cards – Dialog](Cards%20-%20Dialog.md)
- [Cards – Reveal](Cards%20-%20Reveal.md)
- [Mega Menu](Mega%20Menu.md)
- [Modal Dialogs](Modal%20Dialogs.md)

## Using the guides

1. Open the relevant guide for the design or component and follow its links to shared implementation guidance.
2. Turn applicable checkpoints into acceptance criteria. Record the WCAG criterion and level separately from any additional design-system requirement.
3. Test in the host product with the supported browser and assistive technology combinations. Include keyboard-only use, screen reader use, resize and reflow, forced colours, reduced motion and blocked scripts where relevant.
4. Record the barrier, its impact, the affected component or journey, and the result of retesting after the fix.

The examples appear as code on GitHub. Copy a self-contained HTML example to a local HTML file to try it in a browser. Snippets with sample images, media, routes or project styles need those resources supplied by the implementing project.

## Shared implementation guidance

| Topic | Implementation | Related checks or variations |
| --- | --- | --- |
| Dialogs | [Modal Dialogs](Modal%20Dialogs.md) | [Cards – Dialog](Cards%20-%20Dialog.md), [Dynamic Content](Dynamic%20Content.md) |
| Disclosure | [Cards – Reveal](Cards%20-%20Reveal.md) | [Accordion](Accordion.md), [Mega Menu](Mega%20Menu.md) |
| Forms | [HTML Form Elements](HTML%20Form%20Elements.md) | [Forms](Forms.md) for validation and recovery, [Labels](Labels.md) for naming |
| Titles, landmarks and skip links | [Structure](Structure.md) | [Metadata](Metadata.md), [Landmarks](Landmarks.md), [Keyboard Navigation](Keyboard%20Navigation.md) |
| Resize and reflow | [Typography](Typography.md#responsiveness) | [Progressive Enhancement](Progressive%20Enhancement.md) |

See [LICENSE](LICENSE) for licence and contributor information.
