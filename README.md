# claude-skills

A curated Claude Code marketplace of plugins and skills maintained by David Else.

## Plugins

| Plugin | Description |
| --- | --- |
| [software-engineering](./software-engineering) | Engineering practices: implementation planning, TDD, DDD, .NET/C# API design, React web-app design. |

## Install as a marketplace

Launch Claude Code and run:

```
/plugins
```

Choose **Marketplaces → Add**, then paste this repository's URL (or a local path to a clone). Once added, install any plugin listed above from the marketplace browser.

## Install a single plugin locally

If you just want to try one plugin without adding the marketplace:

```bash
claude --plugin-dir ./software-engineering
```

## Repository layout

- `.claude-plugin/marketplace.json` — marketplace manifest listing every plugin in this repo.
- `<plugin-name>/.claude-plugin/plugin.json` — per-plugin manifest.
- `<plugin-name>/skills/<skill-name>/SKILL.md` — individual skills, triggered by their frontmatter `description`.

See [CLAUDE.md](./CLAUDE.md) for contributor guidance (authoring conventions, adding a new plugin to the marketplace).

## License

[MIT](./LICENSE)
