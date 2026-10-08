# Generation, Prompt Library & Bulk Contract

## Prompt template
Minimum fields: id, title, body, variables, tags, createdAt, updatedAt and version identity. Optional provider hints must not make the base prompt unusable elsewhere.

## Variables
Use an explicit syntax chosen during M3. Missing required variables block enqueue. Preview shows the resolved prompt. Escaping/literal variable syntax must be tested.

## Preset resolution
Resolution order must be deterministic and visible. Recommended model: project defaults -> selected preset -> row overrides. The resolved snapshot is stored on the job.

## Generation job
A job stores stable ID, batch ID if any, input snapshot, provider/model, normalized settings, reference asset IDs, timestamps, status, attempts, output asset IDs and normalized error data.

## Batch
A batch is a collection of independently recoverable jobs. Batch progress derives from job states; it is not a separate invented percentage.

## Queue
Concurrency is configurable within safe/provider limits. Pending jobs can be cancelled. Active cancellation is offered only if supported. Restart recovery must identify interrupted work.

## Retry
Retry Failed never reruns succeeded jobs by default. Retry keeps historical attempts and can use the same immutable snapshot unless the user explicitly chooses to create a modified new job.

## Output
Outputs get unique collision-safe names and metadata links to their job. Partial provider responses are never presented as completed assets unless valid output exists.

## Errors
Normalize at least: validation, authentication, rate-limit/quota, network/timeout, provider rejection, unsupported setting, cancelled and unknown. Preserve a safe human-readable message and diagnostic detail without secrets.

## Import/export
Prompt/preset exports are versioned and validated. Asset export may include an optional manifest mapping filenames to job/prompt/settings metadata.

## Reference-informed batch execution clarifications

The following clarifies existing M4–M7 contracts; it does **not** activate implementation or change the roadmap. The legacy Studio's queue/provider implementation is reference evidence only (see `EXISTING-STUDIO-LEARNINGS.md`).

### Immutable engine selection

- Each enqueued item records its selected `providerId`, exact `modelId`, capability snapshot, resolved prompt, style/character/reference versions, aspect/settings, cost/quota acknowledgment and a unique request/attempt identity.
- Provider/model selection is **locked for an item and for a batch configured as single-model**. If the chosen model runs out of quota or becomes unavailable, pause/fail truthfully; do **not** count a different engine's image as that item's result.
- An optional fallback is a user-visible **new choice** (and, where necessary, a new batch/job attempt with a new snapshot). Show what changed and whether it costs money. Never silently alter past job history.

### Queue semantics and crash recovery

- Distinguish `queued`, `running`, `succeeded`, `failed`, `cancelled` plus an explicit persisted **interrupted/recoverable** condition where warranted by M1 storage design. `quota`/`blocked` may be normalized batch-level pause reasons rather than fabricated terminal job success.
- Bound workers by engine limits and measured local resources. Serialize writes for the same job/asset, persist completed outputs atomically and treat re-entrant retries with stable identities/idempotency safeguards.
- Honor a provider's actual `Retry-After` where supplied; use bounded exponential backoff for safe transient failures. Separate permanent validation/auth errors from retriable network/rate errors. Never endlessly retry chargeable calls.
- Restart with completed outputs verified and unfinished work clearly labeled. **Retry Failed** selects failed items only, preserves previous attempts and does not resend successful or in-flight paid requests without checking their state.
- A cancelled pending item must not start later. Active cancellation must report whether the provider actually stopped the work and whether usage may already have been charged.

### Evidence, output and extension/provider integrity

- Save each item's real output MIME/dimensions/size, selected provider/model, timestamps and provenance alongside an optional human-readable manifest. Reject empty/corrupt media and contradictory provider metadata.
- Browser-assisted providers (for example an optional human-operated Flow companion) must report `waiting_for_user` distinctly from `generating`. Never bypass user actions, CAPTCHA, account terms or unsupported APIs. Never promise automatic bulk where a user must press Generate.
- Local/free vs external/paid capabilities must be visible on the selection screen. Never infer that an external provider is free indefinitely, even if a limited quota once worked.

### M5 acceptance extensions

A representative mixed-result batch must prove: same-model integrity; a quota or 429 pause; safe retry of only failed rows; process/restart recovery with successful outputs untouched; corrupt/missing output rejection; accurate counts; and a complete export manifest matching files and metadata.
