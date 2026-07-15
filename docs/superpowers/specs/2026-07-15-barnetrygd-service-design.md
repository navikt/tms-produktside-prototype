# Barnetrygd service — design spec

## Problem

Implement Barnetrygd as a new third service in `tms-produktside-prototype`.
The original Figma frame (node `378:19162`) is outdated compared with the
current dagpenger/sykepenger service design, so Barnetrygd should use the same
overall landing-page structure as the existing services and vary only by data.

## Goal

`/produktside-prototype/barnetrygd` renders with the same shared white-card
service layout as dagpenger/sykepenger; its links resolve to working subpages;
dagpenger and sykepenger stay visually unchanged.

## Landing layout

- **Saken din card**: same white raised `.card + .sak` layout as the other
  services, heading "Saken din", Barnetrygd status lines, the existing
  decorative gauge, and the secondary "Saksoversikt" button.
- **Behandling card**: same white raised `.card + .behandling` layout, title
  "Søknad utvidet barnetrygd", subtitle "Mottatt 16. august.", warning tag
  "Venter på deg", and a link row "Vi mangler 2 vedlegg fra deg" to `/soknad`.
- **Sak actions**: same two-card row ("Meld endring", "Send klage").
- **Situasjonstilpasset innhold**: same placeholder section as today.
- **No meldekort** for Barnetrygd, matching the existing optional-data pattern
  (Sykepenger also omits meldekort).

## Data model

Barnetrygd uses the existing service shape:

- `sak.heading`, `sak.statusLines`, `sak.gaugeLabel`, `sak.actionLabel`
- `behandling.title`, `behandling.subtitle`, `behandling.tag`,
  `behandling.tagVariant`, `behandling.link`
- `sakActions`, `tilpassetHeading`
- `soknad`, `saksoversikt`, `dokument` blocks so existing subpage templates work

Do not add Barnetrygd-only landing fields such as `sak.tag`, `gaugeVariant`,
`behandling.body`, `behandling.button`, `saksoversiktLink`, or `historikk`.

## Template (`[service].astro`)

Keep the shared rendering path for every service:

- `sak.actionLabel` is required and always renders the secondary button.
- `behandling.link` is required and always renders the link row.
- No combined-status-card branch, no Barnetrygd-only CTA branch, and no landing
  Historikk branch.

## Styles (`_index.module.css`)

Reuse the existing tokenised styles from the DESIGN.md migration. Do not add
Barnetrygd-only `surface-soft`, moderate tag, combined-card, or landing
Historikk classes.

## Deliberate deviations

- Shared chrome (nav.no decorator/header, breadcrumb, user, footer) keeps the
  existing prototype implementation — the Figma export states the decorator is a
  non-1:1 reference/placeholder. The avatar "2 nye oppgaver" + notification dot
  is a decorator detail; the global `user` block is left as-is.
- Static prototype: links are navigational/placeholder; no backend.

## Verification

`pnpm build` succeeds; `/produktside-prototype/barnetrygd` and subpages return
200; unknown services return 404; Barnetrygd's landing does not contain the
outdated Figma-only markers ("Godkjent", "Send vedlegg", "Oversikt over saken
din", or landing "Historikk"); dagpenger/sykepenger stay unchanged.

## Files touched

- `src/data/services.json`
- `src/pages/[service].astro`
- `src/pages/_index.module.css`
- `docs/superpowers/specs/2026-07-15-barnetrygd-service-design.md`
