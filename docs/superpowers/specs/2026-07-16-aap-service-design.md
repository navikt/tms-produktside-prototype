# AAP service — design spec

## Problem

Add AAP as a new service in `tms-produktside-prototype` from Figma node
`181:12603`. The Figma prototype is visually outdated only in its color/system
treatment; its layout and content should be implemented 1:1 for AAP while using
the app's unified DESIGN.md tokenized CSS colors.

## Goal

`/produktside-prototype/aap` renders the Figma landing structure and content as
an AAP service. The route uses the same shared chrome (header, service switcher,
breadcrumb, user block) as the other prototype services, and all linked subpages
resolve through the existing dynamic routes.

## Landing design

The AAP landing uses a dedicated data-driven `figmaStatus` variant in
`[service].astro`, only for services that opt into `landingVariant:
"figmaStatus"`.

Visible landing content follows the Figma frame:

- Status block with the heading `Status i saken din`.
- Behandling panel:
  - title `Søknad`
  - subtitle `Mottatt 16. august.`
  - warning tag `Venter på deg`
  - body `Vi mangler 2 vedlegg fra deg. Frist for å ettersende dem er 30.
    august.`
  - accent CTA `Send vedlegg`
- Action cards: `Meld endring`, `Send klage`, and full-width
  `Oversikt over saken din`.
- Meldekort section:
  - `Timer uke 6 og 7` / `Send om 16 dager`
  - actionable card `Periode 2. feb - 15. feb` + `Frist mandag` +
    `Send meldekort`
  - previous rows `Timer uke 2 og 3` / `Utbetalt 19 450,-` and
    `Timer uke 1 og 2` / `Utbetalt 16 450,-`
- Existing situasjonstilpasset placeholder section.
- Landing `Historikk` with `Søknad mottatt`.

## Color/system adaptation

Use layout-specific classes to match the Figma structure, but do not copy raw
Figma color literals like `#f5f6f7`, `#ffebc7`, or `#d5f6db`. Surfaces, text,
links, buttons, borders and tags should use the existing tokenized
`_index.module.css` palette so AAP is visually unified with the rest of the
prototype.

## Subpages

Provide `soknad`, `saksoversikt`, and `dokument` data so the existing dynamic
subpage templates render for AAP and all links resolve.

## Verification

- AAP regression script prints `AAP uses the Figma layout with unified CSS
  colors.`
- `pnpm build` succeeds.
- `/produktside-prototype/aap`, `/aap/soknad`, `/aap/saksoversikt`, and
  `/aap/dokument` return 200.
- Unknown services return 404.
- Existing services continue to render.
