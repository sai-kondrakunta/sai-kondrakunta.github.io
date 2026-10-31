# Sai Kondrakunta Photography

A static, dark single-page site: your night work up top, four kinds of shoots, a rail of recent frames, and a booking form.

## Put it on GitHub Pages

1. Create a new public repo on GitHub, for example `sai-kondrakunta.github.io` (that name makes the site live at https://sai-kondrakunta.github.io).
2. Upload everything in this folder (index.html, site.css, tokens.css, base.css, images/) to the repo's main branch.
3. In the repo: Settings → Pages → Source: "Deploy from a branch", branch `main`, folder `/ (root)`. Save.
4. The site is live a minute or two later.

## Two things to fill in

- **Booking form:** GitHub Pages can't receive form posts. Make a free form at formspree.io (or similar), copy its endpoint URL, and paste it into `data-endpoint=""` on the `<form>` in index.html. Until then the form says "Nothing was sent".
- **Contact email:** replace the dashed "your email address" in the footer with the address you want clients to use.

## Swapping photos

Every picture is a plain `<img src="images/…">`. Drop new files into `images/` (about 1200px on the long side for the grid and rail, 2400px for the hero) and change the `src`.
