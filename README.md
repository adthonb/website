# Adthon's Personal Website (`website`)

Source repository for [adthonb.github.io](https://adthonb.github.io/), built with [Lektor](https://www.getlektor.com/) and [Terminal.css](https://terminalcss.xyz/).

---

## Architecture Overview

This project uses a two-repository setup modeled after [ericmjl/website](https://github.com/ericmjl/website):

- **`website/` (This Repository)**: Source code, content models, Jinja2 templates, and Markdown content.
- **`adthonb.github.io/` (`../adthonb.github.io`)**: Destination repository that serves the compiled static HTML/CSS on GitHub Pages.

---

## Prerequisites

- Python 3.11+
- [uv](https://docs.astral.sh/uv/) (Python package manager)

To install `uv` (if not already installed):
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

---

## Quick Start

### 1. Install Dependencies

From the `website` directory, `uv` will automatically manage the virtual environment:

```bash
cd website
uv sync
```

### 2. Start the Local Development Server

```bash
uv run lektor server
```

Open [http://127.0.0.1:5000](http://127.0.0.1:5000) in your browser. The server supports live auto-reload whenever you edit content, templates, or styles.

---

## Content Management

### Adding a New Blog Post

Create a new directory under `content/blog/<post-slug>/` with a `contents.lr` file:

```markdown
_model: blog-post
---
title: My First Post
---
pub_date: 2026-09-20
---
author: Adthon
---
summary: A brief summary of this post for list views.
---
body:

Write your post content here in standard Markdown.
```

### Adding a Project

Create a new directory under `content/projects/<project-slug>/` with a `contents.lr` file:

```markdown
_model: project
---
name: Project Name
---
date: 2026-09-20
---
url: https://github.com/adthonb/example-project
---
description:

Description of what you built, the architecture, and technologies used.
```

---

## Building and Publishing to GitHub Pages

### 1. Build the Static Output

Compile the site directly into your deployment repository:

```bash
uv run lektor build -O ../adthonb.github.io
```

### 2. Push Changes to GitHub Pages

Navigate to the `adthonb.github.io` directory and push:

```bash
cd ../adthonb.github.io
git add .
git commit -m "Publish new post"
git push origin main
```

---

## Project Structure

```
website/
├── assets/                     # Static files copied directly into build output
│   └── static/css/custom.css   # Terminal.css overrides & dark mode styles
├── content/                    # Site content hierarchy (.lr files)
│   ├── contents.lr             # Homepage content
│   ├── about/contents.lr       # About page
│   ├── blog/contents.lr        # Blog index and posts
│   └── projects/contents.lr    # Projects index and entries
├── models/                     # Schema definitions for content types
│   ├── blog.ini, blog-post.ini
│   ├── page.ini
│   └── projects.ini, project.ini
├── templates/                  # Jinja2 HTML templates
│   ├── layout.html             # Base shell, header, nav, dark mode toggle
│   ├── page.html, blog.html, blog-post.html
│   └── projects.html, project.html
├── personal-site.lektorproject # Lektor configuration
├── pyproject.toml              # Dependencies managed by uv
├── uv.lock                     # Lockfile
├── AGENTS.md                   # AI agent development & style guide
└── README.md                   # This file
```

---

## License

All content is copyright &copy; Adthon B. All rights reserved.
