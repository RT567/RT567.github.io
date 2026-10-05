# RT567.github.io — root user site: history, structure, rules

Live: https://rt567.github.io/  ·  Repo: github.com/RT567/RT567.github.io (branch `main`, Pages serves `/`, legacy build)

## What this repo is now

The **root of every GitHub Pages site the owner has**. `index.html` at the root is a bare, unstyled-ish list of links to each project site (each project is its own repo and its own Pages site under `rt567.github.io/<repo>/`, except the two FBi folders which live in this repo). The old e-portfolio still exists at `/eportfolio/` but is not linked.

```
index.html            <- landing page: <ul> of links, alphabetical, "(made for X)" notes
eportfolio/           <- the 2023–2025 e-portfolio (index.html, style2.css, images, resume.pdf/.docx). Unlinked.
fbiautotracklist/     <- GENERATED. FBi Radio tracklists. Written by an automated job. Do not hand-edit.
fbimap/               <- GENERATED-ish. FBi program similarity map (index.html + data.json).
ai-notes/             <- these notes
```

## Timeline

| date | event |
|---|---|
| 2023-09-04 | Site created as an **e-portfolio** for job hunting: `index.html` + `style2.css`, banner, resume download, sections for skills / experience / academic work with tech logos (C, Clojure, Java, Maven, Postgres, Python, Git, Linux, Windows, Nagios, ROS, FPGA/Arduino, inverted-pendulum video). |
| 2023-09-09 → 10-07 | Content fixes; two rounds of edits from a friend's suggestions ("tess suggestions", "tess-changes"). |
| 2024-08-11/12 | Wording tweaks, new resume link. |
| 2025-03-03/10 | "Spruce up": animated water background (`water.gif`); edits from advice from Vinuk. This is the last real portfolio content update — the portfolio is **frozen as of 10 March 2025**. |
| 2026-08-30 | `fbiautotracklist/` added: auto-generated tracklists of FBi Radio shows. ~30 rapid UI commits the same night (calendar, badges, platform icons, mobile pass). An out-of-date banner added to the portfolio the same day. |
| 2026-08-30 → ongoing | A background job commits `fbiautotracklist: update YYYY-MM-DD HH:MM` every ~15–20 min. See "The auto-updater" below. |
| 2026-08-31 | `fbimap/` added: "FBi program similarity map (past year of tracklists, artist overlap)"; later search box, genre lookups, guaranteed graph connectivity. |
| 2026-09-02 | **Restructure.** Everything portfolio-related moved into `eportfolio/` (`git mv`, paths inside were all relative so nothing broke; note `Maven Image.png` has a space and needed its own `git mv`). Root `index.html` replaced by the link list. Portfolio banner changed to say "accurate as of 2024". Links added: websight, latina-map (made for jackson), curlysim, fbiautotracklist, fbimap, cake-cutter (made for reggie), landmarks, moongrader. Links removed: eportfolio (owner: "get rid of it on the landing page"), github-slideshow (a 2021 GitHub Learning Lab starter repo; deleting the repo itself needs `gh auth refresh -h github.com -s delete_repo` which hasn't been run). `moonboard` renamed to `moongrader` (repo rename + link). List sorted alphabetically. |
| 2026-09-30 | Links added: snowpack, and four new silly sites (each its own repo + Pages): autism-test, every-onion, magic-8-ball (links to `/magic-8-ball/set/`, the rigging page, on Rob's request; the bare `/magic-8-ball/` is what recipients get), put-it-in-the-backpack. Later the same day: reflection (a pure black page), and the landing page became a dated chronological timeline. |
| 2026-10-05 | Link added: beer-compass (arrow to the nearest open pub/bar/bottle shop in Sydney; repo RT567/beer-compass). |

## The auto-updater (important)

The FBi projects have their own, much fuller notes: `~/silly/autotracklist/ai-notes/` (README + numbered docs; `07-current-state.md` is kept current). Read those before touching anything FBi-related.

A systemd **user** timer on this PC runs the FBi tracklist generator and pushes into this repo:

- `systemctl --user list-timers | grep autotracklist` → `autotracklist.timer` → `autotracklist.service`
- `ExecStart=/home/r/silly/autotracklist/.venv/bin/atl auto`, `WorkingDirectory=/home/r/silly/autotracklist`
- It writes into `fbiautotracklist/` in **this working copy** (`/home/r/silly/RT567.github.io`), commits, and pushes.

Consequences for anyone editing here:
1. Expect `fbiautotracklist/episodes.json` (and friends) to be modified/uncommitted at any moment. Leave them alone; the job owns them.
2. Before committing your own change run `git pull --rebase --autostash`, and `git add` only the files you touched (never `git add -A`, never `git commit -a`).
3. If you hit `.git/index.lock` exists, the job is mid-commit; wait a few seconds and retry.
4. Don't rename/move `fbiautotracklist/` or `fbimap/` without also changing the generator in `~/silly/autotracklist`.

## Landing page conventions

(Changed 2026-09-30: was an alphabetical list; Rob asked for a chronological timeline.)

- Plain `<ol>` timeline, **oldest first**, one `<li><time>YYYY-MM-DD</time><a href="/<repo>/">name</a></li>` per site.
- The date is the **first release**, not last update: the repo's `created_at` (≈ first Pages deploy) in AEST,
  or the first commit of the folder for `fbiautotracklist/` and `fbimap/`. Ties on the same day go in release order.
- Optional dedication after the link: ` (made for jackson)`, ` (made for reggie)`.
- magic-8-ball links to `/magic-8-ball/set/` (the rigging page), on Rob's request.
- Title `rt567`, heading `stuff`, a few lines of inline CSS (18px sans / 14px on phones, 600px max-width,
  small monospace grey dates with a hanging indent, no timeline line). Keep it very, very basic.
- **Star ratings** (added 2026-09-30): an inline `<script>` at the bottom adds 1–5 stars after each link, but only
  for sites the visitor has opened from this page; after rating they see the average, and hovering/tapping the stars
  lets them change their vote. Backend is a Cloudflare Worker + D1 in `~/silly/ratings` (read its ai-notes). The
  script keys ratings by the link's first path segment, so new `<li>`s get stars automatically.
  Unlisted overview of all ratings at `/ratings/` (not linked; for Rob). After a vote the row shows your pick with
  "thanks ✓" for 1.5s, then the average as "avg 3.0 · 4 votes".
  2026-10-05: the list is one CSS grid (date | name | stars; `li { display: contents }`) so every row's stars start
  at the same x; "(made for …)" notes removed; dates .7em. Phones (≤440px) stack "avg 4.5" over "4 votes"
  in .62em type (`small.avg` inline-flex column) so rows never wrap and keep their height.
- When a new Pages site appears in the owner's account (`gh api repos/RT567/<repo>/pages`), append it at the bottom with today's date.

## Portfolio (`eportfolio/`)

Static, no build. `index.html` links `style2.css` and assets by relative path. There's a yellow "NOTE: this site is out of date — accurate as of 2024." box near the top. Owner considers it historical; don't modernise unless asked.

## Deploy / verify

`git push` → Pages build ~1 min. `gh api repos/RT567/RT567.github.io/pages/builds/latest --jq .status` shows `building` / `built`. Verify with `curl -s https://rt567.github.io/ | grep '<li>'`.
