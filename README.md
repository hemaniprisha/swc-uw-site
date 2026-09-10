# Star Wars Club @ UW — website

Public website for Star Wars Club at the University of Washington (SWC UW). Five static
pages, no build step, no backend — implemented from the design handoff in
[`design-reference/README.md`](design-reference/README.md).

## Pages
- [`index.html`](index.html) — Home
- [`about.html`](about.html) — About
- [`events.html`](events.html) — Events
- [`officers.html`](officers.html) — Officers
- [`join.html`](join.html) — Join

Shared styles live in [`css/style.css`](css/style.css). Images are in [`assets/`](assets).

## Running it locally
No build tools required — it's plain HTML/CSS. From this folder:

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000`.

## Deploying (GitHub Pages)
This repo has no remote configured yet. To publish it:

```bash
gh repo create swc-uw-site --public --source=. --push
```

(or create the repo on github.com and `git remote add origin <url> && git push -u origin main`),
then in the repo's **Settings → Pages**, set the source to the `main` branch, root folder.
`.nojekyll` is already included so GitHub Pages serves the files as-is.

## Known placeholders (see design-reference/README.md for detail)
- Headshots pending for Reece Thompson and Jenna Elle Pampo (styled "HEADSHOT PENDING").
- Three event rows on the Events page are marked `TBD`.
- The mailing-list field on the Join page is not wired to a backend (no endpoint chosen yet —
  pick Formspree / Google Form / Mailchimp / Netlify Forms, or remove the field).

## Editing content
Officer info, event rows, and copy are plain HTML in each page — edit directly. If this grows
past five pages, consider extracting officers/events into a small JSON file or moving to a
static site generator (Next.js/Astro), per the suggestion in the design handoff doc.
