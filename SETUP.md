# Jimat — OAuth homepage & privacy policy

## ✅ LIVE

| Console field | Value |
|---|---|
| Application home page | `https://tansengkai.github.io/Jimat/` |
| Application privacy policy link | `https://tansengkai.github.io/Jimat/privacy.html` |
| Authorized domain | `tansengkai.github.io` |
| Terms of service | *(leave empty)* |

Verified HTTP 200 — 2026-10-24 23:59.

## Hosting layout

- Repo: `tansengkai/Jimat` (public)
- Branch: **`main`** (single branch — `gh-pages` was deleted once Pages could be retargeted)
- Pages source: `main` / `/ (root)`, legacy build, HTTPS enforced
- Live URL: `https://tansengkai.github.io/Jimat/`
- HTML source of truth: `~/.openclaw/workspace/jimat-pages/`

The calendar app keeps `https://tansengkai.github.io/` from its own repo. Both coexist because
Google accepts any path under an authorized domain.

## Update workflow

```bash
cd ~/.openclaw/workspace/jimat-pages
# edit index.html / privacy.html / SETUP.md
git add -A
git commit -m "update pages"
git push "https://x-access-token:$GH_TOKEN@github.com/tansengkai/Jimat.git" main
# Pages rebuilds automatically in ~30-60s
```

## PAT notes (learned 2026-10-24)

The fine-grained PAT must include, under **Repository permissions**:

| Permission | Needed for |
|---|---|
| Contents: Read and write | pushing files |
| **Pages: Read and write** | `GET/PUT/POST /repos/:owner/:repo/pages`, triggering builds |
| Metadata: Read | mandatory baseline |

Without **Pages: Read and write** every Pages endpoint returns
`403 Resource not accessible by personal access token` — that was the original blocker.

Fine-grained PATs are **per-repository**. A token scoped to `Jimat` only cannot write to
`tansengkai.github.io`; to touch multiple repos, the token must list each one (or use a
classic PAT with `repo` scope).

## Google Cloud Console

Scope used by the app:

```
https://www.googleapis.com/auth/drive.file
```

`drive.file` is **non-sensitive** → no Google verification review required. Set User type =
External, then **Publish App → In production** so refresh tokens do not expire every 7 days
(the recurring failure mode on the Calendar app).

If Jimat lives in a different Google account than the calendar project, re-verify ownership of
`tansengkai.github.io` in Search Console first.

## Verification commands

```bash
curl -o /dev/null -w "%{http_code}\n" https://tansengkai.github.io/Jimat/
curl -o /dev/null -w "%{http_code}\n" https://tansengkai.github.io/Jimat/privacy.html
curl -s https://tansengkai.github.io/Jimat/privacy.html | grep -c "Limited Use"
```
