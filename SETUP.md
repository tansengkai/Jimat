# Jimat — OAuth homepage & privacy policy

## ✅ LIVE (published 2026-10-24)

| Console field | Value |
|---|---|
| Application home page | `https://tansengkai.github.io/Jimat/` |
| Application privacy policy link | `https://tansengkai.github.io/Jimat/privacy.html` |
| Authorized domain | `tansengkai.github.io` |
| Terms of service | *(leave empty)* |

Both URLs verified HTTP 200.

## How it is hosted

The pages live on the **`gh-pages` branch** of `tansengkai/Jimat`. GitHub Pages for that repo is
served from `gh-pages`, which gives the `/Jimat/` path.

The calendar app keeps `https://tansengkai.github.io/` (its own repo) untouched, so its OAuth
consent screen still works.

Source of truth for the HTML lives at `~/.openclaw/workspace/jimat-pages/`.

## Updating the pages

```bash
cd ~/.openclaw/workspace/jimat-pages
# edit index.html / privacy.html
git add -A && git commit -m "update"
git push origin main          # main = repo content
# then mirror to gh-pages:
git subtree push --prefix . origin gh-pages   # or copy files and force-push as before
```

## Notes / gotchas

- The fine-grained PAT used on 2026-10-24 (`github_pat_11AEI…`, expires **2026-11-23**) has
  **no Pages permission**, so `POST /repos/:owner/:repo/pages` returns 403. Pages was instead
  picked up automatically from the `gh-pages` branch. To toggle Pages settings via API later, the
  token needs **Repository permissions → Pages: Read and write**.
- The token is also scoped to the **Jimat repo only** — it cannot write to
  `tansengkai.github.io`, which is why the pages were not placed there.

## Google Cloud Console

Scope used by the app:

```
https://www.googleapis.com/auth/drive.file
```

`drive.file` is **non-sensitive** → no Google verification review required. Set User type =
External, then **Publish App → In production** so refresh tokens do not expire every 7 days
(that was the recurring failure mode on the Calendar app).

If Jimat is in a *different Google account* than the calendar project, re-verify ownership of
`tansengkai.github.io` in Search Console first.
