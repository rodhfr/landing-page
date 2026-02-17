# landing-page

Personal portfolio and blog built with [Hugo](https://gohugo.io/) using the [Gokarna](https://github.com/gokarna-theme/gokarna-hugo) theme.

Live at [dev.rodhfr.space](https://dev.rodhfr.space/)

## Prerequisites

- [Hugo Extended](https://gohugo.io/installation/) (v0.150.0+)
- Git

## Getting Started

Clone the repo with the theme submodule:

```bash
git clone --recurse-submodules https://github.com/rodhfr/landing-page.git
cd landing-page
```

If you already cloned without `--recurse-submodules`, initialize the theme:

```bash
git submodule update --init --recursive
```

## Local Development

```bash
hugo server -D
```

The site will be available at `http://localhost:1313/`.

## Build

```bash
hugo --gc --minify
```

The output is generated in the `public/` directory.

## Deployment

The `build.sh` script handles the full build pipeline for Cloudflare Workers, installing all required dependencies (Dart Sass, Go, Hugo, Node.js) automatically.

## Project Structure

```
.
├── content/          # Markdown content (pages and posts)
├── static/           # Static assets (images, SVGs, files)
├── themes/gokarna/   # Hugo theme (git submodule)
├── hugo.toml         # Site configuration
└── build.sh          # Cloudflare build script
```
