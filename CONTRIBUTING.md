# Contribute to the 86d documentation

Thank you for your interest in improving the 86d docs. The full contribution guide for the project (framework, modules, and docs) lives at [86d.app/docs/resources/contributing](https://86d.app/docs/resources/contributing). This file covers the docs site specifically.

## How to contribute

### Option 1: edit on GitHub

1. Open the page you want to edit on [86d.app/docs](https://86d.app/docs).
2. Click the **Edit on GitHub** link.
3. Make your changes and submit a pull request.

### Option 2: local development

```bash
git clone https://github.com/86d-app/docs
cd docs
npm i -g mint
mint dev
```

The dev server runs at `http://localhost:3000` and hot-reloads on file changes.

### Checking links

```bash
mint broken-links
```

Run this before opening a PR. We treat broken internal links as a blocker.

## Commit messages

This repository uses [Conventional Commits](https://www.conventionalcommits.org/) with a **required scope**. Git hooks enforce the format on every commit.

```
type(scope): subject
```

**Types:** `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`

**Scopes:** `site`, `concepts`, `guides`, `cli`, `modules`, `resources`, `config`, `repo`

**Examples:**

```
docs(guides): add module installation steps
fix(concepts): correct storefront lifecycle diagram
chore(config): update mintlify navigation
```

Rules:

- Use imperative, lowercase subject (no trailing period)
- Keep the subject under 72 characters when possible (100 max)
- One logical change per commit

Run `bun install` after cloning to install commit hooks.

## Writing guidelines

- **Active voice.** "Run the command" not "the command should be run."
- **Address the reader.** Use "you" instead of "the user" or "one."
- **One idea per sentence.** Aim for 15 to 20 words per sentence.
- **Lead with the goal.** The first paragraph of every page should answer "what is this and why do I care."
- **Sentence case for headings.** "Module configuration" not "Module Configuration."
- **No em dashes.** Use a comma, a colon, a semicolon, parentheses, or a sentence break instead. We grep for them in CI.
- **Code formatting** for file paths, command names, environment variables, and code identifiers. Not for prose emphasis.
- **Show it working.** Include working examples next to every API or CLI reference.
- **Protect private context.** Publish no personal data, private roadmap sequence, provider negotiation, exploit detail, or unshipped commercial terms.
- **Link shared concepts.** Define a product rule once, then link to that page instead of copying it.

## What goes in this repo

- Documentation pages under `<section>/<page>.mdx`.
- Mintlify configuration (`docs.json`).
- Static assets that are unique to the docs site.

The Store Runtime, 86d.app, and docs live in separate repositories. The 86d.app repository contains 86d Console and the Control Plane implementation. Verify product behavior in source, but edit only this docs checkout for a docs-only pull request.

## Page templates

Each content type has a consistent shape:

- **Concept pages** (`concepts/`): start with a one-paragraph "what is this and why does it exist", then the conceptual model, then a short list of practical references.
- **How-to guides** (`guides/`): start with the goal, then numbered steps, then troubleshooting.
- **Reference pages** (`configuration/`, `cli/`, `modules/`): start with a one-paragraph summary, then a structured reference (tables, `<ParamField />`, code blocks).
- **Tutorial pages**: start with prerequisites, then numbered steps that build on each other, then a "what next" section.

If your edit straddles types, pick the closest fit and stick to its shape.

## Frontmatter

Every page must have YAML frontmatter with at least `title` and `description`. Mintlify uses both fields for navigation and search.

```yaml
---
title: "Sentence-case page title"
description: "One- or two-sentence summary that completes the thought 'this page is about ...'."
---
```

## Reporting documentation bugs

Open an issue at [github.com/86d-app/86d/issues](https://github.com/86d-app/86d/issues) with the `docs` label. Include the page URL and the specific section that is wrong.
