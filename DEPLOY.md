# Deploy checklist

- [ ] 1. Log in to github.com (create a free account if you don't have one).
- [ ] 2. Create a new repo called `lift`. Make it **public** — GitHub Pages on a private repo needs a paid plan. Public is fine: it's just the app code, your workout data never leaves your phone.
- [ ] 3. Upload the files. No terminal needed: in the new repo, click "uploading an existing file" and drag in all 7:
  - `index.html`
  - `sw.js`
  - `manifest.webmanifest`
  - `icon-180.png`, `icon-192.png`, `icon-512.png`
  - `README.md`
  Commit.
- [ ] 4. Repo Settings > Pages > Source: "Deploy from a branch", Branch: `main`, Folder: `/ (root)`. Save.
- [ ] 5. Wait a minute, then on your **phone** open: `https://<your-username>.github.io/lift/`
- [ ] 6. In Safari: Share button > **Add to Home Screen** > Add.
- [ ] 7. Open it from the home screen icon (full-screen, no browser bar). Done.

First workout: tap the suggested day, log sets, hit Finish. Afterwards do Settings > Export JSON once to confirm backup works.

Later updates: edit the file in the repo (or re-upload), then close and reopen the app twice — the service worker picks up new versions on the next load.
