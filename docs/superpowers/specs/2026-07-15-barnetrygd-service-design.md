# Barnetrygd service — design spec

## Problem

Implement the Figma "Barnetrygd" innlogget-produktside (node `378:19162`) as a
new third service in `tms-produktside-prototype`, alongside dagpenger and
sykepenger, reusing the data-driven `[service].astro` template and the
DESIGN.md-tokenised `_index.module.css`.

## Goal

`/produktside-prototype/barnetrygd` renders the Figma landing frame; its links
resolve to working subpages; dagpenger and sykepenger stay visually unchanged.

## Landing layout (from Figma)

- **Status card** — one rounded (12px) container on a soft grey surface
  (`#f5f6f7`) holding two joined sub-cards (inner corners 4px):
  - *Sak*: "Status" heading + success tag "Godkjent"; status lines ("Barnetrygd
    for Ola Nordmann", "Fram til 1. januar 2034", "Neste utbetaling…kr 2012"); a
    neutral gauge with "…" label. No secondary button.
  - *Behandling*: "Søknad utvidet barnetrygd" + subtitle "Mottatt 16. august." +
    warning tag "Venter på deg"; body "Vi mangler 2 vedlegg fra deg. Frist for å
    ettersende dem er 30. august."; accent button "Send vedlegg".
- **Sak actions**: "Meld endring" + "Send klage" (row), then full-width
  "Oversikt over saken din" → saksoversikt.
- **Situasjonstilpasset innhold**: heading + placeholder blocks (as today).
- **Historikk** (on landing): small-dot timeline, one entry ("20 august 2025" /
  "Søknad mottatt" / links "Se søknaden", "Trekk søknaden"), then
  "Se hele saksoversikten" → saksoversikt.
- **No meldekort.**

## Data model additions (`services.json`, all additive/optional)

- `sak.tag`, `sak.tagVariant`; `sak.actionLabel` becomes optional; optional
  neutral gauge.
- `behandling.body`; `behandling.button { label, href }`; `behandling.link`
  optional.
- `saksoversiktLink` (full-width action label).
- `historikk { entries: [{ date, title, links[] }], seHeleLabel }`.
- Subpage blocks `soknad`, `saksoversikt`, `dokument` (authored content so links
  resolve; not part of the provided frame).

## Template (`[service].astro`)

Extend the `Service` interface; render each new piece only when its data is
present, so dagpenger/sykepenger output is unchanged:
- status tag; conditional `.btnSecondary`; combined-status-card modifier;
- behandling `subtitle` / optional `body` / optional accent `button` / optional
  `link`;
- full-width "Oversikt over saken din" action;
- landing Historikk section reusing the small-dot timeline + "Se hele
  saksoversikten" link.

## Styles (`_index.module.css`)

New tokens (DESIGN.md/Aksel): `--ax-bg-success-moderate #d5f6db`,
`--ax-bg-warning-moderate #ffebc7`, `--ax-text-info #002942`,
`--ax-accent-900 #004ea3`, and a solid soft-surface token `#f5f6f7`.
New classes only (existing base classes untouched): combined status-card
variant, "moderate" success/warning tags, behandling body text, neutral gauge,
landing historikk block (reuse `.dotList`/timeline styles). Reuse `.btnAccent`.

## Deliberate deviations

- Shared chrome (nav.no decorator/header, breadcrumb, user, footer) keeps the
  existing prototype implementation — the Figma export states the decorator is a
  non-1:1 reference/placeholder. The avatar "2 nye oppgaver" + notification dot
  is a decorator detail; the global `user` block is left as-is.
- Static prototype: links are navigational/placeholder; no backend.

## Verification

`pnpm build` succeeds; preview `/produktside-prototype/barnetrygd` matches the
frame; all landing links resolve (200, no 500); dagpenger/sykepenger unchanged.

## Files touched

- `src/data/services.json`
- `src/pages/[service].astro`
- `src/pages/_index.module.css`
- `docs/superpowers/specs/2026-07-15-barnetrygd-service-design.md`
