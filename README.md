# Lumen Lab — Hugo + Tailwind CSS

A research-group website built with [Hugo](https://gohugo.io) and [Tailwind CSS v4](https://tailwindcss.com).

It includes:

- A **full-viewport hero** on the landing page with an animated chevron that smoothly scrolls down to a **News** section.
- A **sticky top bar** linking **Research, Team, Resources, Contact**.
- A **Team** page of avatar cards with bios and **ORCiD / LinkedIn / Bluesky** icons (each icon appears only when a link is provided; members without a photo get an automatic monogram).
- A **Contact** page with an embedded **Google Map** (no API key required) plus contact details and a form.
- A **Research** page that lists publications grouped by year, each with a **PDF** button (and optional DOI / Code links).

## Requirements

- **Hugo Extended**, v0.128.0 or newer (the `css.TailwindCSS` pipe needs the *extended* build).
  Check with `hugo version` — the output must contain `extended`.
- **Node.js** 20+ (provides the Tailwind CLI binary that Hugo calls).

## Quick start

```bash
# 1. Install the Tailwind CLI (declared in package.json)
npm install

# 2. Run the dev server with live reload
hugo server

# 3. Open the printed URL (usually http://localhost:1313)
```

Build a production site into `public/`:

```bash
hugo --gc --minify
```

## Making it yours

Almost everything is data-driven — you rarely need to touch the templates.

| What | Where |
|------|-------|
| Site title, tagline, affiliation, contact info, map location | `hugo.toml` → `[params]` |
| Top-bar links | `hugo.toml` → `[[menu.main]]` |
| Homepage news cards | `data/news.yaml` |
| Team members, bios, social links | `data/team.yaml` |
| Publications + PDF links | `data/papers.yaml` |
| Research themes | `data/research.yaml` |
| Resources (software/data/teaching) | `data/resources.yaml` |
| Page intros (markdown) | `content/*.md` |

### Hero image
By default the bundled `static/images/hero.svg` is used. To use a photo, drop it at
`static/images/hero.jpg` and set `heroImage = "images/hero.jpg"` in `hugo.toml`.

### Team photos
Put images in `static/images/team/` and set `avatar: "images/team/name.jpg"` on the member
in `data/team.yaml`. Leave `avatar` blank to use the generated initials monogram.

### Publication PDFs
Place PDFs in `static/papers/` and reference them in `data/papers.yaml`, e.g.
`pdf: "papers/my-paper.pdf"`. The placeholders currently in that folder are safe to delete.

### Google Map
Set `mapQuery` in `hugo.toml` to any address or place name. It uses Google's keyless
embed, so nothing else is needed.

### Contact form
The form posts to `params.formEndpoint` if you set one (e.g. a Formspree URL); otherwise it
falls back to a `mailto:` link.

## Design

- Type: **Fraunces** (display) + **Hanken Grotesk** (body), loaded from Google Fonts.
- Palette and design tokens live in `assets/css/main.css` under `@theme` — change the
  `--color-*` and `--font-*` values to re-skin the whole site.

## Notes on Tailwind + Hugo

Hugo writes the classes it uses to `hugo_stats.json` and mounts it for Tailwind to scan
(configured in `hugo.toml`). Tailwind v4 also auto-detects classes by scanning the project,
so the two approaches reinforce each other. If you ever add classes via JavaScript string
concatenation, add an explicit `@source` line in `main.css`.
