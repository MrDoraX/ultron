# Ultron

**Free local MVP** — voice-first personal assistant with a **red Jarvis-style HUD**.

- Offline-first (no API keys required)
- Optional cloud LLM boost when online **and** a key is present
- Electron desktop (Windows / Linux) + Capacitor Android / iOS
- Phone → PC via **Tailscale** (or same Wi‑Fi LAN)

> **Honest limits:** wake word works only while the app is **open and the screen is on**.  
> No screen-off / always-on wake. Not bank-grade biometrics.  
> No “unlimited / uncensorable / free forever” claims.

---

## FREE install (quick start)

```bash
git clone https://github.com/MrDoraX/ultron.git
cd ultron
npm install
cp .env.example .env   # optional — leave keys empty for fully offline
npm run build          # must pass
```

| Goal | Command |
|------|---------|
| Web UI | `npm run dev` |
| Desktop + phone bridge | `npm run electron:dev` |
| Production check | `npm run build` |

Chrome/Edge recommended for Web Speech on desktop.

---

## Downloadable PC builds (Electron → `release/`)

### Windows portable (recommended download)

**Already built on this workspace:**

`release/Ultron-1.0.0-portable.exe` (~73 MB) — single file, no installer.

**Rebuild on Windows (easiest) or Linux (Wine-capable):**

```bash
cd ultron
npm install
npm run electron:build:win
```

Outputs:

| File | What it is |
|------|------------|
| `release/Ultron-1.0.0-portable.exe` | **Portable** — copy & run |
| `release/Ultron-1.0.0-win-x64.exe` | NSIS installer (if targeted) |
| `release/win-unpacked/Ultron.exe` | Unpacked folder run |

Double-click the portable `.exe` on Windows. Pair code + LAN/Tailscale IPs appear in **Panel** (bridge port **8765**).

### Linux AppImage / unpacked

```bash
npm run electron:build:linux
```

| File | What it is |
|------|------------|
| `release/Ultron-1.0.0-linux.AppImage` | Portable AppImage (~109 MB) |
| `release/linux-unpacked/ultron` | Unpacked binary |

```bash
chmod +x release/Ultron-1.0.0-linux.AppImage
./release/Ultron-1.0.0-linux.AppImage
# or:
./release/linux-unpacked/ultron
```

### Both platforms (when toolchain allows)

```bash
npm run electron:build:all
```

Env: `ULTRON_BRIDGE_PORT=8765` (default).

---

## Android APK (Capacitor + Android Studio)

**Requirements:** Android Studio (SDK + JDK 17+), USB debugging or emulator.  
The `android/` project is in the repo (mic: `RECORD_AUDIO`).

### Steps (Studio — recommended)

```bash
cd ultron
npm install
npm run cap:sync:android    # builds web → syncs into android/
npm run cap:open:android    # opens Android Studio
```

In Android Studio:

1. Wait for Gradle sync to finish.  
2. **Build → Build Bundle(s) / APK(s) → Build APK(s)**.  
3. Install debug APK on device/emulator.  
4. Grant **Microphone** on first launch.

**Debug APK path:**

`android/app/build/outputs/apk/debug/app-debug.apk`

### Steps (CLI)

```bash
# Needs ANDROID_HOME + JDK on PATH
npm run android:debug
# same APK path as above
```

Wake listening is **foreground / screen-on only** (no screen-off wake).  
After web code changes: always re-run `npm run cap:sync:android`.

---

## iOS (macOS + Xcode)

```bash
npm run cap:sync:ios
npm run cap:open:ios
```

Grant Microphone. WKWebView STT may be limited — text / tap-to-talk fallback remains. No Siri-style screen-off wake.

---

## Login · multi-user · voice

1. **Register** (username + password or PIN) — **PBKDF2-SHA-256** hashes (never plaintext).  
2. **Voice enrollment** → later: **login** (mandatory) → **voice unlock**.  
3. **Hello sir** once after first full unlock (`greetedOnce` per user).  
4. Data isolated: `u:{userId}:settings | voiceprint | memories | chat`.  
5. TTS: **Jarvis Hindi style** (`hi-IN` male); words: **Roman Urdu + English**.  
6. Wake: **Ultron** — app open + screen on only.

## Features

Confirm actions · hot phrases (lights/music/lock/brief/quiet/emergency) · yaad-rakh memory · daily brief · quiet hours · privacy LED · red HUD animations · offline-first + auto cloud when key present · Tailscale phone→PC.

## Phone ↔ PC (Tailscale)

1. Tailscale on PC + phone (same account).  
2. Run Electron on PC → note **pair code** + port `8765`.  
3. Phone Panel → Tailscale IP (`100.x.x.x`) or MagicDNS + code → **Connect**.  
4. `Ultron PC pe YouTube khol`

Same Wi‑Fi: LAN IP works. Optional `relay/` is dev-only.

## Env (optional)

| Variable | Purpose |
|----------|---------|
| `VITE_OPENAI_API_KEY` / `VITE_ANTHROPIC_API_KEY` | Cloud boost |
| `ULTRON_BRIDGE_PORT` | Bridge port (default `8765`) |
| `VITE_ULTRON_RELAY_URL` | Dev relay (prefer Tailscale) |

## Push to GitHub

Remote: `https://github.com/MrDoraX/ultron`  
Local commit is ready; CLI push needs **your** login (agent has no git credentials):

```bash
cd /workspace/ultron   # or your clone
git remote add origin https://github.com/MrDoraX/ultron.git  # if missing
gh auth login          # or SSH
git push -u origin main
```

Do **not** invent or paste tokens into the repo or chat. See `PUSH.md`.
