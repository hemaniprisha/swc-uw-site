# Handoff: Star Wars Club @ UW — club website

## Overview
A five-page public website for **Star Wars Club at the University of Washington (SWC UW)**, a registered student organization. Its job is to explain what the club actually does, show the next event, introduce the officers, and funnel visitors into the club Discord. Visual concept: a **"datapad / holonet terminal"** — dark starfield-and-hologram chrome for hero and navigation areas, warm lavender daylight sections for readable content. Palette is derived from the club's own logo (a lavender roundel with a husky in a Vader helmet).

## About the design files
The files in this bundle are **design references created in HTML** — a working prototype showing intended look and behavior, **not production code to copy directly**.

`SWC UW Website.dc.html` is authored in a proprietary streaming-component format (a `<x-dc>` template + a `Component` logic class). Do not try to run or port that runtime. Read it as a spec: the markup structure, inline styles, and copy are all literal and exact.

The task is to **recreate these designs in the target codebase's environment** using its established patterns — Next.js/React + Tailwind is a natural fit, or plain static HTML/CSS if the club wants zero-build hosting (GitHub Pages). If no codebase exists yet, choose the framework and implement there. Static site generation is appropriate: there is no backend beyond an optional mailing-list form.

## Fidelity
**High-fidelity.** Colors, typography, spacing, hover states, and copy are final. Recreate faithfully. The only deliberately unfinished pieces are:
- two officer headshots (Reece Thompson, Jenna Elle Pampo) — currently a striped "HEADSHOT PENDING" placeholder,
- event schedule rows marked `TBD`,
- the mailing-list form is non-functional (no endpoint chosen).

---

## Global structure

Single-page-app style routing across five views: `home`, `about`, `events`, `officers`, `join`. In production these should be **real routes** (`/`, `/about`, `/events`, `/officers`, `/join`) with normal `<a href>` navigation, not client state. The prototype uses state only because it's a single file.

Page shell: `background:#0a0714`, `font-family: Barlow, system-ui, sans-serif`, `min-height:100vh`. Content containers are `max-width:1240px; margin:0 auto; padding-inline:28px`.

### Header (sticky, on every page)
- `position:sticky; top:0; z-index:50`
- `background: rgba(10,7,20,.92)`, `backdrop-filter: blur(10px)`
- `border-bottom: 1px solid rgba(198,179,232,.16)`
- Inner: flex row, `justify-content:space-between`, `gap:20px`, `padding:14px 28px`, `flex-wrap:wrap`
- **Left brand cluster** (flex, `gap:11px`, links to home, `text-decoration:none`):
  - Logo `assets/logo.png` — `38×38`, `border-radius:50%`, `box-shadow:0 0 0 1px rgba(198,179,232,.4)`
  - Wordmark: `font: 800 15px/1 'Bricolage Grotesque'`, `color:#f2ecff`, `white-space:nowrap` (required — it must never wrap). "Star Wars Club " in `#f2ecff` + "@ UW" in `#c6b3e8`.
  - Tag: `font: 400 9.5px 'JetBrains Mono'`, `letter-spacing:.22em`, `color:#9b86cf`, `border-left:1px solid rgba(198,179,232,.25)`, `padding-left:11px`, `white-space:nowrap`, `overflow:hidden`, `text-overflow:ellipsis`, `min-width:0`. Text: `RSO // SEATTLE SECTOR`
- **Nav** (flex, `gap:4px`, `flex-wrap:wrap`). Items: `01 About`, `02 Events`, `03 Officers`, `04 Join`
  - `font: 600 11px 'Chakra Petch'`, `letter-spacing:.14em`, `text-transform:uppercase`, `color:#c2b3dd`, `padding:9px 13px`, `border:1px solid transparent`
  - hover: `color:#fff`, `border-color: rgba(198,179,232,.45)`
  - **active page**: an absolutely-positioned bar inside the link — `left:13px; right:13px; bottom:3px; height:2px; background:#c6b3e8; box-shadow:0 0 10px #c6b3e8`
