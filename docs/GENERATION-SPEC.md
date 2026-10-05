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
