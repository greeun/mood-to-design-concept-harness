---
name: mood-to-design-concept-harness
description: A Planner→Generator→Evaluator harness that redefines vague screen associations, images, energy, and moods into an actual design concept. It runs a diverge (N distinct concept directions — ask the user for the count, default 3–4) → user-select → converge (sequentially generate concept brief A → frontend code B → image-generation prompt C) flow. Palette and motion aesthetics are delegated to a human checkpoint where the user picks from rendered output, and the Evaluator only scores divergence diversity, traceability, code render, prompt completeness, and example-based cliché. Triggers — KO "무드를 디자인 컨셉으로", "기운을 디자인으로", "연상을 컨셉으로", "분위기 컨셉 잡아줘", "무드보드 컨셉", "감성 디자인 컨셉", "이 느낌으로 디자인", "컨셉 발산 수렴", "무드 to 디자인 하네스". EN "mood to design concept", "vibe to design", "atmosphere to design brief", "turn this mood into a design", "mood board concept harness", "feeling to UI concept", "diverge converge design concept", "design concept from a vibe".
version: 1.0.0
---

# mood-to-design-concept-harness

A harness that turns a vague visual association, mood, or atmosphere ("like a submerged city", "cool and dreamlike")
into a **usable design concept**. It does not pull it out in one shot.
The **diverge → user-select → converge** loop is this skill's identity.

Convergence produces three sequential, independent deliverables.

- **A — Design Concept Brief**: concept name, mood narrative, color palette (hex), typography mood (named font character + role), motion/texture (named easing curves), layout principles, references described in words.
- **B — Frontend Code**: implements A. **Actually renders without errors** and uses A's exact hex/typography verbatim.
- **C — Image-Generation Prompt**: carries A's mood. Includes composition + lighting + texture + atmosphere keywords, drop-in ready to paste as-is. Append a soundscape descriptor only when the mood strongly implies ambient sound AND the user opts in (optional).

This harness ports Anthropic's *"Harness Design for Long-Running Application Development"*
principles (role separation, context reset, rubric evaluation, self-evaluation bias prevention) into the mood→design-concept domain.

## This skill's core principle: aesthetics by the human, only the verifiable by the machine

An LLM cannot reliably judge "is this palette beautiful", "is this motion rhythm good" (a sensory limitation).
So **the user looking at the rendering and choosing is the load-bearing human-visual checkpoint (P-1).**
The Evaluator **does not score beauty or rhythm.** Instead it scores only verifiable proxies:
divergence diversity, traceability, code render, prompt completeness, example-based cliché judgment.
An Evaluator that scores "is this palette pretty?" is **miscalibrated**, and that itself is a defect to be fixed.

## Do not build the visual craft by hand — delegate to vetted design skills (no bare-hand SVG/CSS)

> Field lesson: visuals that an LLM hand-codes from scratch in CSS/SVG (swatch cards, hand-drawn mockups) are
> **proven to come out tacky** — a direct corollary of the "aesthetics are an LLM weakness" principle above. Do not build visuals by hand.

- **Do not generate visuals with bare-hand SVG/CSS — hand-drawn illustration (flowers, shapes, icons, decorative SVG paths) is also entirely forbidden.**
  (Verified: an LLM-drawn petal SVG is just as tacky as a CSS mockup.) B (code) and the divergence visuals are **delegated
  to vetted design/visual skills.** **If decorative/illustrative imagery is needed, make it with a real image source (image generator), or, if none, leave a labeled image
  placeholder frame — and never draw it yourself.** The power of the visual comes from typography, layout, color, whitespace, and light.
- **Right after activation (step 1), discover and bind the available design skills.** Bind a well-regarded one as the B production engine, with
  reference-DB types (palettes, font pairings, UX) as auxiliaries.
- **Current preference (use first if present):** `design-taste-skill-pack` (= taste-skill-beta-v2, brief→style→premium
  frontend router) + `ui-ux-pro-max` (50 styles, palettes, font pairings, UX guide DB).
