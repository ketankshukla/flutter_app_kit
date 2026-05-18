# Enabling the GitHub Actions CI workflow

`android-build.yml` lives here (instead of `.github/workflows/`) because the initial push was made with a `gh` token that did not have the `workflow` scope.

## To enable CI (one-time)

Pick **one** option.

### Option A — refresh your gh token

```powershell
gh auth refresh -s workflow
# Follow the browser prompt to authorize. Then:
mkdir .github\workflows -Force
Move-Item .\ci\android-build.yml .github\workflows\android-build.yml
git add .github/workflows/android-build.yml ci/android-build.yml
git commit -m "ci: enable Android build workflow"
git push
```

### Option B — paste via GitHub web UI

1. On GitHub.com, navigate to your repo.
2. Click **Actions** → **New workflow** → **set up a workflow yourself**.
3. Name the file `android-build.yml`.
4. Paste the contents of `ci/android-build.yml` from this folder.
5. Commit on `main`.

## Required GitHub secrets (for either option)

Settings → Secrets and variables → Actions → New repository secret:

- `ANDROID_KEYSTORE_BASE64` — base64 of `android/app/upload-keystore.jks`
- `ANDROID_KEYSTORE_PASSWORD`
- `ANDROID_KEY_PASSWORD`
- `ANDROID_KEY_ALIAS` (typically `upload`)
- `GOOGLE_SERVICES_JSON` — full JSON contents
- `REVENUECAT_ANDROID_KEY` — `goog_...`

Generate the base64 keystore secret with:

```powershell
[Convert]::ToBase64String([IO.File]::ReadAllBytes("android\app\upload-keystore.jks")) | Set-Clipboard
```
