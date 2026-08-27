# HTML, CSS, and Bootstrap Guidance

Use this reference when working on Amplivec interfaces built with HTML, CSS, and optionally Bootstrap.

## General Rules

- Keep design tokens in CSS custom properties.
- Prefer shared classes and component styles over inline styles.
- Avoid hardcoded colors, spacing, radius, and shadow values when a token exists.
- If Bootstrap is present, extend it coherently instead of fighting it one component at a time.

## Tokens

- Keep the canonical brand tokens in a central stylesheet similar to `variables.css`.
- Add conceptual token names when the project matures, but preserve traceability to the current brand values.
- Do not scatter duplicate token definitions across page-specific stylesheets.

## Bootstrap Integration

The landing already uses Bootstrap 5 and overrides component variables for buttons.

Follow that pattern:

- Override Bootstrap component custom properties before replacing components entirely.
- Use Bootstrap layout primitives for containers, grids, spacing, and responsive utilities.
- Create Amplivec-specific classes only when Bootstrap primitives are not enough or when the pattern is semantically reusable.

## Shared Partials and Layout

- Keep navbar, footer, and other repeated structures shared.
- Centralize repeated visual primitives such as cards, icon chips, badges, alerts, and form controls.
- Do not duplicate the same component markup with slightly different classes in many pages.

## CSS Organization

- Use one place for tokens.
- Use shared component styles for reusable UI.
- Keep page-specific styles focused on layout or one-off composition.
- Remove dead styles instead of leaving parallel unused variants.

## State Styling

- Define `:hover`, `:focus`, `:active`, and disabled states for custom components.
- Do not rely on default browser focus if it becomes invisible in the dark theme.
- Keep state behavior consistent between buttons, links, and custom interactive cards.

## When Extending Missing Patterns

If the current project needs inputs, tables, alerts, drawers, or modals that the landing does not yet show:

- Add them as reusable shared components.
- Base them on the documented Amplivec foundations.
- Keep their tokens centralized.
- Avoid introducing a second visual system through a plugin or page-local CSS.
