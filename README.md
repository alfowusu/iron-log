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
- **Cardio log:** quick entry for run, walk, bike, row or other: duration, optional distance (mi) and effort. Entries show in the Log, Stats → Cardio has weekly minutes with a chart, and cardio counts toward the weekly goal (one per day, toggle in Stats → Cardio; on by default).
- **Body measurements:** optional waist, chest, hips, arms and thighs (in) under Stats → Body, with trend charts. The weekly weigh-in banner reminds you about them too, once you've started tracking.
- **Cloud backup (optional):** automatic backup to the private repo `alfowusu/trainer-data`. See below.
- **Volume tab:** recovery status per muscle, this week's calendar, and weekly set targets.
- **Stats tab:** body weight log, PRs (auto-detected), per-exercise progress chart, sessions per week, and an editable session log (fix sets and reps, edit notes, delete entries).
- **Settings (⚙️):** location, intensity, difficulty, goal, weekly goal, and Profile & data.
- **Backup:** ⚙️ → Profile & data → Your data → Backup JSON / Restore / CSV export.
- **Installable and offline:** add it to your home screen from Safari's Share menu or Chrome's ⋮ menu.

## Data
Everything is stored in `localStorage` on your device. The storage keys are unchanged from Iron Log (`ig_h6`, `ig_s6`, `ig_w5`, `ig_sets_*`, …), so existing history carries over. The upgrades only add new keys: `ig_prog`, `ig_ladder`, `ig_wk_target` (weekly goal), `ig_block` (training block), `ig_wdismiss` (weigh-in reminder), `ig_datefix` (log of date-key fixes), `ig_cardio`, `ig_cardio_counts` and `ig_meas`, plus an optional `effort` field on logged exercises. Cloud backup uses `pt_gh_token` (the token) and `pt_cloud` (sync status). These start with `pt_`, not `ig_`, so they are never included in backups or exports.

Days are keyed by your **local** date. Older versions used UTC, so evening sessions could land on the next day. On first load, those entries move back to the correct local day, but only when that day is free. This never overwrites anything and only runs once.

## Cloud backup setup (one time, about 2 minutes)
The app can back itself up to your **private** GitHub repo `alfowusu/trainer-data` so your coach can read your log and you can restore on a new phone.

1. On your phone, open this link. It pre-fills the token's name, owner (alfowusu), 1-year expiry and the **Contents** permission:
   <https://github.com/settings/personal-access-tokens/new?name=Personal%20Trainer%20backup&description=Lets%20the%20Personal%20Trainer%20app%20save%20backups%20to%20alfowusu%2Ftrainer-data&target_name=alfowusu&expires_in=366&contents=write>
2. Under **Repository access**, choose **Only select repositories** and pick **alfowusu/trainer-data**. GitHub can't pre-fill this step.
3. Under **Permissions**, check that it shows **Contents: Read and write**. Metadata: Read-only is added automatically. Don't add anything else.
4. Tap **Generate token** and copy it. It starts with `github_pat_`.
5. In the app, go to **⚙️ → Profile & data → Cloud backup**, paste the token and tap **Connect & back up**. You should see "✓ Last synced today …".

From then on, the app backs up after every finished session, weigh-in, measurement and cardio log, and when you open it (at most every 5 minutes). If you're offline, it waits and uploads when you're back. It never slows the app down.
- **New phone:** paste a token the same way, then tap **Restore from cloud**. A new or emptier device never overwrites a bigger cloud backup without asking.
- **Disconnect** removes the token from the phone. The backup on GitHub stays.
- The token is stored only on that phone. It is never put in the repo or in any backup. If it expires (after 1 year), the app shows "GitHub rejected the token". Disconnect, make a new token with the same link and paste it in.

The backup is `data/backup.json`, in the same format as **Backup (JSON)**. Every sync is a commit, so the repo history is also a version history.

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
