# Lift - personal workout app

Single-file static web app. Tracks your Upper A / Lower A / Upper B / Lower B rotation, logs sets, shows last session and PRs so you've got something to chase.

## Features

- Auto-suggests the next day in the rotation (UA > LA > UB > LB), with a manual override.
- Per-set rep buttons centered on what you did *that set* last time. First set was 11, sets 2-4 were 8? You'll see different ranges per set.
- Custom value input ("+/-" button) opens an inline number pad box for anything outside the suggested range.
- Finished exercises collapse to a one-line summary (tap to reopen), so the next one moves up.
- Weight pre-fills from your last session and stays editable per workout.
- "Last" line and "PR" line on every exercise.
- Silent rest timer (90s default, plus 60s / 2m / 3m chips). Visual by default; optional vibrate-on-end toggle in Settings (Android only - iOS has no vibration API).
- Progression hints: match or beat every set from last session at the same weight and the exercise card tells you to add weight next time.
- Screen stays awake during an active session (Wake Lock API), so the rest timer doesn't vanish behind the lock screen.
- Settings sheet: edit/add/reorder/delete exercises, change units (lb/kg/secs/steps/m). Plan edits made mid-session sync into the active workout.
- Export / import JSON backup. Uses the share sheet on iOS (Save to Files / AirDrop), falls back to a download elsewhere.
- Genuinely works offline via a service worker (network-first, cache fallback). Add to home screen on iOS/Android for full-screen.

## Storage

All data lives in `localStorage` on the device. Nothing leaves your phone. The app requests persistent storage (`navigator.storage.persist()`) to reduce the chance the browser evicts it, but clearing browser data still wipes history - use Settings > Export JSON occasionally as backup.

## Deploy to GitHub Pages

1. Create a new repo on GitHub (e.g. `lift`). Make it private if you prefer.
2. Push these files to the repo root:
   - `index.html`
   - `sw.js`
   - `manifest.webmanifest`
   - `icon-180.png`, `icon-192.png`, `icon-512.png`
   - `README.md`
3. In the repo: Settings > Pages > Source = "Deploy from a branch", Branch = `main`, Folder = `/ (root)`. Save.
4. Wait ~30 seconds. The URL will be `https://<your-username>.github.io/lift/`.
5. Open that URL on your phone, then "Add to Home Screen" so it launches full-screen.

### One-time terminal setup (if you don't already have the repo)

```bash
cd /path/to/this/folder
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin git@github.com:<your-username>/lift.git
git push -u origin main
```

Then enable Pages in the repo Settings as above.

## How rep buttons work

Each set looks at *that set's* prior value independently. Example, Incline DB Bench last session was 70 lb / 11-8-8-8:

- Set 1 buttons: 9, 10, 11, 12, 13, 14
- Set 2 buttons: 6, 7, 8, 9, 10, 11
- Set 3 buttons: 6, 7, 8, 9, 10, 11
- Set 4 buttons: 6, 7, 8, 9, 10, 11

Tap the number you hit. Outside the range? Tap "+/-" and enter it.

## Editing exercises

Tap the gear icon. Pick a day. Rename, change weight/unit/sets/count type, reorder with Up/Down, delete, or add new ones. Changes save instantly.

## Notes

- Weight unit stays per-exercise (lb or kg) since you alternate based on what the equipment displays.
- "Heavy DB Holds" uses seconds, "DB Hold Lunges" uses steps, "Farmers Carries" uses meters - already configured.
- "Bicep Finishers" and "Wrist + Reverse Curls" are placeholders (weight 0, 3 sets reps). Edit them to whatever you actually do.
