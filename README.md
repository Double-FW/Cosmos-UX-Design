# Cosmos - UX Design System Documentation

A collection of universally applicable accessibility and inclusive design techniques for people designing, building, testing and improving web design systems. The guides cover page foundations, interaction, form controls, best practices and reusable components.

## Guide index

### Page foundations

- [Affordances, Signifiers and Design Conventions](Affordances%2C%20Signifiers%20and%20Design%20Conventions.md) — clues that help people recognise and predict actions.
- [Page Structure](Structure.md) — document titles, page markup and skip links.
- [Headings](Headings.md) — section labels, heading levels and heading navigation.
- [Landmarks](Landmarks.md) — roles, names and landmark navigation checks.
- [Page Metadata](Metadata.md)
- [Layout](Layout.md)
- [Typography](Typography.md)
- [Colour, contrast and meaning](Colour%2C%20Contrast%20and%20Meaning.md)
- [Making Important Information Stand Out](Making%20Important%20Information%20Stand%20Out.md) — visual emphasis and screen reader announcements.
- [Motion and Animation](Motion%20and%20Animation.md)
- [Progressive Enhancement](Progressive%20Enhancement.md)

### Interaction and content

- [Anchors and Links](Anchors%20and%20Links.md)
- [Designing the Screen Reader Journey](Designing%20the%20Screen%20Reader%20Journey.md) — task-level review of reading, focus and feedback.
- [Focus](Focus.md)
- [Keyboard Navigation](Keyboard%20Navigation.md)
- [Functional Alternative Text](Functional%20Alternative%20Text.md)
- [Icons](Icons.md)
- [Dynamic Content](Dynamic%20Content.md)
- [Notifications and Status Messages](Notifications%20and%20Status%20Messages.md)

### Service journeys

- [Customer Surveys and Feedback](Customer%20Surveys%20and%20Feedback.md) — accessible feedback throughout the customer journey.
- [Alternatives to Deceptive Patterns](Alternatives%20to%20Deceptive%20Patterns.md)
- [Informed Choice and Preferences](Informed%20Choice%20and%20Preferences.md)
- [Refusal, Cancellation and Exit](Refusal%2C%20Cancellation%20and%20Exit.md)
- [Reminders and Saved Actions](Reminders%20and%20Saved%20Actions.md) — optional reminders and a reliable route back.
- [Saving and Resuming a Task](Saving%20and%20Resuming%20a%20Task.md) — draft feedback, return routes and recovery.
- [Showing What Changed Before Confirmation](Showing%20What%20Changed%20Before%20Confirmation.md) — review changed details before committing.
- [Support, Escalation and Handoffs](Support%2C%20Escalation%20and%20Handoffs.md)

### Forms

- [Calendars and Date Pickers](Calendars%20and%20Date%20Pickers.md) — date entry, calendar selection and availability.
- [HTML Form Elements](HTML%20Form%20Elements.md)
- [Forms](Forms.md)
- [Labels](Labels.md)
- [Rating Stars](Rating%20Stars.md)

### Components

- [Accessible Chatbots](Accessible%20Chatbots.md) — conversation, announcements and alternative support.
- [Accordion](Accordion.md)
- [Badge](Badge.md)
- [Call to Action](Call%20to%20Action.md)
- [Breadcrumbs](Breadcrumbs.md)
- [Carousels and Sliders](Carousels%20and%20Sliders.md) — paged slides, scrolling lists and value inputs.
- [Disclosure](Disclosure.md)
- [Drawer](Drawer.md) — non-modal side panels and modal drawers.
- [Mega Menu](Mega%20Menu.md)
- [Modal Dialogs](Modal%20Dialogs.md)

### Additional component and interaction guides

- [Cards](Cards.md)
- [Client-side Routing](Client-side%20Routing.md)
- [Charts, Graphs and Maps](Charts%2C%20Graphs%20and%20Maps.md) — data visualisation, structured alternatives and routes.
- [Comments](Comments.md)
- [Data Tables](Data%20Tables.md)
- [Expandable Content Previews](Expandable%20Content%20Previews.md)
- [Filter and Sort](Filter%20and%20Sort.md)
- [Global Navigation](Global%20Navigation.md)
- [Infographics](Infographics.md)
- [Information Panels](Information%20Panels.md)
- [Load More](Load%20More.md)
- [Metadata Strips](Metadata%20Strips.md)
- [Promotional Cards](Promotional%20Cards.md)
- [Search](Search.md)
- [Share Tools](Share%20Tools.md)
- [Site Menus](Site%20Menus.md)
- [Tabs](Tabs.md)
- [Video Controls](Video%20Controls.md)

## Using the guides

1. Open the relevant guide for the design or component and follow its links to shared implementation guidance.
2. Turn applicable checkpoints into acceptance criteria. Record the WCAG criterion and level separately from any additional design-system requirement.
3. Test in the host product with the supported browser and assistive technology combinations. Include keyboard-only use, screen reader use, resize and reflow, forced colours, reduced motion and blocked scripts where relevant.
4. Record the barrier, its impact, the affected component or journey, and the result of retesting after the fix.

The examples appear as code on GitHub. Copy a self-contained HTML example to a local HTML file to try it in a browser. Snippets with sample images, media, routes or project styles need those resources supplied by the implementing project.

For the ACT-style checks, record the tested variant, method, observed outcome and evidence. Use pass, fail, not applicable or not run. These local checks cover the specified component behaviour. A WCAG conformance assessment also needs the other applicable requirements and journey states.

## Shared implementation guidance

| Topic | Implementation | Related checks or variations |
| --- | --- | --- |
| Dialogs | [Modal Dialogs](Modal%20Dialogs.md) | [Opening a dialog from a card](Cards.md#opening-a-dialog-from-a-card), [Dynamic Content](Dynamic%20Content.md) |
| Disclosure | [Disclosure](Disclosure.md) | [Accordion](Accordion.md), [Mega Menu](Mega%20Menu.md) |
| Forms | [HTML Form Elements](HTML%20Form%20Elements.md) | [Forms](Forms.md) for validation and recovery, [Labels](Labels.md) for naming |
| Document titles, page markup and skip links | [Page Structure](Structure.md) | [Page Metadata](Metadata.md), [Keyboard Navigation](Keyboard%20Navigation.md) |
| Heading wording, levels and navigation | [Headings](Headings.md) | [Page Structure](Structure.md) |
| Landmark roles, names and navigation | [Landmarks](Landmarks.md) | [Page Structure](Structure.md) |
| Resize and reflow | [Typography](Typography.md#responsiveness) | [Progressive Enhancement](Progressive%20Enhancement.md) |

See [LICENSE](LICENSE) for licence information.
