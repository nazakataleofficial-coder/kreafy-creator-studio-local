# Product Requirements — Kreafy Creator Studio Local

## Vision
A local-first creator production workspace that turns repeatable AI content workflows into organized, inspectable projects rather than scattered prompts and files.

## Primary user
A creator/operator producing many related visual assets who needs speed, prompt reuse, batch execution, consistency and control without losing local ownership.

## Core jobs
- Create/open a project and keep its assets/settings together.
- Build, save, tag, search, duplicate and version useful prompts.
- Turn many prompt rows into an explicit generation queue.
- Apply shared character/style/negative-prompt/settings presets without destructive copy-paste.
- See each job's status and retry failures.
- Inspect output metadata and find the prompt/settings that created an asset.
- Export selected assets predictably.

## MVP boundaries
MVP focuses on image-production workflow and its reusable foundation. Video editing, social scheduling, autonomous publishing, cloud collaboration, billing and a marketplace are not MVP requirements unless later approved in the roadmap.

## Product invariants
Local project data is authoritative. Source files are immutable unless the user explicitly replaces/deletes them. Provider credentials never enter repository/project exports. Every generation has an ID, input snapshot, timestamps, status and output/error record.

## Success criteria
A user can create a project, create reusable prompts/presets, enqueue a multi-item image batch, watch truthful job state, recover from individual failures, inspect outputs and export them without manually reconstructing what happened.

## Non-goals
Do not promise perfect identity consistency across arbitrary models/providers. The app can preserve references/settings and make consistency workflows easier; provider/model behavior still determines results.

## Approved future specialist tool — Long Video -> Pro Reels
A future Creator Studio module may turn long-form user media into multiple polished short-form reels. Its specialist contract is `docs/PRO-REEL-ENGINE.md`. The core must remain local/free with no required paid API, preserve source media, expose explainable clip/edit decisions and reuse shared projects/assets/queue/history/export infrastructure.

This approval does **not** move Pro Reels into the current image-production MVP.
