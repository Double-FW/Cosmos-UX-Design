# Accessibility Techniques Knowledgebase

A practical collection of accessibility guides for reviewing, improving, and partially remediating design systems, components, patterns, and web experiences.

## Purpose

This knowledgebase helps design, engineering, QA, and product teams identify accessibility barriers and improve reusable design-system assets before issues reach production.

It is intended to support:

- design-system remediation
- component accessibility reviews
- pattern documentation
- manual and automated testing
- accessibility acceptance criteria
- progressive enhancement
- assistive technology compatibility
- inclusive UX quality

## What this repository contains

The guides cover foundations and component-level techniques, including:

- Typography
- Page structure and landmarks
- Metadata
- Progressive enhancement
- Motion and animation
- Modal dialogs
- Rating stars
- Mega menus
- Access modes such as no-visual, no-audio, no-pointer, forced-colour, and reduced-motion testing

Each guide combines design intent, implementation guidance, code examples, and tester checkpoints.

## How to use this with a design system

Use these guides to review and remediate design-system assets in stages:

1. Audit existing foundations  
   Review typography, metadata, structure, colour, motion, and responsive behaviour.

2. Review reusable components  
   Check whether each component has semantic HTML, keyboard support, visible focus, assistive technology support, and robust fallback behaviour.

3. Add accessibility documentation  
   Each component should include usage guidance, interaction rules, content guidance, and testing checkpoints.

4. Create acceptance criteria  
   Convert the checkpoints into design, development, and QA acceptance criteria.

5. Validate across access modes  
   Test with keyboard-only navigation, screen readers, forced colours, text resize, zoom, reduced motion, no audio, and degraded network or device conditions.

6. Track remediation progress  
   Log issues by component, severity, user impact, and remediation status.

## Recommended remediation workflow

```text
Identify barrier
↓
Map to affected component or foundation
↓
Review relevant guide
↓
Update design, code, and documentation
↓
Test across access modes
↓
Add regression checks
↓
Publish updated design-system guidance
