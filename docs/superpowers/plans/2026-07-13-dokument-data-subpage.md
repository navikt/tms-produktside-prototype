# Dokument/data sub-page Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a new prototype "Dokument/data" sub-page, reachable from every timeline link on `/soknad`, that mirrors the existing sub-page layout and shows a large placeholder card.

**Architecture:** A new data-driven Astro page `src/pages/[service]/dokument.astro` reuses the `_index.module.css` module and the same `getStaticPaths`/`svc` pattern as `soknad.astro`. Content comes from a new `dokument` block in `services.json`. Timeline links in `soknad.astro` are rewired from `href="#"` to the new route.

**Tech Stack:** Astro 5 (SSR via `@astrojs/node`, `output: 'server'`), CSS Modules. Package manager: pnpm. No test framework — verification is via `pnpm build` plus preview-server HTML checks.

## Global Constraints

- Base path is `/produktside-prototype` (set in `astro.config.mjs`); build routes to `import.meta.env.BASE_URL` for internal links.
- Norwegian UI copy, verbatim: page title `Dokument/data`, placeholder `Dokument eller data`, breadcrumb crumbs `Min side` and the service title (e.g. `Dagpenger`).
- Reuse existing classes/design tokens in `src/pages/_index.module.css`; do NOT introduce Tailwind or new dependencies.
- Every service in `services.json` must resolve the new route (currently `dagpenger` and `sykepenger`).
- Exact card values from Figma node `2137:40638`: background `var(--ax-bg-raised)`, `height: 516px`, `border-radius: 12px`, flex column, contents centered, `padding: 20px 16px`. Placeholder text (node `2137:40639`): italic, `font-size: 18px`, `line-height: 1.3`, `color: var(--ax-text-neutral)`, centered, `max-width: 200px`.

---

### Task 1: Add `dokument` content block to services data

**Files:**
- Modify: `src/data/services.json`

**Interfaces:**
- Consumes: nothing.
- Produces: `services.<id>.dokument = { title: string; placeholder: string }` for every service, read by Task 3 as `svc.dokument`.

- [ ] **Step 1: Add a `dokument` block to the `dagpenger` service**

In `src/data/services.json`, inside the `"dagpenger"` object, add a `"dokument"` key. Place it right after the `"soknad"` object's closing brace (before `"tilpassetHeading"`). Result for that region:

```json
      },
      "dokument": {
        "title": "Dokument/data",
        "placeholder": "Dokument eller data"
      },
      "tilpassetHeading": "Situasjonstilpasset innhold"
    },
```

- [ ] **Step 2: Add the same `dokument` block to the `sykepenger` service**

Inside the `"sykepenger"` object, add the identical block after its `"soknad"` object (before its `"tilpassetHeading"`):

```json
      },
      "dokument": {
        "title": "Dokument/data",
        "placeholder": "Dokument eller data"
      },
      "tilpassetHeading": "Situasjonstilpasset innhold"
    }
```

- [ ] **Step 3: Verify the JSON is valid**

Run: `cd /Users/Nima.Hakimi/Projects/min-side/tms-produktside-prototype && node -e "const d=require('./src/data/services.json'); console.log(d.services.dagpenger.dokument, d.services.sykepenger.dokument)"`
Expected: prints `{ title: 'Dokument/data', placeholder: 'Dokument eller data' } { title: 'Dokument/data', placeholder: 'Dokument eller data' }` with no JSON parse error.

- [ ] **Step 4: Commit**

```bash
cd /Users/Nima.Hakimi/Projects/min-side/tms-produktside-prototype
git add src/data/services.json
git commit -m "Add dokument content block to services data

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>"
```

---

### Task 2: Add card styles to the CSS module

**Files:**
- Modify: `src/pages/_index.module.css`

**Interfaces:**
- Consumes: existing CSS custom properties `--ax-bg-raised`, `--ax-text-neutral`.
- Produces: two class names used by Task 3 — `.dokCard` (the white placeholder card) and `.dokPlaceholder` (the italic centered text).

- [ ] **Step 1: Append the two classes**

Add to the end of `src/pages/_index.module.css`:

