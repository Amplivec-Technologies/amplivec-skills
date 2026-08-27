# UX Guidelines

Use these rules when designing or modifying the behavior and composition of Amplivec interfaces.

## Hierarchy

- Make the main task or message obvious within the first screenful.
- Use strong heading hierarchy and restrained secondary text.
- Keep supporting copy concise.
- Let spacing, grouping, and alignment do most of the hierarchy work.

Institutional screens may be more editorial.

Product screens should be more operational and scan-friendly.

## Primary, Secondary, and Destructive Actions

- Each area should have a clearly identifiable primary action.
- Primary actions should use the strongest filled style.
- Secondary actions should remain available but visually subordinate.
- Destructive actions must be clearly separated from neutral actions and should require confirmation when the action is hard to reverse.

Do not present multiple competing primary buttons inside the same decision zone unless the workflow truly requires it.

## Forms and Validation

- Prefer clear labels over placeholder-only inputs.
- Show required context before the user submits whenever practical.
- Keep validation messages near the affected field.
- Explain what went wrong and how to fix it.
- Preserve entered data after validation failures whenever possible.
- Use input grouping and spacing consistently.

For long or high-risk forms:

- Break information into logical sections.
- Keep submit actions easy to find.
- Distinguish save, cancel, and destructive actions clearly.

## Feedback

- Every meaningful operation should produce feedback.
- Success states should confirm what happened.
- Error states should explain the issue in plain language.
- Warning states should help the user avoid mistakes before they commit them.
- Background operations should expose loading or progress feedback when waiting is noticeable.

Avoid silent failure.

## Empty States

- Empty states should explain why the area is empty.
- If there is a next best action, show it.
- Use iconography only as support, not as the sole explanation.
- Keep empty states calmer than hero marketing sections.

## Loading States

- Prefer preserving layout while data loads.
- Use spinners sparingly for very small regions or indeterminate waits.
- Prefer skeletons or reserved layout blocks when screen structure matters.
- Prevent repeated submissions during in-flight actions.

## Navigation

- Keep navigation labels short and predictable.
- Reflect the product structure rather than internal implementation details.
- Show the current location clearly.
- Avoid navigation patterns that behave differently across similar sections without reason.

Institutional navigation may prioritize exploration.

Product navigation should prioritize orientation, repeatability, and efficient return to frequent tasks.

## Modals and Confirmation

- Use modals for short focused tasks, confirmations, or contextual detail.
- Do not hide critical workflow steps behind unnecessary modal layers.
- Confirm destructive or high-impact actions explicitly.
- Keep modal action labels specific, such as `Eliminar miembro` instead of `Aceptar`.

## Tables, Filters, and Search

- Use tables when comparison across rows matters.
- Keep filters close to the dataset they affect.
- Make active filters visible.
- Search should be easy to locate and clearly scoped.
- On smaller screens, prioritize the columns or fields users act on most.

## Error Prevention

- Prevent invalid actions earlier when possible.
- Use defaults that reduce accidental mistakes.
- Make irreversible consequences explicit.
- Avoid ambiguous labels like `Procesar` when a more specific verb is available.

## Consistency Rules

- Similar actions should look and behave similarly across products.
- Repeated business objects should use the same labels and visual treatment within a project.
- Existing visual debt must not be copied automatically into new work.
- Preserving a product decision is not the same as preserving an accidental inconsistency.
