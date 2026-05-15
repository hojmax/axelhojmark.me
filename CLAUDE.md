# axelhojmark.me

Static personal site hosted on GitHub Pages. Single `index.html`, a few image assets, and a `CNAME`. No build step.

## When changing images, always bust caches

Browsers, Google Image Search, and social-media OG/Twitter crawlers cache aggressively. **Only when an image file's contents change** (even if the filename stays the same), you must invalidate every cache that might serve the old version for that image. Edits that don't touch image bytes (HTML/CSS/text changes) do not require any cache bump.

If you *are* updating multiple images at once, bump them in lockstep (same `?v=N` increment across all favicons) so they stay in sync — but again, only the ones whose contents actually changed.

### Favicons and icons (`favicon.ico`, `favicon-*.png`, `apple-touch-icon.png`)

Bump the `?v=N` query string on every `<link rel="icon">` / `<link rel="apple-touch-icon">` in `index.html`. Increment the number by one across all of them so they stay in sync (e.g. `?v=4` → `?v=5` everywhere).

### Social/OG images (`headshot*.jpg`, anything referenced by `og:image`, `twitter:image`, or schema.org JSON-LD)

Query strings are **not** reliable here — Google Images, Slack, Twitter, LinkedIn, and Facebook key their caches off the bare URL and frequently ignore `?v=` params. **Rename the file** instead:

1. `git mv headshot-vN.jpg headshot-vN+1.jpg` (or pick a new suffix).
2. Update every reference in `index.html`: `og:image`, `twitter:image`, the JSON-LD `"image"` field, and the `<img src>` in the hero.
3. Grep to confirm no stale references remain: `grep -n headshot index.html`.

After deploying, optionally request re-indexing in Google Search Console to speed up cache refresh (otherwise it can take days to weeks).

## Footer "Last updated" text

The footer in `index.html` reads `Last updated <Month Year>`. When you make changes to the site **and** the current month/year no longer matches what the footer shows, update the footer to the current month and year. If the footer already reflects the current month, leave it alone.

## Other notes

- The site is plain HTML/CSS — no framework, no bundler. Edit `index.html` directly.
- `CNAME` points the GitHub Pages deployment at `axelhojmark.com`. Don't touch it without reason.
- Inline SVG symbols (e.g. `#icon-lesswrong`) live in the `<svg width="0" height="0">` block near the top of `<body>`; they are not image files and don't need cache busting.
