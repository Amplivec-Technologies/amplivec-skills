# Design Foundations

Use these foundations as the canonical visual base for Amplivec interfaces.

## Identity Summary

Amplivec currently presents itself through a dark-first interface built on deep navy backgrounds, restrained blue branding, and orange emphasis for interaction and key highlights.

The visual tone is:

- Technical.
- Modern.
- High-contrast.
- Rounded rather than sharp.
- Softly luminous rather than flat.

## Color System

The following values are directly verified in `amplivec-landing-page/css/variables.css` and should be treated as the initial canonical tokens.

### Core brand tokens

| Conceptual token | CSS token | Value | Usage |
| --- | --- | --- | --- |
| `color.brand.white` | `--brand-white` | `#edeee8` | Primary light text, inverse surfaces |
| `color.brand.pale` | `--brand-pale` | `rgba(237, 238, 232, 0.7)` | Secondary text on dark backgrounds |
| `color.brand.shine` | `--brand-shine` | `rgba(237, 238, 232, 0.05)` | Hover glow overlay |
| `color.brand.black` | `--brand-black` | `#00072c` | Primary dark background |
| `color.brand.smoke` | `--brand-smoke` | `rgba(0, 7, 44, 0.7)` | Translucent navbar/background overlay |
| `color.brand.shadow` | `--brand-shadow` | `rgba(0, 7, 44, 0.2)` | Card surface tint |
| `color.brand.gray` | `--brand-gray` | `#4b5579` | Neutral support color |
| `color.brand.dark` | `--brand-dark` | `#061651` | Gradient background midpoint |
| `color.brand.night` | `--brand-night` | `rgba(6, 22, 81, 0.7)` | Dark overlay variant |
| `color.brand.primary` | `--brand-primary` | `#123498` | Brand blue, primary action base |
| `color.brand.neon` | `--brand-neon` | `rgba(18, 52, 152, 0.1)` | Soft blue glow shadow |
| `color.brand.sky` | `--brand-sky` | `#dde2f6` | Light support surface, icon chips |
| `color.brand.cloud` | `--brand-cloud` | `rgba(221, 226, 246, 0.5)` | Light overlay variant |
| `color.brand.accent` | `--brand-accent` | `#f15a2b` | Main highlight, interactive emphasis |
| `color.brand.glow` | `--brand-glow` | `rgba(241, 90, 43, 0.2)` | Accent glow variant |

### Practical color roles

Use these conceptual roles when implementing in any stack:

- Primary background: `color.brand.black`
- Elevated dark background: `color.brand.dark`
- Translucent surface: `color.brand.shadow` or `color.brand.smoke` depending on density
- Primary text on dark: `color.brand.white`
- Secondary text on dark: `color.brand.pale`
- Brand support color: `color.brand.primary`
- Main interaction accent: `color.brand.accent`
- Light support surface: `color.brand.sky`

### Standardization decisions

Use orange as the main attention and interaction color.

Use blue as a brand anchor and default primary button fill, but do not let blue compete with orange for micro-interactions.

Keep the UI predominantly dark unless a project intentionally defines a light variant while preserving these same brand relationships.

## Backgrounds and Surfaces

The landing establishes these stable patterns:

- Main pages use a dark horizontal gradient from `color.brand.black` to `color.brand.dark` and back.
- Elevated surfaces are translucent rather than solid white or solid gray.
- Small icon containers use a light desaturated surface with orange icons.

Standardize this as:

- Page shells may use gradients for institutional pages.
- Product applications may simplify the page shell to a flat dark background, but should preserve the same dark family.
- Cards, panels, modals, and sidebars should feel related through subtle translucency, soft contrast, or low-intensity glow rather than harsh borders.

## Typography

The landing does not define a custom font family. It inherits Bootstrap 5's default sans-serif stack.

Treat the current canonical typography direction as:

- Neutral sans-serif.
- System-friendly.
- Clear and practical rather than decorative.
- Strong weight for headings.

### Typographic hierarchy verified from usage

