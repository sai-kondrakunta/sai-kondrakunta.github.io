# Sai Kondrakunta Photography — project notes for Claude Code

A static one-page photography site for getting clients and bookings, hosted on GitHub Pages.
No build step, no framework, no package.json: plain HTML + CSS + a little inline JS.

## Files

- `index.html` — the whole page: nav, hero, "Four kinds of work" plates, statement, "Recent frames" rail, "Portrait sessions, Brooklyn" grid, booking form, footer. Inline JS at the bottom handles the form, the plate→form preselect, the animations and the lightbox.
- `site.css` — page styles. Its first lines are a Hallmark stamp comment; keep them at the top.
- `tokens.css` — every colour, font, size, space and duration as a CSS variable (Darkroom theme).
- `base.css` — base rules (resets, focus rings, image slots, reduced motion).
- `images/` — the photos. Every photo has a 1200px long-edge copy for the page (`A740826.jpg`) and a 2400px copy the lightbox opens (`A740826-2400.jpg`), exported with metadata stripped.
- `vendor/` — third-party scripts, kept local (no CDN, works from `file://`): Lenis 1.3.27 (smooth scroll, github.com/darkroomengineering/lenis) and PhotoSwipe 5.4.4 UMD (lightbox, github.com/dimsemenov/PhotoSwipe). Their CSS files are the only place literal colours appear; `site.css` re-themes PhotoSwipe with tokens.
- Originals live on Sai's Mac in `~/Pictures/claude/`: `Untitled Export/` (the NYC night set) and `Photography/` (`nishthingz/` and `omerkhanlive/` are the Brooklyn portrait sessions; `NYC10-03-26/` repeats the night set).

## Motion

- Hero photo "develops" on load (opacity + scale) and drifts slower than the page on scroll; headings with `data-split` rise in word by word.
- Photos with `data-shutter` are uncovered by a paper-coloured shutter (`::after`, `scaleY`) while the print settles from `scale: 1.18`.
- The statement (`data-lit`) lights up word by word with scroll position.
- Any `a.shot` inside a `[data-gallery]` list opens in the PhotoSwipe lightbox; give it `data-pswp-width/height` of the 2400 copy.
- Reduced motion: Lenis is not started, nothing is hidden, the lightbox opens without zoom.

## Design rules (keep the look consistent)

The design came from the Hallmark design skill (github.com/Nutlope/hallmark), Darkroom theme.

- Never write a literal colour or font-family outside `tokens.css`; use the `--color-*` and `--font-*` variables.
- Headings: Sofia Sans Extra Condensed, uppercase, weight 200 (400 for small item titles). Body: Barlow.
- One accent colour (amber, `--color-accent`), used only on the single filled button ("Send the enquiry"), focus rings and hover on plate numbers. Never as a background band or on headings.
- Zero border radius everywhere. No shadows, no gradients, no glow.
- Photos are shown in their natural colour (the theme's black-and-white filter was deliberately removed).
- Labels and captions are sentence case, untracked, `--color-muted`, one step smaller. No all-caps tracked labels, no emoji icons.
- Animate only `transform` and `opacity`, never `transition: all`; every animation needs a `prefers-reduced-motion` fallback.
- `overflow-x: clip` on html/body (never `hidden`); image grid tracks use `minmax(0, 1fr)`.
- The hero headline, its one sentence and the "Book a shoot" button must be visible at 1280x800 without scrolling.
- Never invent numbers, client names, testimonials, prices or logos. Only use facts Sai gives.

## Open TODOs

1. **Booking form endpoint.** GitHub Pages can't receive POSTs. Sai creates a Formspree (or similar) form; put its URL in `data-endpoint=""` on the `<form>` in `index.html`. Until then the form says "Nothing was sent".
2. **Contact email.** Replace the dashed `<span class="fill" data-fill="email">your email address</span>` in the footer with a `mailto:` link once Sai chooses an address. Do not use his personal Gmail unless he says so.
3. Optional: Instagram link in the footer, a custom domain (`CNAME` file).

## Deploy to GitHub Pages

GitHub user: `sai-kondrakunta`. Sai keeps his email private on GitHub, so commits must use his noreply address. Get it with:

```sh
gh api user --jq '"\(.id)+\(.login)@users.noreply.github.com"'
```

First deploy (repo name makes the site live at https://sai-kondrakunta.github.io):

```sh
git init -b main
git config user.name "Sai Kondrakunta"
git config user.email "<noreply address from above>"
git add index.html site.css tokens.css base.css images vendor README.md CLAUDE.md .nojekyll
git commit -m "Photography site"
gh repo create sai-kondrakunta.github.io --public --source=. --push
gh api -X POST repos/sai-kondrakunta/sai-kondrakunta.github.io/pages -f "source[branch]=main" -f "source[path]=/"
```

Later updates: commit and `git push`; Pages redeploys in a minute or two. Check status with `gh api repos/sai-kondrakunta/sai-kondrakunta.github.io/pages --jq .status`.

## Checking changes

Open `index.html` directly in a browser, or run `python3 -m http.server 8000` and visit http://localhost:8000. Check desktop (1280x800 and 1440 wide) and phone (390 wide): no horizontal scroll, nav fits on one line on phones, hero fits the first screen.
