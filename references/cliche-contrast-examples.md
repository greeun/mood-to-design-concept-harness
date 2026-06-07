# Cliché Contrast Examples — mood-to-design-concept-harness

> This file does two things:
> 1. **Teaches the user/Generator** — the difference between translating a mood into slop vs into a reading unique to this mood.
> 2. **Calibrates the Evaluator's C2 (Originality & Depth) judgment.**
>
> **Cliché blocking is done with contrast examples, not a banned-word list.** The Evaluator does not scan words.
> For each translation it asks: **"does this stick to anyone like the slop examples, or does it stick to THIS mood only?"**
> If it's on the slop side, deduct C2. If it's on the distinctive side, credit C2.
> For each C2 judgment the Evaluator cites which of the pairs below it used.
>
> Core diagnostic question: **"if I attached this translation as-is to a different vague mood (e.g. 'warm', 'futuristic',
> 'elegant'), would it still make sense?" → if yes, slop (sticks to anyone). If it makes sense only for this mood, distinctive.**

---

## Why it is slop (common pattern)

A slop translation **does not read** the mood; it slides into AI design's default attractor:
generic gradient blob hero, glassmorphism cards, neon-on-dark, full-screen video hero,
plastered "ethereal/elevated/seamless" adjectives, meaningless 3-column layout. Because these come out the same for any
mood request, *they describe no mood.* A distinctive translation catches one concrete implication of the mood and translates it
into **a single intentional device of form, space, or movement.**

---

## Contrast pairs

### Pair 1 — "cool and dreamlike"

| | Slop (sticks to anyone) | Distinctive (sticks to this mood only) |
|---|---|---|
| Palette | purple→pink gradient blob, "ethereal" | desaturated glacial blue-grey + **one** cold mint accent |
| Typography | generic geometric sans, perfect alignment | loose letter-spacing + slightly mis-registered baseline (a half-remembered dream) |
| Motion | bouncy spring micro-interaction | 800ms slow ease-out drift, fades bleeding like fog |
| Diagnosis | the gradient blob that shows up in any "dreamy" request | "predawn fog over still water" — only this mood |

### Pair 2 — "like a submerged city"

| | Slop | Distinctive |
|---|---|---|
| Visual | full-screen water video + text overlay | a single device where content slowly desaturates·blurs as if sinking beneath the surface on scroll |
| Layout | standard hero + 3-column cards | an asymmetric grid where heavy concrete masses sink vertically |
| Diagnosis | water footage also sticks to "ocean", "summer", "spa" | translates the implication of "submerged" (downward, pressure, refraction) into structure |

### Pair 3 — "a grey overcast eerie alley"

| | Slop | Distinctive |
|---|---|---|
| Mood handling | dark-mode + red horror font (jump-scare cliché) | uniform matte grey + a single cold light source falling from afar, the stillness of empty whitespace |
| Diagnosis | the horror template sticks to any "horror game" | reads the *loneliness* side of "eerie" (not horror) — go here on re-steer |

### Pair 4 — "warm and cozy" — a contrast of the opposite mood

| | Slop | Distinctive |
|---|---|---|
| Palette | beige + rounded corners + "soft, friendly" adjectives | dusk amber + slightly charred texture, well-worn asymmetric whitespace |
| Diagnosis | "soft/friendly + rounded corners" is every D2C brand | translates the *time-accumulated* implication of "cozy" (age, touch) |

> This pair shows that "cozy"'s slop ("beige + rounded") is the *same laziness* as Pair 1's "dreamy" slop, in *different words* —
> sliding into the default attractor without reading the mood. Same diagnostic question for any mood.

### Pair 5 — render-stage slop (font·tone) — where the mood dies in *implementation*, not the concept

> Even if the concept (A) is distinctive, if B's render slides into defaults the mood disappears on screen. This does not
> surface well until a person sees it, so step 7's EARLY human gate must catch it (an actual run failed exactly here).

| | Slop (slid into defaults) | Distinctive (keeps the mood to the end) |
|---|---|---|
| Font (multilingual) | hand Korean to a Latin-only display font (Archivo Black, etc.) → **silent fallback to the system default gothic** → the intended "heavy signboard" dies as a rounded default font | a character-matched font that actually covers Korean glyphs (e.g. heavy signboard → 검은고딕/Black Han Sans), Korean·Latin unified in the same heavy family |
| Text tone | the title in flat **pure-white/near-white (#F5F5F5)** → reads as "default text", breaking the mood | a mood-matched value/tint (e.g. a cold slate-grey), or a depth gradient that "sinks from light above to shadow below" |
| Diagnosis | a rounded default font·pure-white text shows up on **any site** — a mood-agnostic default | the font character and text value directly read *this mood's implication* |

> Note: the final call on tone (beauty) is delegated to the person. But (a) **font script-coverage failure** and
> (b) **flat pure-white text** are verifiable default-slide signals — the Evaluator's font script-coverage probe and the tone
> self-check catch these, leaving only the fine aesthetic call to the person.

### Pair 6 — faking "atmosphere" with a CSS gradient (the most common dark-hero slop)

> Laying a **radial glow/light pool** in a corner to make a dark hero "look fancy" — AI design's #1 slop.
> It does not read the mood; it is decorative light that sticks to any dark page.

| | Slop (fake air) | Distinctive (real air) |
|---|---|---|
| Air/light | `radial-gradient(...rgba blush...)` corner glow, vague "moonlight washiness", blobs | **a real photo** as the background (loremflickr/picsum/image-gen) + a legibility scrim. Or a labeled image placeholder |
| Gradient allowance condition | decorative · "for atmosphere" → slop | only when it reads the mood's *physics* (e.g. "submerged"→a vertical gradient darkening with depth, traced to a mood word) |
| Diagnosis | that glow can be pasted onto a "warm/futuristic/elegant" dark hero as-is → slop | comes only from this mood's actual scene/physics |

> Rule: **do not make air with a decorative CSS gradient glow.** Air comes from real images. A gradient is allowed only when
> it reads the mood's physics and traces to a mood word (traceability).

---

## Self-review check for the Generator

Before handing off A/B/C, ask of each core choice:
1. Would this choice still make sense if attached as-is to a different vague mood? → if yes, slop. Dig again.
2. *Which concrete implication* of the mood did this choice translate into form/space/movement? If you can't say, it's slop.
3. Did it slide into a pattern in the slop column above (gradient blob, glassmorphism, neon-on-dark, full-screen video,
   "ethereal/seamless" plastering, meaningless 3-column)? → if so, replace it with an intentional device.

## C2 judgment check for the Evaluator

- Hold each translation against the closest pair above. If it resembles the slop column, deduct C2; if it resembles the distinctive column, credit it.
- **Cite the pair you used** in the judgment (e.g. "C2=2/5, Pair 1 slop side — purple-pink gradient blob hero,
  this sticks to any 'dreamy' request").
- **Do not scan a word list.** The presence of the word "ethereal" is not an automatic deduction —
  look at whether the translation *sticks to anyone*. A word scan is itself miscalibration.
