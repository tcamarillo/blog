# Setup Commands Reference

> **📖 See [TUTORIAL.md](TUTORIAL.md) for detailed setup instructions**

This file contains quick reference commands for different platforms. For complete setup guides with explanations, visit the main tutorial.

## Platform-Specific Installation

### macOS
See [Installation - macOS](TUTORIAL.md#macos-with-homebrew) in TUTORIAL.md

### Linux / WSL2 / Ubuntu
See [Installation - Linux](TUTORIAL.md#linux-wsl2--debian--ubuntu) in TUTORIAL.md

### Windows
See [Installation - Windows](TUTORIAL.md#windows) in TUTORIAL.md

## Quick Project Setup

### Create New Site (All Platforms)
```bash
hugo new site my-blog
cd my-blog
git init
git submodule add https://github.com/luizdepra/hugo-coder themes/coder
echo 'theme = "coder"' >> hugo.toml
hugo new posts/my-first-post.md
hugo server -D
```

### Clone Existing Repository
```bash
git clone https://github.com/YOUR-USERNAME/YOUR-REPO.git
cd YOUR-REPO
git submodule update --init --recursive
hugo server -D
```

## Common Commands

```bash
# Start development server
hugo server -D

# Create new post
hugo new posts/my-post.md

# Build for production
hugo --minify

# Check version
hugo version

# Update theme
git submodule update --remote
```

**For more details, see [TUTORIAL.md](TUTORIAL.md)**

