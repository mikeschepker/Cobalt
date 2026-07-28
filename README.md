# Cobalt

![screenshot](screenshot/home.png)

A clean, modern [Micro.blog](https://micro.blog) theme in blue, dark by design (no light mode). Avatar and site title on the left of a compact header, navigation on the right, and a simple footer. Includes IndieWeb microformats (h-card, h-entry) for webmentions and cross-posting.

## Using this theme on Micro.blog

1. Push this repository to a public GitHub repo (e.g. `github.com/yourname/cobalt-theme`).
2. In Micro.blog, go to **Design → Edit Custom Themes → New Theme**.
3. Point it at your GitHub repo and save. Micro.blog will pull in `layouts/` and `static/` from this repo.
4. Set your avatar, site title, and navigation links from the normal Micro.blog **Design** and **Preferences** screens — the theme reads all of that automatically:
   - Avatar & name → `.Site.Author.avatar` / `.Site.Author.name`
   - Site title → `.Site.Title`
   - Nav links → the "main" menu (Design → Navigation)
   - Extra footer links (optional) → a "footer" menu

The header intentionally has no tagline/bio line — just the avatar, title, and nav.

**Important:** Micro.blog merges this repo's `config.json` directly into your live site's config, and its values take precedence over your real dashboard settings for anything under `title`, `author`, `params.description`, or `menu`. That's why `config.json` here deliberately omits all of those — don't add them back (not even as placeholders), or you'll overwrite your real title and author info with whatever you typed in this file. `config.json` should only ever contain this theme's own custom settings (the `cobalt_*` params below), never anything Micro.blog's dashboard already manages.

## Theme settings

`plugin.json` declares a settings form that appears on the theme's page in Micro.blog's dashboard (Design → your theme → Settings):

- **Show full post text on the home and archive pages** — a checkbox. Off by default, which shows an excerpt instead. Untitled, short microblog-style posts always show in full either way, since there's nothing to excerpt.
- **Excerpt length, in characters** — only used when the checkbox above is off. Defaults to 280 if left blank.
- **Custom code for `<head>`** — raw HTML/JS pasted here is inserted right before `</head>` on every page. Use for analytics snippets, extra meta tags, etc.
- **Custom code before `</body>`** — raw HTML/JS inserted right before the closing `</body>` tag. Use for tracking scripts that should load late.

Both code fields are inserted unescaped (`safeHTML`), so only paste code you trust.

## SEO

`layouts/partials/seo.html` (included from `head.html` on every page) adds:

- A self-referencing canonical link, skipped automatically on pages (like `/archive/`) where Micro.blog/Hugo already emits its own `rel="canonical"`, to avoid conflicting duplicates
- A `<meta name="description">` truncated to 160 characters, sourced from the page's own description/summary (or the site description on the home page)
- Open Graph and Twitter Card tags (title, description, url, type, image — falling back to your avatar if a post has no `Params.image`)
- JSON-LD `BlogPosting` structured data on individual posts (headline, dates, author, url, image)

The home page also gets a visually-hidden `<h1>` (the site title) for heading-hierarchy correctness, since list and single pages already have a visible one.

## Local development

Since `config.json` intentionally has no title/author/description (see above), create an untracked `config.dev.json` alongside it for local preview only:

```json
{
	"title": "Your Site Title",
	"author": { "name": "Your Name", "avatar": "/images/avatar.jpg", "username": "yourusername" },
	"params": { "author": { "name": "Your Name", "avatar": "/images/avatar.jpg", "username": "yourusername" }, "description": "Used only for the <meta name=\"description\"> tag, not shown in the header." },
	"menu": { "main": [ { "name": "About", "url": "/about/", "weight": 1 } ] }
}
```

Then run Hugo with both configs merged (never commit `config.dev.json`):

```
hugo server --config config.json,config.dev.json
```

## Structure

- `layouts/_default/baseof.html` — base page shell (head, header, footer)
- `layouts/partials/header.html` + `navigation.html` — compact single-row header: avatar/title on the left, nav on the right
- `screenshot/home.png` — theme screenshot shown above, referenced from this README for the Micro.blog theme directory
- `layouts/partials/footer.html` — RSS/JSON feed links, copyright
- `plugin.json` — the theme settings form (excerpt vs. full post, custom head/footer code)
- `layouts/partials/seo.html` — canonical link, meta description, Open Graph/Twitter Card tags, JSON-LD for posts
- `layouts/index.html`, `layouts/_default/list.html` — post lists (home, categories, tags)
- `layouts/_default/list.archivehtml.html` — the `/archive/` page: a full unpaginated post history plus a category-jump dropdown, overriding Micro.blog's unstyled built-in archive template
- `layouts/post/single.html`, `layouts/_default/single.html` — individual posts and pages
- `static/css/style.css` — all styling; dark-only, no light theme

## Customizing the color

All colors are CSS custom properties at the top of `static/css/style.css`. Change `--blue-400` / `--blue-500` (the `--accent` / `--accent-hover` values) to shift the accent color.
