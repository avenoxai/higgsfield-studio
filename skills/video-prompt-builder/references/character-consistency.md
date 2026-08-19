# Character Consistency — 4-Layer System + NanoBanana Asset Flow

Character drift is the #1 killer of multi-clip AI videos. These 4 layers, stacked, kill drift.

---

## The 4-Layer System

### Layer 1: Reference Sheet (hero asset)
A static image that locks the character's identity. Generated via NanoBanana Pro.

**Format:**
- 4 views side-by-side: front / 3-4 / side profile / back
- White background, no shadows, no decorations, no text
- Subject centered, consistent proportions across all angles
- 16:9 wide composition
- Art style declared explicitly (cartoon illustration, B&W pencil-shaded, semi-realistic painterly, chibi anime, loose sketch, etc.)

**Prompt template (NanoBanana):**
```
Character reference sheet on pure clean white background. Four views side-by-side
in one horizontal image: front view, 3/4 view, side profile view, back view.

Subject: [CHARACTER DESCRIPTION — canonical style, outfit locked, expression locked]

Same character, same outfit, same art style across all four angles. White
background, no shadows, no decorations, no text. Consistent proportions.
Reference sheet layout, equal spacing between views. Wide 16:9 composition.
```

### Layer 2: Identity Line (prompt discipline)
The EXACT SAME descriptor string, verbatim, in every clip prompt. Never paraphrase. Pin 2-3 hard physical traits + age range + one signature detail.

**Examples:**
- `"30-year-old Turkish man, shaved head, angular jaw, black linen shirt, silver chain — same character as previous shots"`
- `"anthropomorphic humanoid cobra, iridescent deep sapphire and midnight blue scales, cobra hood partially flared, glowing electric-cyan eyes, long black navy wool trench coat with high collar"`
- `"chibi anime girl, mossy green bob-cut leafy hair, freckles across cheeks, grey collared shirt with soft pink pastel collar"`

**Anti-pattern:** Alternating between "young woman" / "girl" / "lady" — pick one and stick with it across all clips.

### Layer 3: Reference Slot Assignments (Seedance @Image syntax)
Use the reference sheet from Layer 1 as a character anchor in every clip that features the character.

```
@Image1 = start frame (if using last-frame-to-first-frame chaining)
@Image2 = character anchor (the ref sheet)
@Image3 = second character anchor (if multi-character scene)
```

### Layer 4: Last-Frame Chaining (extend Layer 1 with shot continuity)
Extract the final frame of clip N → use as start frame of clip N+1.

Combines with the reference sheet: the ref sheet handles identity drift; the last-frame handles environment/pose drift.

---

## Multi-Character Scenes

### Limits
- **1-2 characters per shot** is the practical ceiling for Seedance
- **3 characters** is possible but identity bleed / amalgamation risk is high
- **4+ characters** = accept mixed-media aesthetic as the solution (different art styles keep them visually distinct even when Seedance mixes features)

### The Amalgamation Glitch
When two characters touch or overlap closely, Seedance can merge their features into one face. Avoid prompts that have characters:
- Embracing, hugging, fighting in close contact
- Both looking directly at each other in tight 2-shot framing
- Sharing a prop (passing a cup, etc.)

If you need intimacy/contact, split into two clips — one per character — and match-cut between them.

### The "One Face Per Shot" Rule
Only show a character's face clearly in ONE shot per multi-shot sequence. Use back/side/obscured angles for other shots.

### Multi-character prompt pattern
```
@Image1 = Character A ref sheet
@Image2 = Character B ref sheet

SCENE: Character A [detailed identity line] on the left side of frame.
Character B [detailed identity line] on the right side of frame.
They do not touch. Clear spatial separation. [Action] happens between them.
```

---

## NanoBanana Asset Flow (when user requests images)

Run this flow ONLY when the user explicitly asks ("also create the images" / "generate the refs" / "make the assets"). Default is prompts-only.

### The generate script
```bash
bash ~/.claude/skills/nanobanana/scripts/generate.sh "prompt" output_path [ref_image]
```

