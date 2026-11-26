# Cosmos - UX Design System Documentation

A collection of universally applicable accessibility and inclusive design techniques for people designing, building, testing and improving web design systems. The guides cover page foundations, interaction, form controls, best practices and reusable components.

## Guide index

- [Design Patterns](#design-patterns)
- [Design Practices](#design-practices)

### Design Patterns

Reusable components and service journeys that help people complete a specific task.

#### Navigation and finding content

- [Breadcrumbs](Breadcrumbs.md)
- [Filter and Sort](Filter%20and%20Sort.md)
- [Global Navigation](Global%20Navigation.md)
- [Load More](Load%20More.md)
- [Mega Menu](Mega%20Menu.md)
- [Search](Search.md)
- [Site Menus](Site%20Menus.md)

#### Browsing and revealing content

- [Accordion](Accordion.md)
- [Cards](Cards.md)
- [Carousels and Sliders](Carousels%20and%20Sliders.md) — paged slides, scrolling lists and value inputs.
- [Disclosure](Disclosure.md)
- [Expandable Content Previews](Expandable%20Content%20Previews.md)
- [Metadata Strips](Metadata%20Strips.md)
- [Promotional Cards](Promotional%20Cards.md)
- [Tabs](Tabs.md)

#### Panels and dialogs

- [Drawer](Drawer.md) — non-modal side panels and modal drawers.
- [Information Panels](Information%20Panels.md)
- [Modal Dialogs](Modal%20Dialogs.md)

#### Data, input and media controls

- [Calendars and Date Pickers](Calendars%20and%20Date%20Pickers.md) — date entry, calendar selection and availability.
- [Data Tables](Data%20Tables.md)
- [Drag and Drop](Drag%20and%20Drop.md) — accessible moves, reordering and alternatives to dragging.
- [Rating Stars](Rating%20Stars.md)
- [Video Controls](Video%20Controls.md)

#### Feedback, sharing and support

- [Badge](Badge.md)
- [Chatbots](Chatbots.md) — conversation, announcements and alternative support.
- [Comments](Comments.md)
- [Customer Surveys and Feedback](Customer%20Surveys%20and%20Feedback.md) — accessible feedback throughout the customer journey.
- [Notifications and Status Messages](Notifications%20and%20Status%20Messages.md)
- [Share Tools](Share%20Tools.md)
- [Support, Escalation and Handoffs](Support%2C%20Escalation%20and%20Handoffs.md)

#### Transactions, choices and returning to a task

- [Authentication](Authentication.md) — sign-in, verification and account recovery.
- [Baskets and Checkout](Baskets%20and%20Checkout.md) — basket editing, order review, payment and recovery.
- [Call to Action](Call%20to%20Action.md)
- [Refusal, Cancellation and Exit](Refusal%2C%20Cancellation%20and%20Exit.md)
- [Reminders and Saved Actions](Reminders%20and%20Saved%20Actions.md) — optional reminders and a reliable route back.
- [Saving and Resuming a Task](Saving%20and%20Resuming%20a%20Task.md) — draft feedback, return routes and recovery.
- [Showing What Changed Before Confirmation](Showing%20What%20Changed%20Before%20Confirmation.md) — review changed details before committing.

### Design Practices

Principles and techniques that apply across components, pages and journeys.

#### Page structure and navigation

- [Anchors and Links](Anchors%20and%20Links.md)
- [Client-side Routing](Client-side%20Routing.md)
- [Headings](Headings.md) — section labels, heading levels and heading navigation.
- [Landmarks](Landmarks.md) — roles, names and landmark navigation checks.
- [Layout](Layout.md)
- [Page Metadata](Page_Metadata.md)
- [Page Structure](Page_Structure.md) — document titles, page markup and skip links.

#### Visual design and content

- [Charts, Graphs and Maps](Charts%2C%20Graphs%20and%20Maps.md) — data visualisation, structured alternatives and routes.
- [Colour, contrast and meaning](Colour%2C%20Contrast%20and%20Meaning.md)
- [Functional Alternative Text](Functional%20Alternative%20Text.md)
- [Icons](Icons.md)
- [Infographics](Infographics.md)
- [Motion and Animation](Motion%20and%20Animation.md)
- [Typography](Typography.md)

#### Interaction and changes

- [Affordances, Signifiers and Design Conventions](Affordances%2C%20Signifiers%20and%20Design%20Conventions.md) — clues that help people recognise and predict actions.
- [Dynamic Content](Dynamic%20Content.md)
- [Focus](Focus.md)
- [Keyboard Navigation](Keyboard%20Navigation.md)
- [Progressive Enhancement](Progressive%20Enhancement.md)

#### Form design

- [Forms](Forms.md)
- [HTML Form Elements](HTML%20Form%20Elements.md)
- [Labels](Labels.md)

#### Understanding and choice

- [Alternatives to Deceptive Patterns](Alternatives%20to%20Deceptive%20Patterns.md)
- [Designing the Screen Reader Journey](Designing%20the%20Screen%20Reader%20Journey.md) — task-level review of reading, focus and feedback.
- [Informed Choice and Preferences](Informed%20Choice%20and%20Preferences.md)
- [Making Important Information Stand Out](Making%20Important%20Information%20Stand%20Out.md) — visual emphasis and screen reader announcements.

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

## Related Double-FW guidance

| If you need guidance on | Read |
| --- | --- |
| Cognitive design, familiarity and decision-making | [Cognitive UX Design](https://github.com/Double-FW/Cognitive-UX-Design/blob/main/Cognitive%20Design%20Principles.md) |
| Consistent experiences across input and output modes | [Multimodal UX Design](https://github.com/Double-FW/Inclusive-Multimodal-UX/blob/main/Multimodal_UX_Design.md) |
| Typography for products and brands | [Accessible Typography for UX and Brand Systems](https://github.com/Double-FW/Accessible-Typography-for-UX-and-Brand-Systems/blob/main/Accessible_Typography_for_UX_and_Brand_Systems.md) |
| Accessible data visualisation and maps | [Inclusive Design of Charts, Graphs, Maps and Dynamic Complex Graphics](https://github.com/Double-FW/Accessible_Charts_Graphs_and_Maps/blob/main/Inclusive_Design_Charts_Maps_and_Complex_Graphics.md) |
| Chat interfaces, transcript navigation and announcements | [Inclusive Chatbot Interface Reference](https://github.com/Double-FW/Accessible-Chatbots/blob/main/Inclusive_Chatbot_Interface_Reference.md) |
| Playback, media preferences and interface controls | [Accessible Interactive Media Control](https://github.com/Double-FW/Inclusive-Media-Interfaces/blob/main/Accessible_Interactive_Media_Control.md) |
| Identifying patterns that undermine informed choice | [Deceptive Patterns and Inclusive Experience Evaluation](https://github.com/Double-FW/Deceptive_Patterns_and-Inclusive_Experience_Evaluation/blob/main/README.md) |
| Planning accessibility testing and recording evidence | [QA Accessibility Testing](https://github.com/Double-FW/QA_Accessibility_Testing/blob/main/README.md) |

## Licence

See the [MIT licence](LICENSE).
