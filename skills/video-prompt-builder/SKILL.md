---
name: video-prompt-builder
description: Build scroll-stop, high-retention AI video plans for Seedance 2.0 on Higgsfield — from brief to clip-by-clip prompts, stitching plan, and optional NanoBanana Pro asset pipeline. Triggers on any request to create/plan/storyboard a video for Seedance, Higgsfield, or AI video generation — brand film, launch video, ad, short-form social, launch teaser, scene, sequence, shot list, storyboard. Also triggers on phrases like "video prompt", "Seedance prompt", "plan a video", "8-second clip", "multi-clip video", "launch video", "scroll-stop video", "retention-optimized video", "Crypto Twitter video", "TikTok video", or when the user describes what should happen in a video and needs it translated into generation-ready structure. Operates in hybrid mode — asks focused clarifying questions when critical information is missing, otherwise ships the full plan in one pass.
---

# Video Prompt Builder — Scroll-Stop Framework for Seedance 2.0

Build 4-layer video plans that stop the scroll, hold retention, and ship clip-by-clip prompts ready to paste into Higgsfield. This is not a shot-list generator — it is an audience-weaponized scroll-stop engine.

**Scope:** Seedance 2.0 on Higgsfield only. Other models (Hailuo 2.3, Veo 3.1, Kling) are documented in the vault but this skill targets Seedance exclusively.

## The Framework (4-Layer Stack)

```
LAYER 1 — PSYCHOLOGY  ───→  universal mechanisms (orienting response, loops, parasocial, variable-reward, tribe)
LAYER 2 — AUDIENCE    ───→  tribe-specific encoding (CT / masculine-philosophy / health-women)
LAYER 3 — NARRATIVE   ───→  multi-clip spine (4-7 × 8s) with anchor placement + loop architecture
LAYER 4 — EXECUTION   ───→  Seedance prompt craft, reference slots, stitching, content-filter preflight
```

Every clip must pass all four layers. Miss one → scroll-loss.

## How to invoke this skill

Hybrid mode — if the user gives enough context, ship the full plan in one pass. If critical information is missing, ask **one focused question at a time** before proceeding. Never over-interrogate.

**Must-have before starting:**
- What is this video? (brand film / token launch / ad / post / short)
- Target duration and clip count (or the skill infers: 4 clips / 32s default, 6 clips / 48s for launches)
- Audience (if unclear — ask which tribe: CT, masculine-philosophy, health-women, or custom)
- The signature moment / payoff (what must the viewer see at the climax?)

**Nice-to-have (infer if missing):**
- Platform (default: X/CT + TikTok 9:16)
- Aesthetic/mood (default: match audience preset)
- Anchor placement (default: hero-late at clip N-1 for launches)
- Character/subject descriptions (if generic, push for specifics)

## Execution phases

Run these in order. Skip a phase only if the user has already locked that layer.

### Phase 0 — Audience Intake (run when tribe is unclear)

Read [references/audience-presets.md](references/audience-presets.md). Pick or build the audience profile. Must establish:
- Sub-tribe (1-5 in CT; match equivalents for other audiences)
- Enemy (what they define themselves against)
- Insider vocabulary (what words mark in-group)
- Cringe triggers (what they mock)
- Status currency (what winning looks like to them)
- Aesthetic code (color/lighting/typography that signals "for me")

If audience doesn't match any preset cleanly, ask the user: "Who are we writing this for? Give me 3 sentences describing them — what they self-identify as, what they reject, what makes them feel seen."

### Phase 1 — Narrative Spine Design

Read [references/narrative-spines.md](references/narrative-spines.md). Select structure based on video type:

- **Crypto-launch spine** (6 clips / 48s) — default for token launches, protocol reveals
- **PAS / PASTOR** (4-5 clips) — direct-response, problem-solution ads
- **7-shot spec-ad** (6-7 clips) — brand films, cinematic product reveals
- **AIDA compressed** (4 clips / 32s) — short product teasers
- **Pixar story spine** (5 clips) — transformation/founder arcs
- **Music-video build+drop** (5-7 clips) — when you have a locked track

Then lock:
- Clip count (4-7, each 8s generated)
- Per-clip job (Hook / Confirm / Accelerate / Pre-hero / Hero / Tag)
- Anchor placement (hero-early / hero-late / double-anchor)
- Loop architecture (which loops open where, which partially resolve, which close in tag)

### Phase 2 — Psychology Layer

Read [references/psychology-mechanisms.md](references/psychology-mechanisms.md). For each clip, mark which psychological mechanisms are active. At minimum:

- **C1**: orienting-response trigger + loop A open + tribal identity encode
- **C2**: loop A escalation + parasocial imprint (if founder-led)
- **Danger-zone clip (usually C3)**: pattern interrupt + loop B open
- **Hero clip**: variable-reward payoff + signature image
- **Final clip**: loop-back to C1 frame + final question or reframe

