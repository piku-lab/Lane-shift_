# Lane Shift — Capacitor + AdMob build

This folder is a minimal Capacitor project wrapping `www/index.html` (your game,
already wired for `@capacitor-community/admob` with your real ad unit IDs).

The `android/` native project is **not** committed — GitHub Actions generates it
fresh on every push (`npx cap add android`), then patches in your AdMob App ID
and builds the APK for you. Termux never needs the Android SDK.

## One-time setup

1. **Create an empty GitHub repo** on github.com (no README/license — keep it empty).
2. **Get a Personal Access Token**: GitHub → Settings → Developer settings →
   Personal access tokens → generate one with `repo` scope. Copy it somewhere safe.

## In Termux

```bash
pkg update -y
pkg install git nodejs -y

# unzip the project you downloaded from Claude, then:
cd lane-shift-app

git init
git branch -M main
git config user.name "YOUR_NAME"
git config user.email "YOUR_EMAIL"

git add -A
git commit -m "Capacitor + AdMob setup"

# use your token in place of a password
git remote add origin https://YOUR_GITHUB_USERNAME:YOUR_TOKEN@github.com/YOUR_GITHUB_USERNAME/YOUR_REPO.git
git push -u origin main
```

## Watching the build

1. Go to your repo on github.com → **Actions** tab.
2. The "Build Android APK" workflow starts automatically on push (takes a few minutes).
3. When it finishes, open the run → scroll to **Artifacts** → download
   `lane-shift-debug-apk` → unzip → install `app-debug.apk` on your phone.

Every future `git push` rebuilds it automatically.

## Things to double check before you push

- `capacitor.config.json` → `appId` is currently `com.laneshift.game`. Change it
  to whatever package name you actually registered (or plan to register) in
  Play Console / AdMob, if different.
- The AdMob App ID and ad unit IDs are already inside `www/index.html` and get
  injected into the manifest by the workflow — no need to edit them unless
  AdMob gives you new ones.
- This builds a **debug** APK (unsigned, fine for testing installs directly on
  your phone). For a Play Store release build you'll need a signing keystore —
  ask me and I'll add that step to the workflow.
- Real ads won't render in an emulator/unregistered test device and can take
  up to an hour to start serving on brand-new ad units. Use
  Settings → AD DIAGNOSTICS in the app to confirm the AdMob plugin loaded.
