---
name: tdd
description: "Use when asked to use TDD, write tests first, or do red-green-refactor, without a specific technology context. Detects project stack and delegates to tdd-net or tdd-web."
---

# TDD (Stack Detector)

Detect the current project's technology stack, then invoke the appropriate skill.

## Rules

1. Inspect the project root for stack indicators (see below).
2. Invoke the matching skill via the Skill tool.
3. Follow that skill exactly from that point forward.

## Detection

| Indicator | Stack | Invoke |
|---|---|---|
| `*.csproj`, `*.sln`, `global.json`, `nuget.config` | .NET/C# | `software-engineering:tdd-net` |
| `package.json` with `react`, `vite`, or `vitest` | React/Web | `software-engineering:tdd-web` |
| Both present | Ask user which applies |  |
| Neither present | Ask user which applies |  |
