# Documentation site instructions for AI agents

Mintlify documentation site for [86d](https://86d.app), Aspen theme. Pages are MDX files with YAML frontmatter; navigation and configuration live in `docs.json`. Two top-level tabs: **Documentation** (Get started, Concepts, Configuration, Guides, CLI, Operations, Resources) and **Modules** (one reference page per first-party Module, grouped by domain).

The `mint` CLI is not a repo dependency. Run it from the `docs/` directory: `bunx mint dev` to preview locally, `bunx mint broken-links` to check internal links.

## Source of truth

The 86d codebase is split across two sibling repos. `docs/` is a third sibling and is published independently.

- **Target product behavior and vocabulary:** `../prd/README.md`. Follow its reading route for architecture, commercial terms, agents, and launch claims. Module storage kinds and isolation authority live in `../prd/contexts/store-runtime/module-system.md`; this site projects shipped behavior, not the full contract.
- **Current implementation:** the source and tests below. Verify them before claiming a Feature exists, is available, or behaves a certain way.
  - **Framework** (Modules, CLI, Templates, runtime, registry): `../public/`. CLI commands, flags, behavior: `../public/packages/cli/src/commands/`. Module names, options, contracts: `../public/modules/<name>/src/` and `../public/apps/registry/registry.json`. Template structure: `../public/templates/<name>/`. Framework environment variables: `../public/.env.example`.
  - **86d.app and Control Plane** (86d Accounts, Businesses, Store lifecycle, provisioning, Cloud billing, agents): `../private/`. Platform environment variables and provisioning behavior: `../private/.env.example` and `../private/packages/api/src/router/provisioning.ts`.

When target context and code differ, document the current behavior accurately or label future behavior clearly. Never present a target decision as shipped. Never keep a stale implementation claim because an older page says it is current.

The canonical Module registry is `https://raw.githubusercontent.com/86d-app/86d/main/apps/registry/registry.json`, generated from `../public/apps/registry/registry.json`.

## Who reads these pages

Two readers, one page. Write for both.

- A **merchant** wants to know what a capability does for their Store, what they have to decide, and what it costs them if it goes wrong. They open the page mid-task and stop reading when they have the answer.
- An **agent** wants the exact contract: options, endpoints, request and response shapes, components, and failure behavior. It cannot ask a follow-up question, so an omitted field becomes a wrong action against a live Store.

Open every capability page with what it does for a Store in plain words. Put the contract underneath. Never make a merchant read an endpoint table to learn what a Feature is for, and never make an agent infer a field name from prose.

## Editing rules

- **Always run Mintlify commands from the `docs/` directory.**
- **Every page needs `title` and `description`** in frontmatter. Both are required.
- **Every page needs a status callout.** Capability pages carry the maturity callout for their current maturity. Narrative pages carry the in-development callout. See [Maturity](#maturity).
- **Sentence case for headings.** "Module configuration", not "Module Configuration". Capitalized defined terms keep their capitals inside a heading.
- **Use "you", not "the user".**
- **Active voice. Short sentences. One idea per sentence.**
- **No em dashes, and no en dashes misused as em dashes.** This is a hard rule, checked by the dash grep in [Health gates](#health-gates). Use a comma, a colon, a semicolon, parentheses, or a sentence break.
- **No `easy`, `simple`, `quick`, `very`, `just`, or `really`.** Say the concrete thing instead: "one command", "two fields", "no configuration".
- **No weasel words.** Replace `typically`, `generally`, `most`, and `often` with the actual number or condition.
- **Bold for UI elements** (`Click **Settings**`) and for load-bearing facts. Not for emphasis.
- **Code formatting** for file paths, command names, environment variables, and identifiers. Not for prose emphasis.
- **Internal links** are root-relative paths without the `.mdx` extension (`/concepts/modules`).
- **Every code block gets a language tag.** TypeScript is the default.
- **Units take a space and an uppercase unit**: `64 KB`, `200 ms`. Seconds stay bare: `30s`.
- **Do not hard-wrap paragraphs.** One paragraph is one line in source.
- **No `---` horizontal rules** between sections.

## Capitalization

Capitalize a noun when it names an 86d concept that has a definition page or a glossary entry, and link it the first time it appears on a page. This is what tells a reader that **Order** means the specific commercial agreement 86d defines, not the everyday word.

Capitalized: Store, Business, Storefront, Store Admin, Store Runtime, Module, Feature, Integration, Connection, Template, Product, Variant, Inventory, Cart, Checkout, Checkout Request, Order, Payment, Payment Connection, Fulfillment, Shipping, Parcel, Customer, Shopper, Guest, Command, Workflow, Change Set, Standing permission, Managed Deployment, 86d Balance, Store allocation, Promotional Credit, Purchased Funds, Management fee, Billing profile, Stable, Beta, Experimental, Deprecated.

Product and commercial names keep their capitals: 86d.app, 86d Console, 86d Cloud, 86d.store, Launch, Premium, Enterprise, Control Plane, 86d Payments, Third-party Payments.

Ordinary uses of the same word stay lowercase: a Store you own, but in-store pickup. A Product record, but product photography.

Do not use bare "dashboard", "console", "analytics", or "telemetry". Name which product owns it, what it measures, and where the data goes.

## Maturity

Read `../public/apps/registry/registry.json` before you state a maturity anywhere, and never promote a page past what the registry records. Use each Module's recorded `maturity` and `maturityEvidence`; do not cache a repository-wide maturity summary in this guide.

Capability pages carry the maturity in two places:

1. A `tag` frontmatter field, which Mintlify renders beside the page title in the sidebar.
2. A callout under the frontmatter.

```mdx
---
title: "Discounts"
description: "…"
tag: "Experimental"
---

<Warning>
  **Experimental.** This Module has no test or production evidence recorded yet. Read it, run it locally, and hold off on real Orders. See [maturity levels](/resources/versioning).
</Warning>
```

Narrative pages that do not document one capability carry the in-development callout and no `tag`:

```mdx
<Warning>
  **In development.** 86d is being built in the open. Every capability is Experimental until it earns evidence, so check [maturity levels](/resources/versioning) before you rely on anything here.
</Warning>
```

## Public projection boundary

Published docs cover shipped behavior, setup, concepts, and safe operation. Keep private roadmap order, launch evidence detail, provider negotiations, unshipped commercial terms, security exploit detail, and personal or project-private data out of `docs/`.

COSMOBIA is the founder's own Store and an internal proof loop. It is not a merchant-facing subject. Do not name it in published docs.

**Do not name the supplier behind an 86d-managed service.** A merchant operates 86d, so the companies 86d buys hosting, payments, email, or shipping capacity from stay out of these pages when describing 86d Cloud, 86d Payments, managed email, or managed AI. Describe what 86d does, not who 86d bought it from.

The exceptions are real and narrow. Name a provider when the merchant is choosing it themselves: a host they deploy to (Railway, Vercel, a Docker server of their own), a [Connection](/concepts/connections) they bring (their Stripe account, their EasyPost key), or a Module that exists to talk to that company. Those are the merchant's relationships, not 86d's.

When a planned contract helps a reader understand a migration:

1. Label it as planned or in development.
2. Keep the statement to the smallest useful contract.
3. Link to the current configuration or Module page.
4. Link to `/resources/versioning` so maturity stays explicit.

Use these pages as the public conceptual sources and link to them instead of restating their definitions:

- `/concepts/architecture`: 86d.app, 86d Console, Control Plane, and Store Runtime boundaries
- `/concepts/commerce-model`: Cart, Checkout, Order, Payment, Fulfillment, and Shipping
- `/concepts/connections`: Integration and Connection boundaries
- `/concepts/agentic-design`: what an agent can do today and what is planned
- `/resources/versioning`: maturity and evidence
- `/resources/glossary`: public vocabulary

## Common tasks

### Add or update a Module page

1. Confirm the Module exists in `../public/modules/<name>/` and `../public/apps/registry/registry.json`.
2. Create or edit `modules/<name>.mdx` with frontmatter (`title`, `description`, `tag`).
3. Add the page to the correct group in `docs.json` under the Modules tab.
4. Match the shape of a well-developed page, for example `modules/products.mdx`: what it does for a Store, source and npm links, installation, configuration, what a merchant sets up, Store endpoints, admin endpoints, components, related pages.
5. Cross-link from related pages.

### Update a CLI command reference

1. Read the actual command in `../public/packages/cli/src/commands/<name>.ts`.
2. Update `cli/commands.mdx` to match flags, sub-commands, and behavior.
3. If a top-level command was added or removed, also update `cli/overview.mdx`.

## Documentation gaps

Before expanding a short Module page, check the current source, tests, registry entry, and failure behavior. Page length is not maturity evidence, and this guide does not keep a static gap inventory.

## What not to touch

- `LICENSE` and `.git/`.
- Files in `../public/` and `../private/`. Treat both framework repos as read-only when working in `docs/`.

## Health gates

Both must exit zero before committing, run from `docs/`:

```bash
bunx mint broken-links
python3 -c "import re,sys,glob;[sys.exit(1) for f in glob.glob('**/*.mdx',recursive=True) if re.search('[\\u2013\\u2014]',open(f).read())]"  # em/en dash grep
```

## Git safety

**Agents never push.** Local work stays local until the operator publishes it. This covers every publication path: `git push` and all its variants, `gh`, and any tool that uploads branches or rewrites remote history.

## Commits

Every commit follows [Conventional Commits](https://www.conventionalcommits.org/) with a **required scope**: `type(scope): subject` — imperative, lowercase subject, no trailing period, under 72 characters when possible. Husky and commitlint enforce the format locally. See `CONTRIBUTING.md` for the full contributor guide.

**Types:** `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`

**Scopes:** `site`, `concepts`, `guides`, `cli`, `modules`, `resources`, `config`, `repo`. Scope is the directory you changed (`concepts/` → `concepts`); non-obvious mappings: root-level pages (`index.mdx`, `introduction.mdx`, `quickstart.mdx`, `deployment.mdx`) → `site`, `operations/` → `resources`, `configuration/` → `config` (as are `docs.json` and Mintlify config), cross-cutting repo or hook changes → `repo`.

**Agent rules:**

- Commit only when the user asks, or when finishing a self-contained docs slice that passes both health gates.
- One logical change per commit. Split unrelated work (for example a concepts rewrite and a CLI reference update) into separate commits.
- Let the hooks run: `git commit --no-verify` only when the user explicitly requests it.

## Reporting back

End any PR-shaped task with a brief change summary: changed files (one line each), new pages created, gaps left for a maintainer, and any open questions.
