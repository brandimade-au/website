# Brandi Made — Website

Live site: **https://brandimade.com.au**

Static HTML/CSS/JS. No build step, no dependencies.
AI agents: read `CLAUDE.md` before making changes.

## Deployment

Deployed automatically by Netlify from this repository.

| Setting | Value |
|---|---|
| Netlify site name | `brandimade` |
| Netlify site ID | `057d2e97-6aa4-4990-ab6e-6e431b656afb` |
| Production branch | `main` |
| Build command | *(none)* |
| Publish directory | `.` (repository root) |
| Environment variables | *(none)* |
| Netlify Forms | Enabled |

## How changes reach the live site

1. Work happens on a branch, never on `main`.
2. Push the branch and open a pull request.
3. Netlify builds a **Deploy Preview** at a temporary URL.
4. Review the preview.
5. Merge the pull request → Netlify publishes to `brandimade.com.au`.

Nothing reaches production without a merge. Previous deploys remain available in
the Netlify dashboard for instant rollback.

## Forms

| Form | Page | Status |
|---|---|---|
| `quote-request` | `index.html` | Live and receiving submissions |
| `referral` | `referrals.html` | **Broken** — see `CLAUDE.md` |
| `quote` | — | Stale orphan from an old deploy, unused |

Submissions are emailed to `service@brandimade.com.au`.
