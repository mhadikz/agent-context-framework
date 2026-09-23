# Design Tokens & Visual Constraints

## Core Theme
- This service has no product UI. Do not add a frontend framework to this repository.
- **Color Palette:** Not applicable until a status or admin UI is explicitly in scope. If it is added, Primary `#[Hex]`, Secondary `#[Hex]`, Background `#[Hex]`.
- **Typography:** Human-readable API messages follow `docs/context/VOICE.md`, not a visual theme.

## Component Layout Rules
- The client-facing layout is the HTTP contract: consistent resource names, status codes, and the error envelope in `docs/context/CONTRACT.md`.
- Do not invent a second error JSON shape for a single module.
- List responses are paginated and ordered by a stable key.
- If an admin or docs page is later added, it must be responsive, use one spacing scale, and must not reimplement rules the API already enforces.
- Interactive states on any future UI (hover, focus, disabled) must be explicit and high-contrast.
