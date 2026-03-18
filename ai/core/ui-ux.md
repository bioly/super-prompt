# UI / UX Guidelines

Interface design principles and conventions. Apply these when generating, modifying, or reviewing UI code.

> This file is a template. Remove sections that do not apply to your project type (e.g. a pure API service has no UI concerns).

---

## Principles

- **Function before form** — the UI must work correctly before it looks polished.
- **Obvious over clever** — users should never have to guess what an element does.
- **Consistent over novel** — reuse existing patterns and components before inventing new ones.
- **Accessible by default** — accessibility is not a feature, it is a baseline requirement.
- **Responsive by default** — every UI change must work on the full range of supported screen sizes.

---

## Component Conventions

- Use the project's established component library. Do not introduce a second one.
- Prefer composition over inheritance. Build complex UI from simple, single-purpose components.
- Components own their own local state. Shared or server state lives in the appropriate layer.
- Name components by what they render, not by their position: `UserCard`, not `RightSidebarTop`.
- Keep components short. If a component file exceeds ~150 lines, consider extracting sub-components.

---

## Accessibility (a11y)

These are requirements, not suggestions:

- All interactive elements (`button`, `a`, custom controls) are keyboard-focusable and operable.
- All images have meaningful `alt` text. Decorative images use `alt=""`.
- Form inputs have associated `<label>` elements (or `aria-label`/`aria-labelledby`).
- Colour is not the only means of conveying information.
- Minimum contrast ratio: **4.5:1** for normal text, **3:1** for large text (WCAG AA).
- Dynamic content changes are announced via ARIA live regions where appropriate.
- Test with a screen reader before shipping significant UI changes.

---

## Responsive Design

- Mobile-first: design for the smallest screen, then enhance for larger.
- Use relative units (`rem`, `%`, `vw`) for layout; avoid fixed pixel widths on containers.
- Breakpoints: use the project's established breakpoint scale. Do not introduce custom breakpoints.
- Touch targets: minimum 44×44px on mobile.

---

## State and Feedback

| State | UI requirement |
|---|---|
| Loading | Show a loading indicator. Disable interactive elements to prevent duplicate actions. |
| Empty | Show an intentional empty state, not a blank screen. |
| Error | Show a clear, human-readable error message. Provide a recovery action where possible. |
| Success | Confirm the action completed. Do not leave the user guessing. |
| Disabled | Visually distinguish disabled elements. Explain why they are disabled if not obvious. |

---

## Forms

- Validate inline (on blur) with clear error messages adjacent to the field.
- Show all required fields as required. Do not rely on post-submit errors alone.
- Preserve user input on validation failure. Never clear a form on error.
- Submit buttons are disabled while submission is in progress.
- Use appropriate input types (`email`, `tel`, `number`) for built-in browser validation and mobile keyboards.

---

## Copy and Language

- Write in the second person: "Your account", "You have 3 messages".
- Use plain language. No jargon the user would not know.
- Error messages say what went wrong and what the user can do: "Payment failed. Check your card details and try again." Not: "Error code 4002".
- Use sentence case for UI text. Title Case for proper nouns and navigation labels only.

---

## Performance

- Images: use modern formats (WebP, AVIF), compress, and specify dimensions to avoid layout shift.
- Lazy-load below-the-fold images and non-critical components.
- No layout shift after initial render (target CLS < 0.1).
- Largest Contentful Paint target: < 2.5s on a mid-range device on 4G.

---

## Design Tokens

- Colours, spacing, typography, and radii come from the design token system. Do not hardcode hex values or pixel sizes.
- If a value is not in the token system, flag it — do not add one-off values silently.
