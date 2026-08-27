# Source Analysis

This file documents how the current `amplivec-landing-page` was interpreted when defining `design-standards`.

## Files Analyzed

- `css/variables.css`
- `css/style.css`
- `index.html`
- `partials/navbar.html`
- `partials/footer.html`
- `js/shared-layout.js`

## Deliberate and Reusable Decisions Detected

- Dark-first brand presentation.
- Navy-to-deep-blue background family.
- Orange used for emphasis, hover, focus-adjacent states, and highlighted text.
- Blue used as a stable brand base and primary button fill.
- Rounded geometry across cards, icon containers, pills, and indicators.
- Soft glow and translucency instead of hard borders.
- Clear text hierarchy using Bootstrap display sizes plus heavier custom section titles.
- Bootstrap Icons as the current icon library.
- Shared navbar and footer partials.
- Bootstrap grid and utilities as the core responsive mechanism.

## Circumstantial Landing-Specific Details

These were treated as page composition choices, not universal rules:

- Full-screen sections across most of the landing.
- Single centered hero CTA.
- Carousel presentation for values.
- Marketing-oriented section sequencing.
- Large amount of vertical whitespace appropriate for institutional communication.

## Inconsistencies or Gaps Not Promoted to Standards

- `--nav-height` exists but is not actively used in the current CSS.
- `--brand-night`, `--brand-cloud`, and `--brand-glow` are defined but not clearly exercised in the current page.
- There is no explicit custom typography token system beyond Bootstrap usage and a few CSS adjustments.
- The landing does not define forms, tables, badges, alerts, or modals.
- There are no custom media queries, so responsive intent is inferred mainly from Bootstrap classes.
- Focus states are only partially explicit because many interactions rely on Bootstrap defaults plus link color changes.

## Ambiguities Requiring Human Judgment

- Whether Amplivec should continue using Bootstrap's default sans-serif stack or adopt a custom brand typeface later.
- Whether product applications should keep the full gradient shell or simplify to flatter dark surfaces.
- Whether semantic palettes for success, warning, danger, and info should remain product-specific until formally defined.
- Whether blue should remain the primary fill for most CTAs in applications, or whether orange should take over that role more broadly.

## Deliberate Standardization Choices

- Orange was standardized as the main interaction and emphasis accent because it consistently marks hover, focus-adjacent, and highlighted content.
- Blue was retained as the primary brand anchor and initial primary button fill because it is explicitly tokenized and used for `.btn-primary`.
- Rounded geometry and soft glow were promoted to core identity because they repeat across components and are not isolated one-off choices.
- Bootstrap itself was not promoted as the design system. It was treated as the current implementation vehicle.

## Deliberately Excluded From the Standard

- The values carousel as a required interaction pattern, because it is content-specific and not a general UI need.
- Full-screen sections as a mandatory layout strategy, because they fit a landing page better than dense product applications.
- Marketing copy structure, because editorial tone belongs to content strategy rather than to the design system.
- Any assumption that missing patterns such as forms or tables should inherit raw Bootstrap defaults without Amplivec adaptation.
