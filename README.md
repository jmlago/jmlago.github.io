# JM Blog

A small static site for essays and field notes. The visual system is editorial: warm black type on white paper, green ornament, generous reading measure, and an animated conceptual orbit on the landing page.

There is no build step or framework. The site is plain HTML, CSS, and a little JavaScript, and is deployed with GitHub Pages.

## Local preview

From the repository root:

```bash
npx --yes http-server . -p 8000 -c-1
```

Then open <http://localhost:8000/>. Stop the server with `Ctrl+C`. The first run may download the small `http-server` package into the npm cache; it does not modify the repository.

## Structure

```text
.
├── index.html             # Landing page and animated orbit
├── 404.html               # Custom not-found page
├── css/
│   ├── style.css          # Landing and 404 styles
│   └── essay.css          # Essays and field-note styles
├── posts/
│   ├── _TEMPLATE.html     # Starting point for a new essay
│   └── *.html             # Published essays
├── skills/
│   ├── *.html             # Readable field notes
│   └── *.md               # Downloadable source files
├── sitemap.xml
└── .nojekyll
```

## Add an essay

1. Copy `posts/_TEMPLATE.html` to a kebab-case filename in `posts/`.
2. Fill in the title, description, essay number, date, tags, epigraph, and article body.
3. Add an `.essay-entry` to the Essays section in `index.html`.
4. Add the public URL to `sitemap.xml`.

The article theme supports normal prose, headings, lists, blockquotes, tables, code blocks, footnotes, and optional quaestio-style callouts. KaTeX renders `$inline$` and `$$display$$` mathematics.

## Add a field note

Create matching `.html` and `.md` files under `skills/`, use `skills/tmux-multi-agent-orchestrator.html` as the HTML reference, add the note to `index.html`, and register its URL in `sitemap.xml`.

## External dependencies

- Google Fonts: EB Garamond and UnifrakturMaguntia
- KaTeX 0.16.9 for essay mathematics
- js-sha3 0.9.3 for the short content hash in essay colophons

The site remains readable if these scripts fail; the web fonts fall back to Georgia and formulas remain as source text.

## Deploy

Push the desired branch to the GitHub Pages source branch. The included `.nojekyll` file keeps Pages from processing the static files with Jekyll.
