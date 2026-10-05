# Cross-site work session — 2026-10-05 (hub / handoff)

One long session touched most of the sites linked from the stuff page. This is the index; each repo's own
`ai-notes/` has the detail. Local checkouts live in `~/silly/<repo>` (incubator: `~/incubator`).

## What changed, per site (all pushed and live unless marked)
| site | repo notes | what |
|---|---|---|
| stuff page (this repo) | 01 (ratings bits), this doc | Incubator listed via `/incubator/` redirect (rating key); "(made for …)" notes removed; list is one CSS grid so every row's stars align; ratings: "thanks ✓" for 1.5 s then "avg 4.5 · 4 votes" (phones stack "avg 4.5" over "4 votes"); unlisted `/ratings/` overview with a "remove my votes (this browser)" button |
| ratings API (`~/silly/ratings`, Cloudflare Worker, no git remote) | 02 | atomic upsert, JSON+CORS errors, `POST /forget`; Rob's own 17 votes deleted from D1 (3 home-IP voters) |
| autism-test | 04 | six items (new "how smart do you feel" Q2; comfort media no longer names the film; Eisenberg line-up at Q4; checklist edits), mobile report shows the chart straight after the score, intro trimmed |
| incubator (`~/incubator`, Incubatorr account) | — (no ai-notes) | drawings grow 50% faster on touch devices. Pushing needs the Incubatorr gh account: `gh auth switch -u Incubatorr && git push && gh auth switch -u RT567` (Rob runs it; the agent may not use that account's token) |
| talkingbeers | 04 | start-location picker on load (fixed centre pin + Start here), form = day · time window · stops, stays snap to quarter hours and stretch ≤30 min to meet specials, Lime/What filters/hint removed, map credit shrunk, legend flag |
| snowpack | 03 | phone layout (tap to select, one compact card at the bottom, weak layers / av report pop-outs, scene shifted up); PC weak list capped above the report; render on demand (fixes shimmer when another WebGL page is rendering) |
| moongrader (+ source in `~/silly/moonboard-stack/my-app2`) | 01 | grading status no longer hidden behind the board; board centred and fits phones (shared `board-left`, `#app` clip, viewport width 430 on small phones); logo narrowed; 38 MB dev output and dead code removed. Backend (Render) works but takes 50–100 s per grade — Rob: "it is what it is" |
| landmarks | 02 | phone-only CSS (zoomed drawing, bigger controls, no sideways scroll). Uses CSS `zoom` (Firefox 126+) |
| curlysim | 02–05 | first-person legs + true 1.98 m boards for everyone; crowd 1–20 along the whole beach (north/south seat limits from the headlands); wave-train jam fixed (the "sudden quarter speed"); literature-based physics; seabed from the NSW Marine LiDAR 2018 survey; seat just outside where set waves break, capped at Hs 2 m; ocean tiling/phone quality (perf agent) |

## Not pushed yet
- **curlysim real day/night** (commit 838f1c3, local): removes the old "always late morning" sun; nights are
  moonlit and readable. Waiting for Rob to look at it (bd curlysim-l9b).

## Open threads (bd issues filed in curlysim)
Sandbars/rips from the LiDAR data, tide, second swell, crest bending, rendered wave trough — see
`curlysim/ai-notes/05-…` "Next session".

## Working notes for the next agent
- Rob tests in the same Chrome window the agent drives (chrome-devtools MCP). Two "bugs" today were him and
  the agent using it at once, and heavy WebGL pages in other windows changed measurements. Use an
  `isolatedContext` per page, close test pages after, and ask before assuming a bug.
- Local dev servers on this PC (Rob keeps them running): quiz 8123, incubator 8124, stuff-page test copy
  8125 (points at the local ratings worker 8787), root site 8126, moongrader 8352, talkingbeers 8370,
  snowpack 8380 (vite), curlysim 8390 (vite). `nocache_server.py` was a scratch script (session scratchpad).
- Rob's style: wants things pushed live once he's happy, asks "TLDR" often; surfer-scale heights are
  "measured from the back" (≈ half the face); 2 m Hs ≈ "six foot", about the biggest that matters at Curly.
