# JM Blog - CRT Terminal Theme

A blog with retro CRT terminal aesthetics and glitch effects.

## Structure

```
blog/
├── index.html          # Main page with interactive terminal
├── 404.html            # Custom error page
├── robots.txt          # Search engine directives
├── sitemap.xml         # SEO sitemap
├── css/
│   └── style.css       # CRT/glitch styles
├── posts/
│   ├── _TEMPLATE.html  # Template for new posts
│   └── *.html          # Individual posts
├── images/             # Images for posts
└── .nojekyll           # For GitHub Pages
```

## How to add a new post

### 1. Create the HTML file in `/posts/`

Copy `_TEMPLATE.html` and modify the content.

### 2. Add it to index.html

In the `posts` array in the JavaScript:

```javascript
const posts = [
  { file: 'MY_NEW_POST.log', url: 'posts/my-new-post.html', date: '2049-12-07', size: '3.2K', title: 'My New Post', excerpt: 'Short description...' },
  // ... other posts
];
```

## Content formatting

### LaTeX

Inline: `$E = mc^2$` → $E = mc^2$

Display (block):
```
$$
P_n = \sum_{k=m}^{n} \binom{n}{k} p^k (1-p)^{n-k}
$$
```

### Code with syntax highlighting

```html
<pre><code class="language-python">
def hello():
    # This is a comment
    print("Hello, world!")
</code></pre>
```

Supported languages: `python`, `javascript`, `solidity`, `bash`, `rust`

### Images

```html
<figure>
  <img src="../images/my-image.png" alt="Description">
  <figcaption>Optional caption</figcaption>
</figure>
```

### Blockquotes

```html
<blockquote>
  This is a quote.
</blockquote>
```

## Deploy on GitHub Pages

1. Push all files to your repo
2. Go to Settings → Pages
3. Source: `main` branch, `/ (root)`
4. The `.nojekyll` file is included to prevent Jekyll processing

## CRT Effects

- **Scanlines**: subtle horizontal lines
- **Flicker**: very subtle flicker (0.5%)
- **Glitch bars**: random green bars
- **Vignette**: darkening at edges
- **Right glow**: glow on the right side
- **Block cursor**: blinking rectangular cursor

## Terminal commands (index.html)

- `ls` / `ls -la` - List posts
- `cat <file>` - Preview a post
- `cd <file>` - Open a post
- `clear` - Clear screen
- `whoami` - Author info
- `neofetch` - System info
- `history` - Command history
- `download` - Download all posts as .txt (for AI/LLM consumption)
- `help` - Show help

## Keyboard shortcuts

- `Tab` - Autocomplete filenames
- `↑` `↓` - Navigate command history
- `Ctrl+L` - Clear screen
- `Backspace` / `Escape` - Go back to index (from a post)
