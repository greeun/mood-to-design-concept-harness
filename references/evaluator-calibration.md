# Evaluator Calibration — mood-to-design-concept-harness

> Few-shot score anchors (V1-5, V1-8). Concrete 1/3/5 anchors per criterion. The Evaluator reads this file before scoring and
> aligns its scores to these anchors (prevents score drift). Article: "calibrating the evaluator using
> few-shot examples with detailed score breakdowns ... reduced score drift across iterations."
>
> Anchor example mood: use **"cool and dreamlike"** as the running example. Apply the same *judgment logic* to
> other moods (the anchor is an example of the logic, not specific to that mood).
>
> When tuning: if you find a case the Evaluator missed in a real run, add a counter-example to that criterion's anchors (P-3 (c)).
>
> Anchor-density note: the 2×-weighted weak axes (C1 Fidelity, C2 Originality) get **two** worked examples per level,
> and the 1× verification axes (C3, C4) get **one** per level. Because score drift is larger on the axes Claude is weak at (V1-3),
> they need denser anchors — an intentional asymmetry.

---

## C1 — Fidelity (association fidelity) · 2×

Does the mood actually come alive in A/B/C. Measure = traceability (does each choice cite to a mood word).

**1/5 — actively betrays the mood / no traceability**
- Example A: the palette is warm coral + lively orange and the motion is a bouncy spring. It runs straight against
  "cool/dreamlike." And none of the choices cite a mood word.
- Example B: A names cool colors but B (code) does not use A's hex and used an arbitrary bright blue — the mood
  evaporated at the render stage.

**3/5 — only half alive**
- Example A: the cool blue palette is there, but the motion is snappy (120ms ease-in) and the layout is a standard SaaS 3-column grid —
  the color is right but the movement and space betray "dreamlike." Traceability is on only some choices.
- Example B: the hex is matched cold, but the typography is a tight geometric sans, so "dreamlike" looseness does not come alive.

**5/5 — the mood is obvious / every choice is traced**
- Example A: desaturated glacial blue-grey, low contrast, slow ease-out drift (`cubic-bezier(.22,1,.36,1)`
  ~800ms), soft-focus whitespace — and every choice is annotated "← 'cool'" / "← 'dreamlike'."
- Example B: A's exact hex and fonts appear verbatim in the rendered DOM, and fidelity holds through the later sections.

---

## C2 — Originality & Depth · 2×

Is it a reading unique to this mood, or did it flee to slop. **Judged by contrast against `cliche-contrast-examples.md`.**

**1/5 — generic "dreamy" slop kit**
- Example A: purple-pink gradient blob hero + glassmorphism cards + overuse of "ethereal." This sticks to any vague
  mood — it is not at all a reading unique to "cool/dreamlike." (Same as the slop side of the cliche-contrast pair "dreamy → gradient blob.")
- Example B: the dark-mode + neon cyan accent default. A lazy cliché that equates "cool" with neon.

**3/5 — avoided the worst slop but safe/obvious**
- Example A: the safe reading "blur + blue." No slop blob, but no insight unique to this mood —
  "cool dreamy = blurry blue" is what anyone thinks of.
- Example B: it chose a calm grey-blue, but typography·layout·motion are all default, so it lacks depth as a concept.

**5/5 — a concrete, surprising reading unique to this mood**
- Example A: "predawn fog over still water: near-monochrome with one cold mint accent, type set loose
  and slightly mis-registered like a half-remembered dream" — type set slightly mis-registered to translate
  "a half-remembered dream" into form. This could describe *only this mood*.
- Example B: taking the "submerged" implication, a single intentional device where content slowly desaturates as if
  sinking beneath the surface on scroll — not a recolor but a structural translation of the mood.

---

## C3 — Translatability (concreteness of translation) · 1×

Is it concrete enough to drop straight into code/prompts.

**1/5 — adjectives only**
- Example A: a list of adjectives like "calm, mysterious, soft." No hex, no named font, no easing.
- Example C: the image prompt is a single line "a dreamy cool scene" — no composition/lighting/texture/atmosphere at all.

**3/5 — some concrete, some empty**
- Example A: hex and fonts are named, but motion only says "smooth," not a usable curve.
- Example C: composition/atmosphere are there, but lighting or texture is missing, so it is not a drop-in.

**5/5 — everything drop-in ready**
- Example A: a full hex palette + named font character·weight + named easing (e.g. `cubic-bezier(.22,1,.36,1)`
  ~800ms).
- Example C: a complete prompt with composition + lighting + texture + atmosphere all specified, ready to paste
  straight into an image generator.

---

## C4 — Divergence Quality · 1×

Are the user-specified N candidates (default 3–4) genuinely different "worlds", or a recolor. (Judged by the candidate-comparison table. The anchor below is an N=4 example, but apply the same variation logic to smaller counts — e.g. for N=2, ask "are the two candidates genuinely different worlds.")

**1/5 — 4 color swaps of the same layout**
- Example: 4 candidates with the same 3-column grid + same typography, only the color changed to blue/teal/grey/navy.
  Layout logic·typography philosophy·spatial mood all identical. The user has no real choice → the P-1 gate is an empty shell.

**3/5 — 2 distinct, 2 near-duplicate**
- Example: "predawn lakeside haze" and "submerged brutalist" are genuinely different worlds, but the other two are
  variants of one of them with only a slight color change — the variation breadth is only half real.

**5/5 — all 3–4 genuinely different worlds**
- Example: "submerged brutalist concrete" (heavy concrete mass + slow vertical sinking motion) /
  "predawn lakeside haze" (near-monochrome + soft-focus whitespace) /
  "drowned-neon rain alley" (high contrast + wet neon reflection) /
  "frosted minimal lab" (cold white + frost texture + extreme whitespace) —
  each a different layout logic·typography philosophy·spatial mood. The user has a genuine choice.

---

## Cautions when scoring (preventing miscalibration)

- Do not judge C2 with a **banned-word scan** (deducting just because the word "ethereal" appears). Judge with contrast-pair
  logic: "does this stick to anyone, or to this mood only."
- Do not score **beauty/rhythm** on any criterion. "Is this mint accent pretty" is the user-delegated domain.
  C1 looks at "does mint trace to 'cool'", C2 at "is the single mint accent a reading unique to this mood" —
  not whether it is pretty.
- Attach evidence to every score. C1 = cited mood-word, C2 = contrast pair used, C3 = hex/easing strings·render result,
  C4 = candidate-comparison table.
