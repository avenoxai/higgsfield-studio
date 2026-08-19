# Multi-Clip Stitching — Hide The Seam

Every AI-video seam is a tell. These 8 techniques hide them. Pick one per transition.

---

## 8 Seam-Hide Techniques

### 1. Audio-first J-cut (HIGHEST ROI — use as default)
Bring the next clip's audio in 0.5-1s before the visual cut. The viewer's ear commits to the next beat before the eye sees the splice. Works on 90% of transitions.

**How to prompt:** N/A — this is an NLE edit, not a Seedance directive. Plan in the stitch-plan.md file.

### 2. Whip Pan (highest Seedance-friendly)
End clip N with fast horizontal motion blur; start clip N+1 mid-whip. Seedance handles whip motion well if prompted explicitly.

**End-of-clip prompt:**
```
Final second: fast horizontal camera whip to the right, heavy directional motion
blur, the frame smears into a blur of color.
```

**Start-of-next-clip prompt:**
```
First second: camera emerges from a fast horizontal whip pan, motion blur
resolving into the new scene as speed decelerates rapidly.
```

The blur frames hide identity drift entirely.

### 3. Through-the-Object Cut
Move camera through a foreground element (flame, smoke, door, hand) at the cut. The object swallows the seam.

**End-of-clip prompt:**
```
Final 1.5 seconds: camera pushes forward into the [object] filling the frame
until the shot is completely engulfed in [black / fire / smoke / fabric].
```

**Start-of-next-clip prompt:**
```
First 1.5 seconds: camera emerges from [matching object], pulling back to reveal
the new environment.
```

### 4. Match Cut (highest-craft)
End clip N and start clip N+1 on the same shape/motion/color field. Circle at end of N (coin edge) → circle at start of N+1 (iris, sun).

**Both clips:** prompt end/start frames toward matching geometric/organic shapes. Hardest technique to execute — bias both prompts explicitly.

### 5. Speed Ramp Through Transition
Ramp clip N's final 0.5s to 150-200% in NLE; hard cut; ramp clip N+1's first 0.5s back down to normal. Reads as one continuous move.

NLE edit, not Seedance directive.

### 6. Blackout / White Flash (cheapest fallback)
End clip N into darkness or overexposed flare; open clip N+1 from the same. Good for disguising lighting/wardrobe drift.

**End-of-clip prompt:**
```
Final second: the scene rapidly fades to pure black / pure white, a single
highlight bloom consuming the frame.
```

### 7. Anchored Object (best for character continuity)
One object lives in both the last frame of clip N and the first frame of clip N+1 (token, drink, hand, prop).

Use last-frame-to-first-frame chaining — grab final frame of N, use as start image for N+1 generation.

### 8. Last-Frame-to-First-Frame Chaining (single highest consistency move)
Extract the final frame of clip N. Use it as the start-frame reference for clip N+1. The overlap forces visual continuity.

Seedance 2.0 supports this via @Image1 reference slot. When building clip N+1:
```
@Image1 = [last-frame.png from clip N] — start frame for this clip
```

Then the clip N+1 prompt begins with the ending state of clip N. 80% of character/environment drift disappears.

---

## Grade Matching (post-production, 60% of AI-ness reduction)

Apply one LUT / Lumetri curve across all clips. Match black point, highlights, skin tones.

Seedance outputs drift in color temperature across generations MORE than they drift in content. Unifying the grade is the single cheapest way to hide AI-ness.

**Target grade per audience (from audience-presets.md):**
- CT degen: amber/teal, slight crush on shadows
- Stoic/philosophy: desaturated, high-contrast B&W-adjacent
- Health-women: warm cream/beige, soft highlights, no crush
- Masculine self-improvement: cold blue shadows + warm highlights

---

## Pacing Curve — 8s ≠ 8s On Screen

Pros cut INTO their generations. Typical on-screen durations for a 6-clip, 48s-generated sequence:

