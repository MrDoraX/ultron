# Push Ultron to GitHub

Remote: **https://github.com/MrDoraX/ultron** (private).

## Status

- Full source committed locally on this box (`main`).
- Agent seeded remote with README + package configs via GitHub MCP.
- **`git push` failed here** — no `gh` / HTTPS credentials on the agent (by design; no tokens invented).

## Push the full tree from your machine

```bash
cd /workspace/ultron

git remote -v
# should show: https://github.com/MrDoraX/ultron.git

gh auth login
# or: git remote set-url origin git@github.com:MrDoraX/ultron.git

git push -u origin main
```

If remote already has the seed commits and histories diverge:

```bash
git pull --rebase origin main
git push -u origin main
```

## Built artifacts (local only — gitignored)

| Artifact | Path |
|----------|------|
| Windows portable | `release/Ultron-1.0.0-portable.exe` |
| Linux AppImage | `release/Ultron-1.0.0-linux.AppImage` |

These are **not** committed (`release/` in `.gitignore`). Copy from the build machine or rebuild with the README commands.
