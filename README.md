# Ocealgo Internal Tools

A collection of small browser-based utilities for the Ocealgo team — data converters, generators, and other day-to-day helpers.

**Live:** https://ocealgo.github.io/internal-tools/

Every tool is a single self-contained HTML file. No build step, no backend, no install. Open the dashboard in any browser, pick a tool, get work done.

---

## Available tools

| Tool | What it does |
|------|--------------|
| [Contact Converter](contact-converter/) | Converts Zoho Books contact exports into the Ocealgo party-import Excel format. |

> The dashboard at the repo root (`index.html`) auto-renders this list as searchable cards.

---

## Repo structure

```
internal-tools/
├── index.html              ← dashboard / landing page (lists all tools)
├── README.md               ← you are here
├── contact-converter/
│   ├── index.html          ← the tool itself
│   └── README.md           ← per-tool docs
├── _template/              ← starter you can copy when adding a new tool
│   ├── index.html
│   └── README.md
└── ...                     ← future tools each get their own folder
```

**Conventions**

- Each tool lives in its own folder at the repo root.
- The folder name is the tool's URL slug (`contact-converter` → `…/contact-converter/`).
- Each tool's main entry point is named `index.html` so the URL stays clean.
- Tools should be **fully client-side** — no backend, no upload of user data anywhere. Everything runs in the user's browser.

---

## Adding a new tool

1. Copy the `_template/` folder and rename it to your tool's slug:
   ```bash
   cp -r _template my-new-tool
   ```
2. Edit `my-new-tool/index.html` — build the tool.
3. Update `my-new-tool/README.md` — describe what it does.
4. Register the tool in the dashboard. Open the root `index.html`, find the `TOOLS` array, and append an entry:
   ```js
   {
     name: 'My New Tool',
     desc: 'One or two sentences explaining what it does.',
     path: 'my-new-tool/',
     icon: '🛠️',
     tag: 'Excel',           // short category label shown on the card
     keywords: 'extra terms for search'
   }
   ```
5. Commit and push. GitHub Pages auto-deploys from the `main` branch.

---

## Deployment

Hosted on **GitHub Pages**, served from the `main` branch root.

To enable Pages on a fresh fork or new repo:

1. Go to **Settings → Pages**
2. Under **Build and deployment**, set:
   - **Source:** Deploy from a branch
   - **Branch:** `main` / `(root)`
3. Save. Within ~1 minute, the site goes live at `https://<owner>.github.io/<repo>/`.

Custom domain (optional): add a `CNAME` file at the repo root with the desired domain, configure DNS, and enable HTTPS in Pages settings.

---

## Tech notes

- All tools use vanilla HTML/CSS/JS by default. Stay buildless unless you have a strong reason.
- For Excel parsing/generation, tools use [SheetJS](https://sheetjs.com/) loaded from `cdn.jsdelivr.net`. If your team is behind a firewall that blocks public CDNs, inline the library into the HTML file instead.
- All file processing happens **in the user's browser**. No data is uploaded to any server. This is by design — keep it that way for any tool that touches Ocealgo business data (contacts, pricing, distributors, etc.).

---

## License

Internal use only. Code is the property of Ocealgo.