| Clip | Generated | On-screen | Job |
|---|---|---|---|
| C1 | 8s | 6-7s | Hook, breathe a little |
| C2 | 8s | 5-6s | Confirm |
| C3 | 8s | 3-4s + punctuation | Accelerate |
| C4 | 8s | 4-5s | Pre-hero |
| C5 | 8s | 7-8s | Hero — let it breathe |
| C6 | 8s | 3-4s | Tag / loop-back |

Total runtime 30-38s from 48s of generated footage. **Trim for rhythm, not block size.**

### Punctuation Inserts (1-2s between 8s clips)
Drop a single cut-in between high-tension clips: logo flash, typographic card, still-with-micromotion, frame-grab push-in. Breaks the "generated metronome" feel.

### The S-Curve
- Front: slower cuts (7s, 6s) — let audience settle
- Mid: fastest cuts (3s, 3s, 2s) — acceleration
- Hero: held (7-8s) — let the signature breathe
- Tag: quick (3s) — punctuation close

### Anti-Pattern
"7 clips × 7s flat" = dead-flat AI slideshow. Pacing must have an S-curve.

---

## Audio-First Editing (always)

Music leads the cut. For multi-clip AI specifically, audio is your primary continuity layer because visual seams need masking.

### Rules:
1. **Pick the track before generating.** BPM, drops, arrangement dictate clip count and cut points.
2. **Snare-back-beat rule.** Cut on snares, not kicks. Snares are the eye's rhythmic anchor.
3. **Every clip boundary lands on/1-2 frames before a snare.**
4. **VO added LAST, never first.** VO hides weak transitions; starting with VO over-relies on it.
5. **Pre-cut the audio bed** in Logic/Splice/Ableton with transition points flagged. Cut visuals against the waveform.

### When to Use Music vs Sound Design

| Content type | Default |
|---|---|
| Token launch, brand film, product reveal | Sound design bed > music (reads less AI-commercial) |
| Music-video / dance / aesthetic montage | Track first |
| Philosophy/stoicism | Sparse ambient + deep VO |
| Health/wellness | Warm piano/trending TikTok audio |
| CT meme video | Meme sound-effect library, 8-bit, or ironic classical |

### BPM-Matched Camera Motion
PJ Ace's trick: match camera move duration to musical phrasing.

At 120 BPM, 4 beats = 2s. So: "camera pushes in over 2 seconds" matches a 4-beat phrase.

For Seedance, this means timing camera moves in prompts to musical units.

---

## Loop-Back (final-frame to first-frame)

Colin & Samir data: looped shorts get 143% more views.

Final frame of clip N (the last clip) visually mirrors first frame of clip 1. Seamless rewatch = algorithm signal + Zeigarnik payoff.

**How to achieve:**
- Write C1 opening frame first
- When writing the final clip, reverse-engineer its end to mirror that opening frame
- If the video has a signature object (ticker, product, face), ensure both C1 and final-clip-end contain it in similar framing

---

## The Stitch-Plan Document (output to vault)

Every video's stitch-plan.md should contain:

```markdown
# Stitch Plan — [Video Name]

## Track
- Title:
- BPM:
- Duration used:
- Drop positions:
- Cut points (beat-aligned):

## Transitions
| Cut | Technique | NLE notes | Last-frame / First-frame chaining? |
|---|---|---|---|
| C1→C2 | Audio-first J-cut | Audio enters at 6.5s, visual cut at 7.0s | No |
| C2→C3 | Whip pan | Speed ramp 150% last 0.3s | Yes (last frame → C3 start) |
| C3→C4 | Through-object (smoke) | | Yes |
| C4→C5 | Match cut (circle) | | No |
| C5→C6 | Audio-first J-cut | Track drop at 7.2s of C5 | No |

## Pacing Curve
| Clip | Generated | On-screen | Position justification |
|---|---|---|---|
| C1 | 8s | 6.5s | Hook — let Frame 1 breathe then accelerate |
| C2 | 8s | 5s | Confirm, trim |
| ... | | | |

## Grade
- LUT applied:
- Shadow tone:
- Highlight tone:
- Skin adjustment:

## VO (if any)
- Voice:
- Script timing:
- Sync points:

## Loop-back
- C1 frame 0 mirrors: [final frame description]
- Rewatch triggers: [what makes loop seamless]
```
