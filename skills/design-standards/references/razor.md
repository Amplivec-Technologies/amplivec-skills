# ASP.NET MVC and Razor Guidance

Use this reference when applying Amplivec design standards in MVC, Razor views, or related server-rendered interfaces such as Membriana.

## General Rules

- Keep shared UI in layouts, partials, view components, tag helpers, or equivalent reusable structures.
- Centralize design tokens and shared component styles in global assets.
- Do not duplicate CSS per view when the pattern is meant to be shared across the product.

## Layout and Shared Structure

- Use the main layout to establish shell-level identity, including background family, typography, and global navigation zones.
- Keep repeated navigation, footer, alert, modal, and form patterns in shared partials or reusable server-side components.
- Preserve consistency across Portal, BackOffice, Admin, or equivalent areas unless a deliberate product distinction exists.

## CSS and Asset Strategy

- Store brand tokens in a shared stylesheet.
- Define shared component classes once.
- Keep feature-specific view styles isolated only when they are truly local to the feature.
- Avoid creating a new button, card, or form style in each area of the application.

## Razor Markup

- Keep markup semantic and predictable.
- Reuse established class names for equivalent components.
- Preserve accessible labels, validation messaging, and heading structure.
- Avoid embedding style attributes in views except for exceptional one-off cases.

## Forms and Validation

- Keep label, field, helper, and validation structures consistent across views.
- Align MVC validation summaries and field messages with the same visual language used by the rest of the interface.
- Preserve server-side validation behavior while improving visual clarity.

## Progressive Normalization

Many MVC products evolve screen by screen.

When normalizing an existing application:

- Improve the requested surface first.
- Extract shared styles when repetition is already clear.
- Do not refactor the complete UI shell unless the task asks for it.
- Prefer incremental convergence toward the shared design system.
