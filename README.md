<p align="center">
  <a href="https://86d.app">
    <img src="https://86d.app/logo" height="96" alt="86d" />
  </a>
</p>

<p align="center">
  <a href="https://x.com/86d_app"><strong>X</strong></a> ·
  <a href="https://www.linkedin.com/company/86d"><strong>LinkedIn</strong></a> ·
  <a href="https://github.com/86d-app/86d"><strong>GitHub</strong></a>
</p>
<br/>

> [!WARNING]
> 86d is under active development and is not ready for production use. Proceed with caution.

This repository hosts the [86d documentation site](https://86d.app/docs), built with [Mintlify](https://mintlify.com). Start with [What is 86d?](https://86d.app/docs/introduction), [Architecture](https://86d.app/docs/concepts/architecture), and [Versioning and maturity](https://86d.app/docs/resources/versioning).

Pages are written for two readers at once: a merchant who wants to know what a capability does for their store, and an agent that needs the exact contract underneath. `AGENTS.md` holds the full style rules, including which nouns are capitalized and how maturity is stated on a page.

## Local development

```bash
npm i -g mint
mint dev
```

The dev server runs at `http://localhost:3000` and hot-reloads on file changes.

## Structure

- `docs.json`: Mintlify configuration and navigation.
- `index.mdx`: home page.
- `introduction.mdx`, `quickstart.mdx`, `deployment.mdx`: getting-started pages.
- `concepts/`: product authority, commerce records, Connections, Modules, Templates, Storefront, Store Admin, and agent operations.
- `configuration/`: `config.json`, environment variables, storage, authentication.
- `cli/`: CLI overview and command reference.
- `guides/`: how-to guides.
- `modules/`: one reference page per first-party Module, plus the catalog.
- `operations/`: security, testing, observability, managed Workflows, launch evidence, troubleshooting.
- `resources/`: glossary, FAQ, Public Beta status, Cloud plans, versioning, contributing, changelog.
- `resources/mcp.mdx`: the read-only Docs MCP server.

## Checks

```bash
mint broken-links
python3 -c "import re,sys,glob;[sys.exit(1) for f in glob.glob('**/*.mdx',recursive=True) if re.search('[\u2013\u2014]',open(f).read())]"  # must return 0
```

## Contributing

See [Contributing](https://86d.app/docs/resources/contributing).
