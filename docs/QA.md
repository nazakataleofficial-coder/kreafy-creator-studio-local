# Quality Gates

## For every milestone
- Acceptance criteria mapped to tests or explicit manual verification.
- Typecheck/lint/build pass when the chosen stack provides them.
- No secrets or machine-specific paths committed.
- Error/empty/loading states checked.
- Restart/persistence behavior checked for stored data.
- Existing tests remain green.

## Critical scenarios
1. Create project -> restart -> reopen with intact metadata.
2. Missing/moved project path produces recovery UI, not data destruction.
3. Historical job snapshot remains unchanged after editing its source prompt/preset.
4. Batch with successes + failures reports both correctly.
5. Retry Failed does not rerun successful jobs.
6. Duplicate output names never silently overwrite files.
7. Provider auth/rate/network errors are distinguishable and safe.
8. App interruption during generation does not mark unfinished work successful.
9. Bulk validation identifies bad rows before execution.
10. Exported files and optional manifest match selected assets.

## Release gate
A release candidate requires a clean-machine smoke test, migration/backup test, representative large batch test, accessibility keyboard pass and documented known limitations.

## Future Pro Reel quality gate
The Pro Reel milestone additionally requires the real long-video end-to-end acceptance test in `docs/PRO-REEL-ENGINE.md`. Completion cannot be inferred from unit tests or a demo render. Verify multiple candidate clips, free-core processing, synchronized playable outputs, creative typography/motion, safe masking fallback, editable-event rerender, restart/recovery, provenance and manifests/QC.
