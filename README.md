# Cobalt

![screenshot](screenshot/home.png)

A clean, modern [Micro.blog](https://micro.blog) theme in blue, dark by design (no light mode). Avatar and site title on the left of a compact header, navigation on the right, and a simple footer. Includes IndieWeb microformats (h-card, h-entry) for webmentions and cross-posting.

## Using this theme on Micro.blog

1. Push this repository to a public GitHub repo (e.g. `github.com/yourname/cobalt-theme`).
2. In Micro.blog, go to **Design → Edit Custom Themes → New Theme**.
3. Point it at your GitHub repo and save. Micro.blog will pull in `layouts/` and `static/` from this repo.
4. Set your avatar, site title, tagline/description, and navigation links from the normal Micro.blog **Design** and **Preferences** screens — the theme reads all of that automatically:
   - Avatar & name → `.Site.Author.avatar` / `.Site.Author.name`
   - Site title → `.Site.Title`
   - Tagline/bio → `.Site.Params.description`
   - Nav links → the "main" menu (Design → Navigation)
   - Extra footer links (optional) → a "footer" menu

`config.json` and `theme.toml` in this repo are only used for local Hugo development/preview below — Micro.blog manages your actual site settings separately, so you don't need to edit them for a live blog.

## Theme settings

`plugin.json` declares a settings form that appears on the theme's page in Micro.blog's dashboard (Design → your theme → Settings):

- **Show full post text on the home and archive pages** — a checkbox. Off by default, which shows an excerpt instead. Untitled, short microblog-style posts always show in full either way, since there's nothing to excerpt.
- **Excerpt length, in characters** — only used when the checkbox above is off. Defaults to 280 if left blank.
- **Custom code for `<head>`** — raw HTML/JS pasted here is inserted right before `</head>` on every page. Use for analytics snippets, extra meta tags, etc.
- **Custom code before `</body>`** — raw HTML/JS inserted right before the closing `</body>` tag. Use for tracking scripts that should load late.

Both code fields are inserted unescaped (`safeHTML`), so only paste code you trust.

## Local development

```
hugo server
```

Edit `config.json` with your own title, avatar path, and description to preview locally. Static avatar images can go in `static/images/`.

## Structure

- `layouts/_default/baseof.html` — base page shell (head, header, footer)
- `layouts/partials/header.html` + `navigation.html` — compact single-row header: avatar/title/tagline on the left, nav on the right
- `screenshot/home.png` — theme screenshot shown above, referenced from this README for the Micro.blog theme directory
- `layouts/partials/footer.html` — RSS/JSON feed links, copyright
- `plugin.json` — the theme settings form (excerpt vs. full post, custom head/footer code)
- `layouts/index.html`, `layouts/_default/list.html` — post lists (home, categories, tags)
- `layouts/post/single.html`, `layouts/_default/single.html` — individual posts and pages
- `static/css/style.css` — all styling; dark-only, no light theme

## Customizing the color

All colors are CSS custom properties at the top of `static/css/style.css`. Change `--blue-400` / `--blue-500` (the `--accent` / `--accent-hover` values) to shift the accent color.
