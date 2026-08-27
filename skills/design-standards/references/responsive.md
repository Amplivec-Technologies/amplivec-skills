# Responsive Design

Amplivec's current landing relies primarily on Bootstrap's responsive grid and utility classes. Treat that as the initial responsive baseline.

## Core Rules

- Build mobile-first.
- Preserve content hierarchy across breakpoints.
- Do not create separate visual languages for desktop and mobile.
- Reduce density on smaller screens without removing core actions.

## Verified Patterns From the Landing

- Navbar expands on large screens and collapses on smaller screens.
- Content sections use centered containers and grid columns that stack vertically on smaller widths.
- Footer content shifts from horizontal to vertical layout.
- Cards and content groups rely on Bootstrap gaps and column stacking instead of custom media queries.

## Standardization Decisions

- Prefer grid, flex, and utility-based responsive behavior over many one-off breakpoint overrides.
- Let layouts stack naturally before inventing bespoke mobile compositions.
- Preserve readable spacing even when reducing density.
- Keep headings large enough to preserve brand character, but avoid oversized hero typography in dense application contexts.

## Institutional Interfaces

- Allow more whitespace on larger viewports.
- Allow centered composition and large hero headlines.
- Keep CTAs visually prominent.

## Product Applications

- Increase density compared with the landing.
- Preserve the same color, radius, icon, and action language.
- Adapt navigation, filters, tables, and forms for repeated workflows.
- Avoid horizontal overflow as the default solution.

## Tables and Data-Dense Screens

- Decide early how key data behaves on small screens.
- Collapse low-priority columns before compressing everything uniformly.
- Preserve access to row actions.
- Consider cards or stacked summaries only when they improve usability, not just because the table is wide.

## Interactive States Across Breakpoints

- Hover cannot be the only discoverability mechanism.
- Focus states must remain visible on all breakpoints.
- Sticky or fixed elements must not obscure key content on smaller screens.
