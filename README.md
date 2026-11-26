# Cosmos - UX Design System Documentation

A collection of universally applicable accessibility and inclusive design techniques for people designing, building, testing and improving web design systems. The guides cover page foundations, interaction, form controls, best practices and reusable components.

## Guide index

### Design Patterns

Reusable components and service journeys that help people complete a specific task.

- [Accordion](Accordion.md)
- [Authentication](Authentication.md) — sign-in, verification and account recovery.
- [Badge](Badge.md)
- [Baskets and Checkout](Baskets%20and%20Checkout.md) — basket editing, order review, payment and recovery.
- [Breadcrumbs](Breadcrumbs.md)
- [Calendars and Date Pickers](Calendars%20and%20Date%20Pickers.md) — date entry, calendar selection and availability.
- [Call to Action](Call%20to%20Action.md)
- [Cards](Cards.md)
- [Carousels and Sliders](Carousels%20and%20Sliders.md) — paged slides, scrolling lists and value inputs.
- [Chatbots](Chatbots.md) — conversation, announcements and alternative support.
- [Comments](Comments.md)
- [Customer Surveys and Feedback](Customer%20Surveys%20and%20Feedback.md) — accessible feedback throughout the customer journey.
- [Data Tables](Data%20Tables.md)
- [Disclosure](Disclosure.md)
- [Drag and Drop](Drag%20and%20Drop.md) — accessible moves, reordering and alternatives to dragging.
- [Drawer](Drawer.md) — non-modal side panels and modal drawers.
- [Expandable Content Previews](Expandable%20Content%20Previews.md)
- [Filter and Sort](Filter%20and%20Sort.md)
- [Global Navigation](Global%20Navigation.md)
- [Information Panels](Information%20Panels.md)
- [Load More](Load%20More.md)
- [Mega Menu](Mega%20Menu.md)
- [Metadata Strips](Metadata%20Strips.md)
- [Modal Dialogs](Modal%20Dialogs.md)
- [Notifications and Status Messages](Notifications%20and%20Status%20Messages.md)
- [Promotional Cards](Promotional%20Cards.md)
- [Rating Stars](Rating%20Stars.md)
- [Refusal, Cancellation and Exit](Refusal%2C%20Cancellation%20and%20Exit.md)
- [Reminders and Saved Actions](Reminders%20and%20Saved%20Actions.md) — optional reminders and a reliable route back.
- [Saving and Resuming a Task](Saving%20and%20Resuming%20a%20Task.md) — draft feedback, return routes and recovery.
- [Search](Search.md)
- [Share Tools](Share%20Tools.md)
- [Showing What Changed Before Confirmation](Showing%20What%20Changed%20Before%20Confirmation.md) — review changed details before committing.
- [Site Menus](Site%20Menus.md)
- [Support, Escalation and Handoffs](Support%2C%20Escalation%20and%20Handoffs.md)
- [Tabs](Tabs.md)
- [Video Controls](Video%20Controls.md)

### Design Practices

Principles and techniques that apply across components, pages and journeys.

- [Affordances, Signifiers and Design Conventions](Affordances%2C%20Signifiers%20and%20Design%20Conventions.md) — clues that help people recognise and predict actions.
- [Alternatives to Deceptive Patterns](Alternatives%20to%20Deceptive%20Patterns.md)
- [Anchors and Links](Anchors%20and%20Links.md)
- [Charts, Graphs and Maps](Charts%2C%20Graphs%20and%20Maps.md) — data visualisation, structured alternatives and routes.
- [Client-side Routing](Client-side%20Routing.md)
- [Colour, contrast and meaning](Colour%2C%20Contrast%20and%20Meaning.md)
- [Designing the Screen Reader Journey](Designing%20the%20Screen%20Reader%20Journey.md) — task-level review of reading, focus and feedback.
- [Dynamic Content](Dynamic%20Content.md)
- [Focus](Focus.md)
- [Forms](Forms.md)
- [Functional Alternative Text](Functional%20Alternative%20Text.md)
- [Headings](Headings.md) — section labels, heading levels and heading navigation.
- [HTML Form Elements](HTML%20Form%20Elements.md)
- [Icons](Icons.md)
- [Infographics](Infographics.md)
- [Informed Choice and Preferences](Informed%20Choice%20and%20Preferences.md)
- [Keyboard Navigation](Keyboard%20Navigation.md)
- [Labels](Labels.md)
- [Landmarks](Landmarks.md) — roles, names and landmark navigation checks.
- [Layout](Layout.md)
- [Making Important Information Stand Out](Making%20Important%20Information%20Stand%20Out.md) — visual emphasis and screen reader announcements.
- [Motion and Animation](Motion%20and%20Animation.md)
- [Page Metadata](Page_Metadata.md)
- [Page Structure](Page_Structure.md) — document titles, page markup and skip links.
- [Progressive Enhancement](Progressive%20Enhancement.md)
- [Typography](Typography.md)

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
| Dialogs | [Modal Dialogs](Modal%20Dialogs.md) | [Opening a dialog from a card](Cards.md#revealing-content-or-opening-a-dialog), [Dynamic Content](Dynamic%20Content.md) |
| Disclosure | [Disclosure](Disclosure.md) | [Accordion](Accordion.md), [Mega Menu](Mega%20Menu.md) |
| Forms | [HTML Form Elements](HTML%20Form%20Elements.md) | [Forms](Forms.md) for validation and recovery, [Labels](Labels.md) for naming |
| Document titles, page markup and skip links | [Page Structure](Page_Structure.md) | [Page Metadata](Page_Metadata.md), [Keyboard Navigation](Keyboard%20Navigation.md) |
| Heading wording, levels and navigation | [Headings](Headings.md) | [Page Structure](Page_Structure.md) |
| Landmark roles, names and navigation | [Landmarks](Landmarks.md) | [Page Structure](Page_Structure.md) |
| Resize and reflow | [Typography](Typography.md#responsiveness) | [Progressive Enhancement](Progressive%20Enhancement.md) |

See [LICENSE](LICENSE) for licence information.
