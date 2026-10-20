# Personal Trainer

Personal Trainer (formerly Iron Log) is a mobile-first training app served as a single static page (`index.html`) on GitHub Pages. React is bundled locally in `vendor/` and the JSX is precompiled, so it loads fast and works offline after the first visit.

## Features
- **One-tap start:** Today suggests a session; tap **▶ Start today's session**. The energy check-in is optional.
- **Weekly goal streak:** "X of Y this week" (goal 2–6, default 3) and how many weeks in a row you've hit it.
- **Training blocks:** 2–3 main lifts per session type stay the same for about 4 weeks (separately for Gym and Home) so you can add weight. Accessories rotate, swaps stick for the rest of the block, and after 4 weeks you're offered a new block.
- **AI Pick sessions** and manual sessions (Push, Pull, Legs, Upper, Full Body and more), or build your own.
- **Training location:** Gym or Home (bodyweight only). Older "Apartment Gym" settings are moved to Gym automatically.
- **Energy check-in and auto intensity** (High / Moderate / Light / Recovery).
- **Re-entry:** after 10+ days off you get a lighter re-entry session instead of a "fresh, go heavy" one.
- **Set-by-set logging** with a rest timer, form tips, videos and swaps ("Can't do this?" only lists moves your current location supports).
- **Effort rating per exercise** (Easy / Just right / Hard). This drives the rule-based coach:
  - All reps hit and **Easy** → +10 lb on lower body, +5 lb on upper body.
  - **Just right** → +5 lb on compound lifts, +2.5 lb on isolation lifts.
  - **Hard** → hold the weight.
  - Missed reps → hold. Missed and Hard, or missed two sessions in a row → 10% deload.
  - Bodyweight moves rated Easy **level up** to a harder version next time (incline → push-up → decline → archer; squat → split squat → Bulgarian → assisted pistol; and so on). You can tap "go back" to undo.
- **Volume tab:** recovery status per muscle, this week's calendar, and weekly set targets.
- **Stats tab:** body weight log, PRs (auto-detected), per-exercise progress chart, sessions per week, and an editable session log (fix sets and reps, edit notes, delete entries).
- **Settings (⚙️):** location, intensity, difficulty, goal, weekly goal, and Profile & data.
- **Backup:** ⚙️ → Profile & data → Your data → Backup JSON / Restore / CSV export.
- **Installable and offline:** add it to your home screen from Safari's Share menu or Chrome's ⋮ menu.

## Data
Everything is stored in `localStorage` on your device. The storage keys are unchanged from Iron Log (`ig_h6`, `ig_s6`, `ig_w5`, `ig_sets_*`, …), so existing history carries over. The upgrades only add new keys: `ig_prog`, `ig_ladder`, `ig_wk_target` (weekly goal), `ig_block` (training block), `ig_wdismiss` (weigh-in reminder) and `ig_datefix` (log of date-key fixes), plus an optional `effort` field on logged exercises.

Days are keyed by your **local** date. Older versions used UTC, so evening sessions could land on the next day. On first load, those entries move back to the correct local day, but only when that day is free. This never overwrites anything and only runs once.

## Editing and building
`index.html` is **generated**, so don't edit it directly.

1. Edit `src/index.src.html`. It's the readable source, with JSX in `<script type="text/babel">` blocks.
2. Build:
   ```sh
   npm install      # first time only (installs @babel/standalone)
   npm run build    # compiles JSX → plain JS and writes index.html
   ```
3. Bump `VERSION` in `sw.js` so installed copies pick up the update.
4. Commit `src/index.src.html`, `index.html` and `sw.js`, then push to `main`. GitHub Pages serves the repo root.

`vendor/` holds React 18.2.0 production UMD builds. The service worker precaches them along with the page and icons.
