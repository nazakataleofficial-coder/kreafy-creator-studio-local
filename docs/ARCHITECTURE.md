# Architecture Contract

This document defines boundaries, not a forced framework. Claude must inspect the chosen stack before implementation and record material stack decisions.

## Layers
1. UI: views/components and interaction state.
2. Application: use cases such as createProject, enqueueBatch, retryJob.
3. Domain: provider-neutral entities/contracts.
4. Infrastructure: filesystem/database/provider adapters/OS integration.

UI must not call provider SDKs or arbitrary filesystem paths directly.

## Core entities
Project, Asset, PromptTemplate, PromptVersion, Preset, GenerationJob, GenerationBatch, ProviderConfig and ExportRecord.

## Project contract
Each project has a stable ID and schema version. User media/output paths are referenced through project-managed metadata. Migrations are explicit and backup-safe.

## Job state machine
queued -> running -> succeeded | failed | cancelled.
Retry creates a new attempt/history entry; it must not erase the prior failure record.

## Snapshot rule
At enqueue time resolve and snapshot the effective prompt, variables, references, provider/model and generation settings. Later edits to a prompt/preset must not mutate historical jobs.

## Provider boundary
A provider adapter exposes capabilities, validates settings, starts generation, normalizes progress/result/errors and supports cancellation only when the provider truly supports it. Unsupported features must be disabled/labelled, never simulated.

## Persistence
Prefer transactional writes for metadata. Use atomic file replacement where practical. Large binary assets must not be stored as encoded blobs inside normal metadata records. Define migrations before schema changes after M1.

## Security/privacy
Secrets belong in OS-appropriate secure storage or an explicitly approved local secret mechanism, never Git or portable project exports. Logs must redact secrets. Network calls happen only for features/providers the user invokes.

## Reliability
Operations that can be retried need stable IDs/idempotency thinking. App restart must not convert unknown interrupted jobs into false successes; recover them into an explicit recoverable/interrupted state or normalized failure according to implementation design.

## Future Pro Reel boundary
`docs/PRO-REEL-ENGINE.md` defines a future specialist media pipeline. It must reuse Project, Asset, job/queue/history, Preset and Export concepts rather than create a disconnected product silo. Raw2Reel's technical RenderPlan is the base; the Pro Reel layer adds a versioned CreativeRenderPlan for editorial events. Paid/cloud capabilities remain optional adapters and must never become hidden core dependencies.

## Existing-Studio reference — architecture decisions to validate

These are **design constraints**, not a change to the active M0 milestone. See `EXISTING-STUDIO-LEARNINGS.md` for the source-observed evidence and its limits.

### Reuse ideas, not the separate product

Kreafy Creator Studio Local remains a new, independent, local-first application. The existing Kreafy Studio is a reference for queue behavior, persistence, editing workflows and render recovery; it is **not** a dependency, migration target or repository to copy wholesale. Keep the current UI/application/domain/infrastructure boundaries and the existing M0–M8 roadmap.

### Durable handoffs and recovery

- Define project-scoped asset IDs, immutable source references and a durable metadata contract so future Script / Image / Voice / Video tools can exchange assets without sharing in-memory component state.
- Persist jobs, attempts and resolved provider/model/settings **before** dispatch. An interrupted job must transition into an explicit recoverable/error state after restart, never silently become `succeeded`.
- For long-running or chunked work, use a versioned fingerprint of inputs/settings/engine, part indices, checksums and validated output metadata. On resume, reject stale/corrupt checkpoints, avoid duplicate billing where possible and revalidate the final result.
- Storage failure (permissions, exhausted disk, deleted/moved files) must have a truthful error/recovery path. A best-effort draft save is not equivalent to a durable committed project.

### Renderer boundary (future Pro Reel work, **not** M1–M8)

Keep editable events and a versioned render plan separate from backend-specific execution. **Native FFmpeg is a candidate**, browser ffmpeg.wasm is a reference/fallback candidate, and HyperFrames may be assessed as an **optional motion-graphics component only after a source/license/prototype review**. Compare them for output quality, feature coverage, reproducibility, RAM/CPU/GPU use, packaging and failure recovery. Do not assume a successful 720p browser render proves 1080x1920 capability or equivalent audio/frame timing.

### Provider, privacy and deployment boundary

- Provider adapters must disclose accurate feature support, quota/rate limits, offline capability and cost before jobs are queued. Requested provider and model cannot change silently during a batch; fallback requires a new explicit choice/snapshot.
- Use the project's approved OS-appropriate secret handling. **Do not copy API-key-in-browser-storage patterns into the new implementation.** Credentials cannot enter project exports or logs.
- Remote services may be optional adapters only. Do not introduce a hidden mandatory dependency on the existing Studio's hosting, accounts, browser extension or proprietary integrations.
- Keep this public repository free of private source files, deployment secrets, model checkpoints, customer media and internal service URLs.
