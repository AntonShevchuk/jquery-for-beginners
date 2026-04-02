# Multilingual Book (RU / UK / EN) Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Restructure the jquery-book repository to support three languages (Russian, Ukrainian, English) using GitBook's native multilingual feature.

**Architecture:** GitBook multilingual books use a `LANGS.md` file at the root that lists available languages. Each language lives in its own subdirectory (`ru/`, `uk/`, `en/`) with identical file structure. Shared assets (images, SVGs) stay at root level in `.gitbook/assets/`. Each language directory has its own `README.md`, `SUMMARY.md`, and `book.json`. The root `book.json` contains shared config (plugins, styles), while per-language `book.json` overrides `language` and `title`.

**Tech Stack:** GitBook, Markdown

---

## File Structure

After migration, the repo will look like:

```
jquery-book/
├── LANGS.md                          # NEW — language selector
├── book.json                         # shared config (plugins, styles)
├── .gitbook/assets/                  # shared images/SVGs (unchanged)
├── cover.jpg                         # shared
├── cover_small.jpg                   # shared
├── favicon.ico                       # shared
├── CLAUDE.md                         # stays at root
├── LICENSE.txt                       # stays at root
├── .gitignore                        # stays at root
│
├── ru/                               # MOVED — all current .md files
│   ├── README.md
│   ├── SUMMARY.md
│   ├── book.json                     # NEW — {"language": "ru", "title": "jQuery для начинающих"}
│   ├── about.md
│   ├── author.md
│   ├── license.md
│   ├── thanks.md
│   ├── changelog.md
│   ├── 0_html_css_javascript/
│   ├── 10_go_on/
│   ├── 20_attributes_and_properties/
│   ├── 30_events/
│   ├── 40_animation/
│   ├── 50_document_manipulation/
│   ├── 60_ajax/
│   ├── 70_deferred_and_callbacks/
│   ├── 80_forms/
│   ├── 90_create_plugin/
│   ├── 100_last_chapter/
│   └── appendix/
│
├── uk/                               # NEW — Ukrainian translation (copy of ru/)
│   ├── README.md
│   ├── SUMMARY.md
│   ├── book.json                     # {"language": "uk", "title": "jQuery для початківців"}
│   ├── ... (same structure as ru/)
│
└── en/                               # NEW — English translation (copy of ru/)
    ├── README.md
    ├── SUMMARY.md
    ├── book.json                     # {"language": "en", "title": "jQuery for Beginners"}
    ├── ... (same structure as ru/)
```

### Key decisions

- **Assets stay shared.** Images in `.gitbook/assets/` are language-neutral (screenshots, diagrams). All languages reference them via `../.gitbook/assets/` (one level up from language dir). Text on SVG diagrams is minimal and in English already, so no duplication needed.
- **Per-language `book.json`** overrides only `language` and `title`. Plugins and styles are inherited from root `book.json`.
- **`cover.jpg` and `cover_small.jpg`** stay at root (shared cover). If you later want per-language covers, place them inside each language directory.
- **`SUMMARY.md`** is per-language — titles get translated, file paths stay identical.
- **Code examples are not translated** — jQuery code is the same in all languages. Only surrounding text, comments, and headings change.

---

## Task 1: Restructure — move Russian content into `ru/` subdirectory

**Files:**
- Create: `LANGS.md`
- Create: `ru/book.json`
- Modify: `book.json` (remove `language` and `title`, keep shared config)
- Move: all `.md` files and chapter directories into `ru/`
- Modify: `ru/SUMMARY.md` (no path changes needed — paths are relative to `ru/`)
- Modify: all `ru/**/*.md` that reference `.gitbook/assets/` — prefix with `../`

- [ ] **Step 1: Create the `LANGS.md` file at root**

```markdown
* [Русский](ru/)
* [Українська](uk/)
* [English](en/)
```

- [ ] **Step 2: Move all content files into `ru/`**

```bash
mkdir -p ru

# move chapter directories
git mv 0_html_css_javascript ru/
git mv 10_go_on ru/
git mv 20_attributes_and_properties ru/
git mv 30_events ru/
git mv 40_animation ru/
git mv 50_document_manipulation ru/
git mv 60_ajax ru/
git mv 70_deferred_and_callbacks ru/
git mv 80_forms ru/
git mv 90_create_plugin ru/
git mv 100_last_chapter ru/
git mv appendix ru/

# move root-level content .md files
git mv README.md ru/
git mv SUMMARY.md ru/
git mv about.md ru/
git mv author.md ru/
git mv license.md ru/
git mv thanks.md ru/
git mv changelog.md ru/
```

