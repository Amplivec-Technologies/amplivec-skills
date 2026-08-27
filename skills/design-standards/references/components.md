# Components

Use these component rules to keep Amplivec interfaces visually consistent.

## General Rules

Always check whether the target project already has a shared implementation for the component.

If a shared implementation exists and already fits Amplivec's current standards, extend it instead of recreating it.

If a component is not present in the landing, derive it from the documented foundations instead of inventing an unrelated style.

## Buttons

The landing establishes two clear button directions.

### Primary button

Verified implementation:

- Fill: `color.brand.primary`
- Text: `color.brand.white`
- Hover and active: `color.brand.accent`

Standardize this as:

- Primary actions use a filled button.
- Default primary fill may start from blue.
- Hover, active, and high-attention emphasis should move toward orange.
- Button text must remain high contrast.

### Light or inverse button

Verified implementation:

- Fill: `color.brand.white`
- Text: `color.brand.black`
- Hover and active: `color.brand.accent` with light text

Standardize this as:

- Use inverse buttons on dark hero sections or high-contrast banners.
- Keep inverse buttons limited to contexts where strong contrast is needed.

### Button rules

- Use large rounded geometry for prominent CTAs.
- Pair icons with text when the action benefits from faster scanning.
- Keep one primary action per visual group whenever possible.
- Secondary actions should not visually overpower the primary action.
- Disabled buttons must look intentionally inactive and must not use hover-like emphasis.

## Links

Verified behavior:

- Navigation links are light by default.
- Hover and focus switch to orange.
- Footer links inherit text color and underline on hover.

Standardize this as:

- Interactive text should always have a visible hover and focus treatment.
- Orange is the preferred hover and focus color for links in dark contexts.
- Underlines are appropriate for lower-emphasis footer or textual links.
- Do not rely on color alone when context requires stronger affordance.

## Cards and Panels

The card is one of the clearest reusable patterns in the landing.

Verified characteristics:

- No visible border.
- Rounded corners at `1.25rem`.
- Translucent dark surface.
- Soft glow shadow.
- Strong title, softened body copy.
- Slight upward movement on hover.

Standardize this as:

- Use cards for grouped content, features, modules, metrics, and previews.
- Preserve the rounded dark surface language.
- Use hover lift only when the card is interactive or benefits from discoverability.
- Static informational cards do not need exaggerated motion.

## Navigation

Verified navbar characteristics:

- Sticky top.
- Dark translucent background.
- Backdrop blur.
- Subtle bottom divider.
- Logo on the left, navigation on the right.
- Mobile collapse via Bootstrap.

Standardize this as:

- Primary navigation should remain stable, legible, and visually separated from scrolling content.
- On dark shells, prefer translucent navigation with blur only when readability remains strong.
- Keep the logo area simple and uncluttered.
- Keep top-level navigation labels concise.

## Icon Containers

Verified patterns:

- Light desaturated background.
- Orange icon.
- Rounded `1rem` container.
- Fixed square dimensions.

Standardize this as:

- Use icon containers to frame features, categories, modules, or empty states.
- Keep container size consistent within a section.
- Do not place unrelated colors in icon containers unless the product uses a documented semantic palette.

## Forms

The current landing does not implement forms.

Use these derived standards:

- Inputs, selects, and textareas should reuse the dark surface language.
- Text must preserve high contrast against dark fields.
- Focus state should be clearly visible and should align with the orange interaction accent.
- Validation and helper text must remain readable without depending exclusively on color.
- Group labels, help text, and errors consistently.
- Avoid per-view form styling when a shared field style can be defined once.

### Derived visual direction

- Field radius should stay within the established rounded family.
- Field borders should remain subtle at rest.
- Focus should be more visible than rest, using contrast, outline, glow, or border reinforcement.
- Primary submit actions should follow the primary button rules.

## Tables

The current landing does not implement tables.

Use these derived standards for product applications:

- Keep tables visually lighter than dense enterprise grids.
- Use spacing and alignment before heavy borders.
- Preserve a dark background family.
- Make sortable, filterable, or clickable affordances explicit.
- Keep row hover subtle.
- Avoid making tables the only path to critical actions on small screens.

## Badges and Indicators

The landing does not implement badges.

Use these derived standards:

- Keep badges compact, readable, and clearly subordinate to headings.
- Prefer pill or high-radius geometry.
- Use semantic meaning consistently when semantic colors are introduced.
- Do not use badge colors as decoration without a stable meaning.

## Modals and Dialogs

The landing does not implement modals.

Use these derived standards:

- Modal surfaces should feel like elevated variants of cards or panels.
- Preserve the dark-first surface language.
- Maintain clear separation between title, body, and actions.
- Primary and secondary actions must remain visually distinct.
- Destructive confirmations must be explicit and should never rely on ambiguous icon-only actions.

## States

When a component supports interactive states, define them deliberately:

- `hover`: stronger emphasis, usually toward orange or slightly increased contrast.
- `focus`: visible outline or ring sufficient for keyboard users.
- `active`: stable pressed state distinct from hover.
- `disabled`: reduced emphasis without harming readability.
- `loading`: indicate progress without layout shift when possible.

Never leave focus styling to browser defaults if it becomes invisible against the dark UI.
