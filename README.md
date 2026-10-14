# Lift - personal workout app

Single-file static web app. Tracks your Upper A / Lower A / Upper B / Lower B rotation, logs sets, and holds a benchmark (weight + target reps per set) for every exercise that only ever moves up.

## Features

- Auto-suggests the next day in the rotation (UA > LA > UB > LB), with a manual override.
- Every exercise has a benchmark, e.g. Flat DB Bench 80 lb x 11 / 9 / 9. Rep buttons for each set centre on that set's target.
- Custom value input ("+/-" button) opens an inline number pad box for anything outside the suggested range.
- One exercise on screen at a time. A numbered strip at the top shows status (green tick = done, green outline = partly done, yellow = on screen); tap any number to jump when a machine is taken. Swipe left/right also switches.
- Finishing an exercise's last set jumps to the next unfinished one automatically.
- Back arrow returns to the day picker without losing anything; a Resume card brings you back. Reopening the app mid-workout drops you straight back in.
- Weight pre-fills from the benchmark weight. Changing it during a workout only affects that workout until you Finish.
- "Last" line and "PR" line on every exercise.
- Silent rest timer (90s default, plus 60s / 2m / 3m chips). Visual by default; optional vibrate-on-end toggle in Settings (Android only - iOS has no vibration API).
- Once all sets are logged, the card tells you what will happen to the benchmark at Finish.
- Screen stays awake during an active session (Wake Lock API), so the rest timer doesn't vanish behind the lock screen.
- Settings sheet: edit/add/reorder/delete exercises, benchmarks, notes, units (lb/kg) and count type (reps/secs/steps/m). Plan edits made mid-session sync into the active workout.
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

## How benchmarks work

Applied when you tap **Finish workout**:

1. **Same weight:** raised only if every set matches or beats its target and at least one beats it. The new benchmark is exactly what you did. 11/9/9 -> you do 12/9/10 -> new benchmark 12/9/10. You do 12/8/10 -> stays 11/9/9. Falling short never lowers it.
2. **Heavier weight:** the heavier weight becomes the default and the reps you hit become the benchmark (even if lower).
3. **Lighter weight, or a set not logged:** nothing changes.

Rep buttons: target minus 2 to plus 3 (target outlined), so Flat DB Bench set 1 (target 11) shows 9-14. Outside the range? Tap "+/-".

## Editing exercises

Tap the sliders icon. Pick a day. Rename, change weight/unit/sets/count type, type a benchmark like `11/9/9` (it sets the number of sets), add a note (e.g. "per side"), reorder, delete, or add new ones. Changes save instantly.

## Updating the default plan

Defaults live in `DEFAULT_PLAN` in `index.html`. After changing them, bump `PLAN_VERSION`: the new defaults are then applied once to the plan saved on the phone. History is never touched, and exercises listed under `aka` (old names) keep their history.
