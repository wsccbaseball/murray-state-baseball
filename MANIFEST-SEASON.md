# MSU Season Filters — MANIFEST

**Date:** 2026-09-15  
**Repo:** https://github.com/wsccbaseball/murray-state-baseball  
**Commit message (intended):** `MSU: spring/fall season filters like WS`

## Goal
Port Walters State spring/fall `season` + `game_type` filters onto Murray State season pages, matching WS `ws-season-hitting.html` / `ws-season-pitching.html` behavior while keeping Murray branding and MSU Supabase.

## Files updated (under `/workspace/msu-parity/`)

| File | Change |
|------|--------|
| `ms-season-hitting.html` | Season dropdown (`seasonSel`), Game Type (`typeSel`), games fetch includes `season,game_type`, `seasonLabel()` / newest-season default / intersquad fallback, game multi-select filtered by season+type, dynamic header/card labels. **SAVE ALL RACER CARDS (ZIP) preserved.** MSU Supabase `cmljtqgnctrtfszmowtf` only. |
| `ms-season-pitching.html` | Same season/type filter port as hitting. ZIP preserved. MSU Supabase only. |
| `ms_csv_uploader.html` | Games upsert now sets `season` + `game_type`. Season inferred from date (month 8–12 → `YYYY-fall`, else `YYYY-spring`). Default `game_type` = `game`; optional Game Type select in modal. |
| `MANIFEST-SEASON.md` | This file. |

## Left alone (confirmed OK)
| File | Status |
|------|--------|
| `ms-game-summary.html` | Already has season/type UI + MSU Supabase. No changes needed. |

## Behavior (parity with WS)
- Distinct `games.season` values populate `seasonSel`
- `typeSel`: Games only / Intersquads only / All
- Game multi-select rebuilt from current season+type
- Labels: `Murray State · Spring 2026` via `seasonLabel()`
- Default to newest season by date; if that season has no real (`game`) games, default type to `intersquad`
- Query params: `?season=` / `?type=` / legacy `?intersquad=1`

## Push status
`gh` is **not** authenticated on this box. No GitHub MCP upload tool available. Files are ready under `/workspace/msu-parity/` for manual GitHub web upload (or `gh auth login` then push).

## Manual upload checklist
Upload these to `main` at https://github.com/wsccbaseball/murray-state-baseball :
1. `ms-season-hitting.html`
2. `ms-season-pitching.html`
3. `ms_csv_uploader.html`
4. `MANIFEST-SEASON.md` (optional)
