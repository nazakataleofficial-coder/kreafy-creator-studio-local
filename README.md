# Kreafy Creator Studio Local

Local-first AI creator workspace for repeatable content-production workflows.

## Status
Foundation/specification phase. Planned features are not implemented until their milestone acceptance criteria pass.

## Product principles
- Local-first: projects and assets stay local by default.
- Human-controlled AI: generation assists; the user reviews and approves.
- Reproducible: prompts, settings, provider metadata and outputs are traceable.
- Non-destructive: never silently overwrite source media.
- Modular: providers sit behind adapters; UI must not depend on one provider.
- Honest UI: no fake progress, fake success, or unimplemented capability.

## Initial pillars
1. Project workspace
2. Master Prompt Library
3. Bulk Image Generation
4. Reusable character/style/prompt presets
5. Generation queue, retry and history
6. Asset browser + metadata
7. Local export
8. Provider adapters

## Source of truth
Before implementation read `CLAUDE.md`, `AGENTS.md`, and everything in `docs/`.

## Build workflow
- ChatGPT: product planning, PRD, prompt/spec review
- Claude Code: primary implementation
- Codex: optional targeted final review/testing, not a normal development dependency

## Build rule
Implement one milestone at a time. A milestone is complete only when its acceptance criteria pass. Do not silently expand scope.

## Current stage
M0 — foundation and architecture lock. Next target: M1 app shell + local project system.
