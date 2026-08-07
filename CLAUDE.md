# CLAUDE.md — JM Blog codebase guide

## Overview

Static personal blog hosted at `jmlago.github.io`. It uses an editorial white-paper aesthetic with EB Garamond typography, green ornamental accents, and a canvas orbit on the landing page. There is no framework or build tool.

## Directory structure

```text
/
├── index.html             # Landing page, catalog, and orbit animation
├── css/style.css          # Landing page and 404 theme
├── css/essay.css          # Shared essay and field-note theme
├── posts/
│   ├── _TEMPLATE.html     # Copy for new essays
│   └── *.html             # Published essays
├── skills/
│   └── *.html + *.md      # Readable and downloadable field notes
├── sitemap.xml
├── 404.html
└── .nojekyll
```

## Repository guides

- Create a blog post: `.claude/skills/create-blog-post.md`
- Create a downloadable field note: `.claude/skills/create-skill.md`

## Content conventions

- Dates: `YYYY-MM-DD`, represented with `<time datetime="YYYY-MM-DD">`.
- URLs: kebab-case HTML filenames.
- Landing items: semantic `.essay-entry` markup inside Essays or Notes; there is no JavaScript content registry.
- First article paragraph: add `class="dropcap"`.
- Article metadata: `.qnum`; synopsis or quotation: `.epigraph`.
- Keep article text inside `<article><div class="wrap">…</div></article>`.

### Mathematics

- Inline: `$E = mc^2$`
- Display: `$$\frac{a}{b}$$`

KaTeX 0.16.9 is loaded only on pages that contain mathematics.

### Code and prose

Use `<pre><code class="language-python">…</code></pre>` for code. The language class documents the format; the light theme intentionally does not require a syntax highlighter.

Available article elements include `<strong>`, `<em>`, `<a>`, `<blockquote>`, `<ul>`, `<ol>`, tables, `.disputa`, and `.notes`.

## Design tokens

- `--paper`: `#fff`
- `--ink`: `#191817`
- `--muted`: `#6f6d68`
- `--hair`: `#e7e5df`
- `--green`: `#1e8a3c`

Keep new public pages within this palette and reuse one of the two shared stylesheets.

## Local validation

Run `npx --yes http-server . -p 8000 -c-1` from the repository root and open `http://localhost:8000/`. Check the landing page and changed content at both desktop and mobile widths. Update `sitemap.xml` whenever a public URL is added or materially changed.

## External dependencies

- Google Fonts: EB Garamond and UnifrakturMaguntia
- KaTeX 0.16.9 for mathematics
- js-sha3 0.9.3 for essay content hashes
