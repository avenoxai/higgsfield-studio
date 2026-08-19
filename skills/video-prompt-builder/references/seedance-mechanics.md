# Seedance 2.0 Mechanics — How The Model Actually Thinks

Distilled from extensive real-world experimentation. Use these as operating constraints, not suggestions.

---

## Prompt Parsing Mechanics

- **Left-to-right attention decay.** First 2-3 instructions land reliably. After ~8 specific requirements, only 4-5 actually execute. Put the most important beats FIRST.
- **Not a keyword list.** Seedance is an LLM intent classifier that reads holistically. Stripping individual bad words doesn't work — the model reads context, stacking, narrative purpose.
- **Parallel English-biased keyword scanner** runs alongside the LLM as a fast-path safety net. This is why Chinese prompts bypass many filter cases.

## The Sweet Spot

- **60-100 words per clip.** Hard rule.
- **Under 30 words** → generic output, model fills too many variables from statistical mean.
- **30-60 words** → functional but thin.
- **100-200 words** → diminishing returns.
- **Over 200 words** → model drops later instructions entirely.

## Specificity Hierarchy (strongest → weakest impact per word)

1. **Lighting** description — highest quality lift per word of any prompt element
2. **Subject specificity** — face geometry, clothing detail, object detail
3. **Camera movement** — timed, single primary move
4. **Style anchors** — art-direction vocabulary
5. **Quality modifiers** — resolution, "cinematic," etc. — weakest; often actively degrade output

**Avoid entirely:** "fast" (unqualified), "cinematic" (alone), "epic," "amazing," "stunning," "lots of movement," "glow." These describe feelings, not visual instructions. Seedance handles vague emotion words poorly.

## Physics Over Captions (Critical)

Seedance responds dramatically better to physics descriptions than caption-style prompts. The model learned mass-and-force signatures from real footage — physics words activate those signatures directly.

| Weak (caption) | Strong (physics) |
|---|---|
| "The mech punches the monster" | "The mech's powerful drilling arm swiftly thrusts forward, brutally impaling the monster's head with a sickening crunch and a violent explosion of dark crimson blood" |
| "The woman walks into the room" | "She steps through the doorway, her weight transferring forward, coat tails billowing behind from the draft, dust motes swirling in the amber shaft of light she disrupts" |
| "The car drives fast" | "The car rips through the corner, rear tires losing grip for a split second, dust and small stones spraying outward from the undercarriage, suspension compressing as weight transfers" |

Physics descriptions include: mass transfer, recoil, muscle tension, dust/particle behavior, cloth dynamics, weight shifts, air displacement, impact aftermath.

## The 5-Layer Stack (per clip)

Structure every prompt in this exact order:

```
1. SUBJECT       — who/what is in the frame, pinned to center of gravity
2. ACTION        — what must move (kinetic anchor)
3. CAMERA        — one primary move, timed
4. STYLE         — visual flavor without hijacking motion
5. CONSTRAINTS   — guardrails at the end
```

Example:

```
SUBJECT: A lean anthropomorphic cobra in a long navy trench coat, iridescent
sapphire scales on hands and face, glowing electric-cyan eyes, high collar up.

ACTION: He walks slowly through the smoky backroom, each step measured,
scales catching amber rim-light, breath visible in the cool air.

CAMERA: Low-angle tracking shot, 35mm lens, Steadicam smooth, camera holds
pace with him, subtle forward push across the 8 seconds.

STYLE: Amber and teal color grade, heavy low-key shadows, volumetric fog,
35mm film grain, Reservoir Dogs meets The Godfather.

CONSTRAINTS: sharp clarity, natural colors, stable picture, no blur,
no ghosting, no flickering. Character face stable, scales consistent.
No dialogue, no text, no logo.
```

## The "Fast" Trap

Using "fast" unqualified accelerates every kinetic element — camera, subject, background motion, particles, everything. Result: jitter and compression artifacts.

**Fix:** Make ONE element fast, explicitly hold others steady.

| Wrong | Right |
|---|---|
| "fast action scene, the warrior runs through the forest, the camera follows, leaves flying" | "The warrior sprints through the forest at high speed, camera tracks alongside at matching pace with smooth gimbal stabilization, background movement at natural walking speed" |

## Camera Language Rules

- One primary camera move per generation, OR a clearly sequenced compound move with timestamps.
- **Always separate subject motion from camera motion.** Ambiguity causes jitter.
  - Wrong: "Spinning camera around a dancing person"
  - Right: "The dancer spins slowly; camera holds fixed framing"
- Use descriptive rhythm ("slow," "smooth," "gentle") over technical specs (f-stops, ISO, mm) when possible. The model responds to descriptive language better than numbers, though 35mm / 50mm / anamorphic / Steadicam / dolly are understood.

