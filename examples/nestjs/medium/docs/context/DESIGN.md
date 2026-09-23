# Design Tokens & Visual Constraints

## Core Theme
- This repository is an API. Do not add a product frontend here.
- **Color Palette:** Not used by the service. A future admin UI uses Primary `#[Hex]`, Secondary `#[Hex]`, Background `#[Hex]` from one token source.
- **Typography:** API `message` strings follow `docs/context/VOICE.md`.

## Component Layout Rules
- The stable layout consumers depend on is `/v1`: resource names, pagination, and one error envelope.
- Do not add a module-specific error JSON or an unversioned breaking field.
- Any future admin UI must be responsive, must use the shared tokens, and must call the application service instead of reimplementing rules.
- Hover, focus, and disabled states on that UI must be explicit and high-contrast.
