# Jimat — OAuth homepage & privacy policy

Two static pages to satisfy Google Cloud Console → **OAuth consent screen → Branding**:

| Console field | Value |
|---|---|
| Application home page | `https://tansengkai.github.io/Jimat/` |
| Application privacy policy link | `https://tansengkai.github.io/Jimat/privacy.html` |
| Authorized domain | `tansengkai.github.io` |
| Terms of service | *(leave empty)* |

## Publish (5 minutes)

### 1. Push the pages to the Jimat repo

```bash
# from this folder
cd ~/.openclaw/workspace/jimat-pages
git init -b main
git add index.html privacy.html SETUP.md
git commit -m "Add GitHub Pages site: homepage + privacy policy for Google OAuth consent"
git remote add origin https://github.com/tansengkai/Jimat.git
git push -u origin main
```

(You'll be asked for your GitHub username + a Personal Access Token with `repo` scope as the
password. Create one at github.com → Settings → Developer settings → Personal access tokens.)

### 2. Enable Pages

Jimat repo → **Settings → Pages**

- Source: **Deploy from a branch**
- Branch: **main**, folder: **/ (root)**
- Save, wait ~60 seconds

### 3. Verify

- https://tansengkai.github.io/Jimat/
- https://tansengkai.github.io/Jimat/privacy.html

Both must return HTTP 200 and show the Jimat pages (not the calendar app).

## Why the path `/Jimat/` and not the root

The root `https://tansengkai.github.io/` is already serving the **Personal Calendar Assistant**
pages used by your Calendar OAuth app. Google allows any path on an authorized domain, so Jimat
gets its own path and neither app is disturbed.

## Console settings that matter

**Authorized domain** must still be `tansengkai.github.io` — that domain is already verified in
Search Console under your Google account. If Jimat lives in a **different Google Cloud project**
but the **same Google account**, verification carries over and you can add the domain immediately.
If it's a different Google account, re-verify ownership of `tansengkai.github.io` in Search Console
first (HTML file upload to the Pages site is the easiest method).

**Publishing status — this is the trap that bit the Calendar app:**

- If the app stays in **Testing**, Google expires refresh tokens after **7 days**.
- For Jimat you use the `drive.file` scope, which is **non-sensitive**. Non-sensitive scopes do
  **not** require Google's verification review.
- So: set User type = **External** → **Publish App** → **In production**. There's no review needed
  for `drive.file`, and refresh tokens stop expiring after 7 days.

Verify later: if the token JSON has no `refresh_token_expires_in` field, the refresh token is
permanent.

## Scope used by the app

```
https://www.googleapis.com/auth/drive.file
```

`drive.file` lets the app read/write **only the files it created**. It cannot browse the rest of
the user's Drive. This is the right scope for app-data backup and keeps you out of the restricted
scope audit process entirely.

## Files

- `index.html` — app homepage: what Jimat does, data handling summary, links
- `privacy.html` — full privacy policy incl. Google API Services User Data Policy / Limited Use
  section, data deletion, and revocation instructions