```css
/* Dokument/data underside */
.dokCard {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 516px;
  padding: 20px 16px;
  background: var(--ax-bg-raised);
  border-radius: 12px;
}
.dokPlaceholder {
  margin: 0;
  max-width: 200px;
  font-style: italic;
  font-size: 18px;
  line-height: 1.3;
  text-align: center;
  color: var(--ax-text-neutral);
}
```

- [ ] **Step 2: Verify the file still parses as part of the build (deferred to Task 3)**

No standalone check here; the CSS module is validated when the page that imports it builds in Task 3. Proceed to commit.

- [ ] **Step 3: Commit**

```bash
cd /Users/Nima.Hakimi/Projects/min-side/tms-produktside-prototype
git add src/pages/_index.module.css
git commit -m "Add Dokument/data card styles

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>"
```

---

### Task 3: Create the Dokument/data page

**Files:**
- Create: `src/pages/[service]/dokument.astro`

**Interfaces:**
- Consumes: `services.<id>.dokument` from Task 1; `.dokCard` and `.dokPlaceholder` from Task 2; existing classes `page`, `meny`, `logoRole`, `logo`, `divider`, `role`, `menyBtn`, `subPage`, `user`, `userText`, `userName`, `userStatus`, `avatar`, `subHeader`, `breadcrumb`, `h1` from `_index.module.css`.
- Produces: route `/produktside-prototype/[service]/dokument` for every service key.

- [ ] **Step 1: Create the page file**

Create `src/pages/[service]/dokument.astro` with exactly this content (header/user/breadcrumb structure mirrors `soknad.astro`; breadcrumb second crumb links back to the service page; body is the placeholder card):

```astro
---
// Dokument/data – underside (prototype)
// Lenket fra tidslinjen på søknadssiden (/[service]/soknad).
import styles from "../_index.module.css";
import data from "../../data/services.json";

interface Dokument {
  title: string;
  placeholder: string;
}
interface Service {
  title: string;
  dokument: Dokument;
}

const services = data.services as Record<string, Service>;

export function getStaticPaths() {
  return Object.keys(data.services as Record<string, unknown>).map((service) => ({
    params: { service },
  }));
}

const { service } = Astro.params;
const svc = services[service as string];
if (!svc) {
  return new Response(`Ukjent tjeneste: ${service}`, { status: 404 });
}
const { user } = data;
const dokument = svc.dokument;
const baseUrl = import.meta.env.BASE_URL;
---

<html lang="no">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>{dokument.title} – Min side</title>
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link
      href="https://fonts.googleapis.com/css2?family=Source+Sans+3:ital,wght@0,400;0,600;0,650;0,700;1,400&display=swap"
      rel="stylesheet"
    />
  </head>
  <body>
    <div class={styles.page}>
      <!-- Meny -->
      <header class={styles.meny}>
        <div class={styles.logoRole}>
          <span class={styles.logo}>NAV</span>
          <span class={styles.divider}></span>
          <span class={styles.role}>
            {user.role}
            <svg width="20" height="20" viewBox="0 0 24 24" fill="none" aria-hidden="true"><path d="M6 9l6 6 6-6" stroke="#49515e" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"/></svg>
          </span>
        </div>
        <button class={styles.menyBtn} type="button">
          Meny
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none" aria-hidden="true"><path d="M4 7h16M4 12h16M4 17h16" stroke="#005bb6" stroke-width="1.6" stroke-linecap="round"/></svg>
        </button>
      </header>

      <main class={styles.subPage}>
        <!-- Bruker -->
        <div class={styles.user}>
          <div class={styles.userText}>
            <p class={styles.userName}>{user.name}</p>
            <p class={styles.userStatus}>{user.status}</p>
          </div>
          <div class={styles.avatar} aria-hidden="true">
            <svg width="24" height="24" viewBox="0 0 24 24" fill="none"><path d="M12 12a4 4 0 100-8 4 4 0 000 8zM4.5 20a7.5 7.5 0 0115 0" stroke="#fff" stroke-width="1.6" stroke-linecap="round"/></svg>
          </div>
        </div>

        <div class={styles.subHeader}>
          <!-- Brødsmuler -->
          <nav class={styles.breadcrumb} aria-label="Brødsmulesti">
            <a href="#">Min side</a>
            <span>/</span>
            <a href={`${baseUrl}/${service}`}>{svc.title}</a>
          </nav>

          <h1 class={styles.h1}>{dokument.title}</h1>
        </div>

        <!-- Dokument/data-innhold -->
        <div class={styles.dokCard}>
          <p class={styles.dokPlaceholder}>{dokument.placeholder}</p>
        </div>
      </main>
    </div>
  </body>
</html>
```

