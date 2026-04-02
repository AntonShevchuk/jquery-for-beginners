# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is **"jQuery для начинающих"** (jQuery for Beginners) — a Russian-language textbook about jQuery by Anton Shevchuk. Published via GitBook with ISBN 978-617-7150-93-9.

## Technology

- **Platform**: GitBook (content in Markdown)
- **Language**: Russian (all content is in Russian)
- **Plugins**: `ga` (Google Analytics), `exercises`, custom `jquery-book` plugin
- **Custom styles**: `assets/css/website.css` (web), `assets/css/pdf.css` (PDF export)

## Content Structure

Chapters are organized by "progress percentage" (0% through 100%), each in its own directory:

- `0_html_css_javascript/` — Prerequisites: HTML, CSS, JavaScript basics
- `10_go_on/` through `90_create_plugin/` — Core jQuery topics (selectors, attributes, events, animation, DOM manipulation, AJAX, async patterns, forms, plugin development)
- `100_last_chapter.md` — Final chapter (single file, not a directory)
- `appendix/` — jQuery UI, jQWidgets, plugins, version migration guides (2.x, 3.x, 4.x)

Each chapter directory has a `README.md` as its main entry point with subtopics in separate `.md` files.

**Key files:**
- `SUMMARY.md` — Table of contents (defines chapter order and navigation)
- `book.json` — GitBook configuration (plugins, styles, metadata)
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