## Lighting Keywords Library (highest-leverage additions)

Single highest quality-per-word lift per category:

| Keyword | Effect |
|---|---|
| `golden hour` | Warm low sun, long shadows — the single most reliable quality boost |
| `rim light against dark background` | Cinematic edge separation |
| `soft key from 45 degrees` | Flattering talking-head lighting |
| `overcast daylight` / `even overcast diffused light` | Kills flicker in bright scenes |
| `backlit silhouette at sunset` | Dramatic mood |
| `motivated lighting from practical source` | Realism with visible light source |
| `volumetric fog` | Atmospheric depth, pairs with backlight |
| `chiaroscuro` | Godfather-style high-contrast shadows |
| `tungsten practical lighting` | Warm interior realism |
| `neon practical` / `neon rim` | Cyberpunk/modern feel |

## Time-Coded Multi-Shot Syntax (single-generation)

Seedance accepts explicit timestamps inside a 15-second generation:

**Format A (range brackets):**
```
[0-4s]: wide establishing shot, static camera, misty bamboo forest at dawn
[4-9s]: medium shot, slow push-in, fighter steps forward, silk kimono billowing
[9-15s]: close-up, orbit shot, fighter strikes, slow motion
```

**Format B (parenthetical):**
```
(0-3s) macro shot of perfume bottle among pink flowers, shallow depth of field
(3-7s) camera glides closer, a feminine hand enters frame, touches the bottle
(7-12s) slow-motion spray, mist diffuses in air, particles catching rim light
```

Use for: continuous scenes that need 2-3 beats within a single generation. Don't use for multi-clip videos — each clip is its own generation.

## The @ Reference System (Higgsfield, up to 9 slots)

Every uploaded file needs an explicit role. Untagged images get ambiguous processing.

```
@Image1 as character reference (maintain exact facial features and outfit)
@Image2 as environment reference (match lighting and color palette)
@Video1 for camera motion reference (replicate the slow orbit movement)
@Audio1 as background music (sync scene transitions to beat positions)
```

For multi-character scenes:
```
@Image1 = start frame
@Image2 = character anchor (Shiba: tan fur, black suit, smug Doge face)
@Image3 = character anchor (Wojak: bald, trench coat, stoic)
@Image4 = character anchor (Gigachad: B&W pencil-shaded, turtleneck)
...
```

## The Quality Suffix (append to every prompt)

```
sharp clarity, natural colors, stable picture, no blur, no ghosting, no flickering
```

For character shots, add:
```
Face stable without deformation, normal human structure, natural and smooth movements.
```

For cinematic work, add:
```
Cinematic texture, soft film grain.
```

## Iteration Rule — One Variable Per Pass

1. Generate 2-3 baseline options
2. Change ONE variable (camera, lighting, speed modifier, subject detail)
3. Score each against neighbors (never in isolation)
4. Keep the best
5. Change one more variable
6. Repeat

Slower per cycle, converges faster. Isolates what helped vs. hurt.

PJ Ace's public workflow: 5-10 variations per shot, picks on physics realism + motion dynamism, not on "looks cool." For a 6-clip video, that's 30-60 total rolls. Budget accordingly.

## What Breaks Consistency Across Clips (Anti-Patterns)

- Changing lighting descriptors between clips ("golden hour" in C1, "soft daylight" in C2 — face reads different)
- Under-specifying wardrobe — model improvises and drifts
- Switching lens language ("35mm wide" → "85mm portrait" without intent — face geometry changes)
- Re-rolling a single clip in isolation — you'll cherry-pick one that looks great alone but doesn't match C2 and C4
- Paraphrasing the identity line between clips — use verbatim

## Key Constraints

- **Max continuous duration:** 8-10 seconds reliably. Past 10s, character consistency slips, motion gets muddy.
- **Aspect ratios:** 16:9, 9:16, 1:1. 9:16 is default for social short-form.
- **fps:** 24fps cinematic default. Higher fps possible but uses more compute.
- **Multi-reference cap:** 9 images on Higgsfield; 4 on most other platforms.
- **Content filter:** strictest among 2026 models (~37% false-positive rate on legitimate creative). See `content-filter.md` for bypass protocols.

## When to Escalate Out of Seedance (Out of Scope for This Skill)

Not in scope — the user has chosen Seedance 2.0 as the locked model. But flag if:
- Content filter hard-blocks after 3 rewrites + Chinese bypass
- Dialogue-heavy scene with complex lip-sync (Seedance struggles)
- Full-body acrobatics or martial arts requiring crisp physics (Hailuo 2.3 better)
- Ultra-low budget volume testing (Hailuo 2.3 ~3× cheaper)

Tell the user and stop. Don't silently switch models.
