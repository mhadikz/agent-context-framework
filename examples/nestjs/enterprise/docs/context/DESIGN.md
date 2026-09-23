# Design Tokens & Visual Constraints

## Core Theme
- This repository is a platform API, not a product UI.
- **Color Palette:** Any internal admin surface uses the organization design system only. Do not add a local palette.
- **Typography:** Client `message` text follows `docs/context/VOICE.md` and the shared error catalog.

## Component Layout Rules
- Consumers depend on versioned resources, pagination, and one error envelope. That shape is the layout. Do not fork it per context.
- A breaking visual or JSON change is a new version, not a silent edit.
- Future admin UI must be responsive, must use the platform design system, and must call the owning context instead of querying its tables.
- Hover, focus, disabled, and error states on that UI come from the design system and stay high-contrast.
