# Bayou Connections: public site

Source for bayouconnections.com. One page shown as a console, one centered screen per section: Mission, Why Bayou, Work, Trust Lab, Principles, About, Contact.

## Build

Needs Node 18 or newer. There are **no dependencies**: nothing to `npm install`.

```
npm run build      # writes dist/, dist-squarespace/ and docs/TRUTH_REVIEW.md
npm run preview    # serves dist/ at http://localhost:4321
```

You can also open `dist/index.html` directly in a browser.

## What gets built

| Output | Use |
|---|---|
| `dist/` | A complete static site (HTML, media, self-hosted fonts, favicon, sitemap.xml, robots.txt). Works on any static host. |
| `dist-squarespace/bayou-code-block.html` | Paste into one Squarespace Code Block. See `docs/SQUARESPACE_SETUP.md`. |
| `dist-squarespace/upload/` | Every media file for the `bayou-media` GitHub repo, plus its generated README.md. |
| `dist-squarespace/this-update/` | Only the files that changed in the latest media update, plus the README. Upload this folder's contents for an update. |
| `dist-squarespace/header-code-injection.html` | Structured data for Squarespace's header injection. |
| `docs/TRUTH_REVIEW.md` | Every public claim, its source, and whether it needs your review. Regenerated on every build. |

## Where things live

```
site.config.mjs            Authoritative company facts (from Amy). The only place they're set.
src/content/
  vocabulary.mjs           The status and source labels the page uses.
  projects.mjs             Featured work as evidence trails, plus each project's claim ledger.
  story.mjs                Hero, Why Bayou, Trust Lab, Principles, About copy.
  claims.mjs               Site-level claims for the truth review.
  media-log.mjs            Media change log and file descriptions; generates the bayou-media README.
src/components/            Reusable components (plain JavaScript functions returning HTML).
src/styles/                tokens.css (design tokens), base.css, site.css. All scoped to .bayou.
src/client/site.js         The console: one panel per section, tabs, evidence sliders, keyboard arrows. Without it the page still reads top to bottom.
src/components/Console.mjs Tabs and Slider components used by the console.
public/                    Media, fonts (Cormorant Garamond, EB Garamond, Pinyon Script, Instrument Sans; all OFL), favicons, social image.
scripts/build.mjs          The build.
scripts/make-images.mjs    Dev-only: re-renders the social image and favicons (needs Playwright).
docs/                      Setup notes, media manifest, truth review. Never published.
```

## Rules this codebase follows

- **Every factual sentence about a project has a ledger row** in `projects.mjs`, with its source (`amy`, `recording`, `artifact`, `inferred`, `unknown`). The build fails if a tributary has no truth-review row.
- **Nothing is published just because it exists.** Media goes through `docs/MEDIA_MANIFEST.md`.
- **Every media change gets a README.** Add an entry to `src/content/media-log.mjs`; the build writes the bayou-media README and fails if a file is undescribed.
- **YouTube loads only on click.** Posters are served from bayou-media; the privacy-enhanced player appears when a visitor presses Watch.
- **No trackers, no frameworks, no third-party scripts.** The standalone build makes no third-party requests at all. The Squarespace build loads Google Fonts.
- Content in `src/content/` is escaped by default. Only markup we wrote ourselves is inserted raw.

## About Astro

The brief preferred Astro. It couldn't be installed in the environment this was built in (the npm registry was unreachable), and untested code wasn't going to ship. The structure maps one-to-one if you want Astro later: each function in `src/components/` becomes an `.astro` component, `src/content/*.mjs` becomes a content collection, and `src/styles/` carries over unchanged.
