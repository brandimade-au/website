# Brandi Made — Website Project Instructions

Read this file completely before making any change to this repository.

## What this project is

The live website for **Brandi Made** (https://brandimade.com.au), an Australian B2B
promotional products business based in Brisbane. It is a **plain static website**:
hand-written HTML with all CSS inside `<style>` blocks and all JavaScript inside
inline `<script>` blocks. Hosted on Netlify, deployed automatically from this repo.

There is **no build step, no framework, no package manager, no dependencies.**
This is deliberate. Do not change it.

## Absolute rules

1. **Never introduce a build step, bundler, framework, or npm dependency.**
   No React, Vue, Tailwind CLI, Vite, Sass, or `package.json`. If you believe one
   is genuinely required, stop and explain why instead of doing it.
2. **Never change visual design, copy, or functionality unless that is the task.**
   Fixing a heading level does not licence restyling the section.
3. **Change only what the task asks for.** No opportunistic refactors, no
   reformatting whole files, no reordering attributes, no "tidying" unrelated code.
   A diff should be readable line by line and every changed line should be
   traceable to the request.
4. **Never commit secrets** — API keys, passwords, tokens, credentials. There are
   none in this project today. Keep it that way.
5. **Never delete files** without explaining what the file is and why it is safe
   to remove. Wait for approval.
6. **Never push directly to `main`.** See the workflow section below.
7. **Never change the domain, DNS, or Netlify site ID.**

## Repository structure

```
/
├── index.html          Homepage — includes the live quote request form
├── about.html
├── shop.html
├── how-it-works.html
├── faq.html
├── referrals.html      Referral program + referral form + T&Cs
├── thank-you.html      Form success page (noindex)
├── images/             All site images (.webp, .avif, .svg, .png)
├── _redirects          Netlify redirect rules
├── robots.txt
├── sitemap.xml
├── .gitignore
└── CLAUDE.md           This file
```

Files sit at the repository root. Netlify publishes the root directory as-is.
**Do not move files into a `src/`, `dist/`, or `public/` folder.**

## Design system — reuse these, do not invent new ones

CSS custom properties are declared in `:root` at the top of each page's `<style>`
block. Use the variables, never raw hex values:

| Variable | Value | Use |
|---|---|---|
| `--dark` | `#152228` | Nav, footer, primary buttons, headings |
| `--mid` | `#4e8098` | Accents, dividers, hover states |
| `--light` | `#bbd2e4` | Footer text, muted elements on dark |
| `--red` | `#a31621` | Alerts / emphasis (use sparingly) |
| `--offwhite` | `#f5f2ee` | Page background |
| `--white` | `#ffffff` | Cards, panels |
| `--text-dark` | `#1a2d35` | Body copy |
| `--text-mid` | `#4a5e66` | Secondary copy |

Typography: `Playfair Display` (serif) for headings, `DM Sans` for body. Both
loaded from Google Fonts. Do not add further font families or weights.

If you change a shared element — nav, footer, colour variables, button styles —
**you must apply the same change to every page that contains it.** Each page holds
its own copy of the CSS; they are not shared. State clearly in your summary which
files you touched.

## Forms (Netlify Forms — handle with care)

Form submissions go to Netlify and are emailed to `service@brandimade.com.au`.
These are live sales enquiries. Breaking a form costs real revenue.

**Working form — `index.html`:**
```html
<form name="quote-request" method="POST" data-netlify="true"
      netlify-honeypot="bot-field" action="/thank-you.html">
  <input type="hidden" name="form-name" value="quote-request">
```

Rules for any form work:
- The `name` attribute and the hidden `form-name` value **must match exactly**.
- **Never rename an existing form.** Netlify treats a renamed form as a brand new
  one and the old submissions stop flowing. `quote-request` keeps that name forever.
- Every field needs a `name` attribute — Netlify only captures named fields.
- Keep the `bot-field` honeypot.
- Keep `action="/thank-you.html"`.
- Adding a field to an existing form is safe; Netlify picks it up on next deploy.

**Known broken — `referrals.html`:** the submit button calls `submitReferral()`,
which is not defined anywhere, and the inputs are not inside a `<form>` element
with `data-netlify="true"`. The form does not work and captures nothing. This is a
known outstanding bug, not something to silently patch as a side effect of another
task.

## SEO — do not regress these

Every page (except `thank-you.html`) must keep:
- `<title>` and `<meta name="description">`
- `<link rel="canonical">` pointing at the absolute `https://brandimade.com.au/...` URL
- `<meta name="robots" content="index, follow">`
- Its JSON-LD `<script type="application/ld+json">` block

`thank-you.html` must stay `noindex, nofollow` and stay out of `sitemap.xml`.

If you add a new page, add it to `sitemap.xml` with a sensible priority and update
`lastmod`. If you change a page's content meaningfully, update its `lastmod`.

## Accessibility baseline

- One `<h1>` per page; heading levels descend without skipping.
- All `<img>` tags need meaningful `alt` text.
- Keep `aria-label`, `aria-expanded`, and `aria-controls` on the nav hamburger.
- Keep `loading="lazy"` on below-the-fold images.

## Testing before you hand anything over

There is no test suite. Verify manually:

1. Open every page you edited in a browser and confirm it renders.
2. Check the browser console for JavaScript errors — there should be none.
3. Resize to 375px wide (mobile) and confirm layout, nav hamburger, and tap targets.
4. If you touched a form, confirm the `name` and hidden `form-name` still match.
5. If you touched the nav or footer, confirm it on **every** page, not just one.
6. Confirm no `images/` reference points at a file that does not exist.

## Git workflow — follow this exactly

```
1. git checkout main && git pull
2. git checkout -b <short-descriptive-branch-name>
3. Make the change
4. git add <only the files you changed>      # never `git add -A` blindly
5. git commit -m "<clear description of what changed and why>"
6. git push -u origin <branch-name>
7. Open a pull request against main
8. STOP. Tell Sherena the branch name and that the Netlify Deploy Preview is building.
```

**You stop at the pull request.** Sherena reviews the Deploy Preview URL and merges.
Merging to `main` is what publishes to the live site — that decision is hers, never
yours. Do not merge, do not push to `main`, do not deploy via the Netlify CLI.

Branch naming: `fix/`, `feature/`, or `content/` plus a short description.
Example: `fix/referral-form-submission`.

## What to report back

For every task, tell Sherena:
- Which files you changed and what changed in each
- Anything you found but deliberately did not touch
- Anything you were unsure about
- The branch name and the pull request link

Write it plainly. She is not a developer and does not want jargon.
