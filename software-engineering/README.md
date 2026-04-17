# software-engineering

Coding preferences and engineering practices for .NET/C# APIs and React web applications, delivered as a Claude Code plugin.

## Skills

- **implementation-planning** — Constrained planning: goal, one fully-specified first test, high-level further test cases only.
- **tdd** — Test-first development: Red → Green → Refactor, one assertion per test, behaviours not internals, fakes not mocks, Given builders.
- **ddd** — Domain modelling: aggregate roots, bounded contexts, ubiquitous language, domain services, domain events, interface dependencies.
- **api-design** — .NET/C# API conventions: ASP.NET Core controllers, MongoDB, Auth0, vertical slice, Testcontainers integration tests.
- **web-app-design** — React conventions: Vite, Tailwind + DaisyUI, TanStack Query, Storybook + MSW, Vitest for logic only.

## Install

This plugin is distributed through the [claude-skills marketplace](../README.md) at the root of this repository. In Claude Code:

```
/plugins
```

Then **Marketplaces → Add** the root repo, and install `software-engineering` from the marketplace browser.

### Try it without the marketplace

For local, one-off use:

```bash
claude --plugin-dir ./software-engineering
```

run from the repository root.
