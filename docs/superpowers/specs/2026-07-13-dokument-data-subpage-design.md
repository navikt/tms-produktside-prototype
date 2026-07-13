# Dokument/data sub-page design

## Problem

The `/soknad` sub-page shows a "Dette har skjedd" timeline whose event links
(e.g. "Se/endre søknaden", "Trekk søknaden", "Se vedlegget") currently point to
`href="#"` and go nowhere. We want these links to open a new sub-page that shows
a document/data view, matching the Figma design "Konsept - IP - dagpenger
statusside" (node `2137:40595`).

## Goal

Add a new prototype sub-page, reachable from every timeline link in `/soknad`,
that mirrors the existing sub-page layout and displays a large placeholder card
for a document or data view.

## Non-goals

- Real document rendering or per-link content. The page is a single shared
  placeholder for all timeline links (matching the single Figma frame).
- Any backend, data fetching, or interactivity beyond navigation.

## Design

### Route

New file `src/pages/[service]/dokument.astro`, giving the route
`/produktside-prototype/[service]/dokument` (base is `/produktside-prototype`).
It uses `getStaticPaths` over `data.services` exactly like `soknad.astro`, so
both `dagpenger` and `sykepenger` resolve, plus the same `if (!svc)` 404 guard.

### Layout (reuses existing patterns)

The page reuses the `_index.module.css` module and the same structure as
`soknad.astro`:

- `Dekoratør`/`meny` header: NAV logo, `Privatperson` role, `Meny` button.
- `user` block: user name + status + avatar (from `data.user`).
- `subHeader` with breadcrumb `Min side / Dagpenger` — the second crumb links
  to `/${service}` — and an `<h1>` with the page title.
- Main content: one large white rounded card filling the content area with
  centered italic placeholder text, per Figma.

### Content data

Add a `dokument` block to each service in `src/data/services.json`:

```json
"dokument": {
  "title": "Dokument/data",
  "placeholder": "Dokument eller data"
}
```

The page reads `svc.dokument` the same way `soknad.astro` reads `svc.soknad`.
The `Service` interface in `dokument.astro` types this new block.

### Styling

Add two classes to `_index.module.css`:

- `.dokCard` — white background, `border-radius: 8px`, fixed height ~570px,
  flex-centered contents, matching the Figma placeholder card on the light-blue
  page background.
- `.dokPlaceholder` — italic, muted/subtle text color, centered.

Reuse existing `page`, `meny`, `subPage`, `subHeader`, `breadcrumb`, `h1`,
`user` classes.

### Wiring from the timeline

In `soknad.astro`, change every timeline event link from `href="#"` to
`href={`${baseUrl}/${service}/dokument`}`. This requires reading
`import.meta.env.BASE_URL` in `soknad.astro` (as `[service].astro` already
does). All timeline links across all events point to the same page.

## Testing / verification

This is a static Astro prototype with no test suite. Verify by:

1. `pnpm build` succeeds.
2. Running the dev/preview server and confirming:
   - `/produktside-prototype/dagpenger/dokument` renders with header,
     breadcrumb, "Dokument/data" heading, and the placeholder card.
   - `/produktside-prototype/sykepenger/dokument` renders equivalently.
   - Timeline links on `/dagpenger/soknad` and `/sykepenger/soknad` navigate to
     the new page.

## Files touched

- `src/pages/[service]/dokument.astro` (new)
- `src/pages/[service]/soknad.astro` (timeline link hrefs + BASE_URL)
- `src/data/services.json` (add `dokument` block per service)
- `src/pages/_index.module.css` (add `.dokCard`, `.dokPlaceholder`)