### Phase 3 — Clip-by-Clip Prompt Build

Read [references/seedance-mechanics.md](references/seedance-mechanics.md) and [references/effects-breakdown-reference.txt](references/effects-breakdown-reference.txt).

For each clip, produce the 4-section output structure:

1. **SHOT-BY-SHOT EFFECTS TIMELINE** — each sub-beat inside the 8s (1-4s beats), effects stacked, camera move, transition out
2. **MASTER EFFECTS INVENTORY** — for the whole video (all clips combined), numbered effect list with usage count and shot refs
3. **EFFECTS DENSITY MAP** — per-clip density rating (HIGH / MEDIUM / LOW)
4. **ENERGY ARC** — full-video arc, act-by-act

AND produce the actual paste-into-Higgsfield prompt per clip with:
- Shot structure declaration at top (`One continuous shot, 8 seconds, 24fps, 16:9`)
- Reference slot assignments (`@Image1 = start frame, @Image2 = character anchor...`)
- 5-layer stack (Subject → Action → Camera → Style → Constraints)
- 60-100 word sweet spot per clip (hard cap — Seedance drops later instructions past 200 words)
- Micro-action timeline with sub-second timestamps
- Quality suffix + negative constraints
- Content-filter preflight per [references/content-filter.md](references/content-filter.md)

### Phase 4 — Stitch + Audio Plan

Read [references/multi-clip-stitching.md](references/multi-clip-stitching.md). Produce:

- **Transition technique per cut** (audio-first J-cut / whip pan / through-object / match cut / anchored-object / last-frame-to-first-frame chain)
- **Audio plan** — track choice first, BPM-synced cut points, snare-cut rule, VO or no-VO decision
- **Pacing curve** — on-screen duration per clip (8s ≠ 8s on screen; trim for rhythm)
- **Grade matching** — unified LUT across clips
- **Loop-back** — final frame mirrors C1 frame

### Phase 5 — Character Consistency & Assets (optional)

Read [references/character-consistency.md](references/character-consistency.md). Execute only when:
- User has multi-character scenes, OR
- User explicitly asks for images/assets ("also create the images")

If active:
- Design character reference sheets (4-view: front / 3-4 / profile / back, white BG)
- Generate via NanoBanana Pro → save to `~/Desktop/claudedump/{project}/` or equivalent
- Generate start frames / hero frames with multi-ref NanoBanana calls
- Lock identity line (verbatim across all clip prompts)

Only call NanoBanana Pro when the user explicitly says to create images. Default is prompts-only.

### Phase 6 — Vault Output

Write the full plan to the project's vault folder:

```
🏰 300-Empire/{project}/video/{video-name}/
├── brief.md                    # Audience intake + strategy decisions
├── shot-list.md                # Clip-by-clip plan with jobs + signature
├── clip-01-prompt.md           # Ready-to-paste Higgsfield prompt
├── clip-02-prompt.md           # ...
├── stitch-plan.md              # Transitions, audio, pacing curve
├── assets-log.md               # Character refs + hero frames (if used)
└── iteration-log.md            # What worked, what drifted, next iteration
```

If the project folder doesn't exist yet, ask the user before creating it. If the video is generic (not project-linked), write to `🏰 300-Empire/_video-experiments/{video-name}/` instead.

## The Prompt Output Structure (per clip, ready to paste)

```
One continuous shot, 8 seconds, 24fps, 16:9 widescreen. Single take. No cuts.

REFERENCES:
@Image1 = [role: start frame / composition anchor]
@Image2 = [role: character anchor]
@Image3 = [role: ...]

SCENE: [Subject] [Action] [Camera] [Style] [Constraints].

MICRO-BEATS:
• 0.0-1.0s — [beat]
• 1.0-2.0s — [beat]
• 2.0-4.0s — [SIGNATURE beat if this is hero clip]
• 4.0-6.0s — [beat]
• 6.0-8.0s — [beat + transition out]

LIGHTING: [specific — golden hour / rim light against dark / volumetric fog]
CAMERA: [one primary move, timed]
STYLE: [aesthetic code matching audience]
QUALITY SUFFIX: sharp clarity, natural colors, stable picture, no blur, no ghosting, no flickering
DO NOT: [negative constraints — no dialogue, no text overlays, no character drift, etc.]
```

## Core Operating Principles

