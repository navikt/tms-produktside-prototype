# AAP service — design spec

## Problem

Add AAP as a new service in `tms-produktside-prototype`. The provided Figma node
(`181:12603`) is outdated and still named/copy-written as Dagpenger, so it must
be adapted to the current service design rather than copied directly.

## Goal

`/produktside-prototype/aap` renders as a fourth service using the same shared
white-card landing-page design as dagpenger, sykepenger, and Barnetrygd. AAP
uses AAP-specific data and includes meldekort. All subpage links resolve.

## Landing design

- Reuse the shared `[service].astro` template.
- Use the standard `Saken din` card with AAP status lines, existing gauge, and
  `Saksoversikt` secondary button.
- Use the standard behandling card with title `Søknad om AAP`, warning tag
  `Venter på deg`, and link row `Vi mangler 2 vedlegg fra deg`.
- Use the standard two action cards: `Meld endring` and `Send klage`.
- Include the existing meldekort section with AAP data from the Figma reference.
- Use the existing situasjonstilpasset placeholder section.

## Deliberate deviations from Figma

- Do not copy the outdated soft-grey combined status card.
- Do not add full-width `Oversikt over saken din` on the landing page.
- Do not add landing-page `Historikk`.
- Do not add AAP-only CSS tokens/classes.
- Keep colors unified with the rest of the services via the existing DESIGN.md
  tokens and `_index.module.css`.

## Subpages

Provide `soknad`, `saksoversikt`, and `dokument` data so the existing dynamic
subpage templates render for AAP and all links resolve.

## Verification

- `pnpm build` succeeds.
- `/produktside-prototype/aap`, `/aap/soknad`, `/aap/saksoversikt`, and
  `/aap/dokument` return 200.
- Unknown services return 404.
- Existing services continue to render.
- The AAP regression script confirms no outdated Figma-only layout markers were
  introduced.