- Hero title: `display-4`, `fw-bold`, compact line height.
- Section title: `display-6` plus custom `font-weight: 800` and slight negative tracking.
- Card titles: `h4` or `h5`.
- Labels and microcopy: uppercase small text for section overlines.
- Body copy: standard Bootstrap body size with softened contrast.

### Standardization decisions

- Prefer a neutral sans-serif family compatible with Bootstrap's default stack unless a newer Amplivec font decision exists.
- Use strong hierarchy through weight, size, and spacing rather than through many font families.
- Reserve the heaviest weights for headings, section titles, and key actions.
- Keep secondary text visually softer through color, not through tiny sizes.
- Preserve compact headline tracking for section titles when a stronger brand voice is needed.

## Spacing

The landing relies mainly on Bootstrap's spacing scale (`p-4`, `py-5`, `gap-3`, `g-4`, `mb-5`) rather than a custom spacing token map.

Treat that as the current standard:

- Reuse a consistent spacing scale.
- Prefer Bootstrap's spacing tokens or an equivalent abstraction in other stacks.
- Avoid arbitrary one-off margins and paddings.

### Verified spacing anchors

- Card padding commonly uses `1.5rem` via `p-4`.
- Section vertical rhythm commonly uses `3rem` via `py-5`.
- Common gaps use `1rem` and `1.5rem`.
- Carousel inner padding uses `2rem 1.5rem`.
- Navigation height token is `72px` via `--nav-height`.

### Extension rule

If a project needs a formal spacing token scale, derive it from the Bootstrap rhythm already present in the landing instead of introducing a conflicting scale.

## Radius and Geometry

The landing strongly favors rounded geometry.

Verified values:

- Major cards: `1.25rem`
- Icon containers: `1rem`
- Pills and hero CTA: fully rounded via `rounded-pill`
- Carousel indicators: `999px`

Standardize this as:

- Default component family: rounded.
- Large content surfaces: around `1.25rem`.
- Small utility containers and inputs: around `1rem`.
- High-emphasis pills, chips, and indicators: fully rounded.

## Borders and Dividers

The landing uses borders sparingly.

- Navbar and footer use subtle Bootstrap border dividers.
- Cards remove visible borders entirely.

Standardize this as:

- Prefer contrast, spacing, and shadow before heavy borders.
- Use borders as separators, not as the main surface definition.
- When a border is necessary, keep it subtle and within the same dark-light family.

## Shadows and Glow

The landing uses soft glow rather than dense drop shadows.

Verified patterns:

- Card resting shadow: `0 0.5rem 2rem color.brand.neon`
- Card hover shadow: `0 1rem 2rem color.brand.shine`

Standardize this as:

- Use broad, soft, low-opacity shadows.
- Prefer glows that reinforce the brand palette.
- Avoid heavy black shadows, hard ambient blur, or material-style elevation unrelated to the rest of the system.

## Motion and Transitions

Verified timings:

- Card hover transitions: `0.2s ease`
- Carousel movement: `0.6s ease-in-out`

Standardize this as:

- Keep motion subtle and supportive.
- Use short transitions for hover and focus changes.
- Use longer easing only for larger structural movement such as carousels or panel transitions.
- Avoid flashy or decorative motion that competes with usability.

## Iconography

The landing uses Bootstrap Icons.

The visual style is:

- Simple.
- Geometric.
- Readable at small and medium sizes.
- Compatible with text-driven interfaces.

Standardize this as:

- Prefer a single icon family across a product.
- Bootstrap Icons is the current preferred implementation when no other canonical decision exists.
- Keep icons aligned to text baselines or centered within consistent containers.
- Use icons to reinforce labels, not to replace them when the action is not universally obvious.
- Decorative icons should be hidden from assistive technology when they add no meaning.

### Typical icon sizing inferred from the landing

- Feature icon chips: `1.5rem`
- Product icon chips: `1.75rem`
- Footer social icons: `fs-4`

Do not mix unrelated icon styles, stroke weights, or corner language inside the same interface.