- [ ] **Step 3: Create per-language `ru/book.json`**

```json
{
  "language": "ru",
  "title": "jQuery для начинающих"
}
```

- [ ] **Step 4: Update root `book.json` — remove language-specific fields**

```json
{
  "author": "Антон Шевчук",
  "isbn": "978-617-7150-93-9",
  "plugins": [
    "ga",
    "exercises",
    "jquery-book@git+https://github.com/AntonShevchuk/gitbook-plugin-jquery-book.git#0.1.3"
  ],
  "pluginsConfig": {
    "ga": {
      "token": "UA-114378149-1"
    }
  },
  "styles": {
    "website": "./assets/css/website.css",
    "pdf": "./assets/css/pdf.css"
  }
}
```

- [ ] **Step 5: Fix asset paths in all `ru/` markdown files**

All references to `.gitbook/assets/` must become `../.gitbook/assets/` since content is now one level deeper.

```bash
# find and replace in all .md files under ru/
find ru/ -name '*.md' -exec sed -i '' 's|\.gitbook/assets/|../.gitbook/assets/|g' {} +
```

Verify with:
```bash
grep -r '\.gitbook/assets' ru/ | head -20
# all paths should start with ../.gitbook/assets/
grep -r '[^.]\.gitbook/assets' ru/
# should return nothing (no paths without the ../ prefix)
```

- [ ] **Step 6: Create a root `README.md` for the repo**

```markdown
# jQuery for Beginners

A multilingual textbook about jQuery.

Available in: [Русский](ru/) | [Українська](uk/) | [English](en/)
```

- [ ] **Step 7: Verify and commit**

```bash
# check nothing is broken
grep -rn 'gitbook/assets' ru/ | grep -v '\.\./\.gitbook' | head
# should be empty

git add -A
git status
git commit -m "Restructure: move Russian content into ru/ subdirectory for multilingual support"
```

---

## Task 2: Create Ukrainian translation scaffold

**Files:**
- Create: `uk/` — full copy of `ru/` directory structure
- Create: `uk/book.json`
- Modify: `uk/SUMMARY.md` — translate TOC titles
- Modify: `uk/README.md` — translate title

- [ ] **Step 1: Copy `ru/` to `uk/`**

```bash
cp -r ru/ uk/
```

- [ ] **Step 2: Create `uk/book.json`**

```json
{
  "language": "uk",
  "title": "jQuery для початківців"
}
```

- [ ] **Step 3: Translate `uk/SUMMARY.md` — table of contents**

Translate all Ukrainian TOC entries. File paths stay the same. Example:

