# Skill: Create a new blog post

Add an essay to JM Blog without introducing a separate layout or content registry.

## Steps

1. Copy `posts/_TEMPLATE.html` to `posts/your-post-slug.html`.
2. In the new file, update:
   - the meta description and `<title>`;
   - `.qnum` with the sequential essay number, date, and tags;
   - `<h1>` and `.epigraph`;
   - the article body and signature.
3. Keep the first prose paragraph marked `class="dropcap"`.
4. Remove KaTeX assets only if the essay has no mathematics. Keep the content-hash script and colophon.
5. Add the essay to the Essays section in `index.html` using the existing `.essay-entry` markup. Published entries link their title; unpublished entries do not.
6. Add the page to `sitemap.xml` with its public URL, publication date, and priority `0.8`.
7. Serve the repository locally and verify the landing link, article navigation, formulas, code overflow, and mobile layout.

## Formatting

- Inline math: `$E = mc^2$`
- Display math: `$$\frac{a}{b}$$`
- Code: `<pre><code class="language-python">…</code></pre>`
- Quotes: `<blockquote>…</blockquote>`
- Lists: `<ul>` or `<ol>` with `<li>` children
- Quaestio callout: `.disputa` with an optional `.label`
- Footnotes: `sup.fn` references and a `.notes` section

## Naming

- Page: `posts/kebab-case.html`
- Date: `YYYY-MM-DD`
- Essay number: two digits on the landing page and in `.qnum`
