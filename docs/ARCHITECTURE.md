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
