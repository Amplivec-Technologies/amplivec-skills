# Accessibility

Use WCAG 2.2 AA as the target unless the product defines a stricter standard.

## Color and Contrast

- Verify that text remains readable against dark backgrounds and translucent surfaces.
- Do not rely only on the pale secondary text color for small critical content.
- Confirm that orange accents still provide adequate contrast in their actual context.
- Do not communicate status using color alone.

## Focus and Keyboard Navigation

- Every interactive control must have a visible focus state.
- Keyboard users must be able to navigate all controls in a logical order.
- Sticky headers, drawers, and modals must not trap or hide focus incorrectly.
- Icon-only actions must have an accessible name.

## Text and Readability

- Keep line lengths reasonable for long text blocks.
- Preserve strong contrast for core content.
- Do not shrink body text merely to fit more UI.
- Use headings in a meaningful semantic order.

## Icons and Images

- Decorative icons should be hidden from assistive technology.
- Functional icons require an accessible label through adjacent text or explicit attributes.
- Logos and meaningful images require appropriate alternative text.

## Forms

- Associate labels programmatically with fields.
- Expose validation errors in text, not only color or icon.
- Keep helper text close to the associated field.
- Ensure disabled states are distinguishable without becoming unreadable.

## Motion

- Keep motion subtle.
- Avoid animations that distract from task completion.
- Respect reduced-motion preferences when a project supports custom motion.

## Responsive Accessibility

- Mobile navigation, drawers, and collapsed sections must remain keyboard and screen-reader accessible.
- Do not hide important content behind hover-only interactions.
- Ensure touch targets are large enough for repeated use on mobile.
