# Apply DESIGN.md tokens across CSS handling

## Problem

`DESIGN.md` (added in #5) encodes the prototype's visual identity as Aksel
design tokens — colors, typography, radii, spacing and component rules, plus a
list of do's/don'ts. The app's stylesheet (`src/pages/_index.module.css`, the
single CSS module imported by every page) predates a complete token layer and
still carries hard-coded hex/rgba values, off-scale radii (12px), and a
`tag-info` style that diverges from the DESIGN.md definition.

## Goal

Make the app's CSS consistently driven by the DESIGN.md tokens, honoring its
do's/don'ts, without changing the intended layout/structure of the prototype.

## Scope

`src/pages/_index.module.css` only. It is the sole stylesheet and is imported by
all four pages (`[service].astro`, `soknad`, `dokument`, `saksoversikt`), so
tokenizing it applies the design system app-wide.

### In scope

1. **Complete token layer in `:root`** — mirror DESIGN.md faithfully using the
   established `--ax-*` naming convention:
   - Colors: add missing tokens the stylesheet needs
     (`--ax-text-accent-contrast`, `--ax-text-logo`, `--ax-bg-canvas` for the
     off-column backdrop). The token layer holds every DESIGN.md token the CSS
     consumes; DESIGN.md remains the source of truth for the full palette.
   - Radii: `--ax-radius-sm` (4px), `--ax-radius-md` (8px), `--ax-radius-full`.
   - Typography: `--ax-font-family` and the type-scale sizes
     (`--ax-font-size-body-small` 1rem, `--ax-font-size-body-medium` 1.125rem,
     `--ax-font-size-heading-small` 1.25rem).
2. **Refactor rule bodies to consume tokens** for colors, radii, and
   font-family/font-size. Every hard-coded value that maps to a token is
   replaced by the token.
3. **DESIGN.md conformance fixes:**
   - `.tagInfo` → `background: info-100` + `color: text-accent` (per the
     DESIGN.md `tag-info` component), replacing the ad-hoc teal.
   - Off-scale `border-radius: 12px` on cards/placeholders → `--ax-radius-md`
     (8px), per "rounded.md (8px): Cards, buttons and larger containers".
   - Nav-red logo via `--ax-text-logo`; white-on-accent text via
     `--ax-text-accent-contrast`; "lean on tokens, not hard-coded hex".

### Out of scope (with rationale)

- **Inline SVG icon colors** in `.astro` files. Their hard-coded strokes already
  equal DESIGN.md token *values* (`#0067C5`=icon-action, `#005bb6`=
  text-accent-subtle, `#49515e`=text-neutral-subtle). SVG presentation
  attributes cannot reference CSS custom properties; tokenizing them would need
  a `currentColor` template refactor — a separate change, not "CSS handling".
- **Spacing scale.** DESIGN.md's 0/2/6/8 base steps are loose; the prototype's
  paddings are composite and pixel-matched to Figma. Forcing them into tokens
  risks visual regressions for little benefit. Left as-is deliberately.
- **Full typography rewrite.** Only font-size values that map 1:1 to a type
  token (16/18/20px) are tokenized (zero rendered change). Bespoke sizes (14px,
  22px, 24px, 36px) and existing line-height/letter-spacing are preserved to
  avoid regressions in the pixel-matched prototype.
- **Decorative/status colors not in DESIGN.md** — the gauge brown
  (`#874e3b`/`#f8e5dd`, a data-viz element), success/warning tag colors, and
  subtle shadow/overlay rgba values are retained (with comments where relevant).

## Verification

Static Astro prototype, no test suite. Verify with `pnpm build` (must succeed)
and a review of `git diff` confirming only intended token substitutions plus the
documented conformance changes (tag-info palette, 12px→8px radii).

## Files touched

- `src/pages/_index.module.css`
- `docs/superpowers/specs/2026-07-15-apply-design-tokens-design.md` (this file)
