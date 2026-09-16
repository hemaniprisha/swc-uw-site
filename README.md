# Star Wars Club @ UW: website

Public website for Star Wars Club at the University of Washington (SWC UW). Five static
pages, no build step, no backend. Implemented from the design handoff in
[`design-reference/README.md`](design-reference/README.md).

**Live:** https://hemaniprisha.github.io/swc-uw-site/
**Repo:** https://github.com/hemaniprisha/swc-uw-site

## Pages
- [`index.html`](index.html): Home
- [`about.html`](about.html): About
- [`events.html`](events.html): Events
- [`officers.html`](officers.html): Officers
- [`join.html`](join.html): Join

Shared styles live in [`css/style.css`](css/style.css). Images are in [`assets/`](assets).

## Running it locally
No build tools required, it's plain HTML/CSS. From this folder:

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000`.

## Deploying (GitHub Pages)
Already set up: pushing to `main` auto-deploys to the live URL above via GitHub Pages
(source: `main` branch, root folder). `.nojekyll` is included so Pages serves the files as-is.

## Mailing list
The Join page's mailing-list field posts to a Google Form, whose responses land as new
rows in this Google Sheet: https://docs.google.com/spreadsheets/d/1Yl26kgDIJmkkxz22X2WCAX3cQXfK-3ZWH-rBzR0nKRk/edit

To finish wiring it up (about 5 minutes, one time):
1. Go to [forms.google.com](https://forms.google.com) and start a blank form. Add one
   "Short answer" question labeled "Email".
2. Open the Responses tab, click the green Sheets icon, choose "Select existing
   spreadsheet", and pick "SWC UW Mailing List".
3. Click **Send** (top right), then the link icon, and copy the form URL. It looks like
   `https://docs.google.com/forms/d/e/AKfycb.../viewform`. The long ID between `/d/e/`
   and `/viewform` is your **FORM_ID**.
4. Open that live form, right-click, View Page Source, and search for `entry.` right
   after your email question. That number (e.g. `entry.987654321`) is your **ENTRY_ID**.
5. In [`join.html`](join.html), replace `FORM_ID` in the form's `action` attribute and
   `ENTRY_ID` in the email input's `name` attribute with the values above, then push.

Because the form posts to a hidden iframe (the standard no-backend way to submit to a
Google Form from a static site), the page can't confirm the submission actually
succeeded; it shows "Thanks, you're on the list" optimistically. Test it once yourself
after setup to confirm a row appears in the sheet.

## Known placeholders (see design-reference/README.md for detail)
- Headshots pending for Reece Thompson and Jenna Elle Pampo (styled "HEADSHOT PENDING").
- Three event rows on the Events page are marked `TBD`.
- The mailing-list field needs the one-time Google Form setup above before it actually
  collects addresses.

## Editing content
Officer info, event rows, and copy are plain HTML in each page. Edit directly. If this
grows past five pages, consider extracting officers/events into a small JSON file or
moving to a static site generator (Next.js/Astro), per the suggestion in the design
handoff doc.
