# Trusted Upload — Murray State (MSU)

**Status (this parity pass):** UI polish shipped on `ms_csv_uploader.html`.  
**Edge Function secret path:** deferred — needs Supabase project access for secrets + function deploy on the **MSU** project (`cmljtqgnctrtfszmowtf`).

Do **not** copy WS `UPLOAD_SECRET` or WS Edge Function URLs into MSU. MSU has a separate Supabase project.

## What still works today

`ms_csv_uploader.html` continues to upload via **direct anon REST**:

- `POST /rest/v1/pitches`
- `POST /rest/v1/games` (upsert / merge-duplicates) with `display_name`

That path was intentionally left intact so uploads do not break while Edge ingest is not deployed for MSU.

## UI polish shipped

- Fixed modal markup corruption (stray markdown fences around the game-info form).
- Clearer **Display Name** field: placeholder + hint that the name drives report dropdowns.
- Smarter auto-suggest: prefers the opponent team vs Murray, appends `Game 1`.
- Existing display name is still preserved when the game already exists.
- Hub Data card text now mentions display-name tagging.

## What is needed for WS-style trusted upload (later)

Mirror Walters State `trackman-ingest` on the **MSU** Supabase project.

### 1) Edge Function

Deploy a `trackman-ingest` (or `ms-trackman-ingest`) function on project `cmljtqgnctrtfszmowtf` that:

1. Reads header `x-upload-secret` (or equivalent).
2. Compares to Supabase secret `UPLOAD_SECRET` (MSU-specific value).
3. On success: inserts/upserts `games` + `pitches` with **service role** (never expose service role in HTML).
4. Supports a schema/columns probe action if the WS function does (so the client does not probe-insert).

Use the WS function as a reference implementation only — deploy into the MSU project.

### 2) Supabase secrets (Dashboard → Edge Functions → Secrets)

| Secret | Purpose |
|--------|---------|
| `UPLOAD_SECRET` | Shared with the phone browser localStorage field; never commit to GitHub |

Optional later: rotate anon key after guest writes are locked.

### 3) Frontend switch (only after function works)

Update `ms_csv_uploader.html` to match WS `WS_TrackMan_Uploader.html` pattern:

- Password field **Upload Secret** → `localStorage` (e.g. `msu_trackman_upload_secret`).
- All game/pitch writes go to `/functions/v1/trackman-ingest` with `apikey` (anon) + `x-upload-secret`.
- Keep display-name tagging in the same payload.
- Do **not** remove REST upload until the secret path is proven on a real CSV.

### 4) After Friday-style proof (MSU)

Only then tighten RLS: remove open anon insert/update/delete on `games` / `pitches` while keeping anon **SELECT** for hub/reports (or replace with RPCs).  
**Do not change RLS in this parity pass.**

## Related WS docs (reference)

- WS live uploader: `WS_TrackMan_Uploader.html`
- WS handoff notes: `HANDOFF.md` / `TRUSTED-UPLOAD.md` (WS project `mghrapwhvihpwgqryail`)

## Blockers for completing Edge upload in this pass

- No Supabase dashboard / CLI auth for MSU project secrets.
- No `gh` auth on the box to push or manage Actions.
- Must not invent or reuse WS secrets.
