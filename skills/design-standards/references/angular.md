# Angular Guidance

Use this reference when applying Amplivec design standards in Angular applications such as Amplivec Hub or future frontends.

## General Rules

- Keep design tokens in global styles or a clearly shared theming layer.
- Build reusable presentational components for repeated UI patterns.
- Keep feature-specific business rules separate from visual primitives.
- Do not let each feature module define its own button, card, form, or badge language.

## Component Strategy

- Prefer reusable UI components for repeated patterns such as buttons, cards, empty states, alerts, inputs, dialogs, and tables.
- Keep component APIs focused on behavior and semantic variants rather than arbitrary styling knobs.
- Avoid over-parameterized components that become visual escape hatches.

## Styling Strategy

- Centralize tokens for colors, typography, spacing, radius, and shadows.
- Keep shared classes or mixins available for cross-cutting patterns.
- Use local component styles for local layout and composition, not to redefine the design system repeatedly.

## Library Neutrality

- Do not impose Angular Material, Bootstrap, Tailwind, PrimeNG, or another UI library unless the project already uses it or a deliberate architectural decision exists.
- If a library is already present, adapt it to Amplivec's tokens and interaction language instead of mixing unrelated defaults.

## State and Accessibility

- Ensure component states are consistent across the app.
- Preserve visible focus styling under view encapsulation or theme overrides.
- Keep icon-only controls labeled.
- Ensure responsive behavior is built into reusable components rather than solved repeatedly by feature code.

## Large Product Screens

Angular applications will often need denser product UI than the landing.

When building dashboards, CRUD views, or filters:

- Preserve the same dark-first palette and rounded geometry.
- Increase density without abandoning clarity.
- Reuse shared action, table, empty-state, and feedback patterns.
- Avoid screen-specific visual improvisation when a shared component can absorb the need.
