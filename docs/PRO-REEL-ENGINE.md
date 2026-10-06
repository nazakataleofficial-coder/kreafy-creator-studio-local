# Pro Reel Engine — Specialist Product Specification

## Status and scope
This document defines a future specialist engine inside Kreafy Creator Studio Local. It extends the Raw2Reel editing foundation; it does not replace the image-production MVP or authorize implementation before its roadmap milestone is approved.

Product flow:

**Long Video -> Best Moments -> Creative Director -> Pro Edit -> Human Fine-Tune -> Batch Export**

The goal is a polished creator reel that feels intentionally edited rather than template-applied. Do not promise awards, virality, or exact equivalence to Adobe Premiere Pro. Use measurable editorial and technical quality gates.

## Hard rule: zero paid API dependency for core
The complete core workflow must be usable without a paid API or per-render fee.

Core may use local/free/open-source components such as FFmpeg, WhisperX/faster-whisper, Silero VAD, OpenCV, PySceneDetect, DeepFilterNet and libass, subject to actual shipped licenses and compatibility.

Cloud LLMs, generated B-roll/video, premium stock, premium voices and other paid providers are optional adapters only. The UI must clearly label them as optional/cost-bearing before use. Failure or absence of an optional provider must not break the core reel pipeline.

Bundled fonts, SFX, music and templates must have redistribution-compatible licenses and provenance metadata.

## 1. Long-video clip discovery
A long source may produce multiple candidate shorts.

For each candidate preserve:
- source start/end timestamps;
- transcript excerpt;
- why it is a coherent candidate;
- hook evidence;
- completeness of thought;
- overlap/duplicate relationship with other candidates;
- estimated target duration.

Never display a fake viral score. Prefer explainable evidence such as strong opening line, question, reveal, emotional emphasis, useful standalone lesson, or CTA.

Do not reorder speech into a misleading meaning. Candidate selection remains user-controllable.

## 2. Creative Director Engine
After the base Raw2Reel analysis creates speech, scene, face, motion and ContentBeat information, create a semantic visual plan.

For every meaningful beat choose deliberately among:
- hold the speaker cleanly;
- jump cut;
- punch-in/punch-out;
- reframing/crop change;
- headline or keyword typography;
- foreground/background typography;
- callout, number or simple data graphic;
- source-detail cutaway;
- user/local-library B-roll;
- optional licensed/provider B-roll;
- icon/shape/motion graphic;
- SFX cue;
- music energy/ducking cue;
- color/emphasis treatment;
- intentional no-effect hold.

Every visual change must have an editorial reason. Never add motion simply to satisfy an effects quota.

## 3. CreativeRenderPlan
Raw2Reel's RenderPlan remains the technical foundation. Pro Reel adds a versioned CreativeRenderPlan above it.

Each event records:
- timeline start/end;
- source ContentBeat/words;
- layer type;
- transform/crop;
- typography style token;
- animation in/out;
- mask/depth behavior;
- asset reference and provenance;
- audio/SFX cue;
- rationale;
- confidence where applicable;
- fallback behavior.

The final renderer consumes a resolved immutable snapshot so the output is reproducible.

## 4. Advanced typography system
Separate normal captions from creative typography.

Support:
- kinetic headline phrases;
- keyword emphasis;
- number/stat emphasis;
- font pairing tokens;
- controlled text colors;
- stroke/shadow/background treatments;
- per-word or phrase animation where timing is reliable;
- tracked callouts;
- text behind subject;
- text partially occluded by foreground subject;
- foreground text overlays;
- safe-zone aware placement.

Typography must respond to meaning, emotion and screen composition. It must not duplicate every caption as a graphic.

Do not bundle proprietary fonts without distribution rights. Provide open-license defaults and allow local user fonts.

## 5. Subject depth and text-behind-person
Create an optional local subject segmentation/masking adapter.

Requirements:
- stable masks through time;
- feathered edges;
- hair/edge quality fallback;
- no flickering depth effect;
- detect when segmentation confidence is too low;
- fall back to normal foreground typography rather than rendering a broken mask;
- cache reusable masks.

Text-behind-subject is an editorial tool, not a default on every sentence.

## 6. Motion and transitions
Support professional but restrained motion:
- eased punch-ins;
- position/scale keyframes;
- motivated crop changes;
- speed ramps only where source/content supports them;
- blur/motion treatment where technically safe;
- match/mask transitions when justified;
- clean hard cuts as the default when they are better.

No random transition rotation and no effect spam.

## 7. B-roll and visual inserts
Core must work without external B-roll.

Priority:
1. source footage/detail cutaways;
2. user-owned local asset library;
3. redistribution-safe bundled graphics;
4. optional licensed stock adapter;
5. optional AI-generated media adapter.

Every external asset stores source/provider/license/provenance where available. If no suitable legal asset exists, keep the speaker or use a local graphic; never scrape random copyrighted media.

