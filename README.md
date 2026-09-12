# Scan — Offline QR Reader

A web-based QR scanner that works fully offline once it's loaded — no server calls
happen during scanning, everything decodes locally on your phone.

## What it can scan

Live camera or a photo from your gallery, and it recognizes:

- Website links — opens directly
- Wi-Fi codes — shows network name/password, lets you copy the password
- Contact cards (vCard) — shows name/phone/email, lets you call
- Phone numbers — lets you call
- SMS / text message codes — lets you open your messaging app pre-filled
- Email (`mailto:`) — opens your mail app
- Location codes (`geo:`) — opens in Google Maps
- Calendar events — shows event name and location
- Plain text — just displays it

Every scan is also saved to an on-device history (tap the clock icon), stored only
in your phone's browser storage — nothing is uploaded anywhere.

## Important: why you need to host it once (briefly online)

Phone browsers only allow camera access (`getUserMedia`) on a page loaded over
**HTTPS**, or on `localhost`. This is a browser/OS security rule, not something in
this app's control. It does **not** mean the app needs the internet to run — it
only needs to be *served* over https once.

The good news: after the page has loaded once, it registers a **service worker**
that caches every file (`index.html`, the QR-decoding library, icons). From then
on it runs completely offline — you can turn on airplane mode and it still works,
including "Add to Home Screen" so it opens like a normal app icon.

The **"Scan from photo"** button needs no camera permission at all and works even
before you've ever gone online — handy if you already have a QR code saved as an
image.

## How to host it (pick one, all free)

**Option A — GitHub Pages (recommended, permanent link)**
1. Create a new GitHub repository and upload all the files in this folder (keep the folder structure).
2. Go to the repo's Settings → Pages → set source to the `main` branch, root folder.
3. GitHub gives you a link like `https://yourname.github.io/reponame/`.
4. Open that link on your phone once (needs internet this one time).

**Option B — Netlify Drop (fastest, no account needed)**
1. Go to https://app.netlify.com/drop on a computer.
2. Drag this whole folder onto the page.
3. It instantly gives you a live `https://...netlify.app` link — open it on your phone.

**Option C — Test locally on your computer first**
```
npx serve .
```
Then open the shown `http://localhost:3000` address in a desktop browser to check
everything works before hosting it properly (a phone on the same Wi-Fi generally
can't use your computer's IP address for camera access, since it isn't https —
this is just for a quick local check).

## Installing it like an app on your phone

Once you've opened the hosted link on your phone:
- **Android (Chrome):** tap the ⋮ menu → "Add to Home screen" / "Install app".
- **iPhone (Safari):** tap the Share icon → "Add to Home Screen".

After that, launch it from your home screen — it opens full-screen with no browser
bar, and works with no internet connection at all.

## Files in this project

```
qr-scanner-app/
├── index.html          # The scanner UI and all logic
├── jsQR.js              # QR-decoding library, bundled locally (no internet needed)
├── manifest.json         # Makes it installable as an app
├── service-worker.js     # Caches everything for offline use
└── icons/
    ├── icon-192.png
    ├── icon-512.png
    └── favicon.png
```

## Customizing

- Change colors: edit the `:root { ... }` CSS variables near the top of `index.html`.
- Change the app name shown when installed: edit `name`/`short_name` in `manifest.json`.
- Swap icons: replace the files in `icons/` (keep the same filenames and sizes).