```markdown
# Зміст

* [jQuery для початківців](README.md)
* [Про автора](author.md)
* [Про книгу](about.md)
* [Умови розповсюдження](license.md)
* [0% Про HTML, CSS та JavaScript](0_html_css_javascript/README.md)
  * [Семантична верстка](0_html_css_javascript/semantic-markup.md)
  * [Валідний HTML](0_html_css_javascript/markup-validation.md)
  * [CSS-правила та селектори](0_html_css_javascript/css-selectors-and-rules.md)
  * [CSS. Занурення](0_html_css_javascript/advanced-css.md)
  * [Розділяй та володарюй](0_html_css_javascript/divide-et-impera.md)
  * [Трохи про JavaScript](0_html_css_javascript/javascript-introduction.md)
* [10% Підключаємо, знаходимо, готуємо](10_go_on/README.md)
  * [Будь готовий!](10_go_on/be-ready.md)
  * [Селектори](10_go_on/selectors.md)
  * [Селектори та Sizzle](10_go_on/sizzle.md)
  * [Оптимізація](10_go_on/optimization.md)
* [20% Атрибути та властивості елементів](20_attributes_and_properties/README.md)
  * [CSS стилі](20_attributes_and_properties/css.md)
  * [CSS класи](20_attributes_and_properties/classes.md)
  * [Атрибути](20_attributes_and_properties/attributes.md)
  * [Властивості](20_attributes_and_properties/properties.md)
  * [Розміри](20_attributes_and_properties/dimensions.md)
  * [Позиція](20_attributes_and_properties/position.md)
  * [Data-атрибути](20_attributes_and_properties/data-attributes.md)
* [30% Події](30_events/README.md)
  * [Робота з подіями](30_events/handlers.md)
  * [Спливання та обробка подій](30_events/bubbling.md)
  * [Простір імен](30_events/namespaces.md)
  * [«Живі» події](30_events/live.md)
  * [Touch-події](30_events/touch.md)
  * [Оптимізація](30_events/optimization.md)
  * [Доповнення](30_events/inside.md)
* [40% Анімація](40_animation/README.md)
  * [Easing функції](40_animation/easing.md)
  * [Прогрес](40_animation/progress.md)
  * [Крок за кроком](40_animation/step-by-step.md)
  * [В чергу…©](40_animation/queue.md)
  * [Вимкнення](40_animation/off.md)
* [50% Маніпуляції з DOM](50_document_manipulation/README.md)
  * [Створення елементів](50_document_manipulation/create.md)
  * [Маніпуляції над елементами](50_document_manipulation/manipulation.md)
  * [Керування вмістом](50_document_manipulation/html-and-text.md)
  * [Клонування та видалення](50_document_manipulation/clone-and-other.md)
  * [Смуга прокрутки](50_document_manipulation/scroll.md)
* [60% AJAX](60_ajax/README.md)
  * [Метод load()](60_ajax/metod-load.md)
  * [Метод ajax()](60_ajax/ajax-method.md)
  * [Обробники подій](60_ajax/ajax-handlers.md)
  * [Лікуємо JavaScript-залежність](60_ajax/javascript-dependency.md)
  * [Сила JSONP](60_ajax/jsonp.md)
  * [Прокачуємо AJAX](60_ajax/ajax-tuning.md)
* [70% Асинхронність](70_deferred_and_callbacks/README.md)
  * [Deferred](70_deferred_and_callbacks/deferred.md)
  * [When](70_deferred_and_callbacks/when.md)
  * [Callbacks](70_deferred_and_callbacks/callbacks.md)
* [80% Робота з формами](80_forms/README.md)
  * [Події](80_forms/events.md)
  * [Маніпуляція над елементами](80_forms/manipulation.md)
* [90% Пишемо свій плагін](90_create_plugin/README.md)
  * [Перший плагін](90_create_plugin/jquery_plugin.md)
  * [Data реєстр](90_create_plugin/data.md)
  * [Animate](90_create_plugin/animate.md)
  * [Easing](90_create_plugin/easing.md)
  * [Sizzle](90_create_plugin/sizzle.md)
* [100% Остання глава](100_last_chapter/README.md)
* [Доповнення](appendix/README.md)
  * [jQuery UI](appendix/jquery_ui/README.md)
    * [Пишемо свій widget](appendix/jquery_ui/jquery_ui.md)
  * [jQWidgets](appendix/jqwidgets.md)
  * [Ще плагіни](appendix/plugins.md)
  * [Оновлення на версію 2.х](appendix/update-to-version-2.md)
  * [Оновлення на версію 3.х](appendix/update-to-version-3.md)
  * [Оновлення на версію 4.х](appendix/update-to-version-4.md)
* [Історія змін](changelog.md)
* [Подяки](thanks.md)
```

- [ ] **Step 4: Translate `uk/README.md`**

```markdown
---
description: Підручник зрозумілою мовою
layout:
  title:
    visible: false
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# jQuery для початківців

<figure><img src="../.gitbook/assets/cover.jpg" alt=""><figcaption></figcaption></figure>
```

- [ ] **Step 5: Commit scaffold**

```bash
git add uk/
git commit -m "Add Ukrainian translation scaffold (copy of ru/ with translated TOC)"
```

- [ ] **Step 6: Translate chapter by chapter**

The remaining translation work is file-by-file. Translate in this order (highest-impact chapters first):

1. `uk/about.md`, `uk/author.md`, `uk/license.md`, `uk/thanks.md` — short meta pages
2. `uk/0_html_css_javascript/*.md` — chapter 0 (6 files)
3. `uk/10_go_on/*.md` — chapter 10 (4 files)
4. `uk/20_attributes_and_properties/*.md` — chapter 20 (7 files)
5. `uk/30_events/*.md` — chapter 30 (7 files)
6. `uk/40_animation/*.md` — chapter 40 (5 files)
7. `uk/50_document_manipulation/*.md` — chapter 50 (5 files)
8. `uk/60_ajax/*.md` — chapter 60 (6 files)
9. `uk/70_deferred_and_callbacks/*.md` — chapter 70 (3 files)
10. `uk/80_forms/*.md` — chapter 80 (2 files)
11. `uk/90_create_plugin/*.md` — chapter 90 (5 files)
12. `uk/100_last_chapter/README.md` — chapter 100 (1 file)
13. `uk/appendix/**/*.md` — appendix (6 files)
14. `uk/changelog.md` — changelog