## 8. SFX and music
Core SFX may come from a locally installed, redistribution-safe library.

SFX must be event-driven: text hit, transition, reveal, UI/callout, or intentional emphasis. Enforce density limits and loudness rules so voice remains primary.

Music remains optional. User-provided or clearly licensed music is preferred. Duck under speech and preserve intelligibility.

## 9. Style DNA
A Style DNA preset is a versioned collection of editable characteristics, not a promise to clone another creator.

It may store:
- pacing range;
- caption layout;
- typography tokens;
- color tokens;
- zoom intensity/frequency;
- transition density;
- B-roll density;
- SFX density;
- motion intensity;
- preferred framing;
- hook treatment;
- music behavior.

A user may analyze a reference they have the right to use. Extract abstract editing characteristics rather than copying copyrighted graphics, logos, music, footage or a creator's unique assets.

Built-in starting presets may include Premium Clean, Fast Viral, Educational, Cinematic Clean and Pro Dynamic.

## 10. Human fine-tune timeline
One-click generation is the default, but the result must remain editable.

Expose a simplified timeline with tracks/events such as:
- Cuts;
- Captions;
- Creative Text;
- B-roll;
- Motion;
- Mask/Depth;
- SFX;
- Music.

User can disable, replace, move or restyle an event. Prefer dependency-aware partial rerender of the affected range when technically safe; fall back to a full render when required for correctness.

Never pretend a partial render is possible if the render graph requires a full rebuild.

## 11. Batch reel workflow
For one long source:
1. analyze source once;
2. propose candidate clips;
3. user selects all/some;
4. create independent reel jobs sharing cached source analysis;
5. create a CreativeRenderPlan per reel;
6. render independently;
7. failures do not erase successful reels;
8. retry failed only;
9. export selected or all.

Each reel remains traceable to original source timestamps.

## 12. UI contract
Dashboard tool name: **Long Video -> Pro Reels**.

Primary flow:
**Import -> Analyze -> Suggested Clips -> Style -> Create Pro Reels -> Review -> Export**

Suggested Clips shows evidence and timestamps, not fake virality claims.

Advanced controls stay collapsible. Normal users should be able to choose source, style and clips and then create.

During processing show truthful stages from actual job state.

## 13. Performance and local-first rules
- Analyze long media in chunks/samples; never load huge video fully into RAM.
- Cache transcription, source analysis, face tracks and masks.
- Detect CPU/GPU and degrade gracefully.
- Keep CPU-compatible fallbacks for the core.
- Optional heavy effects may warn that they are slow on CPU; they may not silently require a cloud service.
- Preserve original media.
- Resume interrupted jobs where stage outputs are valid.

## 14. Quality gates
A Pro Reel cannot be marked complete until base Raw2Reel QC and creative QC pass.

Creative QC checks include:
- no text covering critical face features unless intentionally designed;
- no broken/flickering subject masks;
- no typography outside safe zones;
- no unreadable font/contrast combination;
- no orphan text after cuts;
- no SFX overpowering speech;
- no excessive repeated transition pattern;
- no missing referenced assets;
- provenance present for external assets;
- first seconds contain meaningful speech/visual activity where source permits;
- CreativeRenderPlan matches rendered duration/timeline.

Use PASS / PASS_WITH_WARNINGS / FAIL. Do not call this a viral score.

## 15. Acceptance test
Given a representative 20+ minute talking-head/podcast source containing several standalone ideas, the system must:
1. analyze it locally without a paid API;
2. propose multiple coherent short candidates with source timestamps and reasons;
3. allow selection of at least three;
4. render each as a vertical reel;
5. use cleaned/synced voice and accurate captions;
6. apply meaning-driven creative typography/motion;
7. demonstrate at least one safe depth/text-behind-subject event when confidence permits, otherwise record the fallback;
8. use only legal/local assets in the free-core test;
9. expose the editable event timeline;
10. modify one event and rerender correctly;
11. survive restart/recovery;
12. export finished reels plus manifests/QC reports.

The milestone fails if a real end-to-end source cannot produce playable, synchronized outputs.

## 16. Implementation boundary
Do not implement this specialist engine during the image-generation MVP milestones.

When its roadmap milestone is activated, the coding agent must read:
- this document;
- the Raw2Reel master specification/reference;
- PRODUCT.md;
- ARCHITECTURE.md;
- UI-UX.md;
- QA.md;
- the current repository/tests.

Reuse shared project, asset, queue, history, preset and export infrastructure instead of creating a second disconnected app.

## 17. Definition of done
Done means the core works without paid APIs, real long-video input produces multiple real reels, creative decisions are inspectable/editable, output survives restart, legal/provenance rules are honored, and end-to-end acceptance passes.

A beautiful UI, unit tests, or a rendered demo alone is not completion.