Supports one reference image. For multi-reference generation (e.g., swapping a character in an existing scene), write a small Python script or use the multi-ref helper at `/tmp/nanobanana_multi.py` if it exists.

### Multi-ref helper pattern
```python
#!/usr/bin/env python3
"""Call Gemini image gen with multiple reference images."""
import json, base64, os, sys, urllib.request, ssl

SSL_CTX = ssl._create_unverified_context()

def main():
    prompt = sys.argv[1]
    output = sys.argv[2]
    refs = sys.argv[3:]  # multiple reference paths

    # ... (load api key, build parts with inlineData for each ref)
    # ... (POST to gemini-3-pro-image-preview, fallback to 2.5-flash-image)
```

### Workflow for multi-character scenes

1. **Generate individual character reference sheets** (one per character, white BG, 4 views each)
2. **Generate the establishing frame** (all characters in scene) — use character refs as multi-input
3. **Iterate until locked** — check each character for likeness/style fidelity; iterate with stricter prompt pinning if any drift
4. **Save to project folder:** `~/Desktop/claudedump/{project}videoassets/` or equivalent
5. **Log in iteration-log.md** what locked, what took multiple rolls, what couldn't lock

### Default asset folder structure
```
~/Desktop/claudedump/{project}videoassets/
├── ref-{character-name}.png       # Layer 1 reference sheets
├── start-frame-{clip-N}.png       # Per-clip starting frames
├── hero-frame-v1.png              # Hero frame / establishing (iterate v1 → v2 → vN)
└── _source/                       # Source inputs (user-provided refs, brand assets)
```

### Iteration rule for assets
- Keep locked versions (v3, vN-locked)
- Delete intermediate rolls (v1, v2, superseded) BEFORE moving on — clutter accumulates fast

---

## Anti-Patterns That Kill Consistency

- Changing lighting descriptors between clips — face reads differently under different light
- Re-rolling a clip in isolation and cherry-picking the best-looking one — it may not match neighbors
- Paraphrasing the identity line — model reinterprets synonyms
- Using multiple lens descriptors across clips ("35mm wide" / "85mm portrait") — face geometry shifts
- Letting Seedance invent wardrobe details — pin every visible garment explicitly
- Generating characters from scratch per clip without feeding the reference sheet — drift guaranteed

---

## When to Accept Mixed-Media Aesthetic as Feature, Not Bug

If your scene has 4+ characters with distinct art styles (Pepe cartoon + Wojak linework + Gigachad B&W pencil + Vitalik semi-realistic + Milady chibi + mfer sketch), DO NOT try to unify them. The mixed-media clash IS the aesthetic.

Prompt:
```
ART-STYLE RULE (critical): each character retains their native art style from
their reference sheet throughout the shot. The mixed-media clash is intentional.
Do NOT unify them into one coherent style — the room itself is rendered
semi-realistic cinematic, while the characters live in their own meme tradition.
```

This works because Seedance has enough style-token capacity to preserve distinct styles if explicitly instructed. The anti-pattern is trying to unify everything into "photorealistic" and losing the meme recognition.

---

## The Continuity Column Table (pre-generation discipline)

Before generating any clip, fill this table for every character and major variable:

| Variable | C1 | C2 | C3 | C4 | C5 | C6 |
|---|---|---|---|---|---|---|
| Character A face | ref-a.png | ref-a.png | ref-a.png | (back-angle) | ref-a.png | ref-a.png |
| Character A wardrobe | navy trench, high collar | same | same | same | same | same |
| Lighting style | amber pendant + teal shadow | same | same + fog | same | golden rim added | same |
| Lens | 35mm wide | 35mm wide | 50mm | 50mm | 35mm | 35mm |
| Color grade | amber/teal | amber/teal | amber/teal | amber/teal | amber/teal | amber/teal |
| Camera move | orbital-right | dolly-push | rack focus | whip-pan-out | crane-down hero | static close |

If any cell changes, it must be intentional. Accidental drift is death.
