---
version: alpha
name: Min side – Innlogget produktside
description: >-
  Design tokens for Nav's innlogget produktside (logged-in product page)
  prototype, derived from the Aksel design system. Reference frame:
  "Konsept - IP - dagpenger" (Figma node 2137:40180).
colors:
  # Semantic roles
  primary: "#2176d4"
  secondary: "#49515e"
  tertiary: "#c30000"
  neutral: "#ffffff"
  # Surfaces
  bg-default: "#ffffff"
  bg-raised: "#ffffff"
  bg-accent-moderate: "#e4eeff"
  bg-accent-strong: "#2176d4"
  bg-neutral-softA: "#001a330a"
  bg-neutral-moderateA: "#000e2913"
  info-100: "#eef6fc"
  # Text
  text-neutral: "#202733"
  text-neutral-subtle: "#49515e"
  text-accent: "#002459"
  text-accent-subtle: "#005bb6"
  text-accent-contrast: "#ffffff"
  text-logo: "#c30000"
  # Brand & action
  brand-blue-1000: "#002942"
  icon-action: "#0067c5"
  gray-700: "#525962"
  # Borders
  border-neutral-subtle: "#cfd3d8"
  border-neutral-subtleA: "#00163030"
typography:
  heading-small:
    fontFamily: Source Sans 3
    fontSize: 1.25rem
    fontWeight: 600
    lineHeight: 1.75rem
    letterSpacing: -0.1px
  body-medium:
    fontFamily: Source Sans 3
    fontSize: 1.125rem
    fontWeight: 400
    lineHeight: 1.5rem
    letterSpacing: 0
  body-small:
    fontFamily: Source Sans 3
    fontSize: 1rem
    fontWeight: 400
    lineHeight: 1.25rem
    letterSpacing: 0.2px
  body-small-strong:
    fontFamily: Source Sans 3
    fontSize: 1rem
    fontWeight: 600
    lineHeight: 1.25rem
    letterSpacing: 0.2px
rounded:
  sm: 4px
  md: 8px
spacing:
  none: 0px
  xs: 2px
  sm: 6px
  md: 8px
components:
  page:
    backgroundColor: "{colors.bg-default}"
    textColor: "{colors.text-neutral}"
    width: 390px
  card-status:
    backgroundColor: "{colors.bg-raised}"
    textColor: "{colors.text-neutral}"
    rounded: "{rounded.md}"
    padding: 20px
  button-accent:
    backgroundColor: "{colors.bg-accent-strong}"
    textColor: "{colors.text-accent-contrast}"
    rounded: "{rounded.md}"
    height: 40px
  link:
    textColor: "{colors.text-accent-subtle}"
    typography: "{typography.body-small}"
  tag-info:
    backgroundColor: "{colors.info-100}"
    textColor: "{colors.text-accent}"
    rounded: "{rounded.sm}"
---

## Overview

This is the visual identity for Nav's **innlogget produktside** ("Min side")
prototype — the logged-in landing surface a user lands on for a specific
benefit (e.g. dagpenger). The tone is calm, trustworthy and public-sector:
generous whitespace, a clean white canvas, and a single blue accent that
carries all interaction. It is a mobile-first layout (390 px reference width)
built entirely on the [Aksel design system](https://aksel.nav.no) tokens.

Content is organised into stacked cards ("Statuskort", "Meldekort", tailored
content) under a breadcrumb + page heading. Each card summarises a piece of the
user's case and links deeper into a task.

## Colors

The palette is high-contrast neutral ink on white, with Nav blue as the sole
interaction driver and Nav red reserved exclusively for the logo.

- **text-neutral (#202733):** Primary ink for headings and body copy.
- **text-neutral-subtle (#49515e):** Secondary text — metadata, periods,
  captions ("Mottatt 16. august", "14. februar – 27. februar").
- **text-accent (#002459):** Deep blue for accented headings and tag text.
- **text-accent-subtle (#005bb6):** Link text and inline interactive labels.
- **icon-action (#0067c5):** Actionable icons (chevrons, avatar icon).
- **bg-accent-strong (#2176d4):** Fill for primary ("accent") buttons.
- **bg-accent-moderate (#e4eeff):** Soft blue highlight surfaces.
- **text-logo (#c30000):** Nav red — logo only. Never use for UI actions.
- **border-neutral-subtle (#cfd3d8):** Hairline dividers and card outlines.

Do not introduce accent colors beyond the blue scale; status semantics come
from Aksel tag components, not ad-hoc colors.

## Typography

The type system is **Source Sans 3** throughout (fallback `system-ui,
sans-serif`), mirroring Aksel's font stack.

- **heading-small (20/28, 600):** Card headings and the section titles
  ("Saken din", "Meldekort", "Dagpenger").
- **body-medium (18/24, 400):** Emphasised body copy inside cards.
- **body-small (16/20, 400):** Default body text, links, list rows.
- **body-small-strong (16/20, 600):** Emphasised labels and interactive row
  text ("Saksoversikt", "Meld endring").

Negative letter-spacing (-0.1px) tightens headings; body text uses a slight
positive tracking (0.2px) for legibility at small sizes.

## Layout

Mobile-first, single column, 390 px reference width.

- **Page padding:** 16px horizontal gutters for card rows.
- **Sticky top menu:** 64px tall header with logo, role switcher and menu.
- **Card rhythm:** Cards are full-width (358px content), stacked vertically
  with consistent vertical gaps; internal card padding is 20px.
- **Spacing scale:** 0 / 2 / 6 / 8 px base steps (Aksel `--ax-space-*`),
  composed up to 16px and 20px for section and card padding.
- **Rows:** List items use a 48–82px row height with a leading label and a
  trailing chevron (24px) or tag/button.

## Shapes

- **rounded.sm (4px):** Tags and small chips.
- **rounded.md (8px):** Cards, buttons and larger containers.

Corners are consistently rounded; avoid sharp 0px corners except for full-width
dividers (1px `border-neutral-subtle` lines).

## Components

- **card-status ("Statuskort"):** White raised card, 8px radius, 20px padding.
  Holds a header (title + optional tag), a status body, and an actions row
  ("Meld endring", "Send klage") with trailing chevrons.
- **button-accent:** Solid blue (#2176d4) fill, white text, 8px radius, 40px
  tall. Primary call-to-action (e.g. "Send meldekort").
- **tag-info:** Light info background (#eef6fc) with deep-blue text (#002459),
  4px radius — used for status badges.
- **link:** Accent-subtle (#005bb6) text using body-small, for inline
  navigation and "Se resultat"-style links.
- **list-row:** Label (body-small-strong) + trailing 24px chevron in
  icon-action blue; used across Saksoversikt, Meldekort and content lists.

## Do's and Don'ts

- **Do** keep blue as the only interactive color; reserve red for the Nav logo.
- **Do** lean on Aksel tokens (`--ax-*`) rather than hard-coded hex values.
- **Do** maintain 16px page gutters and 20px card padding for consistent rhythm.
- **Do** use tag components for status semantics, not colored text.
- **Don't** mix font families — Source Sans 3 only.
- **Don't** use red or other accents for buttons, links, or highlights.
- **Don't** tighten body text tracking; keep the +0.2px on small body sizes.