- **How to delegate:** if the target skill is installed, invoke it with `Skill`. If it is not installed but exists in the repo,
  **Read that skill's `SKILL.md`/`references/*` and apply its guidance (style, palette, font pairings, components).**
  Pass A's concept brief as that skill's "brief" input.
- **If no design skill is available at all**, do not fall back to bare-hand CSS — tell the user (ask which design skill to
  install/specify, or substitute image generation for the divergence). Visual quality is finally judged by the human (P-1).

## Role separation (each a separate `Agent` call, communicating only via files)

| Role | Prompt | Deliverables |
|------|--------|--------------|
| Planner | `references/planner-prompt.md` | `spec.md` |
| Generator | `references/generator-prompt.md` | A, B, C + `generator_report.md` |
| Evaluator | `references/evaluator-prompt.md` | `critique.md` |

Each role prompt is **fully self-contained (zero prior context).** The Generator never sees the intake conversation or
the Planner's reasoning — it reads only the `spec.md` file (V1-12).
The rubric is in `references/rubric.md`, the scoring anchors in `references/evaluator-calibration.md`,
and the cliché contrast pairs in `references/cliche-contrast-examples.md`.

## Activation Flow (orchestrator)

1. **Activation check (model/tier + visual-skill binding — done together).**
   - **(1a) Discover & bind visual skills.** Right after first load, discover the available **design/visual-craft skills**
     (the installed-skills list + the repo's skill folders). Bind a well-regarded, well-known one as the **B production engine**, and bundle
     reference-DB types as auxiliaries. **Current preference: `design-taste-skill-pack` (taste-skill-beta-v2) + `ui-ux-pro-max`.**
     If none exist, ask the user which design skill to use (no bare-hand CSS fallback — see "do not build the visual by hand" above).
     Announce the binding result to the user in one line ("B will be produced with the ◯◯ skill").
   - **(1b) Model detection & tier proposal (Gate 2).** Identify the current model. For Sonnet 4.6+ / Opus class, propose
     **Simplified**; for weaker or older models, propose **Full**. Show the "V1 vs V2 model guide" table below.
     **Wait until the user confirms/overrides the tier.** (The tier is not hard-coded.)
2. **Mood capture.** Take the user's raw associations, mood, and feelings in their own words.
   Do not demand more words — divergence is itself the disambiguation (you diverge even from just "an eerie feeling").
3. **DIVERGE — shown, not told.** First ask the user **"How many directions shall we compare?"**
   (default 3–4, recommended range 2–6; proceed with the default if no answer). Then create the user-specified **N distinct
   candidate directions** (Simplified can run this inline, Full as a separate step). **Regardless of the count, each candidate must be a genuinely
   different "world"** — not a recolor that only swaps the colors of the same concept (C4). E.g. "submerged brutalist /
   predawn lakeside haze / drowned-neon / frosted-lab".
   - **Visual divergence board is mandatory (load-bearing — text alone uses only half of this skill).** A text description +
     a hex list does not convey the mood. **The default mechanism of divergence is the "visual companion":** actually show the
     candidates. **But do not draw them with bare-hand SVG/CSS** (it comes out tacky — see "do not build the visual by hand" above).
     Produce the visuals in this priority order —
     - **(Priority 1) If an image generator is available**, generate a **mood reference image per candidate** and assemble them into a board (best for divergence).
     - **(Priority 2 · when no backend — a verified technique) Put real air in with a real-photo service.** Lay **a real photo as the background**
       of each candidate card (`<img>`), then place only a legibility scrim + typography on top — do not *fake* air with a CSS gradient (slop).
       Sources that are reachable in this environment: **`loremflickr.com/<w>/<h>/<keyword>?lock=N`**
       (keyword-mood-matched real photos, recommended) or `picsum.photos/seed/<seed>/<w>/<h>` (random real photos). Pick the image by the
       mood keyword, and if needed, match only the tone with a light filter (brightness/grayscale/hue).
     - **(Priority 3) The design skill bound in step 1** (design-taste-skill-pack / ui-ux-pro-max) to generate a typography/layout-led
       preview. For the type sample, use **a character-matched font that actually covers the content language's glyphs** (language-aware font gate).
     - **Wrap it in a companion frame (the Superpowers brainstorming-frame pattern):** a top header (title + status) + a content
       grid + a bottom selection-indicator bar, OS-native light/dark. Combine the N cards into one board and render it **once** →
       **inline the PNG into the conversation with Read** + **also open it in the browser with `open` (both by default).**
     **No text+hex lists, ASCII previews, bare-hand CSS mockups, or CSS fake-gradient air.** Render the board once (efficiency principle).
4. **Human-visual selection gate (P-1, load-bearing, Gates 1+3).**
   Present all candidates and **STOP.** The user picks one. Or, if they reject all and re-steer
   → go **back to step 3** with the new steer. Do not force convergence toward a rejected direction. Record the selection.
   (Gate 3 = re-steer: "this is too horror, I want the lonely side" → re-run divergence.)
5. **Plan (convergence begins).** Before dispatching the Planner, **first lock the target medium** — ask the user "Which screen/medium
   shall we implement?" (landing section, web component, app screen, etc.), and if there is no answer, set a reasonable default (a single
   responsive landing section). This keeps the Planner input's `<TARGET_MEDIUM>` slot from being empty or leaking the token verbatim into spec.md.
   Then dispatch the **Planner** `Agent`. Input = user mood words + selected candidate + re-steer notes +
   the locked target medium + soundscape opt-in status. The Planner writes `spec.md` (§3 Design Intent,
   §6 Divergence/Convergence Context, Definition of Done). It stays at the product level (V1-13 — it does not write CSS or exact prompt wording).