Commit after each chapter group. Keep all code examples, `{% hint %}` blocks, `{% embed %}` blocks, and links unchanged — only translate the surrounding Russian text and headings.

---

## Task 3: Create English translation scaffold

**Files:**
- Create: `en/` — full copy of `ru/` directory structure
- Create: `en/book.json`
- Modify: `en/SUMMARY.md` — translate TOC titles
- Modify: `en/README.md` — translate title

- [ ] **Step 1: Copy `ru/` to `en/`**

```bash
cp -r ru/ en/
```

- [ ] **Step 2: Create `en/book.json`**

```json
{
  "language": "en",
  "title": "jQuery for Beginners"
}
```

- [ ] **Step 3: Translate `en/SUMMARY.md` — table of contents**

```markdown
# Table of Contents

* [jQuery for Beginners](README.md)
* [About the Author](author.md)
* [About the Book](about.md)
* [License](license.md)
* [0% About HTML, CSS, and JavaScript](0_html_css_javascript/README.md)
  * [Semantic Markup](0_html_css_javascript/semantic-markup.md)
  * [Valid HTML](0_html_css_javascript/markup-validation.md)
  * [CSS Rules and Selectors](0_html_css_javascript/css-selectors-and-rules.md)
  * [CSS Deep Dive](0_html_css_javascript/advanced-css.md)
  * [Divide and Conquer](0_html_css_javascript/divide-et-impera.md)
  * [A Bit About JavaScript](0_html_css_javascript/javascript-introduction.md)
* [10% Getting Started](10_go_on/README.md)
  * [Be Ready!](10_go_on/be-ready.md)
  * [Selectors](10_go_on/selectors.md)
  * [Selectors and Sizzle](10_go_on/sizzle.md)
  * [Optimization](10_go_on/optimization.md)
* [20% Attributes and Properties](20_attributes_and_properties/README.md)
  * [CSS Styles](20_attributes_and_properties/css.md)
  * [CSS Classes](20_attributes_and_properties/classes.md)
  * [Attributes](20_attributes_and_properties/attributes.md)
  * [Properties](20_attributes_and_properties/properties.md)
  * [Dimensions](20_attributes_and_properties/dimensions.md)
  * [Position](20_attributes_and_properties/position.md)
  * [Data Attributes](20_attributes_and_properties/data-attributes.md)
* [30% Events](30_events/README.md)
  * [Working with Events](30_events/handlers.md)
  * [Event Bubbling](30_events/bubbling.md)
  * [Namespaces](30_events/namespaces.md)
  * [Live Events](30_events/live.md)
  * [Touch Events](30_events/touch.md)
  * [Optimization](30_events/optimization.md)
  * [Under the Hood](30_events/inside.md)
* [40% Animation](40_animation/README.md)
  * [Easing Functions](40_animation/easing.md)
  * [Progress](40_animation/progress.md)
  * [Step by Step](40_animation/step-by-step.md)
  * [Queue](40_animation/queue.md)
  * [Disabling](40_animation/off.md)
* [50% DOM Manipulation](50_document_manipulation/README.md)
  * [Creating Elements](50_document_manipulation/create.md)
  * [Manipulating Elements](50_document_manipulation/manipulation.md)
  * [Managing Content](50_document_manipulation/html-and-text.md)
  * [Cloning and Removing](50_document_manipulation/clone-and-other.md)
  * [Scrollbar](50_document_manipulation/scroll.md)
* [60% AJAX](60_ajax/README.md)
  * [The load() Method](60_ajax/metod-load.md)
  * [The ajax() Method](60_ajax/ajax-method.md)
  * [Event Handlers](60_ajax/ajax-handlers.md)
  * [Curing JavaScript Addiction](60_ajax/javascript-dependency.md)
  * [The Power of JSONP](60_ajax/jsonp.md)
  * [Leveling Up AJAX](60_ajax/ajax-tuning.md)
* [70% Asynchronous Patterns](70_deferred_and_callbacks/README.md)
  * [Deferred](70_deferred_and_callbacks/deferred.md)
  * [When](70_deferred_and_callbacks/when.md)
  * [Callbacks](70_deferred_and_callbacks/callbacks.md)
* [80% Working with Forms](80_forms/README.md)
  * [Events](80_forms/events.md)
  * [Manipulating Elements](80_forms/manipulation.md)
* [90% Writing Your Own Plugin](90_create_plugin/README.md)
  * [Your First Plugin](90_create_plugin/jquery_plugin.md)
  * [Data Registry](90_create_plugin/data.md)
  * [Animate](90_create_plugin/animate.md)
  * [Easing](90_create_plugin/easing.md)
  * [Sizzle](90_create_plugin/sizzle.md)
* [100% The Final Chapter](100_last_chapter/README.md)
* [Appendix](appendix/README.md)
  * [jQuery UI](appendix/jquery_ui/README.md)
    * [Writing Your Own Widget](appendix/jquery_ui/jquery_ui.md)
  * [jQWidgets](appendix/jqwidgets.md)
  * [More Plugins](appendix/plugins.md)
  * [Upgrading to Version 2.x](appendix/update-to-version-2.md)
  * [Upgrading to Version 3.x](appendix/update-to-version-3.md)
  * [Upgrading to Version 4.x](appendix/update-to-version-4.md)
* [Changelog](changelog.md)
* [Acknowledgments](thanks.md)
```

