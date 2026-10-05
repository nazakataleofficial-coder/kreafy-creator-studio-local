# Agent Rules

This repository is spec-driven.

## Roles
ChatGPT owns product planning/spec review. Claude Code is the primary coding agent. Other agents may review or test but must follow the same source-of-truth documents.

## Change discipline
- One milestone/issue per coherent change.
- Read before write.
- Do not change product scope while implementing.
- Record architectural decisions that alter contracts.
- No secrets in Git.
- No generated media, caches, local databases, model weights or user assets in Git.
- Keep commits understandable and rollback-friendly.

## Definition of done
A feature is not done because it renders once. It is done when its acceptance criteria, relevant tests, failure states and persistence behavior pass.
