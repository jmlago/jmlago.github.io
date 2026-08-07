# Skill: Create a downloadable field note

Add a readable note and its downloadable Markdown source to JM Blog.

## Steps

1. Create `skills/your-note-name.md` with the source content.
2. Copy `skills/tmux-multi-agent-orchestrator.html` to `skills/your-note-name.html` and replace its content.
3. Update the meta description, `<title>`, `.qnum`, `<h1>`, `.epigraph`, tags, and article body.
4. Point both `.download-btn` and the colophon source link to `your-note-name.md` with the `download` attribute.
5. Add an `.essay-entry` under Notes in `index.html`.
6. Add the HTML URL to `sitemap.xml` with priority `0.8`.
7. Serve the site locally and verify the note, source download, long code blocks, and mobile layout.

## Formatting

- Use `../css/essay.css`; do not add page-specific terminal or syntax-highlighting themes.
- Mark the first prose paragraph with `class="dropcap"`.
- Use `<pre><code class="language-bash">…</code></pre>` for commands.
- Keep content inside `<article><div class="wrap">…</div></article>`.

## Naming

- Markdown source: `skills/kebab-case.md`
- HTML page: `skills/kebab-case.html`
- Landing label: sequential two-digit note number