1. **Frame 1 is subcortical.** Face / motion / anomaly / pre-verbal trigger in the first 200ms or the video is dead.
2. **Stopping creates a debt.** Pay it within 8s via an open loop that feels 20-40s from resolution. Far gaps → frustration → bounce.
3. **Stack loops; never close them all at once.** At any moment, at least one unresolved thread must be live.
4. **Visual + Text + Verbal parallel channels.** Text wins muted views (50%+ of first-pass).
5. **Parasocial > Production polish.** Eye contact, handheld, intimate setting, second-person singular.
6. **Tribe encoded in frame 1.** The "for me / not for me" gate is the real filter — not quality.
7. **Variable-ratio rewards inside the clip.** Scatter payoffs at irregular intervals.
8. **Loop-back to start.** Final frame visually mirrors C1 frame. Rewatch is both algorithm signal and Zeigarnik payoff.
9. **One variable per iteration.** Never change multiple prompt elements between rolls — you lose signal on what helped.
10. **Physics over captions.** "Powerful drilling arm thrusts forward, impaling with sickening crunch" >> "the mech punches the monster".
11. **Lighting is highest-leverage single element** per word — more quality improvement than any other prompt element.
12. **60-100 word sweet spot per clip.** Beyond 200 words, Seedance drops later instructions.
13. **Separate subject motion from camera motion** always. "Spinning camera around dancing person" = jitter. "Dancer spins, camera holds" = clean.
14. **"Fast" unqualified accelerates everything.** Make ONE element fast, explicitly hold rest steady.
15. **Hero-late for launches, hero-early for paid social.** Double-anchor (C1 tease + C5 payoff) is highest-impact.

## Reference Files

Read these on demand, not all at once:

| File | When to read |
|---|---|
| [references/framework-overview.md](references/framework-overview.md) | Full 4-layer framework with cross-layer multipliers |
| [references/psychology-mechanisms.md](references/psychology-mechanisms.md) | 16 mechanisms (orienting response, Zeigarnik, MrBeast stack, etc.) with tactical moves |
| [references/audience-presets.md](references/audience-presets.md) | CT, masculine-philosophy, health-women presets — tribes, language, aesthetics, cringe detectors |
| [references/narrative-spines.md](references/narrative-spines.md) | 7 named structures mapped to clip counts, with per-clip jobs |
| [references/seedance-mechanics.md](references/seedance-mechanics.md) | How Seedance 2.0 parses prompts — sweet spots, physics-not-captions, 5-layer stack, multi-shot syntax |
| [references/multi-clip-stitching.md](references/multi-clip-stitching.md) | 8 seam-hide techniques, audio-first editing, pacing curve math |
| [references/character-consistency.md](references/character-consistency.md) | 4-layer consistency system, reference slot roles, identity-line discipline, NanoBanana asset flow |
| [references/content-filter.md](references/content-filter.md) | 3-stage filter pipeline, role-based adult anchor, Chinese bypass, shield tokens |
| [references/proven-prompts.md](references/proven-prompts.md) | Locked-in patterns with working examples — launch trailer, brand film, product reveal, philosophy content |
| [references/effects-breakdown-reference.txt](references/effects-breakdown-reference.txt) | Hoka brand film — shot-by-shot effects timeline example (preserved from original skill) |

## Creative Principles (preserved from original skill)

1. **Contrast drives impact.** Alternate high-density and low-density moments.
2. **Signature moments matter.** One hero effect per video, called out explicitly.
3. **Transitions are shots.** Whip pan, bloom flash, motion blur smear — creative moments, not throwaway cuts.
4. **Specificity over vagueness.** "Rotates clockwise by 15-20°" > "camera tilts". "20-25% speed" > "slow motion".
5. **Energy must resolve.** Final moments feel intentional, not like effects budget ran out.

## Tone & Style

- Director's shot notes, not marketing brief
- Bullet points within shot blocks for clarity
- No hype language ("stunning", "breathtaking", "epic") — describe what happens
- Concise but complete — every detail earns its place
- When writing the paste-ready prompt, stay within Seedance's 60-100 word sweet spot per clip

## Duration Calibration

| Target duration | Clip count | Signature effects |
|---|---|---|
| 24s | 3 clips | 1 |
| 32s | 4 clips | 1 |
| 40s | 5 clips | 1-2 |
| 48s | 6 clips (default for launches) | 2 |
| 56s | 7 clips | 2-3 |

If target duration is outside this range, ask why. The framework is optimized for 4-7 × 8s. Single clips or 60s+ videos need different treatment.

## Decision Protocol — When to Ask vs When to Decide

**Ask the user when:**
- Audience tribe is genuinely unclear (not in presets and user hasn't described)
- Signature moment / payoff image isn't obvious from brief
- Character identity needs locking but no references exist
- User hasn't indicated whether they want images generated or just prompts
- Vault project path is ambiguous (no matching folder in `🏰 300-Empire/`)

**Decide without asking when:**
- Clip count is within 4-7 range and duration is standard
- Narrative spine is obvious from video type (launch → crypto-launch spine)
- Anchor placement is standard for the type (launches = hero-late)
- Audience matches a preset cleanly
- Aesthetic code is clear from the audience preset

When deciding unilaterally, state the decision and the reasoning in one line so the user can redirect if wrong.
