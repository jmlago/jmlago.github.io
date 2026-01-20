# Skill: Create New Skill

Add a new Claude Code skill to the JM Blog skills section.

## Steps

1. **Create the .md file** in `skills/`:
   ```
   skills/your-skill-name.md
   ```

2. **Create the .html file** in `skills/` using this structure:
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Skill Title // JM BLOG</title>
     <link rel="stylesheet" href="../css/style.css">
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
       .download-btn {
         display: inline-block;
         background: transparent;
         border: 2px solid var(--green);
         color: var(--green);
         padding: 12px 24px;
         font-family: inherit;
         font-size: 1rem;
         cursor: pointer;
         text-decoration: none;
         margin: 20px 0;
         transition: all 0.2s ease;
       }
       .download-btn:hover {
         background: var(--green);
         color: var(--black);
         box-shadow: 0 0 20px var(--green-glow);
       }
       .download-btn::before { content: "$ "; }
       .skill-badge {
         display: inline-block;
         background: rgba(0, 255, 65, 0.15);
         border: 1px solid var(--green-dim);
         padding: 4px 12px;
         font-size: 0.85rem;
         color: var(--green-dim);
         margin-bottom: 10px;
       }
     </style>
     <link rel="icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><text y='.9em' font-size='90'>⬤</text></svg>">
   </head>
   <body>
     <div class="right-glow"></div>
     <div class="container">
       <a href="../index.html" class="back-link">cd ..</a>
       <article>
         <div class="post-header">
           <span class="skill-badge">CLAUDE CODE SKILL</span>
           <h1>Skill Title</h1>
           <p class="meta">Claude Code // tag1, tag2</p>
         </div>
         <a href="your-skill-name.md" download class="download-btn">download skill.md</a>
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
     <script src="https://cdn.jsdelivr.net/npm/prismjs@1.29.0/prism.min.js"></script>
     <script src="https://cdn.jsdelivr.net/npm/prismjs@1.29.0/components/prism-bash.min.js"></script>
   </article>
   </body>
   </html>
   ```

3. **Add to skills array** in `index.html` (~line 169):
   ```javascript
   { file: 'SKILL_NAME.skill', url: 'skills/your-skill-name.html', size: 'XK', title: 'Skill Title', excerpt: 'One-line description.' },
   ```
   - `file`: SCREAMING_SNAKE_CASE.skill
   - `url`: kebab-case.html path
   - `size`: approximate file size

4. **Add to sitemap.xml**:
   ```xml
   <url>
     <loc>https://jmlago.github.io/skills/your-skill-name.html</loc>
     <lastmod>YYYY-MM-DD</lastmod>
     <priority>0.8</priority>
   </url>
   ```

## File naming

- .md file: `kebab-case.md`
- .html file: `kebab-case.html`
- Terminal name: `SCREAMING_SNAKE.skill`

## Code highlighting

Add Prism components for languages used:
```html
<script src="https://cdn.jsdelivr.net/npm/prismjs@1.29.0/components/prism-python.min.js"></script>
```

Available: python, javascript, solidity, bash, rust
