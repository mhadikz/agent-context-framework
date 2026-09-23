# Design Tokens & Visual Constraints

## Core Theme
- **Color Palette:** Primary `#[Hex]`, Secondary `#[Hex]`, Background `#[Hex]`.
- **Typography:** [e.g., Inter Sans for UI, JetBrains Mono for code blocks].
- Use one spacing scale. Do not invent one-off pixel values per page.

## Component Layout Rules
- All user-facing views must be responsive across mobile, tablet, and desktop viewports.
- Pages and layouts are Server Components. Interactive leaves are client components and contain no data access and no business rules.
- Use `next/image` and the Metadata API. Do not hand-roll document head tags or raw `<img>` for content images.
- Every route that can fail has an `error.tsx`. Every slow route has a `loading.tsx`. Empty, error, and disabled states are visible and high-contrast.
- Focus, hover, and disabled states are explicit. Interactive controls have an accessible name.
- Do not fetch server data in a client component and again on the server for the same view.