- **CTA button** `Join →` — `font: 700 11px 'Chakra Petch'`, `letter-spacing:.16em`, uppercase, `color:#120c22`, `background:#c6b3e8`, `padding:11px 18px`, `margin-left:6px`, clipped corners: `clip-path: polygon(8px 0,100% 0,100% calc(100% - 8px),calc(100% - 8px) 100%,0 100%,0 8px)`; hover `background:#fff`. Links to the Discord invite, `target="_blank" rel="noopener"`.

### Footer (on every page)
- `background:#0e0a18`, `border-top:1px solid rgba(198,179,232,.16)`, `padding:44px 28px 30px`
- Kicker: `font: 400 10px 'JetBrains Mono'`, `letter-spacing:.3em`, `color:#9b86cf` — `◤ HOLONET COMMS · ALL CHANNELS OPEN` followed by a `#c6b3e8` block cursor `█` animated with `blink 1.1s step-end infinite`
- Three comms cards: `grid-template-columns: repeat(auto-fit, minmax(220px,1fr))`, `gap:16px`. Each is an `<a>` with `border:1px solid rgba(198,179,232,.24)`, `padding:18px 20px`, `clip-path: polygon(10px 0,100% 0,100% calc(100% - 10px),calc(100% - 10px) 100%,0 100%,0 10px)`; hover `border-color: rgba(198,179,232,.6)`. Label `400 9.5px 'JetBrains Mono'`/`letter-spacing:.22em`/`#9b86cf`; value `700 15px 'Chakra Petch'`/`#efe9ff`/`margin-top:8px`.
  - `EMAIL` → `uwstarwars@uw.edu` (`mailto:`)
  - `DISCORD` → `discord.gg/mw2XpSzee`
  - `INSTAGRAM` → `@uwstarwars` (`https://instagram.com/uwstarwars`)
- Bottom strip: `margin-top:30px`, `padding-top:20px`, `border-top:1px solid rgba(198,179,232,.12)`, flex row space-between, wraps. Left: 32px round logo + `400 11px 'JetBrains Mono'`, `letter-spacing:.14em`, `#9b86cf` — `STAR WARS CLUB · UNIVERSITY OF WASHINGTON · RSO`. Right, same type: `MAY THE FORCE BE WITH YOU`.
- Disclaimer: `400 12px/1.55 Barlow`, `color:#9b86cf`, `max-width:760px`, `margin-top:18px` — "A student fan organization. Not affiliated with, endorsed by, or sponsored by Lucasfilm Ltd. or The Walt Disney Company." **Keep this line and this contrast** (it was raised from `#6b5c8c` to pass 4.5:1).

---

## Screens

### 1. Home (`/`)

**Purpose:** state what the club is, show the next event, route to Discord.

**Hero** — `background: linear-gradient(180deg,#0a0714 0%,#120c22 60%,#0a0714 100%)`, `position:relative`, `overflow:hidden`, `padding:56px 28px 60px`. Three stacked decorative layers, all `position:absolute; inset:0`:
1. Starfield — multiple 1–1.4px `radial-gradient` dots at `18% 24%`, `44% 70%`, `72% 34%`, `90% 80%`, `8% 62%` in `#fff`, `#e0d3ff`, `#cbb4e8`; `opacity:.75`
2. Scanlines — `repeating-linear-gradient(180deg, rgba(198,179,232,.045) 0 1px, transparent 1px 4px)`, `pointer-events:none`
3. Grid — two 1px `linear-gradient` lines at `rgba(198,179,232,.05)`, `background-size:64px 64px`, masked by `radial-gradient(80% 60% at 50% 40%, #000, transparent)`

Content: `grid-template-columns: repeat(auto-fit, minmax(340px,1fr))`, `gap:34px`, `align-items:center`.

