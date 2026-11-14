# UI Kit and Styling Assessment

## Decision
The frontend uses React (JavaScript) with Vite and Tailwind CSS.
Zod validates form and API data.

## Why Tailwind CSS
- Utility classes let us build custom screens quickly without fighting
  a library's default look.
- Responsive design is built in: classes like sm:, md: and lg: apply
  styles from a screen width upwards, so we can build mobile-first.
- Only the CSS we use ends up in the final build.

## Responsiveness
- Many families and patients will use phones, so every screen is
  designed for small screens first, then enlarged.
- We will test at phone, tablet and laptop widths.

## Accessibility
- Tailwind is a styling tool, not a component library, so it does not
  make a page accessible by itself. We must use semantic HTML,
  labelled form fields, visible focus styles (focus-visible:) and
  enough colour contrast.
- Components like modals, dropdowns and menus are easy to get wrong
  for keyboard and screen-reader users.

## Component library options
| Option | Notes |
|---|---|
| Headless UI / Radix UI | Unstyled, accessible behaviour; we add Tailwind styles |
| shadcn/ui | Ready-made components built on Radix and Tailwind, copied into our repo |
| MUI or Chakra UI | Full component libraries with their own styling |
| Bootstrap | Familiar, but a different styling system from Tailwind |

## Recommendation
Keep Tailwind and add an accessible component layer (for example
Radix-based components) for complex pieces such as modals and menus.


## Risks
- Inconsistent styling if the team does not agree on shared patterns.
- Accessibility gaps if complex components are built from scratch.