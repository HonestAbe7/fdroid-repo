# One-time setup

## 1. Create the repo

On GitHub, create a new **public** repository named exactly `fdroid-repo`.
(A public repo is needed because GitHub Pages on a free account only serves
public repos. It holds APKs only — no source code.)

## 2. Add the files

Add these files to the repo (paths are repo-relative):

- `.github/workflows/fdroid-repo.yml`
- `fdroid/config.template.yml`
- `fdroid/.gitignore`
- `README.md`
- `SETUP.md` (this file)

## 3. Add the three secrets

Repo **Settings → Secrets and variables → Actions → New repository secret**.
The values were sent in chat — paste them exactly:

- `FDROID_KEYSTORE_B64` — the base64 of the repo index signing keystore
- `FDROID_KEYSTORE_PASS` — the keystore password
- `FDROID_KEY_PASS` — the same password again (PKCS12 needs both)

Save the password in your password file too. If the key is ever lost,
everyone has to re-add the repo.

## 4. Turn on GitHub Pages

Repo **Settings → Pages → Build and deployment → Source: GitHub Actions**.

## 5. Upload the first APK

Ball Sort v1.0.1: in the `ball-sort` repo, open **Actions → latest
"Build debug APK" run** and download the `ball-sort-debug-apk` artifact
(a zip — unzip it to get `app-debug.apk`). Upload that APK to
`fdroid/repo/` in this repo. You can rename it to `ball-sort-1.0.1.apk`
first if you like; the name does not matter.

(Or re-run the build workflow in `ball-sort` for a fresh artifact.)

## 6. Run the workflow

Uploading the APK starts the workflow automatically. You can also run it
by hand from the **Actions** tab ("Self-hosted F-Droid repo" → Run workflow).
It installs fdroidserver, builds the signed index, and publishes the site.

## 7. Check it

Visit `https://honestabe7.github.io/fdroid-repo/` — the landing page shows
the repo address and fingerprint.

## 8. Add it in Droid-ify

Droid-ify → **Repositories → +** → paste:

```
```

Check the fingerprint it shows matches this one:

```
4488ADBC5ADB43457ADD261B91BA89072780C83899E4A2294C66518CDC45C6FC
```

Ball Sort should appear, and it installs **over** the version already on
the device — same signing key, no uninstall needed.

## Later

- New version of an app: upload the new APK to `fdroid/repo/`; the workflow
  republishes automatically.
- Another app: upload its APK the same way. Nothing else changes.
- Fully automatic APK pull from the app repos (no manual upload) is a later
  step — it needs a read-only GitHub token because the app repos are private.
