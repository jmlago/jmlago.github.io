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
├── skills/
│   └── *.html + *.md    # Claude Code skills (downloadable)
├── images/              # Post images
├── sitemap.xml          # SEO sitemap (UPDATE when adding content)
├── 404.html             # Custom error page
└── .claude/skills/      # Claude Code skills for this repo
```

## Skills

For detailed step-by-step instructions, use these skills:

- **Create a new blog post**: `.claude/skills/create-blog-post.md`
- **Create a new skill**: `.claude/skills/create-skill.md`

## Quick Reference

### Terminal Commands (index.html)

- `ls [-la]`: List posts
- `ls skills` or `skills`: List Claude Code skills
- `cat <file>`: Preview post or skill
- `cd <file>`: Open post or skill
- `clear`, `whoami`, `neofetch`, `history`, `download`

### Content Formatting

**Math (KaTeX)**:
- Inline: `$E = mc^2$`
- Block: `$$\frac{a}{b}$$`

**Code (Prism)**: `<pre><code class="language-python">...</code></pre>`
Supported: `python`, `javascript`, `solidity`, `bash`, `rust`

**HTML elements**: `<strong>`, `<em>`, `<a>`, `<blockquote>`, `<ul>/<ol>`

### Naming Conventions

- Date format: `YYYY-MM-DD`
- Post URLs: `kebab-case.html`
- Terminal posts: `NNN_SCREAMING_SNAKE.log`
- Terminal skills: `SCREAMING_SNAKE.skill`

### CSS Variables

- `--green`: #00ff41 (primary)
- `--green-bright`: #00ff7f (bold, links)
- `--green-dim`: #00aa2a (meta)
- `--black`: #000000 (background)

## External Dependencies (CDN)

- **KaTeX** 0.16.9: LaTeX rendering
- **Prism** 1.29.0: Code syntax highlighting
- **Fira Code**: Monospace font

## Git Workflow

Single `main` branch. Push directly to deploy via GitHub Pages.
