# CLAUDE.md - JM Blog Codebase Guide

## Overview

Static personal blog with retro CRT terminal aesthetic. Hosted on GitHub Pages at `jmlago.github.io`. No build tools or framework - pure HTML/CSS/JS.

## Directory Structure

```
/
├── index.html           # Main terminal interface (interactive shell)
├── css/style.css        # All styling (CRT effects, typography)
├── posts/
│   ├── _TEMPLATE.html   # Copy this for new posts
│   └── *.html           # Individual blog posts
├── images/              # Post images (currently empty)
├── 404.html             # Custom error page
├── sitemap.xml          # SEO sitemap (UPDATE when adding posts)
├── robots.txt           # Search engine config
└── .nojekyll            # Disables Jekyll processing
```

## Adding a New Post (3-Step Process)

**CRITICAL**: Adding a post requires updating THREE files:

### 1. Create the post file
Copy `posts/_TEMPLATE.html` to `posts/your-post-slug.html` and edit:
- `<title>` tag: `Your Title // JM BLOG`
- `<h1>`: Post title
- `<p class="meta">`: Date format `YYYY-MM-DD // Tag1, Tag2`
- Article content
- Signature: `<div class="signature">JM</div>` or `JM // GenLayer`

### 2. Register in index.html
Add entry to the `posts` array (around line 163):
```javascript
const posts = [
  { file: 'YOUR_POST_NAME.log', url: 'posts/your-post-slug.html', date: 'YYYY-MM-DD', size: 'XK', title: 'Your Title', excerpt: 'One-line description.' },
  // existing posts...
];
```
- `file`: Display name in terminal (use SCREAMING_SNAKE.log format)
- `url`: Actual path to HTML file
- `size`: Approximate file size for terminal display
- Order: Newest posts first

### 3. Update sitemap.xml
Add URL entry:
```xml
<url>
  <loc>https://jmlago.github.io/posts/your-post-slug.html</loc>
  <lastmod>YYYY-MM-DD</lastmod>
  <priority>0.8</priority>
</url>
```

## Post HTML Structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <!-- Standard head with KaTeX + Prism CSS -->
  <title>Post Title // JM BLOG</title>
  <link rel="stylesheet" href="../css/style.css">
  <!-- Prism override styles for terminal look (inline) -->
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
  <div class="status-bar">...</div>
  <!-- Scripts: glitch bars, KaTeX, Prism -->
</body>
</html>
```

## Content Formatting

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
Supported: `python`, `javascript`, `solidity`, `bash`, `rust`

### Other Elements
- **Bold**: `<strong>text</strong>` (renders bright green)
- **Italic**: `<em>text</em>`
- **Links**: `<a href="url">text</a>` (use relative paths for internal links)
- **Blockquote**: `<blockquote>text</blockquote>`
- **Lists**: Standard `<ul>/<ol>` and `<li>`

### Internal Post Links
Use relative paths: `<a href="other-post.html">link text</a>`

## Styling Reference

CSS variables (in `:root`):
- `--green`: #00ff41 (primary text)
- `--green-bright`: #00ff7f (bold, links)
- `--green-dim`: #00aa2a (meta, secondary)
- `--green-glow`: rgba(0,255,65,0.5) (glow effects)
- `--black`: #000000 (background)

Key classes:
- `.post-header`: Title section with border
- `.meta`: Date/tags line (auto-prefixed with //)
- `.signature`: Author line (auto-prefixed with $)
- `.back-link`: Navigation back (auto-prefixed with arrow)

## Terminal Commands (index.html)

The homepage is an interactive terminal with commands:
- `ls [-la]`: List posts
- `cat <file>`: Preview post
- `cd <file>`: Open post
- `clear`: Clear screen
- `whoami`: Author info
- `neofetch`: System info
- `download`: Export all posts as .txt

## External Dependencies (CDN)

All loaded via jsDelivr:
- **KaTeX** 0.16.9: LaTeX rendering
- **Prism** 1.29.0: Code syntax highlighting
- **Fira Code**: Monospace font (Google Fonts)

## Common Tasks

### Edit post content
Just edit the HTML file in `posts/`. No rebuild needed.

### Change post order
Reorder entries in the `posts` array in `index.html`.

### Update author info
Edit the `whoami` command handler in `index.html` (~line 279).

### Add new code language support
Add Prism component script in post template (bottom of body).

### Modify terminal behavior
All terminal logic is in `<script>` at bottom of `index.html`.

## Git Workflow

Single `main` branch. Push directly to deploy via GitHub Pages.

## Important Notes

- No build step - edit files directly
- Date format is always `YYYY-MM-DD`
- Post URLs use kebab-case slugs
- Terminal file names use SCREAMING_SNAKE_CASE.log
- All posts share the same script boilerplate (KaTeX, Prism, glitch bars, keyboard nav)
