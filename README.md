# Summarize — a Micro.blog theme

NCSA Mosaic / 1993-web aesthetic for [Micro.blog](https://micro.blog/). Times New Roman on white paper, period-correct blue/purple/red link colors, double-line `<hr>` under the site header, hanging Tufte-style sidenotes on wide viewports, mustard `<mark>` highlights inside posts.

## Look

- Type: Times New Roman, 19px base, 1.5 line-height, 44em max column.
- Colors: white background, black text, `#0000ee` / `#551a8b` / `#ff0000` for link states. Literal Mosaic defaults.
- Rules: `<hr>` between post cards, `<hr class="double">` under the site header. Those are the only two rule styles.
- No webfonts. No icons. No JavaScript. No dark mode.

## Features

- Home stream renders microblog notes inline; longer posts (with a title) render as titled cards with excerpt and "read more" link.
- Permalink page uses `h-entry` microformats and a `u-syndication` hook for Bluesky / Mastodon cross-posts.
- `/archive/` page grouped by year.
- `/photos/` grid driven by Micro.blog's `PhotosHTML` output.
- Honors `paginate_home` and `paginate_categories` toggles from the Micro.blog Design panel.
- Filters `replies` section out of the home feed and category lists.
- Categories taxonomy at `/categories/`.

## Install on Micro.blog

1. Fork or note this repo's URL.
2. Micro.blog → **Design → Edit Custom Themes → New Theme** → paste the GitHub URL.
3. Micro.blog → **Design → Select Theme** → pick "Summarize".

User CSS edits in the Design panel land in `custom.css` and override `static/style.css` (the theme links `custom.css` after its own stylesheet via the standard `partials/microblog_head.html`).

## Standalone Hugo use

It is a regular Hugo theme too. Drop it under `themes/summarize/`, set `theme: summarize` in your Hugo config, and provide a `config.yaml`/`.toml` with at least:

```yaml
paginate: 25
taxonomies:
  category: categories
markup:
  goldmark:
    renderer:
      unsafe: true
params:
  author:
    username: yourusername
```

Micro.blog generates an equivalent `config.json` automatically.

## Files

| Path | Purpose |
|------|---------|
| `layouts/_default/baseof.html` | Page skeleton |
| `layouts/_default/single.html` | Permalink page (h-entry) |
| `layouts/_default/list.html` | Category / section lists |
| `layouts/_default/list.archivehtml.html` | `/archive/` page |
| `layouts/_default/list.photoshtml.html` | `/photos/` grid |
| `layouts/index.html` | Home stream |
| `layouts/partials/head.html` | `<head>` (links `style.css`, calls `microblog_head`) |
| `layouts/partials/microblog_head.html` | Micro.blog endpoints, `custom.css`, plugin hooks |
| `layouts/partials/microblog_syndication.html` | `u-syndication` hooks for cross-posts |
| `layouts/partials/header.html` | Site header / title block |
| `layouts/partials/footer.html` | Site footer nav |
| `layouts/partials/custom_footer.html` | Empty; user-editable in Design panel |
| `layouts/partials/card.html` | Stream item (note or titled post) |
| `layouts/partials/pager.html` | Numbered paginator with first/last anchors |
| `static/style.css` | Stylesheet |

## License

MIT. See `LICENSE`.
