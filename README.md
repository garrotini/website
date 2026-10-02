# cgarrote.cc

Personal site — Hugo + [gokarna](https://github.com/gokarna-theme/gokarna-hugo), deployed to GitHub Pages via Actions at **https://cgarrote.cc**.

## Stack

| Piece | Detail |
|---|---|
| Hugo | `0.162.1`, pinned in the CI workflow so CI matches local |
| Theme | `gokarna` as a **git submodule** at `themes/gokarna` |
| Hosting | GitHub Pages, `build_type: workflow`, custom domain `cgarrote.cc` (enforced HTTPS) |
| CI | `.github/workflows/deploy.yml` — push to `main` builds & deploys |

## Quick start

```bash
git clone --recursive git@github.com:garrotini/website.git   # --recursive matters!
cd website
hugo serve --noHTTPCache          # http://localhost:1313, hot reload
```

If you cloned without `--recursive`: `git submodule update --init --recursive` — Hugo builds with **no theme** (empty/broken pages) otherwise.

Production build (what CI runs):

```bash
hugo --gc --minify                # output in public/ (not tracked)
```

## Structure

```
website/
├── archetypes/          # front-matter templates used by `hugo new`
│   ├── default.md       #   generic pages → type: "page"
│   └── posts.md         #   blog posts  → type: "post"
├── assets/              # processed by Hugo (minified), site copies WIN over theme's
│   ├── css/main.css     # full copy of gokarna's main.css — edit this for light mode
│   ├── css/dark.css     # full copy — dark mode
│   └── js/              # feather-icons, svg-injector, main.js (theme copies)
├── content/             # the actual site text
│   ├── about.md         # /about/
│   ├── homelab.md       # /homelab/
│   └── projects.md      # /projects/
├── layouts/             # site copies override themes/gokarna/layouts
│   ├── 404.html         # "unknown territory" page
│   ├── index.html       # homepage (avatar, description, social icons)
│   ├── _default/        # baseof, list, single, terms
│   ├── partials/        # header, footer, head, page, post, list-posts, toc, …
│   └── shortcodes/br.html
├── static/CNAME         # copied verbatim into public/ → tells Pages the domain
├── hugo.toml            # site config: params, menu, markup, minify
└── themes/gokarna       # submodule — treat as read-only (see below)
```

`public/`, `resources/_gen/` and `.hugo_build.lock` are generated and git-ignored — never commit them.

## Adding a blog post

```bash
hugo new posts/my-first-post.md
```

`archetypes/posts.md` provides the front matter:

```yaml
---
date: 2026-10-02T00:00:00Z
# description: ""
# image: ""
lastmod: 2026-10-02
showTableOfContents: false
# tags: ["",]
title: "My First Post"
type: "post"
---
```

- Body is plain Markdown; set `showTableOfContents: true` on long posts.
- `draft: true` hides the post (preview locally with `hugo serve --buildDrafts`).
- Posts list automatically at `/posts/`; `tags:` feed `/tags/`.
- Homepage teasers: `showPostsOnHomePage = "recent"` or `"popular"` (+ `numberPostsOnHomePage`) in `hugo.toml` — currently `""` (off).

## Editing pages

`about.md`, `projects.md`, `homelab.md` are normal Markdown files with `type: "page"` in front matter (renders via `layouts/partials/page.html`). Add a new standalone page: `hugo new mypage.md` (default archetype already sets `type: "page"`).

**Linking between pages — always use the `ref` shortcode:**

```markdown
[Home Lab]({{< ref "homelab.md" >}})     # ✅ resolves at build, warns if the page moves
[Home Lab](homelab.md)                    # ❌ 404 at runtime (/about/homelab.md)
[Home Lab](/homelab/)                     # ⚠️ works, but silent if the page is renamed
```

## Navigation menu

All in `hugo.toml` `[menu.main]`, sorted by `weight`:

| Entry | weight |
|---|---|
| About | 1 |
| Projects | 2 |
| Posts | 3 |
| Tags | 4 |
| github / linkedin icons | 5 |

- Icons are **feather** names in `pre = "<span data-feather='user'></span>"` — pick any from https://feathericons.com.
- Entries with **no `name`** render as icon-only links; `custom-head` logic aside, a vertical divider is auto-inserted before the *first* name-less entry (see `layouts/partials/header.html` — `iconsDividerDone` Scratch flag). Add more icon entries at weight 5; don't add `name` if you want them on the icon side.
- External links open normally; add `newPage = true` for `target="_blank"`.

## Styling

- **Light mode**: `assets/css/main.css` — theme colors are CSS vars in `:root` (`--light-primary-color` is a bare `R, G, B` triplet because it's used inside `rgb()`).
- **Dark mode**: `assets/css/dark.css`.
- **Base font size**: `fontSize` param in `hugo.toml` → injected as `--font-size`.
- **Hover accent**: `accentColor` param.
- **Nav icon size**: `.nav-link a svg` / `.nav-item a svg` in `main.css`.
- **404**: `layouts/404.html` (centered block + links; emoji experiment removed).

## Theme overrides — the golden rule

Hugo cascade: **site files (`layouts/`, `assets/`) always beat theme files**. This repo keeps full copies of most gokarna templates/CSS so the theme is effectively decorative for those files.

Consequences:

1. **Never edit `themes/gokarna/*`** — CI clones the theme fresh from GitHub and won't see local submodule edits.
2. When you update gokarna (`git -C themes/gokarna pull && git add themes/gokarna && git commit`), overridden files **keep your local version** — upstream fixes won't appear unless you re-merge them by hand, e.g.:
   ```bash
   diff -u themes/gokarna/assets/css/main.css assets/css/main.css
   ```

## Deploy

- `git push origin main` → Actions builds & publishes.
- Manual trigger: `gh workflow run deploy.yml` or Actions tab → *Run workflow* (`workflow_dispatch`).
- Domain wiring (already done — recreate if the repo is ever moved):
  - Namecheap: four A records `@ → 185.199.108.153/.109/.110/.111`, CNAME `www → garrotini.github.io`
  - GitHub Settings → Pages → Source: **GitHub Actions**; Custom domain `cgarrote.cc`; **Enforce HTTPS** on
  - `static/CNAME` present (survives redeploys)

## Troubleshooting

| Symptom | Cause / fix |
|---|---|
| Local edits to `layouts/`/`assets/` ignored | `hugo serve` started before the dir existed → **restart the server** |
| Site deploys empty / unstyled in CI | checkout missing `submodules: true`, or submodule uninitialized locally |
| A page link 404s (`/about/something.md`) | used a raw `.md` href → switch to `{{< ref "file.md" >}}` |
| Links look fine on `localhost`, broken in prod | hardcoded `http://localhost:1313` — always use `absURL`/`relURL` with `baseURL` |
| `https://` refuses, homepage OK | domain missing in Pages settings or cert pending — Settings → Pages → Custom domain; re-save to trigger validation |
| `about` works but icons/links gone after theme bump | your override copy shadows the theme file — diff & re-apply upstream changes |
