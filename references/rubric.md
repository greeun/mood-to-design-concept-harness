# Rubric — mood-to-design-concept-harness

The rubric the Evaluator uses to score A (Brief) + B (Code) + C (Prompts). 4 criteria, 1–5 points, weighted.

## Core principle: weight what Claude is weak at

Per the article: Claude is by default good at technical accuracy, structure, and basic functionality. What it lacks
without pressure is **originality, depth, specificity, domain taste.** Weight the weak axes 2×, the already-adequate axes 1×.

## Style Magnet warning

"Criterion wording pulled the generator in an unexpected direction. A phrase like 'museum quality' pushed the design
toward a specific visual convergence." → **Write criteria as quality (coherence, depth, traceability), not as
reference (museum-quality, Apple-like).** References live in spec.md §3 Design Intent, not in the rubric.

## 4 criteria

| # | Criterion | Weight | What it measures | Why this weight |
|---|-----------|--------|------------------|-----------------|
| C1 | **Fidelity (association fidelity)** | **2×** | Does the mood the user named ("cool and dreamlike") actually come alive in A/B/C — does it not decay into generic pretty. Measure = traceability: does each palette/typography/motion choice back-cite to a mood word. | "Does this *feel* like that vibe" (aesthetic taste) is the axis Claude is weakest at without pressure. V1-3. |
| C2 | **Originality & Depth** | **2×** | Is it a reading unique to this specific mood, or did it flee to an AI-slop cliché (gradient blobs, generic dark-mode-neon, "ethereal" default). **Judged by contrast examples, not a banned-word list.** | Originality/depth is Claude's canonical weakness. The slop-flight risk is especially high in mood→design translation. V1-3/V1-4. |
| C3 | **Translatability (concreteness of translation)** | 1× | Is the emotion→design translation concrete enough to drop straight into code/prompts — real hex, named font character, motion curve/easing, composition keywords. Is it not just a pile of abstract adjectives. | Concreteness/structure is something Claude does adequately when asked. Mostly verifiable (is there hex? is easing named? does the prompt have composition+lighting+texture+atmosphere?). 1×. |
| C4 | **Divergence Quality** | 1× | Are the N divergence candidates (user-specified, default 3–4) genuinely different "worlds" (different layout logic, typography philosophy, spatial mood), or a recolor of the same concept. The count is user-set, but the variation bar is the same regardless of count. | The added 4th criterion. Justification: the diverge→select→converge loop is this skill's identity — if candidates collapse to near-duplicates, the human-visual checkpoint (P-1) is an empty shell (the user has no real choice). Mostly verifiable (compare real variation of palette/layout principle/typography vs recolor), so 1× — not a taste axis. |

## N/A — beauty and motion rhythm are not rubric criteria (P-1)

The Evaluator **does not score** "is this palette beautiful", "is this motion rhythm good."
These are explicitly delegated to the user at the human-visual checkpoint. The rubric scores only verifiable proxies
(traceability, slop contrast, hex/curve completeness, candidate variation). If the Evaluator scores beauty, that is
miscalibration and a defect to be fixed. Note in critique.md: "beauty/rhythm delegated to user per P-1."

## Scoring Guide

| Score | Meaning |
|-------|---------|
| 5 | Exceeds expectations — a design director is impressed |
| 4 | Meets expectations — solid and professional |
| 3 | Acceptable but noticeably weak — needs improvement |
| 2 | Below expectations — substantial gaps |
| 1 | Unacceptable — fundamental problems |

(For the per-criterion 1/3/5 concrete anchors, see `evaluator-calibration.md`.)

## Verdict Logic (V1-17)

```
All criteria ≥ 4 AND adversarial probes clean        → PASS
A 2× criterion (C1 Fidelity / C2 Originality) < 4      → FAIL
A 1× criterion (C3 Translatability / C4 Divergence) < 3 → FAIL
Any spec.md Definition-of-Done item unverified         → FAIL
Human-visual checkpoint gate not reached/not honored   → FAIL  (P-1 domain addition)
```

## Calibration Checkpoint

After scoring, if every category is ≥4, apply a secondary check:

1. **Design-director lens**: "What would a demanding senior design director catch?"
2. **User lens**: "Would the user who commissioned this mood say 'yes, that's the feeling I described'?"

Add this check's findings to the critique. (Beauty is still not scored — the user lens asks "was the mood translated",
not "is it pretty.")
