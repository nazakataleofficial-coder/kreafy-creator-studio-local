# Roadmap

Milestones are sequential. Do not start the next milestone until the current one meets its exit gate.

## M0 — Foundation
Specs, agent rules and repository hygiene.
Exit: source-of-truth docs exist and implementation boundaries are clear.

## M1 — App Shell + Local Projects
Desktop/local app shell, create/open/recent project, project folder contract, settings, basic navigation and recovery from invalid/missing project paths.
Exit: restart-safe project lifecycle works with automated coverage for project persistence.

## M2 — Asset Library
Import/copy/link policy, thumbnails, metadata, search/filter basics, non-destructive delete/archive semantics.
Exit: assets survive restart and source protection is verified.

## M3 — Master Prompt Library
Prompt CRUD, tags/categories, variables, duplicate/version flow, search, import/export and prompt snapshots.
Exit: prompts are reusable without accidental mutation of historical jobs.

## M4 — Provider Adapter + Single Image Generation
Provider interface, credential handling, capability discovery/config, one real generation path plus test/mock adapter, normalized errors.
Exit: one job can complete/fail/cancel truthfully and persist its record.

## M5 — Bulk Image Generation
Batch input, row validation, shared overrides, queue/concurrency, progress, retry failed, cancel pending, resume/recovery after restart.
Exit: mixed success/failure batch is recoverable without rerunning successful jobs.

## M6 — Consistency Presets
Character/style/reference/negative prompt presets, inheritance/override preview and immutable job snapshots.
Exit: user can see the exact resolved prompt/settings before execution.

## M7 — Production UX + Export
Gallery review, compare, naming templates, selective export, manifests, keyboard flow, accessibility and performance pass.
Exit: end-to-end creator workflow passes QA.

## M8 — Packaging
Installer/update strategy, migration/backups, diagnostics, release checklist.
Exit: clean-machine install and upgrade/recovery tests pass.

Anything beyond M8 requires a new approved spec.

## Approved post-M8 specialist milestone — Pro Reel Engine
Not active during M0–M8. When explicitly activated, implement `docs/PRO-REEL-ENGINE.md` on top of the Raw2Reel foundation and shared Creator Studio infrastructure.

Exit: a real long-form source produces multiple selected, locally processed Pro Reels; creative decisions are inspectable/editable; free-core acceptance, recovery, manifests and QC pass end to end.
