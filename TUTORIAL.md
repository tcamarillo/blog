# Hugo Blog Tutorial - Complete Guide

A comprehensive tutorial for creating, customizing, and deploying a Hugo blog website. This guide is based on a real-world implementation and covers everything from basic setup to advanced customization.

> **📂 Repository Structure:** This tutorial uses the actual files in this repository as examples. You can explore the code alongside reading.

---

## Table of Contents

### Part 1: General Hugo Fundamentals
1. [What is Hugo?](#what-is-hugo)
2. [Installation](#installation)
3. [Project Structure Overview](#project-structure-overview)
4. [Configuration Files (hugo.toml)](#configuration-files-hugotoml)
5. [Themes](#themes)
6. [Content Organization](#content-organization)
7. [Front Matter](#front-matter)
8. [Templates & Layouts](#templates--layouts)
9. [Shortcodes](#shortcodes)
10. [Building & Serving](#building--serving)

### Part 2: Blog-Specific Features
11. [Blog Content Types](#blog-content-types)
12. [Taxonomies (Categories, Tags, Series)](#taxonomies-categories-tags-series)
13. [Authors](#authors)
14. [Archetypes (Content Templates)](#archetypes-content-templates)
15. [Multilingual Support](#multilingual-support)
16. [RSS Feeds](#rss-feeds)

### Part 3: Customization
17. [Quick Customization Guide (Just Change Parameters)](#quick-customization-guide)
18. [Overriding Theme Templates](#overriding-theme-templates)
19. [Custom Shortcodes](#custom-shortcodes)
20. [Adding Custom CSS/JS](#adding-custom-cssjs)

### Part 4: Deployment
21. [GitHub Pages Deployment](#github-pages-deployment)
22. [Using Custom Domains](#using-custom-domains)

### Appendix
- [Repository Files Reference](#repository-files-reference)
- [Common Pitfalls & Best Practices](#common-pitfalls--best-practices)

---

# Part 1: General Hugo Fundamentals

## What is Hugo?

Hugo is one of the most popular open-source static site generators. It's written in Go and is known for its incredible speed and flexibility. Unlike dynamic websites that generate pages on each request, Hugo pre-builds all pages at build time, resulting in:

- ⚡ **Blazing fast** websites
- 🔒 **Secure** (no database or server-side code)
- 💰 **Free hosting** on GitHub Pages, Netlify, etc.
- 📝 **Content in Markdown** - easy to write and version control

---

## Installation

### macOS (with Homebrew)

```bash
# Install Hugo (extended version for SCSS/Sass support)
brew install hugo

# Verify installation
hugo version
```

**Optional: Setup Go and Node.js** (if you need them for custom modules/assets)

```bash
# Install Go
brew install go
echo 'export PATH="/opt/homebrew/opt/go/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
go version

# Install Node.js and Sass
brew install node
npm install -g sass
node -v
npm -v
sass -v
```

### Windows

**Option 1: Chocolatey**
```bash
choco install hugo-extended
hugo version
```

**Option 2: Windows Package Manager**
```bash
winget install Hugo.Hugo.Extended
hugo version
```

### Linux (WSL2 / Debian / Ubuntu)

**Step 1: Install Go 1.22+ (optional but recommended)**
```bash
wget https://go.dev/dl/go1.22.2.linux-amd64.tar.gz
sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.22.2.linux-amd64.tar.gz
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc
source ~/.bashrc
go version
```

**Step 2: Install Hugo**
```bash
wget https://github.com/gohugoio/hugo/releases/download/v0.139.0/hugo_extended_0.139.0_linux-amd64.deb
sudo dpkg -i hugo_extended_0.139.0_linux-amd64.deb
hugo version
```

**Step 3: Install Node.js and Sass** (optional)
```bash
sudo apt-get update
sudo apt-get install apt-transport-https
sudo apt install nodejs npm
sudo npm install -g sass
node -v
npm -v
sass -v
```

**Step 4: (Optional) Uninstall Hugo if needed**
```bash
sudo apt-get remove hugo
```

### Verify Installation

```bash
hugo version
```

You should see output like:
```
hugo v0.139.0-...
```

---

## Project Structure Overview

Here's the essential structure of a Hugo project:

```
my-blog/
├── hugo.toml              # Main configuration file
├── archetypes/            # Content templates for new posts
│   ├── default.md
│   └── posts.md
├── content/               # All your content (posts, pages)
│   ├── posts/             # Blog posts
│   ├── about.md           # Static pages
│   └── contact.md
├── layouts/               # Custom templates (override theme)
│   ├── partials/
│   └── shortcodes/
├── static/                # Static assets (images, files)
├── themes/                # Installed themes
│   └── coder/
├── public/                # Generated site (don't edit!)
└── resources/             # Processed resources cache
```

### Key Directories Explained

| Directory | Purpose |
|-----------|---------|
| `content/` | All your Markdown content goes here |
| `layouts/` | Custom templates to override theme |
| `static/` | Files copied as-is to public/ |
| `themes/` | Downloaded themes |
| `public/` | Output directory (generated site) |
| `archetypes/` | Templates for new content |

---

## Configuration Files (hugo.toml)

The `hugo.toml` (or `config.toml`, `hugo.yaml`, `hugo.json`) is the heart of your Hugo site.

### Essential Configuration

```toml
# Site URL (IMPORTANT: Change this to your domain!)
baseURL = "https://yourdomain.com/"

# Site title
title = "My Awesome Blog"

# Theme name (must match folder in themes/)
theme = "coder"

# Default language
languageCode = "en"
defaultContentLanguage = "en"

# Enable emoji support :smile:
enableEmoji = true
```

### Pagination

```toml
[pagination]
  pagerSize = 6  # Posts per page
```

### Site Parameters

Parameters are accessible in templates via `.Site.Params`:

```toml
[params]
author = "Your Name"
description = "Your site description"
keywords = "blog,tech,programming"
info = ["Developer", "Writer"]  # Multiple roles/titles
email = "you@example.com"
dateFormat = "January 2, 2006"  # Go date format
since = 2024  # Year site started

# Color scheme: "auto", "dark", or "light"
colorScheme = "auto"
hideColorSchemeToggle = false
```

### Social Links

```toml
[[params.social]]
name = "Github"
icon = "fa-brands fa-github fa-2x"
weight = 1
url = "https://github.com/yourusername/"

[[params.social]]
name = "LinkedIn"
icon = "fa-brands fa-linkedin fa-2x"
weight = 2
url = "https://linkedin.com/in/yourusername/"

[[params.social]]
name = "Twitter"
icon = "fa-brands fa-x-twitter fa-2x"
weight = 3
url = "https://twitter.com/yourusername/"
```

### Footer Configuration

```toml
[params.footer]
    trademark = 2024
    rss = true
    copyright = true
    author = true
    bottomText = [
      "Powered by <a href=\"http://gohugo.io\">Hugo</a>",
      "Theme by <a href=\"https://github.com/luizdepra/hugo-coder\">Coder</a>"
    ]
```

### Logo/Branding

```toml
[params.logo]
    logoMark = ">"
    logoText = "$ cd /home/"
    logoHomeLink = "/"
```

---

## Themes

### Finding Themes

Browse themes at: https://themes.gohugo.io/

Popular blog themes:
- **Coder** - Clean, minimal (used in this repo)
- **PaperMod** - Feature-rich
- **Stack** - Card-based layout
- **Blowfish** - Modern, customizable

### Installing a Theme

**Method 1: Git Submodule (Recommended)**
```bash
cd your-site
git submodule add https://github.com/luizdepra/hugo-coder themes/coder
```

**Method 2: Hugo Module**
```bash
hugo mod init github.com/yourusername/your-site
hugo mod get github.com/luizdepra/hugo-coder
```

**Method 3: Direct Download**
```bash
cd themes
git clone https://github.com/luizdepra/hugo-coder
```

### Managing Multiple Themes

You can install multiple themes and easily switch between them.

#### Adding Another Theme (via Submodule)

```bash
# Add first theme (if not already added)
git submodule add https://github.com/luizdepra/hugo-coder themes/coder

# Add second theme
git submodule add https://github.com/dillonzq/LoveIt themes/loveit

# Add third theme
git submodule add https://github.com/adityatelange/hugo-PaperMod themes/papermod
```

#### Initialize All Submodules (After Cloning)

When cloning a repo with multiple theme submodules:

```bash
# First time setup
git clone https://github.com/YOUR-USERNAME/your-blog.git
cd your-blog
git submodule update --init --recursive
```

#### Update All Submodules to Latest

```bash
# Update all submodules
git submodule update --remote --merge

# Or update specific theme
git submodule update --remote themes/coder
```

#### List All Installed Themes

```bash
ls -la themes/
```

You should see directories like: `coder/`, `loveit/`, `papermod/`

### Switching Between Themes

Change the active theme in `hugo.toml`:

```toml
# Using Coder theme
theme = "coder"
```

Switch to another:

```toml
# Using LoveIt theme
theme = "loveit"
```

Then restart the dev server:

```bash
hugo server -D
```

#### Testing Different Themes

Quick way to test multiple themes without editing the config:

```bash
# Test with coder theme
hugo server -D --theme coder

# Test with loveit theme  
hugo server -D --theme loveit

# Test with papermod theme
hugo server -D --theme papermod
```

### Removing a Theme

If you want to remove a theme you no longer need:

```bash
# Remove from submodules
git submodule deinit -f themes/loveit
rm -rf .git/modules/themes/loveit
git rm -f themes/loveit

# Commit the changes
git add .gitmodules
git commit -m "Remove loveit theme"
```

### Activating a Theme

In `hugo.toml`:
```toml
theme = "coder"
```

To switch themes, simply change this line to the folder name of another installed theme.

---

## Quick Project Setup

### Creating a New Hugo Site from Scratch

**macOS:**
```bash
hugo new site my-blog
cd my-blog
git init
git submodule add https://github.com/luizdepra/hugo-coder themes/coder
echo 'theme = "coder"' >> hugo.toml
hugo new posts/my-first-post.md
hugo server -D
```

**Linux/WSL2:**
```bash
hugo new site my-blog
cd my-blog
git init
git submodule add https://github.com/luizdepra/hugo-coder themes/coder
echo 'theme = "coder"' >> hugo.toml
hugo new posts/my-first-post.md
hugo server -D
```

> **Tip:** You can add multiple theme submodules and switch between them by changing `theme = "coder"` in `hugo.toml`

### Using This Repository as a Template

```bash
# Clone this repo
git clone https://github.com/YOUR-USERNAME/presentation_website.git
cd presentation_website

# Update theme submodule
git submodule update --init --recursive

# Install dependencies (if using npm)
npm install

# Start development server
hugo server -D
```

---

## Content Organization

### Creating Pages

```bash
# Create a new page
hugo new about.md

# Create a new blog post
hugo new posts/my-first-post.md
```

### Content Structure

```
content/
├── _index.md           # Homepage content (optional)
├── about.md            # /about/
├── contact.md          # /contact/
├── posts/              # Blog section
│   ├── _index.md       # /posts/ list page
│   ├── post-one.md     # /posts/post-one/
│   └── post-two.md     # /posts/post-two/
└── authors/            # Author profiles
    └── johndoe/
        └── _index.md   # /authors/johndoe/
```

### File Naming Conventions

- `_index.md` - List page for a section (has children)
- `index.md` - Single page as a bundle (resources in same folder)
- `*.md` - Regular content pages

---

## Front Matter

Front matter is metadata at the top of content files. Supports TOML (`+++`), YAML (`---`), or JSON.

### TOML Format (used in this repo)

```toml
+++
title = "My Amazing Post"
date = "2024-01-15"
draft = false
description = "A brief description"
author = "johndoe"
tags = ["hugo", "tutorial"]
categories = ["web development"]
series = ["Hugo Guide"]
+++
```

### YAML Format

```yaml
---
title: "My Amazing Post"
date: 2024-01-15
draft: false
description: "A brief description"
tags:
  - hugo
  - tutorial
---
```

### Common Front Matter Fields

| Field | Description |
|-------|-------------|
| `title` | Page title |
| `date` | Publication date |
| `draft` | If true, not published (unless -D flag) |
| `description` | SEO description |
| `tags` | List of tags |
| `categories` | List of categories |
| `author` / `authors` | Author(s) of the content |
| `slug` | Custom URL segment |
| `aliases` | Redirect old URLs here |
| `weight` | Manual ordering |

---

## Templates & Layouts

Hugo uses Go's `html/template` library. Templates control how content is rendered.

### Template Lookup Order

Hugo looks for templates in this order:
1. `layouts/` (your custom templates)
2. `themes/<theme>/layouts/` (theme templates)

### Base Template Structure

```html
<!-- layouts/baseof.html -->
<!DOCTYPE html>
<html>
<head>
    <title>{{ .Title }}</title>
</head>
<body>
    {{ block "main" . }}{{ end }}
</body>
</html>
```

### Common Template Types

| Template | Purpose |
|----------|---------|
| `baseof.html` | Base template all others extend |
| `home.html` | Homepage |
| `single.html` | Single content page |
| `list.html` | List of content (section pages) |
| `terms.html` | Taxonomy terms list |

### Template Variables

```go
{{ .Title }}              // Page title
{{ .Content }}            // Page content
{{ .Date }}               // Page date
{{ .Site.Title }}         // Site title
{{ .Site.Params.author }} // Custom parameter
{{ range .Pages }}        // Loop through pages
```

---

## Shortcodes

Shortcodes are reusable content snippets. They're like functions for your content.

### Built-in Shortcodes

```markdown
<!-- Embed YouTube video -->
{{</* youtube dQw4w9WgXcQ */>}}

<!-- Embed Twitter/X post -->
{{</* twitter 1234567890 */>}}

<!-- Syntax highlighted code -->
{{</* highlight go */>}}
fmt.Println("Hello, World!")
{{</* /highlight */>}}

<!-- Include another file -->
{{</* include "snippet.html" */>}}

<!-- Figure with caption -->
{{</* figure src="/images/photo.jpg" caption="My photo" */>}}
```

### Custom Shortcodes

Create in `layouts/shortcodes/`:

```html
<!-- layouts/shortcodes/email.html -->
<a href="mailto:{{ .Site.Params.email }}">{{ .Site.Params.email }}</a>
```

Usage in content:
```markdown
Contact me at {{</* email */>}}
```

---

## Building & Serving

### Development Server

```bash
# Start dev server with drafts
hugo server -D

# With specific port
hugo server -D -p 1313

# Bind to network (access from other devices)
hugo server -D --bind 0.0.0.0
```

Access at: http://localhost:1313/

### Building for Production

```bash
# Generate site
hugo

# Generate with minification
hugo --minify

# To specific directory
hugo -d docs
```

Output goes to `public/` by default.

---

# Part 2: Blog-Specific Features

## Blog Content Types

### Standard Blog Post

```toml
# content/posts/my-post.md
+++
title = "How I Built My Blog"
date = "2024-01-15"
description = "A complete guide to setting up Hugo"
authors = ["johndoe"]
tags = ["hugo", "tutorial", "web"]
categories = ["technology"]
series = ["Hugo Deep Dive"]
draft = false
+++

Introduction paragraph...

<!--more-->

This content appears after "Read More" on list pages.
```

### The `<!--more-->` Divider

This special comment creates a content summary:
- Content **before** appears in list pages
- Content **after** appears only on the full post page

---

## Taxonomies (Categories, Tags, Series)

Taxonomies are ways to classify content. Configure in `hugo.toml`:

```toml
[taxonomies]
  category = "categories"
  tag = "tags"
  series = "series"
  author = "authors"
```

### Using Taxonomies in Posts

```toml
+++
tags = ["golang", "programming", "tutorial"]
categories = ["development"]
series = ["Learn Go"]
+++
```

### Taxonomy Pages

Hugo automatically creates:
- `/tags/` - All tags
- `/tags/golang/` - Posts tagged "golang"
- `/categories/` - All categories
- `/series/` - All series

### Creating Translated Taxonomy Terms

For multilingual sites:

```markdown
<!-- content/tags/shortcodes/_index.pt-br.md -->
---
title: "Códigos Curtos"
---
```

---

## Authors

Authors are a custom taxonomy. Create author profiles:

```markdown
<!-- content/authors/johndoe/_index.md -->
---
title: "John Doe"
bio: "Senior developer and tech writer"
photo: "/images/authors/johndoe.jpg"
social:
  twitter: "johndoe"
  github: "johndoe"
---

John is a software developer with 10 years of experience...
```

### Linking Posts to Authors

```toml
+++
authors = ["johndoe", "janedoe"]  # Multiple authors
+++
```

---

## Archetypes (Content Templates)

Archetypes are templates for new content. When you run `hugo new posts/my-post.md`, Hugo uses the archetype.

### Default Archetype

```markdown
<!-- archetypes/default.md -->
+++
draft = true
date = {{ .Date }}
title = '{{ replace .File.ContentBaseName "-" " " | title }}'
+++
```

### Posts Archetype

```markdown
<!-- archetypes/posts.md -->
+++
draft = true
date = {{ .Date }}
title = ""
description = ""
authors = ["yourname"]
tags = []
categories = []
series = ""
+++

Write your intro here...

<!--more-->

Full content...
```

### Usage

```bash
# Uses archetypes/posts.md
hugo new posts/my-new-post.md

# Uses archetypes/default.md
hugo new about.md
```

---

## Multilingual Support

### Configuration

```toml
defaultContentLanguage = "en"

[languages.en]
languageName = "English"
weight = 1

[[languages.en.menu.main]]
name = "About"
url = "about/"
weight = 1

[[languages.en.menu.main]]
name = "Blog"
url = "posts/"
weight = 2

[languages.pt-br]
languageName = "Português"
weight = 2

[languages.pt-br.params]
author = "Seu Nome"
description = "Meu blog pessoal"

[[languages.pt-br.menu.main]]
name = "Sobre"
url = "about/"
weight = 1

[[languages.pt-br.menu.main]]
name = "Blog"
url = "posts/"
weight = 2
```

### Content Files

Create translated versions with language suffix:

```
content/
├── about.md          # English (default)
├── about.pt-br.md    # Portuguese
└── posts/
    ├── my-post.md         # English
    └── my-post.pt-br.md   # Portuguese
```

### Language in Templates

```go
{{ i18n "read_more" }}  // Translated string
{{ .Language.Lang }}    // Current language code
```

---

## RSS Feeds

Hugo generates RSS feeds automatically:

- `/index.xml` - Main feed
- `/posts/index.xml` - Posts feed
- `/categories/tech/index.xml` - Category feed

### Customizing RSS

Override `layouts/_default/rss.xml` or configure in `hugo.toml`:

```toml
[params.footer]
  rss = true  # Show RSS link in footer
```

---

# Part 3: Customization

## Quick Customization Guide

**If you just want to change basic info without touching code, edit these in `hugo.toml`:**

### 1. Basic Site Info
```toml
baseURL = "https://YOUR-DOMAIN.com/"
title = "YOUR SITE TITLE"
```

### 2. Personal Information
```toml
[params]
author = "YOUR NAME"
description = "YOUR DESCRIPTION"
keywords = "your,keywords,here"
info = ["Your Title", "Your Role"]
email = "your@email.com"
```

### 3. Social Links
```toml
[[params.social]]
name = "Github"
icon = "fa-brands fa-github fa-2x"
weight = 1
url = "https://github.com/YOUR-USERNAME/"

[[params.social]]
name = "LinkedIn"
icon = "fa-brands fa-linkedin fa-2x"
weight = 2
url = "https://linkedin.com/in/YOUR-USERNAME/"
```

### 4. Menu Items
```toml
[[languages.en.menu.main]]
name = "About"
weight = 1
url = "about/"

[[languages.en.menu.main]]
name = "Blog"
weight = 2
url = "posts/"
```

### 5. Footer
```toml
[params.footer]
    trademark = 2024
    bottomText = [
      "Your custom footer text",
    ]
```

### 6. Logo
```toml
[params.logo]
    logoMark = ">"
    logoText = "Your Logo Text"
    logoHomeLink = "/"
```

---

## Overriding Theme Templates

To customize theme templates without modifying the theme:

1. Find the template in `themes/<theme>/layouts/`
2. Copy it to `layouts/` (same path structure)
3. Modify your copy

### Example: Custom Footer

```bash
# Copy from theme
cp themes/coder/layouts/partials/footer.html layouts/partials/footer.html
```

Edit `layouts/partials/footer.html`:
```html
<footer class="footer">
    <section class="container">
      © {{ now.Year }} {{ .Site.Params.author }}
      <!-- Add your custom content -->
      <p>Custom footer content here!</p>
    </section>
</footer>
```

### Example: Custom Blog List

```html
<!-- layouts/blog/list.html -->
{{ define "main" }}
<main>
  <header>
    <h1>{{ .Title }}</h1>
  </header>
  
  {{ .Content }}
  
  <div class="posts-grid">
    {{ range .Pages }}
    <article class="post">
      <h2><a href="{{ .Permalink }}">{{ .Title }}</a></h2>
      <time>{{ .Date.Format "January 2, 2006" }}</time>
      <p>{{ .Summary }}</p>
    </article>
    {{ end }}
  </div>
</main>
{{ end }}
```

---

## Custom Shortcodes

Create reusable components in `layouts/shortcodes/`:

### Email Shortcode

```html
<!-- layouts/shortcodes/email.html -->
<a href="mailto:{{ .Site.Params.email }}">{{ .Site.Params.email }}</a>
```

Usage: `{{</* email */>}}`

### Alert/Notice Shortcode

```html
<!-- layouts/shortcodes/alert.html -->
<div class="alert alert-{{ .Get 0 | default "info" }}">
    {{ .Inner | markdownify }}
</div>
```

Usage:
```markdown
{{</* alert "warning" */>}}
This is a warning message!
{{</* /alert */>}}
```

### YouTube with Privacy

```html
<!-- layouts/shortcodes/youtube-privacy.html -->
<div class="video-container">
    <iframe 
        src="https://www.youtube-nocookie.com/embed/{{ .Get 0 }}" 
        frameborder="0" 
        allowfullscreen>
    </iframe>
</div>
```

---

## Adding Custom CSS/JS

### Method 1: Theme Parameters

```toml
[params]
customCSS = ["css/custom.css"]
customJS = ["js/custom.js"]
```

Create files in `static/css/` and `static/js/`.

### Method 2: Asset Pipeline

Create in `assets/` for processing:

```
assets/
├── scss/
│   └── custom.scss
└── js/
    └── custom.js
```

Reference in templates:
```go
{{ $style := resources.Get "scss/custom.scss" | toCSS | minify }}
<link rel="stylesheet" href="{{ $style.Permalink }}">
```

---

# Part 4: Deployment

## GitHub Pages Deployment

### Method 1: GitHub Actions (Recommended)

1. **Create workflow file** `.github/workflows/hugo.yml`:

```yaml
name: Deploy Hugo site to Pages

on:
  push:
    branches: ["main"]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

defaults:
  run:
    shell: bash

jobs:
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.128.0
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb

      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
          fetch-depth: 0

      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v4

      - name: Build with Hugo
        env:
          HUGO_ENVIRONMENT: production
          HUGO_ENV: production
        run: |
          hugo \
            --gc \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

2. **Enable GitHub Pages**:
   - Go to repo Settings → Pages
   - Source: GitHub Actions

### Method 2: Deploy from Branch

1. Build locally and push `public/` to `gh-pages` branch:

```bash
# Build
hugo --minify

# Deploy to gh-pages
cd public
git init
git add .
git commit -m "Deploy"
git push -f git@github.com:USERNAME/REPO.git main:gh-pages
```

2. Configure in GitHub:
   - Settings → Pages → Branch: `gh-pages`

---

## Using Custom Domains

### Step 1: Configure Hugo

In `hugo.toml`:
```toml
baseURL = "https://yourdomain.com/"
```

### Step 2: Create CNAME File

Create `static/CNAME` (or just `CNAME` in root):
```
yourdomain.com
```

This file will be copied to `public/CNAME` during build.

### Step 3: Configure DNS

#### For Apex Domain (yourdomain.com):

Add these A records pointing to GitHub:
```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

#### For Subdomain (blog.yourdomain.com):

Add a CNAME record:
```
blog  CNAME  username.github.io.
```

### Step 4: Configure GitHub

1. Go to Settings → Pages
2. Under "Custom domain", enter your domain
3. Check "Enforce HTTPS" (may take a few minutes)

### Step 5: Verify

```bash
# Check DNS propagation
dig yourdomain.com +short

# Check HTTPS certificate
curl -I https://yourdomain.com
```

---

## Summary Checklist

### Quick Start (Clone & Customize)

1. **Clone this repository**
   ```bash
   git clone https://github.com/YOUR-USERNAME/presentation_website.git
   cd presentation_website
   ```

2. **Install Hugo** (see [Installation](#installation))

3. **Edit `hugo.toml`** - Change:
   - [ ] `baseURL` - Your domain
   - [ ] `title` - Your site title
   - [ ] `[params]` section - Your info
   - [ ] `[[params.social]]` - Your social links
   - [ ] `[languages.en.menu.main]` - Your menu items

4. **Replace content**:
   - [ ] `content/about.md` - Your about page
   - [ ] `content/contact.md` - Your contact info
   - [ ] `content/authors/` - Your author profile
   - [ ] Delete demo posts, add your own

5. **Update CNAME** - Your domain

6. **Test locally**:
   ```bash
   hugo server -D
   ```

7. **Deploy**:
   ```bash
   git add .
   git commit -m "Initial customization"
   git push
   ```

### Common Commands

```bash
# Start dev server with drafts
hugo server -D

# Create new post
hugo new posts/my-post.md

# Build for production
hugo --minify

# Check Hugo version
hugo version

# Update theme (if using submodule)
git submodule update --remote
```

---

## Repository Files Reference

Here's a map of the key files in this repository and what they do:

| File | Purpose |
|------|---------|
| `hugo.toml` | Main configuration - **edit this first!** |
| `content/about.md` | About page content |
| `content/contact.md` | Contact page content |
| `content/posts/*.md` | Blog posts |
| `content/authors/*/` | Author profiles |
| `archetypes/posts.md` | Template for new posts |
| `layouts/partials/footer.html` | Custom footer override |
| `layouts/shortcodes/email.html` | Email shortcode |
| `static/CNAME` | Custom domain for GitHub Pages |
| `.github/workflows/hugo.yml` | Auto-deploy workflow |

---

## Common Pitfalls & Best Practices

### ❌ Avoid These

1. **JavaScript redirects in content**
   ```html
   <!-- BAD: Don't do this -->
   <script>window.location.href = "/about";</script>
   ```
   Instead, use Hugo's `aliases` in front matter:
   ```yaml
   aliases:
     - /old-url/
   ```

2. **Editing theme files directly**
   - Copy to `layouts/` instead to preserve upgrades

3. **Hardcoding URLs**
   ```html
   <!-- BAD -->
   <a href="http://example.com/about">
   <!-- GOOD -->
   <a href="{{ "/about/" | relURL }}">
   ```

4. **CNAME in wrong location**
   - Place in `static/CNAME` so it's copied to `public/`

### ✅ Best Practices

1. **Use `site` instead of `.Site`** (modern Hugo)
   ```go
   {{ site.Title }}  <!-- Preferred -->
   {{ .Site.Title }} <!-- Also works -->
   ```

2. **Sort posts by date**
   ```go
   {{ range .Pages.ByDate.Reverse }}
   ```

3. **Always use descriptions**
   - Better SEO and social sharing

4. **Keep drafts as drafts**
   - Use `draft = true` until ready
   - Preview with `hugo server -D`

5. **Organize static assets**
   ```
   static/
   ├── images/
   │   ├── posts/
   │   └── authors/
   ├── css/
   └── js/
   ```

6. **Use meaningful taxonomies**
   - Tags: specific topics (e.g., "python", "tutorial")
   - Categories: broad topics (e.g., "programming", "career")
   - Series: related posts (e.g., "Hugo Deep Dive")

---

## Additional Resources

- [Hugo Documentation](https://gohugo.io/documentation/)
- [Hugo Themes](https://themes.gohugo.io/)
- [Hugo Discourse](https://discourse.gohugo.io/)
- [Coder Theme Docs](https://github.com/luizdepra/hugo-coder/wiki)
- [GitHub Pages Docs](https://docs.github.com/en/pages)

---

## Troubleshooting

### Site not updating after deploy?
- Clear browser cache (Ctrl+Shift+R)
- Wait a few minutes for GitHub to propagate

### CSS/styles broken?
- Check `baseURL` matches your actual domain
- Ensure HTTPS if using custom domain

### Posts not showing?
- Check `draft = false` in front matter
- Use `hugo server -D` to see drafts

### Theme not loading?
- Verify theme name in `hugo.toml` matches folder name
- Check submodule is initialized: `git submodule update --init`

### Build failing on GitHub Actions?
- Check Hugo version in `.github/workflows/hugo.yml`
- Verify theme submodule is committed

### Taxonomy pages empty?
- Ensure taxonomy names match in `hugo.toml` and front matter
- Check for typos (case-sensitive!)

---

**Happy Blogging! 🚀**
- Check submodule is initialized: `git submodule update --init`

---

**Happy Blogging! 🚀**
