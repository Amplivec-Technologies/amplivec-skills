---
name: design-standards
description: Apply Amplivec design system, visual identity, and UX/UI standards when creating, redesigning, normalizing, implementing, or reviewing user interfaces. Use for HTML/CSS/Bootstrap, ASP.NET MVC/Razor, Angular, and other frontends that must preserve Amplivec's visual consistency, responsive behavior, and accessibility.
---

# Amplivec Design Standards

Use this skill whenever a task affects visual design, UI components, interaction states, responsive behavior, or UX consistency in an Amplivec interface.

This skill defines Amplivec's shared design system. It is based on the current `amplivec-landing-page` as the initial canonical source, but it abstracts that implementation into reusable standards instead of copying the landing literally.

Use other Amplivec skills alongside this one when the task also affects architecture, coding style, Git workflow, or other non-UI concerns.

## Core Behavior

Always inspect the target application's existing UI before changing it.

Always identify:

- Existing design tokens.
- Existing reusable components.
- Existing layout and navigation patterns.
- Existing inconsistencies.
- Whether the project already contains a more recent Amplivec design system implementation than the landing page.

If the product already contains a newer canonical Amplivec implementation, follow that source instead of copying the landing verbatim.

Preserve existing functional behavior unless the task explicitly changes it.

Reuse existing shared components before creating new ones.

Prefer global or shared styles over one-off local overrides.

Do not introduce hardcoded visual values when an existing or defined token can be used instead.

Do not modify unrelated screens only to make them more visually consistent.

Do not introduce new visual dependencies unless there is a justified project-level decision.

## Mandatory Principles

Amplivec's design language is currently defined by these stable principles:

- Dark-first visual system.
- High contrast between background and content.
- Deep navy and blue brand base with orange as the main interactive accent.
- Large rounded geometry.
- Soft glow and translucency instead of hard boxed surfaces.
- Clear hierarchy with strong headings and restrained supporting text.
- Bootstrap Icons style iconography or a close equivalent with consistent stroke and visual weight.
- Editorial spacing for institutional pages and denser adaptations for product applications.

Do not convert every incidental detail of the landing into a universal rule. Distinguish between reusable identity and page-specific composition.

## Task Routing

Read the references that match the task:

- Design tokens, palette, typography, spacing, shadows, iconography: `references/foundations.md`
- Shared components and states: `references/components.md`
- UX criteria, hierarchy, forms, feedback, tables, modals, navigation: `references/ux-guidelines.md`
- Accessibility expectations: `references/accessibility.md`
- Responsive behavior and density by interface type: `references/responsive.md`
- HTML, CSS, and Bootstrap implementation guidance: `references/html-bootstrap.md`
- ASP.NET MVC and Razor implementation guidance: `references/razor.md`
- Angular implementation guidance: `references/angular.md`
- Traceability to the source landing, ambiguities, and non-standardized details: `references/source-analysis.md`

## Interface Types

When working on institutional interfaces, keep the branding more expressive:

- Larger headings.
- More negative space.
- Hero sections when appropriate.
- Editorial composition.

When working on product applications, keep the same identity but adapt for operational density:

- Tighter spacing.
- More persistent navigation.
- Clear action hierarchy.
- Table, form, filter, and state patterns optimized for repeated use.

Do not make an administrative application behave like a marketing landing page.

## When Standards Are Incomplete

Some UI patterns are not explicitly implemented in the current landing, including forms, tables, modals, and CRUD-heavy screens.

When a pattern is missing from the source implementation:

1. Derive the solution from the existing Amplivec foundations first.
2. Prefer the same color logic, radius family, spacing rhythm, and interaction model already visible in the landing.
3. Keep the extension minimal and reusable.
4. Document the extension in the changed project when it becomes a shared component or token.
5. Do not present a derived extension as if it were already a historically established standard.

## Final Check

Before considering a UI task complete, verify:

- The result still looks recognizably Amplivec.
- Tokens are reused consistently.
- Interactive states are defined.
- Responsive behavior is preserved.
- Keyboard focus remains visible.
- Color contrast is acceptable for WCAG 2.2 AA where applicable.
- New shared UI code is implemented at the correct scope for the target stack.
