# Ultron in-app updates (Chrome-style)

End users do **not** need to wipe/reinstall for every release. Ultron checks a remote JSON manifest and shows an **Update available** banner when a newer version exists.

## How it works (end user)

1. On launch (and **Panel → Check for updates**), Ultron fetches:
   - `VITE_UPDATE_MANIFEST_URL` if set, else
   - `https://raw.githubusercontent.com/MrDoraX/ultron/main/update-manifest.json`
2. If `manifest.version` > current app version → banner: **Update available**.
3. **Download & install** (Electron auto-starts this when an update is found):
   - Prefers `win.parts[]` (≤10MB chunks) → downloads each part in-process → concatenates → verifies full `sha256` → applies portable replace + relaunch.
   - Falls back to `win.portableUrl` (single exe) if parts are absent.
   - Progress stays **inside the banner**. On failure: English in-app error + **Retry**.
   - **Never opens a browser** for updates.
4. **Later** dismisses the banner for that version only.
5. Current version is always shown in the Panel (e.g. `v1.2.2`).

## Publishing (maintainer)

```bash
# 1) Bump package.json version
# 2) npm run electron:build:win
# 3) Split into ≤10MB parts:
split -b 10485760 -d -a 2 release/Ultron-X.Y.Z-portable.exe release/parts10/part_
# rename to U01.bin … U08.bin
# 4) Upload U01–U08 to GitHub Release vX.Y.Z
# 5) CI workflow "Assemble portable from parts" joins → Ultron-X.Y.Z-portable.exe
# 6) Update update-manifest.json on main (version, sha256, parts[], portableUrl)
```

Parts are preferred so large single GitHub assets are optional; ≤10MB also avoids Google Drive virus-scan HTML when using Drive `uc?export=download` links.
