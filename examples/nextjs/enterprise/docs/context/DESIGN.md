# Design Tokens & Visual Constraints

## Core Theme
- **Color Palette:** Taken only from the design-system package. Do not add local hex values for product UI.
- **Typography:** [The design-system families]. Do not load a second font for a single route.

## Component Layout Rules
- All user-facing views must be responsive across mobile, tablet, and desktop viewports.
- Compose pages from design-system primitives. A visual one-off is a design-system change, not a local CSS fork.
- Server Components own the page. Client components receive authorized view models and contain no secrets.
- Hover, focus, disabled, loading, empty, and error states come from the design system and meet its contrast rules.
- Do not cache personalized HTML or data as public CDN content.
- Motion and focus order follow the design system. Do not remove focus outlines.
