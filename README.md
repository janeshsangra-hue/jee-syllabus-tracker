# Syllabus Tracker app

An installable web app (PWA): chapter tracker, daily progress, study timer and monthly planner.
It works offline, installs on your laptop and tablet, and keeps your data in step between them.

## 1. Put the app online (once, about 5 minutes)

An installable app has to live at its own web address that starts with https. The easiest free way is GitHub Pages:

1. On github.com press **New repository**. Name it, for example, `syllabus-tracker`. Make it **Public**.
2. Press **Add file, Upload files** and drag in everything from this folder: `index.html`, `manifest.webmanifest`, `sw.js` and the `icons` folder. Press **Commit**.
3. Open **Settings, Pages**. Under "Build and deployment" choose **Deploy from a branch**, pick the `main` branch and the `/ (root)` folder, then Save.
4. After a minute the app is at `https://YOUR-NAME.github.io/syllabus-tracker/`.

(Netlify and Cloudflare Pages work the same way: upload this folder and you get an https address.)

## 2. Install it

- **Android tablet (Chrome):** open your address, tap the menu, then **Install app** (or Add to Home screen).
- **iPad (Safari):** tap Share, then **Add to Home Screen**.
- **Laptop (Chrome or Edge):** click the install icon at the right end of the address bar.

The app opens in its own window with its own icon, and still works with no internet.

## 3. Turn on syncing between laptop and tablet (once, about 5 minutes)

Your data is stored on each device. To share it, the app uses a free Firebase Realtime Database that only you know about.

1. Go to **console.firebase.google.com**, press **Create a project**, give it a name and finish (analytics can be off).
2. In the left menu open **Build, Realtime Database**, press **Create Database**, pick a location and choose **Start in test mode**.
3. Open the **Rules** tab and replace everything with this, then press **Publish**:

```
{
  "rules": {
    "sync": { "$code": { ".read": true, ".write": true } }
  }
}
```

4. Copy the database address at the top of the Data tab. It looks like `https://your-project-default-rtdb.firebaseio.com`.
5. In the app press **Sync**, paste the address and press **Create sync space**. A long sync code appears.
6. On your other device open the app, press **Sync**, paste the same address and the sync code, and press **Join**. (Shortcut: on the first device press **Copy link for my tablet** and open that link on the tablet.)

From then on, changes on one device appear on the other within a few seconds. If one device is offline, its changes are sent as soon as it is back online.

### How safe is it?

The sync code is a random 32-character string and is the only "password": anyone who has both the database address and the code can read and change your data, and nobody else can find it. Treat the code like a password and do not share the "link for my tablet" publicly. Disconnect any time from the Sync window; your data stays on the device.

### Good to know

- If both devices change the **same** part at the same moment, the most recent change wins.
- The page refreshes itself when new data arrives from your other device, but waits while you are typing or while the study timer is running.
- **Export data / Import data** in the Sync window lets you move your data by hand, without Firebase.
- The first visit needs internet so the fonts can load; after that everything is cached.

## Files

- `index.html`: the app
- `manifest.webmanifest`: name, icons and colours for installing
- `sw.js`: lets the app run offline
- `icons/`: app icons


## Recovery update (October 2026)

- Kept the existing project as the base, including its task/chapter data keys, custom sync engine, manifest, icons, and offline service worker.
- Added a responsive guard so the monthly planner's task-panel toggle remains visible and does not shrink away on narrow/portrait layouts.
- Updated the service-worker shell cache version to `syllabus-v2` so the new `index.html` can replace the prior cached shell.
- The daily planner already uses a 6:00 AM planning-day boundary in its date/time logic. Event records retain their real date/time; the timeline groups and positions them relative to the 6:00 AM boundary.
- The task/event editor already stores checkbox visibility separately from completion state; existing `done` values are not repurposed.

Deploy the contents of this folder at the same repository path as your existing `index.html`. Keep the repository's existing GitHub Pages settings. If your existing live site has data, do not clear browser storage; the update preserves the app's existing storage keys.