6. **Generate A→B (A·B only first).**
   - *Simplified*: **a single continuous Generator** `Agent` produces A (brief) → B (code). **B is produced through the design skill
     bound in step 1, not bare-hand CSS** — pass A's concept brief as that skill's brief
     (invoke with Skill if installed, otherwise Read that skill's SKILL.md/references and apply), and match palette/font/UX to
     a vetted guide (the ui-ux-pro-max type). Then self-render B **exactly once** to confirm zero errors + that A's
     hex/typography are applied (V1-15). No sprints.
   - *Full*: per-deliverable sprints (see the "Full tier only" section below).
   - **Efficiency principle (strong model, direct cost hit):** machine render / pixel verification once is enough. Do not **make a person
     wait an hour** while the Generator and Evaluator repeatedly re-scan the same thing — the real judge of aesthetics is the person
     (see "cost vs payload" below).
7. **EARLY craft check gate (P-1, cheap·first — load-bearing).**
   **Immediately show the first rendered B to the user** (screenshot or open the file). Within 2 minutes a person sees the font, tone,
   and obvious mismatches. This is this skill's core lesson — **do not defer the person behind expensive machine verification.**
   - If there is a mismatch (e.g. Korean silently falling back to the wrong font, white clashing with the mood) → a **light inline REFINE**,
     then reconfirm. Catch it here, before running the Evaluator.
   - The LLM cannot judge beauty, font fit, or tone — **this early human check is that verification. Wait for user confirmation.**
8. **Generate C.** After the direction is OK'd by the person, produce C (image prompt; soundscape added only when opted in).
9. **Evaluate (machine, once).** **One** Evaluator `Agent` scores **only verifiable proxies** (cap of 3–5 rounds, V2-2):
   traceability (C1), **font script coverage — does the declared font actually have glyphs for every script in the content (Korean/CJK/Latin)**,
   slop contrast (C2), render·hex/curve completeness (C3), divergence (C4). Beauty·rhythm·tone are **delegated, not scored** (P-1 —
   the person already saw them in step 7). **On a strong model, do not repeat redundant render/pixel scans** — the Generator already rendered
   and the person already saw it. Re-render directly *only when a new defect is suspected.* It writes `critique.md`.
10. **Iterate or finish.** Evaluator FAIL or user dissatisfaction → the Generator reads `critique.md` and makes a
    Strategic Decision (REFINE/PIVOT/ESCALATE, V1-9). Loop within the iteration range (V1-6, late-leap warning V1-10).
    On PASS + final user confirmation, deliver A·B·C.
11. **Tuning-loop reminder.** Periodically run "Evaluator tuning workflow" (a)~(d) below to calibrate the Evaluator.
    And remember the principle that harness components are removal candidates one at a time as the model gets stronger (closing guide below).

## Safety gates (do not proceed without user consent)

1. **Human-visual checkpoint gate (load-bearing, P-1)** — must be reached twice.
   - After divergence (step 4): present the user-specified N candidates (default 3–4) **as visual cards** and STOP until the user picks. Before convergence starts.
   - **Right after the first B render (step 7, EARLY·cheap):** **before** running the expensive Evaluator, STOP until the user looks at the
     render directly and confirms the font, tone, and mood. A run that defers the person behind machine verification violates this gate.
   - **A run that skips this gate is a FAIL.**
2. **Tier-selection gate** — the orchestrator proposes the tier and waits for user confirmation before dispatching roles.
3. **Re-steer gate** — if the user rejects all candidates ("not this, that feeling"), go back to step 3.
   Do not force convergence toward a rejected direction.
4. **Soundscape consent** — sound (C) is opt-in. The skill can fully PASS without sound.
   Propose it only when the mood strongly implies ambient sound AND the user opts in.

## File Handoff Contract (communicate only via files, V1-19)

| File | Author | Contents |
|------|--------|----------|
| `spec.md` | Planner | Design Intent (§3), Divergence/Convergence Context (§6, selected direction), Definition of Done |
| `sprint_contract.md` | Generator↔Evaluator | **Full tier only** — sprint deliverables + observable verification checks |
| `generator_report.md` | Generator | Strategic Decision + deliverable list + self-verification results |
| `critique.md` | Evaluator | Verdict + rubric scores + Blocking Issues + Redirect (optional) |
| `design_memo.md` | Generator | PIVOT proposal + cited critique.md evidence (awaiting Evaluator/user approval) |
| `handoff.md` | Active role | Remaining-work spec on context anxiety (not compaction) |

## Context reset policy

When a context-anxiety signal appears (re-summarizing, "to wrap up", late-stage depth drop, etc., V1-21),
the active role writes `handoff.md` and **a new session of the same role** takes over.
**Never compact — "compaction doesn't clear context anxiety" (V1-22).**
The Simplified tier's continuous Generator follows this same handoff-not-compaction rule.

## Iteration Wisdom

- **Set the iteration cap as a range, not a single value (V1-6).**
  **Full = 5–15 per deliverable / Simplified = 3–5 total Evaluator rounds.**
- **Do not pick the low end (V1-10).** In Anthropic's case study, the Dutch Art Museum design had
  a **breakthrough (late leap) at iteration 10.** Cutting too early misses that breakthrough.
- **Be generous with wall-clock time (V1-7).** Do not rush artificially. Mood translation takes time.
- **An intermediate iteration may be better than the final one (V1-11).** The Evaluator explicitly records
  strengths a prior iteration had ("iteration N's X was better than the current one" is valid and important feedback).

## Cost vs payload — the person early, cheap (the stronger the model)

> Field lesson: one run took about an hour, and while the Generator and Evaluator repeatedly verified
> *"does the hex render exactly"* with headless render + pixel scan, the actual **Korean font silent fallback · the white tone being a bad fit**
> (which a person caught in 2 minutes) was never caught to the end. It made the *real judge (the human eye)* wait an hour while expensively re-verifying
> what the machine *can see* — an obvious design defect.

- **The first render goes to the person first (step 7).** **Before** expensive machine verification, the person sees font, tone, and mood.
- **Machine render / pixel verification once.** If the Generator rendered and the person saw it, there is no reason for the Evaluator to
  repeat the same pixel scan — re-render *only when a new defect is suspected* (G-1: remove non-load-bearing components).
- **Not in conflict with V1-7.** "Be generous with wall-clock time" means do not artificially cut the *creative iteration*, not that you should
  spend time on *redundant machine verification*. Be generous with the creative loop, stingy with redundant verification.

## V1 vs V2 model guide (basis for the tier proposal)

| Model class | Context anxiety | Recommended tier | Notes |
|-------------|----------------|-----------------|-------|
| **Sonnet 4.5** | Strong — wraps up prematurely | Full (V1) | Small sprints, aggressive Evaluator, firm context resets |
| **Opus 4.5** | Largely eliminated | Simplified (V2) | Multi-hour coherent sessions; sprint decomposition droppable |
| **Opus 4.6** | Eliminated; improved planning, long-context, debugging | Simplified or Single-session | 2+ hour builds sustainable; re-examine every component, drop what's not load-bearing |
| **Sonnet 4.6+** (user target) | Low | Simplified (proposed default) | Low context anxiety → propose Simplified by default. The user can always override to Full. |

**General rule (G-1, G-2, G-5):** every harness component encodes an assumption about "what the model cannot do on its own."
On a model upgrade, stress-test each assumption **one at a time** and remove the ones that are no longer load-bearing.
**Radical simplification (removing everything at once) failed; removing one at a time succeeded.**
The harness's role does not disappear as the model gets stronger — it **shifts**, it does not **shrink**.
Anthropic, *"Building Effective Agents"* — **"find the simplest solution possible, and only increase
complexity when needed."** (G-3)

## Evaluator tuning workflow (operating procedure, P-3 / G-4 / V1-20)

An uncalibrated Evaluator is too lenient. Treat the first runs as drafts.
Run (a)~(d) below as an operating procedure. **Skipping any one is a FAIL in the audit.**

(a) **Read the critique logs of completed runs.** Place them side by side with the actual deliverables (A/B/C), and for each rubric
    score ask "would a demanding human design director have given the same score?"

(b) **Identify divergence patterns.** Typical drift: lenient scoring (passing generic/shallow as adequate),
    scoring beauty that should not be scored, missing un-traceable choices, wrongly passing a recolor candidate
    as distinct (C4), judging cliché with a banned-word scan instead of contrast.

(c) **Update evaluator-prompt.md or evaluator-calibration.md with concrete counter-examples.**
    Add new anchors/counter-examples that would catch the missed case (e.g. "I gave this palette 5/5 but it was actually
    slop that sticks to any mood → add this case to the C2 1/5 anchor").

(d) **Re-run on the same input and confirm the Evaluator now catches the previous miss.**
    If it does not, go back to (c). Stop tuning when the Evaluator's verdict correlates with a demanding human expert's pass and
    every blocking issue it raises is reproducible.

## Full tier only — sprint mechanism

(Applies only when the user overrides to Full. Simplified is the default, so there is no separate playbook file.)

- **Per-deliverable sprints**: A, B, C are each one sprint. Before a sprint starts, the Generator writes into
  `sprint_contract.md` (1) this sprint's deliverable, (2) the exact observable checks the Evaluator will run,
  (3) file/format/location, and waits for the Evaluator's approval/revision (V1-18).
- **Per-sprint Evaluator**: for each deliverable, the Evaluator runs the contract checks + adversarial probes (V1-18).
- **File handoff**: inter-sprint communication is via files only (V1-19).
- In Simplified, all of this is **N/A** — no sprint_contract, a single continuous Generator + a single end-of-run
  Evaluator (V2-1/V2-2). In Simplified, the Evaluator writes "N/A — tier=Simplified" in the corresponding slot.

## Closing guide (on model upgrade)

Every structure in this harness (separate roles, sprints, gates, Evaluator) exists to compensate for "what this model is weak at on its own."
When the model is upgraded (G-1, G-5), experiment with removing components one at a time:
does coherence hold without sprints? does quality hold with fewer Evaluator rounds?
**Keep only what is load-bearing.** But **the human-visual checkpoint (P-1) is a sensory limitation, so it does not
disappear with a model upgrade** — as long as the LLM cannot see beauty and rhythm, the gate where the user looks is permanently load-bearing.
