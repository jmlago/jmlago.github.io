# Skill: Create New Blog Post

Add a new blog post to the JM Blog.

## Steps

1. **Create the post file** in `posts/`:
   ```
   posts/your-post-slug.html
   ```
   Use the template from `posts/_TEMPLATE.html`

2. **Edit the post**:
   - `<title>`: `Your Title // JM BLOG`
   - `<h1>`: Post title
   - `<p class="meta">`: `YYYY-MM-DD // Tag1, Tag2`
   - Article content
   - Signature: `<div class="signature">JM</div>` or `JM // GenLayer`

3. **Register in index.html** (~line 163), add to `posts` array:
   ```javascript
   { file: 'XXX_POST_NAME.log', url: 'posts/your-post-slug.html', date: 'YYYY-MM-DD', size: 'XK', title: 'Your Title', excerpt: 'One-line description.' },
   ```
   - `file`: `NNN_SCREAMING_SNAKE.log` format (NNN = sequential number)
   - `url`: actual path to HTML file
   - `size`: approximate file size
   - Order: newest posts first

4. **Add to sitemap.xml**:
   ```xml
   <url>
     <loc>https://jmlago.github.io/posts/your-post-slug.html</loc>
     <lastmod>YYYY-MM-DD</lastmod>
     <priority>0.8</priority>
   </url>
   ```

## Post template structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Post Title // JM BLOG</title>
  <link rel="stylesheet" href="../css/style.css">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.css">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/prismjs@1.29.0/themes/prism-tomorrow.min.css">
  <style>
    pre[class*="language-"], code[class*="language-"] {
      background: rgba(0, 20, 0, 0.8) !important;
      color: #00ff41 !important;
      text-shadow: none !important;
    }
    .token.comment { color: #006600 !important; }
    .token.keyword { color: #00ff7f !important; }
    .token.string { color: #7fff7f !important; }
    .token.number { color: #00ffff !important; }
    .token.function { color: #00ff41 !important; }
    .token.operator { color: #00aa2a !important; }
  </style>
  <link rel="icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><text y='.9em' font-size='90'>⬤</text></svg>">
</head>
<body>
  <div class="right-glow"></div>
  <div class="container">
    <a href="../index.html" class="back-link">cd ..</a>
    <article>
      <div class="post-header">
        <h1>Title</h1>
        <p class="meta">YYYY-MM-DD // Tags</p>
      </div>
      <!-- Content here -->
      <div class="signature">JM</div>
    </article>
  </div>
  <div class="status-bar">
    <span>SYSTEM STATUS: ONLINE // TYPE 'help' FOR COMMANDS.</span>
    <div>
      <a href="https://github.com/jmlago">GitHub</a>
      <a href="https://x.com/josemlago">X</a>
    </div>
  </div>
  <script>
    document.addEventListener('keydown', (e) => {
      if (e.target.tagName === 'INPUT' || e.target.tagName === 'TEXTAREA') return;
      if (e.key === 'Backspace' || e.key === 'Escape') {
        e.preventDefault();
        window.location.href = '../index.html';
      }
    });
    function createGlitchBar() {
      if (Math.random() > 0.7) {
        const bar = document.createElement('div');
        bar.className = 'glitch-bar';
        bar.style.top = Math.random() * 100 + '%';
        bar.style.transform = `translateX(${(Math.random() - 0.5) * 30}px) skewX(-2deg)`;
        bar.style.height = (1 + Math.random() * 5) + 'px';
        document.body.appendChild(bar);
        setTimeout(() => bar.remove(), 50 + Math.random() * 100);
      }
    }
    setInterval(createGlitchBar, 400);
  </script>
  <script src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/contrib/auto-render.min.js"></script>
  <script>
    document.addEventListener("DOMContentLoaded", function() {
      renderMathInElement(document.body, {
        delimiters: [
          {left: '$$', right: '$$', display: true},
          {left: '$', right: '$', display: false}
        ]
      });
    });
  </script>
  <script src="https://cdn.jsdelivr.net/npm/prismjs@1.29.0/prism.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/prismjs@1.29.0/components/prism-python.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/prismjs@1.29.0/components/prism-javascript.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/prismjs@1.29.0/components/prism-solidity.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/prismjs@1.29.0/components/prism-bash.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/prismjs@1.29.0/components/prism-rust.min.js"></script>
</body>
</html>
```

## Content formatting

### Math (KaTeX)
- Inline: `$E = mc^2$`
- Block: `$$\frac{a}{b}$$` (on separate lines)

### Code (Prism)
```html
<pre><code class="language-python">
def example():
    pass
</code></pre>
```

### Other elements
- **Bold**: `<strong>text</strong>`
- **Italic**: `<em>text</em>`
- **Links**: `<a href="url">text</a>`
- **Blockquote**: `<blockquote>text</blockquote>`
- **Lists**: `<ul>/<ol>` with `<li>`

## Naming conventions

- Date format: `YYYY-MM-DD`
- Post URLs: kebab-case slugs
- Terminal file names: `NNN_SCREAMING_SNAKE_CASE.log`
