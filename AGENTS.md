# AGENTS.md

Instructions and architecture guide for AI coding agents working on this website.

---

## 1. Project Overview & Two-Repo Architecture

This repository (`website`) contains the **source code, content, models, assets, and templates** for Adthon's personal website and portfolio.

- **Source Repository**: `website/` (this repository). All authoring, design, and development happen here.
- **Deployment Repository**: `adthonb.github.io/` (`../adthonb.github.io`). Contains purely the compiled static build output hosted on GitHub Pages (`https://adthonb.github.io/`), modeled after [ericmjl/website](https://github.com/ericmjl/website) & [ericmjl.github.io](https://github.com/ericmjl/ericmjl.github.io).
- **Generator**: [Lektor](https://www.getlektor.com/) (`3.3+`).
- **Python Environment**: [uv](https://docs.astral.sh/uv/) (`pyproject.toml`, `.venv`).
- **CSS Framework**: [Terminal.css](https://terminalcss.xyz/) via CDN with custom overrides and dark mode support.
- **Templating**: Jinja2 (`templates/`).
- **Content Format**: Lektor `.lr` files (`contents.lr`).

> **Important**: Never author or edit content directly inside `../adthonb.github.io`. All source files live here. Always rebuild into `../adthonb.github.io`.

---

## 2. Codebase Map & Directory Roles

```
website/
├── assets/                     # Static assets copied verbatim to build output
│   └── static/
│       ├── css/
│       │   └── custom.css      # Terminal.css overrides, dark mode variables, tech cards
│       └── images/             # Static graphics, avatars, post images
├── content/                    # Site content hierarchy (mirrors URL routes)
│   ├── contents.lr             # Homepage (/)
│   ├── about/
│   │   └── contents.lr         # About page (/about/)
│   ├── blog/
│   │   └── contents.lr         # Blog index (/blog/)
│   └── projects/
│       └── contents.lr         # Projects index (/projects/)
├── models/                     # Content models defining schema & fields (.ini)
│   ├── blog.ini                # Blog list model & pagination
│   ├── blog-post.ini           # Individual blog post model
│   ├── page.ini                # Generic content page model
│   ├── project.ini             # Project item model
│   └── projects.ini            # Projects list model
├── templates/                  # Jinja2 templates
│   ├── layout.html             # Base layout (head, nav, footer, dark mode toggle)
│   ├── page.html               # Generic page template
│   ├── blog.html               # Blog listing template with pagination
│   ├── blog-post.html          # Individual blog post template
│   ├── projects.html           # Projects listing template
│   └── project.html            # Individual project template
├── personal-site.lektorproject # Lektor project root configuration
├── pyproject.toml              # Python project configuration (dependencies managed via uv)
├── uv.lock                     # uv lockfile
└── AGENTS.md                   # This instruction file
```

---

## 3. Development Workflow with `uv`

All development commands are executed from this repository root (`website/`):

```bash
# 1. Start local development server with auto-reload (http://127.0.0.1:5000)
uv run lektor server

# 2. Build static output directly into the GitHub Pages repository
uv run lektor build -O ../adthonb.github.io

# 3. Clean build cache if needed
uv run lektor clean --yes

# 4. Add new Python dependencies
uv add <package-name>
```

---

## 4. Content Modeling & Conventions

### Content Files (`contents.lr`)
- Every page is a directory containing a `contents.lr` file.
- Fields in `contents.lr` must match the schema in the corresponding `models/<model>.ini`.
- Use `_model: <model-name>` at the top of `contents.lr` if it differs from the directory default.
- Syntax: `field_name: value` separated by `---`.

### Static URLs
- Always use the Lektor `url` filter in Jinja2 templates: `{{ '/static/css/custom.css'|url }}` or `{{ post|url }}`.

---

## 5. Styling & Terminal.css Dark Mode Guidelines

- Terminal.css does **not** provide a native dark mode toggle. Dark mode is implemented via `.dark-mode` on `<body>` with CSS variables in `assets/static/css/custom.css`:
  ```css
  body.dark-mode {
    --background-color: #1a1a1a;
    --font-color: #e6e6e6;
    --primary-color: #62c4ff;
    --secondary-color: #888888;
    --code-bg-color: #242424;
  }
  ```
- The toggle script must wrap `localStorage` access in `try...catch` blocks to prevent security exceptions on restricted origins or `file://` contexts.
- **Do not** place inline CSS in `templates/layout.html`; place all styles in `assets/static/css/custom.css`.
- Render diagrams using `<pre class="mermaid">` rather than raw markdown code fences.

---

## 6. Writing Voice & Anti-Slop Principles

When drafting or editing content:

- **Preserve the writer's authentic voice**: Keep phrasing direct, hands-on, and personal ("I design and build...", "My work sits between...", "I enjoy solving infrastructure problems, but I still like writing code myself").
- **Concrete over abstract**: State specific numbers and scale (e.g. 14 hospitals, 24 microservices, 36 production nodes, ~5 min &rarr; ~50 sec deployment).
- **No AI slop / cliches**: Avoid words like *delve, foster, leverage, utilize, streamline, robust, cutting-edge, tapestry, beacon, transformative, paramount*.
- **No formatting decoration**: Do not put emojis in headings. Use plain text headings (`About Me`, `Technology Stack`, `Let's Connect`).

---

## 7. Verification & Deployment

```bash
# Verify build succeeds without template or model errors
uv run lektor build -O ../adthonb.github.io

# Inspect changes in the deployment repository
cd ../adthonb.github.io
git status
git diff

# Test locally if needed
python3 -m http.server 5000
```
