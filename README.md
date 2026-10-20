# Personal Trainer

Personal Trainer (formerly Iron Log) is a mobile-first training app in a single `index.html`, hosted on GitHub Pages.

## Features
- **AI Pick sessions** and manual sessions (Push, Pull, Legs, Upper, Full Body and more), or build your own.
- **Training location:** Gym or Home (bodyweight only). Older "Apartment Gym" settings are moved to Gym automatically.
- **Energy check-in and auto intensity** (High / Moderate / Light / Recovery).
- **Re-entry:** after 10+ days off you get a lighter re-entry session instead of a "fresh, go heavy" one.
- **Set-by-set logging** with a rest timer, form tips, videos and swaps ("Can't do this?").
- **Effort rating per exercise** (Easy / Just right / Hard). This drives the rule-based coach:
  - All reps hit and **Easy** → +10 lb on lower body, +5 lb on upper body.
  - **Just right** → +5 lb on compound lifts, +2.5 lb on isolation lifts.
  - **Hard** → hold the weight.
  - Missed reps → hold. Missed and Hard, or missed two sessions in a row → 10% deload.
  - Bodyweight moves rated Easy **level up** to a harder version next time (incline → push-up → decline → archer; squat → split squat → Bulgarian → assisted pistol; and so on). You can tap "go back" to undo.
- **Volume tab:** recovery status per muscle, this week's calendar, and weekly set targets.
- **Stats tab:** body weight log, PRs (auto-detected), per-exercise progress chart, sessions per week, and an editable session log (fix sets and reps, edit notes, delete entries).
- **Backup:** Profile (♂/♀ button) → Your data → Backup JSON / Restore / CSV export.
- **Installable and offline:** add it to your home screen from Safari's Share menu or Chrome's ⋮ menu.

## Data
Everything is stored in `localStorage` on your device. The storage keys are unchanged from Iron Log (`ig_h6`, `ig_s6`, `ig_w5`, `ig_sets_*`, …), so existing history carries over. The upgrade only adds new keys: `ig_prog` and `ig_ladder`, plus an optional `effort` field on logged exercises.

When you change `index.html`, bump `VERSION` in `sw.js` so installed copies update.
