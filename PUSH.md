# Push this repo from your machine

Git credentials are not available in the build agent. Local commit is ready under `/workspace/ultron`.

```bash
cd /workspace/ultron   # or your clone
git remote add origin https://github.com/MrDoraX/ultron.git   # if missing
gh auth login   # or use SSH
git push -u origin main
```

Do **not** invent or paste PATs into chat/repo files.
