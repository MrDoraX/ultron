# Ultron

**Free local MVP** — voice-first personal assistant with a **red Jarvis-style HUD**.

See full README after complete push from local clone.

## FREE install

```bash
git clone https://github.com/MrDoraX/ultron.git
cd ultron
npm install
npm run build
npm run electron:dev
```

### PC downloadable builds

```bash
npm run electron:build:linux   # → release/Ultron-1.0.0-linux.AppImage
npm run electron:build:win     # → release/Ultron-1.0.0-portable.exe
```

### Android APK

```bash
npm run cap:sync:android
npm run cap:open:android
# or: npm run android:debug
# APK: android/app/build/outputs/apk/debug/app-debug.apk
```

Wake word: **Ultron** (app open / screen on only). No unlimited/uncensorable claims.
