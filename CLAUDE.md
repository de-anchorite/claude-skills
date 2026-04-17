# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository purpose

This repo is a **curated collection of Claude Code plugins and skills** maintained by the repo owner. It is published as a single Claude Code **marketplace**: the top-level `.claude-plugin/marketplace.json` enumerates every plugin in the repo so users can add one marketplace entry and install any of the plugins from it.

Contents here are consumed by Claude Code itself — there is no application runtime, build, or test toolchain.

## Layout

- `.claude-plugin/marketplace.json` — marketplace manifest. Lists every plugin in this repo under `plugins[]`, each entry pointing at a plugin directory via `source` (e.g. `./software-engineering`). When adding a new plugin directory, add a corresponding entry here.
- `<plugin-name>/` — one directory per plugin at the repo root. Each contains:
  - `.claude-plugin/plugin.json` — plugin manifest (name, version, description, author, and paths such as `skills`).
  - `skills/<skill-name>/SKILL.md` — one skill per directory. Frontmatter (`name`, `description`) drives when Claude triggers the skill, so descriptions must be specific and list trigger conditions.
  - `README.md` — install notes for that specific plugin.

Current plugins:
- `software-engineering/` — planning, TDD, DDD, .NET/C# API design, React web-app design.

## Skill authoring conventions

When adding or editing a SKILL.md, match the in-repo style:

- Frontmatter `description` states triggers explicitly (e.g. "Apply to all coding tasks").
- Body opens with a short purpose line, then a `## Trigger conditions` section, then numbered **Core rules** phrased as non-negotiables.
- Rules are behavioural and prescriptive — they tell future Claude what to do/forbid, not general advice.
- Skills within one plugin should be internally consistent. For example, `software-engineering`'s tdd / api-design / web-app-design all assume xUnit + Testcontainers on the API side and Vitest + MSW + Storybook on the web side; a new skill in that plugin should not contradict them.

## Adding a new plugin to the marketplace

1. Create `<plugin-name>/.claude-plugin/plugin.json` and populate `<plugin-name>/skills/` (and any other plugin assets).
2. Add a `plugins[]` entry in `.claude-plugin/marketplace.json` with `name`, `source: "./<plugin-name>"`, and a `description`.
3. Optionally add a `<plugin-name>/README.md` with install/usage notes.

## Testing a change

There is no build or test command — these are Markdown + JSON definitions. To exercise a single plugin without going through the marketplace:

```bash
claude --plugin-dir ./<plugin-name>
```

To test the marketplace flow, launch Claude Code, run `/plugins`, add this repo's `.claude-plugin/marketplace.json` as a marketplace, and install the plugin from there.
