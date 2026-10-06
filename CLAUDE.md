# Claude Code Operating Contract

You are the primary implementation agent for Kreafy Creator Studio Local.

## Mandatory startup
Before editing code:
1. Read README.md.
2. Read docs/PRODUCT.md, ROADMAP.md, ARCHITECTURE.md, UI-UX.md, GENERATION-SPEC.md and QA.md.
3. Identify the active milestone.
4. Inspect the existing repository and tests.
5. State a short implementation plan before large changes.

## Non-negotiable rules
- Build only the active milestone. Do not silently add speculative features.
- Preserve local-first behavior and user ownership of assets.
- Never hard-code secrets, API keys, machine paths or provider credentials.
- Keep provider-specific logic behind adapters.
- Never silently overwrite source assets.
- Persist enough metadata to reproduce/diagnose a generation.
- Do not claim success until typecheck/lint/tests/build relevant to the change pass.
- Never delete tests merely to make CI green.
- Prefer small, reviewable commits and reversible changes.
- Reuse shared primitives; avoid giant components and duplicated state.
- UI must expose real state: queued/running/succeeded/failed/cancelled.
- A failed generation must preserve useful error information and be retryable when safe.
- If a spec conflicts with working code, stop and document the conflict rather than inventing a hidden compromise.

## UI loop
For each user-facing feature:
Understand requirement -> implement smallest vertical slice -> run app -> inspect behavior -> fix layout/states -> test keyboard/error/empty/loading states -> verify acceptance criteria.

## Vibe-coding guardrails
Do not rewrite architecture because a new pattern looks fashionable. Do not install a package until existing dependencies/platform APIs are checked. Do not create abstractions before a second real use case. Do not leave mock buttons or fake backend behavior in production UI.

## Completion report
At the end of a task report: changed files, tests run, results, known limitations, and exact next milestone/task.

## Specialist specs
Do not read/build specialist future engines merely because their docs exist. When the **Pro Reel Engine** milestone is explicitly active, `docs/PRO-REEL-ENGINE.md` and the Raw2Reel master specification/reference become mandatory startup reading. Preserve the zero-paid-API core and do not declare completion until its real long-video end-to-end acceptance test passes.