*Left column:*
- Kicker `400 10.5px 'JetBrains Mono'`, `letter-spacing:.32em`, `#9b86cf` — `◤ HOLONET FEED · CH.977 · UW-SEA`
- H1 `800 62px/0.98 'Bricolage Grotesque'`, `letter-spacing:-.03em`, `#fff`, `text-wrap:pretty`: "These are the / nerds you're / **looking for.**" — third line `#c6b3e8`
- Body `400 16px/1.6 Barlow`, `#b0a3ca`, `max-width:460px`: "Movies, games, lightsabers, and arguments about canon. Every other week, all quarter, all welcome."
- **Event console card** (the signature component, reused on Events):
  - `border:1px solid rgba(198,179,232,.32)`, `background: linear-gradient(160deg, rgba(198,179,232,.1), rgba(198,179,232,.02))`, `padding:22px 24px 20px`
  - `clip-path: polygon(14px 0,100% 0,100% calc(100% - 14px),calc(100% - 14px) 100%,0 100%,0 14px)`
  - Header row: `400 10px 'JetBrains Mono'`, `letter-spacing:.2em`, `#c6b3e8` — `NEXT EVENT // 001` left, `● LIVE` right with `pulse 2s ease-in-out infinite`
  - Title `700 26px/1.1 'Chakra Petch'`, uppercase, `#f2ecff` — "Lightsaber Academy"
  - `1px` rule `rgba(198,179,232,.28)`, `margin:14px 0`
  - Spec grid `grid-template-columns: auto minmax(0,1fr)`, `gap:6px 18px`, `400 12px/1.5 'JetBrains Mono'`; labels `#9b86cf`, values `#c2b3dd`:
    `DATE` → `SEP 25 · 1:00–3:00 PM`; `LOC` → `QUAD NORTH · DAWG DAZE`; `REQ` → `NO EXPERIENCE. NO FORCE SENSITIVITY.`
  - Button "All events" → `/events`: `700 11.5px 'Chakra Petch'`, `letter-spacing:.16em`, uppercase, `#120c22` on `#c6b3e8`, `padding:11px 20px`, hover `#fff`

*Right column — hologram projection:*
- Square stage: `width:100%; max-width:360px; aspect-ratio:1`, centered flex
- Glow: `inset:6%`, `border-radius:50%`, `background: radial-gradient(circle, rgba(198,179,232,.3), transparent 68%)`
- Ring: `inset:2%`, `border-radius:50%`, `border:1px solid rgba(198,179,232,.25)`, `overflow:hidden`, containing a scan bar (`height:16px`, `linear-gradient(90deg,transparent,rgba(198,179,232,.5),transparent)`, `filter:blur(3px)`) animated `sweep 4.5s linear infinite`
- Logo `assets/logo.png` at `width:84%`, `filter: drop-shadow(0 0 40px rgba(160,120,235,.5))`, animated `floaty 8s ease-in-out infinite`
- Below: elliptical base glow (`70%` wide, `max-width:300px`, `height:16px`, `radial-gradient(ellipse, rgba(198,179,232,.4), transparent 70%)`) and caption `400 10px 'JetBrains Mono'`, `letter-spacing:.28em`, `#9b86cf` — `PROJECTION STABLE · 99.4%`

**Stat bar** — `background:#0e0a18`, `border-top:1px solid rgba(198,179,232,.14)`, `grid-template-columns: repeat(auto-fit, minmax(220px,1fr))`. Each cell `padding:20px 24px`, `border-right:1px solid rgba(198,179,232,.12)` (last cell none). Label `400 9.5px 'JetBrains Mono'`/`letter-spacing:.22em`/`#9b86cf`; value `700 15px 'Chakra Petch'`/`#efe9ff`/uppercase/`margin-top:6px`.
`MEETINGS → Every other week` · `MEMBERS → All welcome` · `COMMS → Discord · @uwstarwars` · `DUES → None`

