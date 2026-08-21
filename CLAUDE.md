# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A [Quarto](https://quarto.org) website/blog ("causal musings") about causal inference. Content is authored in `.qmd` (Quarto Markdown) files, which can embed executable Python code via Jupyter. Python dependencies are managed with `uv`.

## Commands

```bash
# Render the full site to _site/
uv run quarto render

# Live preview with auto-reload (serves on localhost, watches for file changes)
uv run quarto preview

# Sync/install Python deps from pyproject.toml + uv.lock into .venv
uv sync

# Add a new Python dependency
uv add <package>
```

There is no separate lint/test suite — correctness is verified by rendering the site and checking output in `_site/` or via `quarto preview`. Python code chunks execute with the `.venv` created by `uv` (Python version pinned in `.python-version`).

## Architecture

- `_quarto.yml` — top-level site config: project type (`website`), navbar, theme (`cosmo` + `brand`), and `styles.css` inclusion. Site metadata (title, `site-url`) lives here.
- `index.qmd` — the blog's landing/listing page. Uses Quarto's `listing` mechanism to auto-generate the post feed from the `posts/` directory (sorted by date desc, with an RSS `feed: true`).
- `about.qmd` — the About page (uses the `jolla` about-page template).
- `posts/` — each blog post is a subdirectory (e.g. `posts/welcome/`, `posts/post-with-code/`) containing an `index.qmd` plus any local assets (images). This is the standard Quarto blog convention: one folder per post, discovered automatically by the `listing` in `index.qmd`.
- `posts/_metadata.yml` — shared front-matter defaults applied to every post in `posts/`, notably `freeze: true` (caches/reuses computational output across renders instead of re-executing code every time — see [Quarto's freeze docs](https://quarto.org/docs/projects/code-execution.html#freeze)) and `title-block-banner: true`.
- `_site/` and `.quarto/` — generated render output and Quarto's internal cache/metadata. Do not hand-edit; regenerate via `quarto render`.
- `styles.css` — site-wide custom CSS layered on top of the `cosmo`/`brand` theme.

## Adding a new post

Create a new directory under `posts/` with an `index.qmd` containing YAML front matter (`title`, `author`, `date`, `categories`, optionally `image`). It will automatically appear in the `index.qmd` listing — no manual registration needed.

## Notes

- Navbar links (GitHub, Bluesky) in `_quarto.yml` and social links in `about.qmd` are placeholders and need real URLs.

## Python

Use the Python version pinned by uv.

The project environment is `.venv`.

Do not modify `.venv` directly

## Git

The `main` branch contains source files

Do not commit generated `_ste/` content to `main`

Do not puch, force-push, reset, or delete branches unless explicitly asked.

## Publishing

The site is hosted at rcvalenzuela.github.io using Github Pages

Render and preview locally before publishing