- [ ] **Step 4: Translate `en/README.md`**

```markdown
---
description: A textbook in plain language
layout:
  title:
    visible: false
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# jQuery for Beginners

<figure><img src="../.gitbook/assets/cover.jpg" alt=""><figcaption></figcaption></figure>
```

- [ ] **Step 5: Commit scaffold**

```bash
git add en/
git commit -m "Add English translation scaffold (copy of ru/ with translated TOC)"
```

- [ ] **Step 6: Translate chapter by chapter**

Same order as Ukrainian translation (Task 2, Step 6). Translate Russian text to English, keep code examples and GitBook markup unchanged. Commit after each chapter group.

---

## Task 4: Update CLAUDE.md and verify

**Files:**
- Modify: `CLAUDE.md`

- [ ] **Step 1: Update `CLAUDE.md` to reflect new structure**

Add multilingual section:

```markdown
## Multilingual Structure

The book is available in three languages, each in its own subdirectory:
- `ru/` — Russian (original)
- `uk/` — Ukrainian
- `en/` — English

`LANGS.md` at root controls the language selector. Each language has its own `SUMMARY.md` and `book.json`. Shared assets live in `.gitbook/assets/` — reference them from language dirs as `../.gitbook/assets/`.

When editing content, keep file structure identical across all languages. Code examples are the same — only translate surrounding text.
```

- [ ] **Step 2: Verify GitBook structure**

```bash
# verify LANGS.md exists and is correct
cat LANGS.md

# verify each language has required files
for lang in ru uk en; do
  echo "=== $lang ==="
  test -f "$lang/README.md" && echo "README.md OK" || echo "README.md MISSING"
  test -f "$lang/SUMMARY.md" && echo "SUMMARY.md OK" || echo "SUMMARY.md MISSING"
  test -f "$lang/book.json" && echo "book.json OK" || echo "book.json MISSING"
done

# verify no broken asset paths
grep -rn '\.gitbook/assets' ru/ uk/ en/ | grep -v '\.\./\.gitbook' | head
# should be empty
```

- [ ] **Step 3: Commit**

```bash
git add CLAUDE.md
git commit -m "Update CLAUDE.md for multilingual structure"
```

---

## Translation Workflow (ongoing)

For each file being translated (applies to both UK and EN):

1. Open the Russian source file `ru/path/to/file.md`
2. Open the target file `uk/path/to/file.md` (or `en/`)
3. Translate:
   - Headings and paragraph text
   - Comments inside code blocks (only if they're in Russian)
   - `{% hint %}` block content
   - Table descriptions
4. Do NOT translate:
   - Code (jQuery/JS/HTML/CSS)
   - URLs and file paths
   - `{% embed %}` URLs
   - Technical terms that are standard in English (DOM, AJAX, callback, etc.)
5. Commit after each chapter: `"Translate chapter N to Ukrainian/English"`
