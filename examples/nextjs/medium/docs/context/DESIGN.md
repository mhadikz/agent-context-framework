# Design Tokens & Visual Constraints

## Core Theme
- **Color Palette:** Primary `#[Hex]`, Secondary `#[Hex]`, Background `#[Hex]`, defined once in `packages/ui`.
- **Typography:** [e.g., Inter Sans for UI, JetBrains Mono for code blocks].
- Spacing, radius, and focus rings come from `packages/ui`. Feature code does not redefine them.

## Component Layout Rules
- All user-facing views must be responsive across mobile, tablet, and desktop viewports.
- Shared components live in `packages/ui` and perform no I/O and no authorization.
- Pages compose Server Components. Client components receive view models that are already authorized.
- Interactive states (hover, focus, disabled, loading, empty, error) are explicit and high-contrast.
- Do not build a second button, dialog, or form style inside a feature when `packages/ui` already has one.
- Personalized responses are not cached as public CDN content.
