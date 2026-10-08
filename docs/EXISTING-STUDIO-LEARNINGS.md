# Existing Kreafy Studio — Engineering Learnings (Reference Only)

## Purpose and evidence status

This document captures useful **engineering ideas** from the existing Kreafy Studio, which is a **separate product**. It is input to planning for **Kreafy Creator Studio Local**, not a merger plan, source-code transfer, or declaration that new features are built.

Evidence as of 2026-10-09: a read-only review of the existing Studio's frontend/backend source, related implementation notes, and the current new Studio specifications. **No new Studio feature was implemented; no old Studio test suite, live browser flow, or media-quality benchmark was executed as part of this review.** Code presence does not establish a production-quality acceptance pass.

The reference application and its hosting/accounts remain separate. Do not reproduce private code, credentials, internal deployment configuration, user data, or proprietary assets in this public repository.

## Source-observed patterns worth learning from

| Observed in reference code | Useful principle for the new project | Guardrail / validation needed |
| --- | --- | --- |
| An integrated dashboard spanning scripts, voice, images, and editing | Cross-tool handoffs should reuse stable project/asset identities | The new MVP remains **image-first**; do not add audio/video tabs in M1–M8 without an approved scope change |
| Image-batch runner with item states, concurrency, pause/retry and persistence | Treat each output as its own durable job, with success/failure/retry history | Verify restart recovery, quota handling, and failure isolation in the new storage model |
| Image providers behind a common interface | Capability-aware provider adapters and normalized errors | Lock the requested provider **and model** for each job/batch; never silently mix a cheaper/fallback model into an approved batch |
| Saved image style profiles, snapshots, and reference-image style analysis | Version reusable **style descriptions**, snapshot applied settings and keep provenance | Style extraction is not a promise of exact character identity, layout, or seed consistency |
| Browser-local project storage and background autosave | Preserve drafts and import state across restarts | The new desktop/local design must use its own project-folder contract, explicit migrations, and failure-safe storage; IndexedDB is **not** mandatory |
| Render checkpoints that validate saved parts before resume | Store a fingerprint, part metadata, byte counts and checksums; resume only matching verified work | A checkpoint is never proof of a successful final render. Handle disk-full, corrupted parts and changed inputs safely |
| FFmpeg WebAssembly editor with scenes, transitions, captions, audio, and part-wise rendering | Separate editable timeline data from a deterministic render plan; benchmark chunked rendering and boundary correctness | The reference browser editor is resource-limited (its output is 720p-class). Do not inherit its scene caps, render defaults, or quality claims as product requirements |
| Cross-tool transfer of generated images/transcripts into the editor | Pass durable asset references plus timing/metadata, not fragile temporary UI state | Track missing assets, source ownership and non-destructive imports |
| Google Flow assistance through an optional browser extension | An external-provider adapter can expose human-in-the-loop workflows transparently | Flow generation still requires user action and account quota. Do not automate CAPTCHA, infer official API support, or promise unattended/free bulk |
| Cloud-backed speech, transcription, prompting and image endpoints | Keep provider cost and availability visible to users | They are **not** evidence that a zero-paid-API offline/local core already exists |
| Voice-cloning and voice-design UI prototypes | Honest incomplete-feature labeling matters | Reference cloning is not established as an operational end-to-end capability; never present a mock or incomplete integration as finished |

## Adopt vs. defer, by the existing roadmap

- **M1–M2:** Project lifecycle, asset references, resumable imports and reliable persistence. Translate concepts; design against the new local filesystem requirements.
- **M3–M4:** Versioned prompts, immutable resolved snapshots, provider capability checks, stable job IDs and truthful status/errors.
- **M5:** Per-item batch queue, bounded concurrency, quota-aware pauses, safe retry/backoff and recovery **without regenerating already-successful items**.
- **M6:** Character/style/reference presets, deterministic inheritance and an inspectable resolved prompt/model snapshot.
- **M7–M8:** Cross-tool-ready export manifests, operational diagnostics, failure-state testing and clean-machine recovery.
- **Post-M8 only:** Video timeline, native-vs-browser FFmpeg decisions, chunked video checkpoints, creative editor and Raw2Reel/HyperFrames research. See `PRO-REEL-ENGINE.md`.

## Design decisions that must be proven, not assumed

1. **Browser vs native:** Choose after testing actual media on realistic target computers. Browser FFmpeg can be convenient but has memory, format and resolution/performance trade-offs; native FFmpeg needs packaging and OS-level safety.
2. **Durability:** A queue, project or render must not claim persistence unless it survives a crash/restart and verifies referenced outputs.
3. **Determinism:** Preserve provider/model identity, prompt/settings snapshot, source fingerprints, engine version, output metadata and relevant error details.
4. **Cost truthfulness:** The new core must not depend on paid generation APIs. Optional network providers require explicit opt-in and visible costs/quotas.
5. **Source isolation:** No direct import/linking of the legacy app as a hidden runtime dependency. A reference implementation can inspire a **new, reviewed implementation** only when its milestone is active.
6. **Feature honesty:** Design previews and acceptance tests together. No fake progress, fabricated AI quality scores, simulated voice cloning, unsupported style guarantees or “done” claims based on a nice UI.

## Action boundary

This page does **not** activate implementation, alter the M0–M8 order, authorize automatic publishing, or revise the separate Raw2Reel project's acceptance gate. Claude Code should consult it as **non-authoritative reference evidence** alongside the new project's authoritative `PRODUCT.md`, `ROADMAP.md`, `ARCHITECTURE.md`, `GENERATION-SPEC.md`, `QA.md`, and `CLAUDE.md`.
