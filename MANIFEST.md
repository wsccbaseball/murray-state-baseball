# MSU ↔ WS parity pack — MANIFEST

**Prepared:** 2026-09-15  
**Repo:** https://github.com/wsccbaseball/murray-state-baseball (confirmed)  
**Live Pages:** https://wsccbaseball.github.io/murray-state-baseball/  
**WS reference (read-only):** https://github.com/wsccbaseball/ws-baseball  

**Push status:** `gh` is **not** authenticated on this box — files are staged under `/workspace/msu-parity/` only. No commit URL.

**MSU Supabase (copied from existing MSU pages, not invented):**  
`https://cmljtqgnctrtfszmowtf.supabase.co` + existing anon key from `index.html` / `ms_csv_uploader.html` / reports.  
Team filter: `MUR_RAC` / `mur` substring (existing MSU convention).

---

## Files ready to upload

All paths below are under `/workspace/msu-parity/`:

| File | Action | What changed |
|------|--------|----------------|
| `ms-game-summary.html` | **NEW** | Port of WS `ws-game-summary.html` → Murray branding, MSU Supabase, scope filter `mur` / Murray State Racers, navy `#002147` |
| `opposing-pitcher-scout.html` | **NEW** | Port of WS scout; excludes `MUR_RAC`; Murray copy; navy header |
| `umpire-rankings.html` | **NEW** | Port of WS rankings; MSU Supabase; `favorMSU` / Favor MSU labels; `isMSU()` matches `mur` |
| `pitcher-report.html` | **UPDATE** | JSZip + **SAVE ALL RACER CARDS (ZIP)** for Murray pitchers in selected game (mirrors WS SAVE ALL WS CARDS) |
| `hitter-report.html` | **UPDATE** | JSZip + **SAVE ALL RACER CARDS (ZIP)** for Murray hitters in selected game |
| `ms-season-pitching.html` | **UPDATE** | JSZip + **SAVE ALL RACER CARDS (ZIP)** for filtered season pitcher view (`MUR_Racer_season_pitcher_cards.zip`) |
| `ms-season-hitting.html` | **UPDATE** | JSZip + **SAVE ALL RACER CARDS (ZIP)** for filtered season hitter view (`MUR_Racer_season_hitter_cards.zip`) |
| `ms_csv_uploader.html` | **UPDATE** | Display-name polish + fixed broken markdown fences in modal; **REST upload unchanged** |
| `index.html` | **UPDATE** | Hub links: Full Game Summary, Opposing Pitcher Scout, Umpire Season Rankings; Data card mentions display-name tagging |
| `TRUSTED-UPLOAD-MSU.md` | **NEW** | Docs for Edge Function secret path (deferred) |
| `MANIFEST.md` | **NEW** | This file |

Source scratch (not for upload): `src/ws/`, `src/msu/`.

---

## Shipped vs deferred

### Shipped
1. Full Game Summary (`ms-game-summary.html`)
2. Opposing Pitcher Scout
3. Umpire Season Rankings
4. Bulk Racer card ZIPs on pitcher / hitter / season pitching / season hitting
5. Hub section structure aligned with WS (Game Reports + Scouting + Advanced Metrics)
6. Uploader display-name UI polish (safe; does not break current upload)

### Deferred
- **Trusted Edge upload (`trackman-ingest` + `UPLOAD_SECRET`)** for MSU — needs Supabase secrets access on project `cmljtqgnctrtfszmowtf`. See `TRUSTED-UPLOAD-MSU.md`.
- WS bulk expansions on WS repo (explicitly out of scope).
- Any RLS / policy changes.
- GitHub push (no `gh` auth).

---

## Push steps (when ready)

### Option A — GitHub web upload
1. Open https://github.com/wsccbaseball/murray-state-baseball  
2. Upload / replace the HTML files listed above (and optionally the two `.md` docs).  
3. Commit to `main` (Pages serves `main`).  
4. Hard-refresh https://wsccbaseball.github.io/murray-state-baseball/

### Option B — Contents API (authenticated machine)
```bash
# From a machine with gh auth or a PAT that can write to the repo:
# For each file, PUT /repos/wsccbaseball/murray-state-baseball/contents/<path>
# with message like: "MSU parity: game summary, scout, umpire rankings, Racer ZIP cards"
```

### Option C — local git (your laptop; do not git-clone on the restricted box)
```bash
git clone https://github.com/wsccbaseball/murray-state-baseball.git
cd murray-state-baseball
# copy files from /workspace/msu-parity/*.html and *.md
git add ms-game-summary.html opposing-pitcher-scout.html umpire-rankings.html \
  pitcher-report.html hitter-report.html ms-season-pitching.html ms-season-hitting.html \
  ms_csv_uploader.html index.html TRUSTED-UPLOAD-MSU.md MANIFEST.md
git commit -m "MSU parity with WS: full game summary, scout, umpire rankings, Racer ZIP cards"
git push origin main
```

---

## Quick smoke checks after deploy
1. Hub shows three new tools; navy/gold unchanged.  
2. Full Game Summary loads MSU games; Murray vs Opp scope works.  
3. Opposing Pitcher Scout lists non-`MUR_RAC` teams only.  
4. Pitcher Game Summary → pick game → **SAVE ALL RACER CARDS (ZIP)** downloads a zip.  
5. Uploader still uploads CSV + can save display name (modal fields visible).  
6. No WS Supabase URL/key on any MSU page.

## Exact paths on the box

- `MANIFEST.md` — 4,619 bytes — `/workspace/msu-parity/MANIFEST.md`
- `TRUSTED-UPLOAD-MSU.md` — 3,111 bytes — `/workspace/msu-parity/TRUSTED-UPLOAD-MSU.md`
- `hitter-report.html` — 46,546 bytes — `/workspace/msu-parity/hitter-report.html`
- `index.html` — 16,936 bytes — `/workspace/msu-parity/index.html`
- `ms-game-summary.html` — 35,313 bytes — `/workspace/msu-parity/ms-game-summary.html`
- `ms-season-hitting.html` — 71,905 bytes — `/workspace/msu-parity/ms-season-hitting.html`
- `ms-season-pitching.html` — 68,791 bytes — `/workspace/msu-parity/ms-season-pitching.html`
- `ms_csv_uploader.html` — 19,156 bytes — `/workspace/msu-parity/ms_csv_uploader.html`
- `opposing-pitcher-scout.html` — 21,384 bytes — `/workspace/msu-parity/opposing-pitcher-scout.html`
- `pitcher-report.html` — 41,767 bytes — `/workspace/msu-parity/pitcher-report.html`
- `umpire-rankings.html` — 14,113 bytes — `/workspace/msu-parity/umpire-rankings.html`