**"What a meeting looks like"** — `background:#f7f3ff`, `padding:56px 28px 60px`. Kicker `◤ SECTION // 01` (`400 10.5px 'JetBrains Mono'`, `letter-spacing:.3em`, `#7a68a0`). H2 `800 34px/1 'Bricolage Grotesque'`, `letter-spacing:-.02em`, `#241a3a`, with an inline note `400 11px 'JetBrains Mono'`, `letter-spacing:.18em`, `#7a68a0` — `PICK YOUR PATH`.
Four cards, `repeat(auto-fit, minmax(230px,1fr))`, `gap:16px`; each `background:#fff`, `border:1px solid #e3d8f6`, `border-radius:14px`, `padding:22px 20px 24px`; hover `border-color:#c6b3e8`, `box-shadow:0 10px 30px rgba(90,60,160,.12)`.
Card head: a numbered chip (`700 10px 'JetBrains Mono'`, `letter-spacing:.18em`, `#4a3878` on `#eee6fb`, `padding:5px 9px`, `clip-path: polygon(5px 0,100% 0,100% calc(100% - 5px),calc(100% - 5px) 100%,0 100%,0 5px)`) followed by a `flex:1; height:1px; background:#e3d8f6` rule. **No imagery in these cards** (deliberate — the club doesn't have art for every activity).
Title `600 17px/1.2 'Bricolage Grotesque'`, `#241a3a`. Body `400 13.5px/1.55 Barlow`, `#5c5175`.
- `01` Movie nights — "Saga rewatches, deep cuts, and spirited prequel defenses."
- `02` Lightsaber meets — "Stances, strikes, choreography, and combat-form lore."
- `03` Game nights — "Trivia, charades, tabletop, and general chaos."
- `04` Debate nights — "Best duel. Worst plan. Who shot first."

**Past events** — `background:#efe9f9`, `padding:52px 28px 60px`. Kicker `◤ ARCHIVE // PAST TRANSMISSIONS`, H2 "Past events". Four posters in `repeat(auto-fit, minmax(180px,1fr))`, `gap:16px`; each `aspect-ratio:3/4`, `object-fit:cover`, `border-radius:10px`, `border:1px solid #ddd0f0`.

### 2. About (`/about`)

**Hero** — `linear-gradient(180deg,#0a0714,#140d26)`, starfield layer, `padding:56px 28px 52px`. Two columns `repeat(auto-fit, minmax(320px,1fr))`, `gap:36px`, `align-items:start`.
- Left: kicker `◤ SECTION // 01 · MISSION BRIEFING`; H1 `800 52px/1.02 'Bricolage Grotesque'`, `#fff`, `letter-spacing:-.03em` — "A fun space where / fans meet fans."; two paragraphs `400 16.5px/1.7 Barlow`, `#b0a3ca`, `max-width:520px`:
  1. "Star Wars Club at UW is for anyone who loves the galaxy far, far away — and for anyone who's curious and wants a way in. We watch the movies, play the games, swing the sabers, and argue about the lore. No trivia test at the door."
  2. "If you've seen everything twice, great. If you've never made it past the opening crawl, also great — a good chunk of the club started exactly there."
- Right: **CLUB RECORD** datapad panel — same clipped-corner treatment as the event console. Header `400 10px 'JetBrains Mono'`, `letter-spacing:.22em`, `#c6b3e8` — `CLUB RECORD // UW-SEA`. Spec grid (`auto minmax(0,1fr)`, `gap:10px 20px`, `400 12.5px/1.5 'JetBrains Mono'`, labels `#9b86cf`, values `#c2b3dd`):
  `STATUS → REGISTERED STUDENT ORG` · `CADENCE → EVERY OTHER WEEK` · `TIME → TBD — POSTED IN DISCORD` · `DUES → NONE` · `INTAKE → OPEN ALL QUARTER` · `COMMS → DISCORD · IG · EMAIL`

**"Before you show up" FAQ** — `background:#f7f3ff`, `padding:56px 28px 64px`. Kicker `◤ SECTION // 02 · COMMON QUESTIONS`, H2 "Before you show up". Six cards, `repeat(auto-fit, minmax(280px,1fr))`, `gap:16px`; five are `background:#fff`, `border:1px solid #e3d8f6`, `border-radius:14px`, `padding:22px`, question `600 17px 'Bricolage Grotesque'` `#241a3a`, answer `400 14px/1.6 Barlow` `#5c5175`:
- Do I need to know Star Wars? — "Nope. Plenty of members joined having seen one film or none. Newcomers are the whole point."
- How often do you meet? — "Usually every other week. Times and rooms get announced in the Discord as they're set."
- What happens at a meeting? — "It varies — a movie, a game, a lightsaber session, a debate. Members help pick what's next."
- Are there dues? — "None. Show up, hang out, that's membership."
- Can I join mid-quarter? — "Any time. There's no cutoff and no application — the Discord link is always open."

Sixth card is inverted: `background:#241a3a`, `box-shadow:0 0 30px rgba(120,80,200,.2)`, heading `#fff` "Still have a question?", body `#c2b3dd` "Email us or ask in the Discord — an officer will get back to you.", then a mailto link `700 11px 'JetBrains Mono'`, `letter-spacing:.14em`, `#c6b3e8` → hover `#fff`: `uwstarwars@uw.edu →`

> Note for the developer: an earlier version had a seasonal "orbit around the year" timeline. It was **removed on purpose** — the club has not planned meeting types by season. Don't reintroduce forward-looking schedule content.

### 3. Events (`/events`)

**Hero** — `linear-gradient(180deg,#0a0714,#140d26)` + scanline layer, `padding:52px 28px 56px`. Kicker `◤ SECTION // 02 · TRANSMISSIONS`, H1 "Events" (`800 52px/1.02 'Bricolage Grotesque'`, `#fff`).
Two columns `repeat(auto-fit, minmax(300px,1fr))`, `gap:24px`:
- **Featured event console** — same clipped panel, `padding:26px`. Header `NEXT TRANSMISSION` / `● LIVE`. Title `700 32px/1.05 'Chakra Petch'` uppercase `#f2ecff` — "Lightsaber Training / with Star Wars Club". Spec grid: `DATE → SEPTEMBER 25 · 1:00–3:00 PM`, `LOC → QUAD NORTH`, `TAG → DAWG DAZE`. Body `400 15px/1.65 Barlow`, `#b0a3ca`: "Learn the art of lightsaber combat at Lightsaber Academy! Practice Jedi and Sith-inspired stances, strikes, movement, and choreography while exploring the lore behind iconic combat forms. No experience or Force sensitivity required!" CTA "I'll be there" → Discord.
- **Event art** `assets/event-jedi-academy.png` — the club's own Jedi Academy banner. Must **not** be cover-cropped: `width:100%; height:auto; aspect-ratio:675/338; object-fit:contain; align-self:start; background:#05040a; border:1px solid rgba(198,179,232,.25)`. (Cover cropping cuts off the wordmark and logo badge.)

**"Coming up" schedule** — `background:#f7f3ff`, `padding:52px 28px 58px`. Kicker `◤ SCHEDULED // AUTUMN`, H2 "Coming up".
Rows: `grid-template-columns: minmax(120px,150px) minmax(0,1fr) auto`, `gap:20px`, `align-items:center`, `background:#fff`, `border:1px solid #e3d8f6`, `border-left:3px solid #c6b3e8`, `border-radius:10px`, `padding:18px 20px`; hover `border-left-color:#6f5aa8`, `box-shadow:0 8px 24px rgba(90,60,160,.1)`. Date `700 12px 'JetBrains Mono'`, `letter-spacing:.14em`, `#4a3878`. Title `600 17px 'Bricolage Grotesque'` `#241a3a`; sub `400 13.5px Barlow` `#5c5175`. Status pill `700 10px 'JetBrains Mono'`, `letter-spacing:.16em` — confirmed = `#fff` on `#4a3878`, tentative = `#4a3878` on `#eee6fb`.
- `SEP 25 · 1 PM` — Lightsaber Academy — Dawg Daze / "Quad North · open to everyone" / `CONFIRMED`
- `WEEK 2 · TBD` — First general meeting / "Meet the officers, plan the quarter, eat snacks" / `TBD`
- `WEEK 4 · TBD` — Movie night / "Feature picked by member vote in Discord" / `TBD`
- `WEEK 6 · TBD` — Game night & trivia / "Charade train returns, undefeated champions welcome" / `TBD`

Closing line `400 13.5px Barlow`, `#5c5175`: "Exact times and rooms get posted in the **Discord** — that's the fastest way to know where we are." (link `#5b3fa8`, `font-weight:600`)

**Past events** — identical to the Home archive block, `background:#efe9f9`, H2 "Past events", same four posters.

### 4. Officers (`/officers`)

`background: linear-gradient(180deg,#0a0714,#140d26 70%,#0e0a18)` + starfield, `padding:52px 28px 64px`. Kicker `◤ SECTION // 03 · IMPERIAL COMMAND`, H1 "Officers", intro `400 16px/1.65 Barlow`, `#b0a3ca`, `max-width:560px`: "The people who book the rooms, pick the movies, and take the titles far too seriously. Say hi at any meeting."

Five cards, `repeat(auto-fit, minmax(220px,1fr))`, `gap:20px`. Card: `border:1px solid rgba(198,179,232,.28)`, `background: linear-gradient(170deg, rgba(198,179,232,.09), rgba(198,179,232,.015))`, `padding:22px 20px 24px`, `clip-path: polygon(12px 0,100% 0,100% calc(100% - 12px),calc(100% - 12px) 100%,0 100%,0 12px)`; hover `border-color: rgba(198,179,232,.6)`.

Portrait: `width:100%`, `aspect-ratio:1`, `border-radius:50%`, `overflow:hidden`, `border:1px solid rgba(198,179,232,.3)`, striped fallback `repeating-linear-gradient(135deg, rgba(198,179,232,.14) 0 8px, rgba(198,179,232,.05) 8px 16px)`. Photo fills it (`position:absolute; inset:0; object-fit:cover`). A `sweep 5s linear infinite` scan bar (`height:14px`, `linear-gradient(90deg,transparent,rgba(198,179,232,.45),transparent)`, `filter:blur(3px)`) renders **over** the photo — this is what makes the portraits read as holograms; keep it. When no photo: centered `400 9.5px 'JetBrains Mono'`, `letter-spacing:.2em`, `#a894d0` — `HEADSHOT / PENDING`.

Text: role `700 9.5px 'JetBrains Mono'`, `letter-spacing:.2em`, `#c6b3e8`, `margin-top:16px`; name `600 19px/1.15 'Bricolage Grotesque'`, `#fff`, `margin-top:8px`; title in curly quotes `700 12px 'Chakra Petch'`, `letter-spacing:.06em`, `#b0a3ca`, uppercase, `margin-top:6px`.

| Role | Title | Name | Photo |
|---|---|---|---|
| CO-PRESIDENT | Emperor | Prisha Hemani | `assets/officer-prisha-v2.jpg` |
| CO-PRESIDENT | Supreme Chancellor | Andrew Frederick | `assets/officer-andrew-v2.jpg` |
| SECRETARY | Grand Admiral | Reece Thompson | *pending* |
| ACTIVITIES MANAGER | Grand Moff | Seraphyna Boone | `assets/officer-seraphyna.jpg` |
| SOCIAL MEDIA MANAGER | Minister of Information | Jenna Elle Pampo | *pending* |

### 5. Join (`/join`)

`linear-gradient(180deg,#0a0714,#140d26)` + starfield, `padding:56px 28px 60px`. Two columns `repeat(auto-fit, minmax(320px,1fr))`, `gap:36px`, `align-items:start`.

*Left:* kicker `◤ SECTION // 04 · ENLIST`, H1 "Join the club", body `400 16.5px/1.7 Barlow`, `#b0a3ca`, `max-width:480px`: "No dues, no application, no lore exam. Join the Discord, show up to something, and you're in."
Three numbered steps, `max-width:520px`, `gap:12px`; each `grid-template-columns: auto minmax(0,1fr)`, `gap:16px`, `border:1px solid rgba(198,179,232,.22)`, `padding:16px 18px`. Number chip `700 11px 'JetBrains Mono'`, `letter-spacing:.16em`, `#120c22` on `#c6b3e8`, `padding:6px 9px`. Step title `600 16px 'Bricolage Grotesque'`, `#fff`; body `400 14px/1.55 Barlow`, `#b0a3ca`.
1. Join the Discord — "Every meeting time, room, and movie vote gets posted there first."
2. Come to anything — "Movie night, game night, lightsaber meet — pick whatever sounds fun."
3. That's it — "You're a member. Bring a friend who's never seen a single film."

*Right:* comms panel (clipped, `padding:26px`). Header `COMMS CHANNELS // OPEN`. Full-width Discord button `700 13px 'Chakra Petch'`, `letter-spacing:.14em`, uppercase, `#120c22` on `#c6b3e8`, `padding:15px 20px`, centered, hover `#fff` — "Join the Discord →". Then a spec grid: `EMAIL → uwstarwars@uw.edu` (mailto, `#e6dcff`), `INSTA → @uwstarwars`, `DISCORD → discord.gg/mw2XpSzee`. `1px` rule `rgba(198,179,232,.24)`, `margin:22px 0`. Then `MAILING LIST // OPTIONAL`: email input (`flex:1`, `min-width:170px`, `background: rgba(255,255,255,.06)`, `border:1px solid rgba(198,179,232,.3)`, `color:#f2ecff`, `400 13px Barlow`, `padding:12px 14px`, `outline:none`, placeholder `you@uw.edu`) + Send button (`700 11px 'Chakra Petch'`, `letter-spacing:.16em`, `#120c22` on `#c6b3e8`, `padding:12px 18px`).

**Not wired up.** Pick a no-backend option — Formspree, Google Form, Mailchimp embed, or Netlify Forms — or delete the field and keep Discord/email as the only channels.

---

## Interactions & behavior

- **Nav**: five routes; active route shows the glowing 2px underbar. Prototype scrolls to top on navigation — with real routes this is default browser behavior.
- **Hover**: every card lifts via border-color change (+ soft shadow on light cards); nav links gain a 1px lavender border; buttons go `#c6b3e8 → #fff`. All are instantaneous in the prototype — a `transition: border-color .15s ease, box-shadow .15s ease, background .15s ease` is a welcome refinement.
- **External links**: Discord and Instagram open in a new tab with `rel="noopener"`.
- **Animations** (all decorative, all infinite):
  - `pulse` — `2s`/`3.6s ease-in-out`, opacity `.55 → 1 → .55` (LIVE dot, saber divider)
  - `blink` — `1.1s step-end`, opacity `1 → 0` at 50% (terminal cursor)
  - `floaty` — `8s ease-in-out`, `translateY(0 → -12px → 0)` (hologram logo)
  - `sweep` — `4.5s`/`5s linear`, `translateY(-100% → 400%)` (hologram scan bars)
  - Respect `prefers-reduced-motion: reduce` — disable `floaty`, `sweep`, and `pulse` (this is not in the prototype and should be added).
- **Responsive**: every multi-column grid is `repeat(auto-fit, minmax(Npx,1fr))`, so the layout collapses without media queries. Verify at 375px: the H1s (`62px`/`52px`) should be reduced with `clamp()` — e.g. `clamp(38px, 8vw, 62px)`. The header nav wraps to a second line at narrow widths; a hamburger is a reasonable production upgrade.

## State management
Only UI state: current route (production: the URL) and optionally mailing-list form state (`email`, `submitting`, `submitted`, `error`). No data fetching. Officers and events are static arrays — a small JSON or MDX/CMS file is a good idea so club officers can update them without touching components.

## Design tokens

**Colors**
| Token | Hex | Use |
|---|---|---|
| space-900 | `#0a0714` | page background, darkest hero stop |
| space-800 | `#0e0a18` | footer, stat bar |
| space-700 | `#120c22` | hero mid stop |
| space-600 | `#140d26` | secondary hero stop |
| ink-900 | `#241a3a` | dark cards, light-section headings |
| ink-700 | `#4a3878` | chips, confirmed pill |
| ink-500 | `#5c5175` | body text on light |
| ink-400 | `#6f5aa8` | placeholder text, hover accents |
| lavender-500 | `#c6b3e8` | **primary accent** (from logo) |
| lavender-400 | `#c2b3dd` | secondary text on dark |
| lavender-300 | `#b0a3ca` | body text on dark |
| lavender-200 | `#9b86cf` | mono labels, disclaimer |
| lavender-100 | `#a894d0` | placeholder mono |
| lavender-050 | `#e6dcff` | link text on dark |
| paper-100 | `#f2ecff` | headings on dark |
| paper-200 | `#efe9ff` | stat values |
| light-100 | `#f7f3ff` | light section background |
| light-200 | `#efe9f9` | archive section background |
| light-300 | `#eee6fb` | chips, pills |
| line-light | `#e3d8f6` | light borders |
| line-light-2 | `#ddd0f0` | poster borders |
| link | `#5b3fa8` | inline links on light |

Recurring alphas: `rgba(198,179,232,.045 / .05 / .12 / .14 / .16 / .22 / .24 / .25 / .28 / .3 / .32 / .45 / .6)` — all the lavender accent at varying opacity.

**Typography** — Google Fonts: `Bricolage Grotesque` (600/800, display + card titles), `Chakra Petch` (400/600/700, terminal-flavored UI + uppercase titles), `JetBrains Mono` (400/700, labels, specs, kickers), `Barlow` (400/500/600, body).
Scale: `9.5, 10, 10.5, 11, 11.5, 12, 12.5, 13, 13.5, 14, 15, 16, 16.5, 17, 19, 26, 32, 34, 52, 62`px.
Letter-spacing: `-.03em` (display), `-.02em` (H2), `.14em`, `.16em`, `.18em`, `.2em`, `.22em`, `.28em`, `.3em`, `.32em` (mono/uppercase).

**Radii**: `0` (clipped panels), `8px`, `10px`, `14px`, `50%`, `999px`.
**Clip-path corner cuts**: `5px` (chips), `8px` (nav CTA), `10px` (footer cards), `12px` (officer cards), `14px` (consoles). Formula: `polygon(Npx 0,100% 0,100% calc(100% - Npx),calc(100% - Npx) 100%,0 100%,0 Npx)`.
**Shadows**: `0 8px 24px rgba(90,60,160,.1)`, `0 10px 30px rgba(90,60,160,.12)`, `0 0 30px rgba(120,80,200,.2 / .25)`, `0 0 10px #c6b3e8` (glow underbar), `0 0 40px rgba(160,120,235,.5)` (hologram drop-shadow), `0 0 0 1px rgba(198,179,232,.4)` (logo ring).
**Spacing**: `4, 6, 8, 10, 11, 12, 14, 16, 18, 20, 22, 24, 26, 28, 30, 34, 36, 44, 52, 56, 60, 64`px.

## Assets
All in `assets/` in this bundle. All are the club's own material — no third-party or studio assets.
- `logo.png` — SWC UW roundel (husky in helmet). Source of the whole palette. Used in header, hologram hero, footer.
- `event-jedi-academy.png` — the club's Jedi Academy / Dawg Daze banner (675×338). Events page.
- `poster-lastmeeting.jpg`, `poster-may4th.jpg`, `poster-tesb.jpg`, `poster-anh.jpg`, `poster-jep.jpg` — hand-drawn club posters used in the "Past events" grids. `poster-jep.jpg` is currently unused (it's the Star Wars Jeopardy poster) — available if a fifth archive tile is wanted.
- `officer-prisha-v2.jpg`, `officer-andrew-v2.jpg`, `officer-seraphyna.jpg` — square headshots (420–720px) cropped from photos the club supplied.
- **Pending from the club**: headshots for Reece Thompson and Jenna Elle Pampo; a debate-night poster. Keep the "HEADSHOT PENDING" placeholder until they arrive.
- Optimize for production: convert posters and headshots to WebP/AVIF with width-appropriate sizes; the posters are large phone-camera JPEGs.

## Files
- `SWC UW Website.dc.html` — **the design to recreate**. All five pages, final copy, final styles.
- `SWC UW Site.dc.html` — earlier exploration canvas: three homepage directions (turn 1) and two blends (turn 2). Option `2b` is what the final site is built from. Reference only; do not implement.
- `assets/` — all images referenced above.

## Suggested production stack
Static Next.js (App Router) or Astro + Tailwind, deployed on GitHub Pages / Netlify / Vercel. Fonts via `next/font` or a self-hosted subset (four families is heavy — subset to the weights listed above). Officers and events in a single `content/` JSON so club officers can edit without code. Add `<meta>` OG tags with the logo, and a favicon from `logo.png`.
