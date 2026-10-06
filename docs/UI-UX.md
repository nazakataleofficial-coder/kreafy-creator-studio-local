# UI / UX Rules

## Experience
Professional creator tool: dense enough for production, simple enough for first use. Prefer clear hierarchy over decorative effects.

## Main information architecture
Projects -> Workspace -> Assets / Prompts / Generate / Queue-History / Settings.

## Required states
Every async surface designs loading, empty, success, partial-success, error and disabled states before polish.

## Generation UX
Before enqueue, show resolved item count and validation errors. During execution, show batch progress plus per-job status. One failed row must not hide successful outputs. Retry Failed must target failures only. Cancel must state whether it affects pending jobs, active jobs, or both.

## Bulk UX
Support paste/import of many prompt rows, row-level editing, validation and selection. Shared settings must visibly distinguish inherited values from row overrides. Never make a destructive bulk action one accidental click.

## Prompt Library UX
Fast search, tags/categories, favorites optional only after core CRUD, duplicate before risky experimentation, variable preview, version/history semantics. Editing a template never rewrites an already-enqueued job.

## Visual system
Use consistent spacing/type tokens and accessible contrast. Avoid gratuitous glassmorphism, excessive gradients, tiny text, mystery icons and animation that slows production.

## Accessibility
Keyboard-accessible core actions, visible focus, labels/tooltips where icons are ambiguous, status not conveyed by color alone.

## Performance perception
Large libraries/batches need virtualization/pagination where measured necessary. Never fake completion percentages; indeterminate progress is better than invented precision.

## Future Long Video -> Pro Reels UX
When its milestone is active, expose a dashboard tool named **Long Video -> Pro Reels** with the primary flow: Import -> Analyze -> Suggested Clips -> Style -> Create Pro Reels -> Review -> Export. Suggested clips show source timestamps and explainable evidence, never fabricated viral scores. Review includes a simplified editable event timeline while advanced controls remain collapsible. See `docs/PRO-REEL-ENGINE.md`.
