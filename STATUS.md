# Status — paused 2026-05-13

Local-only Micro.blog theme port. Built and smoke-tested. Not yet pushed or installed.

## What's done

- `~/dev/micro-blog-theme-summarize/` initialized as a git repo. One commit: `87cb4ab initial micro.blog port of ncsa-mosaic theme`.
- Ported the NCSA Mosaic look from `~/.claude/skills/ncsa-mosaic/theme/` into a Micro.blog-shaped Hugo theme.
- Micro.blog contract:
  - `plugin.json` manifest (title "Summarize", version 0.1.0).
  - `partials/microblog_head.html` (IndieAuth/Webmention/Micropub endpoints, `custom.css` after `style.css` so Design-panel CSS wins), `partials/microblog_syndication.html`, blank `partials/custom_footer.html` — verbatim from `microdotblog/theme-blank` master.
  - No shipped `config.json` (Micro.blog generates it).
  - `taxonomies.category = "categories"`.
  - `feed.xml` / `feed.json` baseNames.
  - Custom output formats: `ArchiveHTML` → `/archive/`, `PhotosHTML` → `/photos/`.
  - Honors `Site.Params.paginate_home`, `Site.Params.paginate_categories`.
  - Replies section filtered out of home and category lists.
- Layouts:
  - `layouts/_default/baseof.html` — title block at top level so `define "title"` from sub-templates works (this was a refactor; head.html partial was deleted).
  - `layouts/_default/single.html` — h-entry with `e-content`, `u-url`, `u-syndication`. Microblog (no title) gets a datestamp + categories line. Articles get the full title block.
  - `layouts/_default/list.html` — category / section list, paginated when `paginate_categories` on.
  - `layouts/_default/list.archivehtml.html` — `/archive/` grouped by year.
  - `layouts/_default/list.photoshtml.html` — `/photos/` grid driven by `.Params.photo`.
  - `layouts/index.html` — home stream, paginated when `paginate_home` on.
  - `layouts/partials/{header,footer,card,pager}.html`.
- `static/style.css` — copy of `ncsa-mosaic/theme/static/style.css` plus added blocks for `.note`, `.h-entry`, archive list, photo grid.
- README.md, LICENSE (MIT), .gitignore.

## Smoke test

Built clean against Hugo 0.160.1 in `~/.cache/io.cote.diane.scratch/mb-theme-smoke2/`. Home, archive, photos, single posts (both titled and untitled notes), category taxonomy pages, pagination, all renders. Titles correct on every page kind.

Gotcha hit during dev: Hugo silently skips future-dated content. The first smoke test had content dated `2026-05-13T10:00:00-05:00` which was still in the future at build time. Looked like the theme was broken (templates marked "unused"). It wasn't.

## Not done — pick up here

1. **Push to GitHub.** Requires Coté's OK (egress rule). Command will be:

       cd ~/dev/micro-blog-theme-summarize && gh repo create cote/micro-blog-theme-summarize --public --source=. --push --description "NCSA Mosaic / 1993-web theme for Micro.blog"

   Confirm repo owner (`cote` vs another org) and visibility (`--public` vs `--private`) before running.

2. **Install on incomprehensiblemedia.com.** After push: Micro.blog → Design → Edit Custom Themes → New Theme → paste the GitHub URL. Then Design → Select Theme → "Summarize". Inline edits in the Micro.blog Design panel land in `custom.css` and override `static/style.css` cleanly.

3. **Fix default destination on Micro.blog.** Separate from the theme but blocking for safe posting: `incomprehensiblemedia.com` is currently `microblog-default: true` (per `micro-blog --action config` output 2026-05-13). Anything posted without `--destination` lands there. Flip the default back to `cote.io` (or whichever) at https://micro.blog/account. Not changeable via Micropub API.

   Destination UIDs for reference (so we don't have to re-query):

   | Name | UID |
   |------|-----|
   | cote.io | `https://cote.micro.blog/` |
   | cote.coffee | `https://cotecoffee.micro.blog/` |
   | tanzutalk.micro.blog | `https://tanzutalk.micro.blog/` |
   | drunkandretired.com | `https://drunkandretired.micro.blog/` |
   | **incomprehensiblemedia.com** | `https://incomprehensiblemedia.micro.blog/` |
   | trytanzu.ai | `https://trytanzu.micro.blog/` |
   | cote-test.micro.blog | `https://cote-test.micro.blog/` |

## Open design questions

- README author homepage and the theme footer link both point at `https://github.com/cote/micro-blog-theme-summarize`. Confirm the GitHub owner before pushing — if it's a different account/org, those two strings need a sweep.
- Footer currently links to `/archive/` and `/categories/` and `feed.xml`. If you don't want one of those (e.g. categories empty for a while), strip from `layouts/partials/footer.html`.
- Photo posts use `.Params.photo` (single or list); alt text from `.Params.photo_alt`. Matches Micro.blog's Micropub output. Not exercised yet — first photo post will tell us if anything needs tweaking.
- No `replies/single.html` yet. Micro.blog will fall back to its built-in template for the conversation page, which is fine for now. Add one later if we want replies to inherit the Mosaic look.
