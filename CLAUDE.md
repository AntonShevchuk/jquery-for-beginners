# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is **"jQuery для начинающих"** (jQuery for Beginners) — a multilingual textbook about jQuery by Anton Shevchuk. Published via GitBook with ISBN 978-617-7150-93-9.

## Multilingual Structure

The book is available in three languages, each in its own subdirectory:
- `ru/` — Russian (original)
- `uk/` — Ukrainian
- `en/` — English

`LANGS.md` at root controls the GitBook language selector. Each language has its own `SUMMARY.md` and `book.json`. Shared assets live in `.gitbook/assets/` — reference them from language dirs as `../.gitbook/assets/`.

When editing content, keep file structure identical across all languages. Code examples are the same — only translate surrounding text.

## Technology

- **Platform**: GitBook (content in Markdown)
- **Plugins**: `ga` (Google Analytics), `exercises`, custom `jquery-book` plugin
- **Custom styles**: `assets/css/website.css` (web), `assets/css/pdf.css` (PDF export)
- **Root `book.json`**: shared config (plugins, styles)
- **Per-language `book.json`**: overrides `language` and `title`

## Content Structure

Chapters are organized by "progress percentage" (0% through 100%), each in its own directory:

- `0_html_css_javascript/` — Prerequisites: HTML, CSS, JavaScript basics
- `10_go_on/` through `90_create_plugin/` — Core jQuery topics (selectors, attributes, events, animation, DOM manipulation, AJAX, async patterns, forms, plugin development)
- `100_last_chapter/` — Final chapter
- `appendix/` — jQuery UI, jQWidgets, plugins, version migration guides (2.x, 3.x, 4.x)

Each chapter directory has a `README.md` as its main entry point with subtopics in separate `.md` files.

**Key files (per language):**
- `SUMMARY.md` — Table of contents (defines chapter order and navigation)
- `book.json` — Per-language config (language, title)
- `about.md` — Book description and formatting conventions
- `author.md`, `license.md`, `thanks.md`, `changelog.md` — Metadata pages

## Content Conventions

- **Bold** (`**text**`) marks critical concepts readers must remember
- Inline code uses monospace for HTML/JS snippets
- `{% hint style="info|warning|danger" %}` blocks for callouts (GitBook syntax)
- `>` blockquotes for side notes
- Code examples are hosted externally at https://github.com/AntonShevchuk/jquery-for-beginners
- Diagrams/images are stored in `.gitbook/assets/`

## Working with This Project

There is no build system or package.json — content is authored as Markdown and published through the GitBook platform. Edits are made directly to `.md` files.