- [ ] **Step 2: Build to verify the page compiles and route is generated**

Run: `cd /Users/Nima.Hakimi/Projects/min-side/tms-produktside-prototype && pnpm build`
Expected: build completes with no errors. (SSR output — routes are compiled, not emitted as static HTML files.)

- [ ] **Step 3: Verify the rendered page via the preview server**

Run:
```bash
cd /Users/Nima.Hakimi/Projects/min-side/tms-produktside-prototype
pnpm preview &
sleep 3
curl -s http://localhost:4321/produktside-prototype/dagpenger/dokument | grep -o "Dokument/data\|Dokument eller data\|Dagpenger"
curl -s http://localhost:4321/produktside-prototype/sykepenger/dokument | grep -o "Dokument/data\|Dokument eller data\|Sykepenger"
```
Expected: the dagpenger call prints `Dokument/data`, `Dagpenger`, and `Dokument eller data`; the sykepenger call prints `Dokument/data`, `Sykepenger`, and `Dokument eller data`. Then stop the preview server: find its PID with `lsof -ti:4321` and run `kill <PID>`.

- [ ] **Step 4: Commit**

```bash
cd /Users/Nima.Hakimi/Projects/min-side/tms-produktside-prototype
git add "src/pages/[service]/dokument.astro"
git commit -m "Add Dokument/data sub-page

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>"
```

---

### Task 4: Wire timeline links to the new page

**Files:**
- Modify: `src/pages/[service]/soknad.astro`

**Interfaces:**
- Consumes: route from Task 3 (`${baseUrl}/${service}/dokument`).
- Produces: nothing downstream.

- [ ] **Step 1: Add `baseUrl` to the frontmatter**

In `src/pages/[service]/soknad.astro`, in the frontmatter (the `---` block), after the line `const soknad = svc.soknad;` add:

```astro
const baseUrl = import.meta.env.BASE_URL;
```

- [ ] **Step 2: Point the timeline links at the new page**

In the same file, find the timeline links block:

```astro
                    {event.links.map((link) => <a href="#">{link}</a>)}
```

Replace it with:

```astro
                    {event.links.map((link) => (
                      <a href={`${baseUrl}/${service}/dokument`}>{link}</a>
                    ))}
```

- [ ] **Step 3: Build to verify**

Run: `cd /Users/Nima.Hakimi/Projects/min-side/tms-produktside-prototype && pnpm build`
Expected: build completes with no errors.

- [ ] **Step 4: Verify the links point to the new route in rendered HTML**

Run:
```bash
cd /Users/Nima.Hakimi/Projects/min-side/tms-produktside-prototype
pnpm preview &
sleep 3
curl -s http://localhost:4321/produktside-prototype/dagpenger/soknad | grep -o 'href="/produktside-prototype/dagpenger/dokument"' | head -1
curl -s http://localhost:4321/produktside-prototype/sykepenger/soknad | grep -o 'href="/produktside-prototype/sykepenger/dokument"' | head -1
```
Expected: the first call prints `href="/produktside-prototype/dagpenger/dokument"`; the second prints `href="/produktside-prototype/sykepenger/dokument"`. Stop the preview server: `lsof -ti:4321` then `kill <PID>`.

- [ ] **Step 5: Commit**

```bash
cd /Users/Nima.Hakimi/Projects/min-side/tms-produktside-prototype
git add "src/pages/[service]/soknad.astro"
git commit -m "Link soknad timeline entries to Dokument/data page

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>"
```

---

## Notes for the implementer

- The repo has pre-existing uncommitted WIP on the `packageManager` field and `if (!svc)` 404 guards. Do NOT stage or revert those; only stage the files each task lists.
- Work happens on branch `nimkimi/dokument-subpage` (already created).
- The `ital` axis was added to the Google Fonts `<link>` in `dokument.astro` so the italic placeholder renders correctly; the other pages don't need it.
