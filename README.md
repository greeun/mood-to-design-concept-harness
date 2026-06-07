<div align="right">

**English** | [한국어](./README.ko.md)

</div>

# mood-to-design-concept-harness

A [Claude Code](https://claude.com/claude-code) skill that turns a **vague mood, vibe, or atmosphere** ("like a submerged city", "cool and dreamlike") into a **usable design concept** — through a **Planner → Generator → Evaluator** harness.

It does not pull a finished aesthetic out in one shot. Its identity is the **diverge → user-select → converge** loop: it shows you several genuinely different concept "worlds", you pick one by looking at rendered output, and only then does it converge into concrete deliverables.

This harness ports the principles from Anthropic's *["Harness Design for Long-Running Application Development"](https://www.anthropic.com/engineering)* (role separation, context reset, rubric evaluation, self-evaluation bias prevention) into the mood→design-concept domain.

## Why this skill exists

An LLM cannot reliably judge *"is this palette beautiful"* or *"is this motion rhythm good"* — that is a sensory limitation. Two consequences shape the whole design:

1. **Aesthetics are judged by the human; only the verifiable is scored by the machine.** The user looking at a rendering and choosing is a **load-bearing human-visual checkpoint (P-1)**. The Evaluator does **not** score beauty or rhythm — it scores only verifiable proxies (divergence diversity, traceability, code render, prompt completeness, example-based cliché).
2. **Visual craft is delegated, never hand-rolled.** Visuals an LLM hand-codes from scratch (bare CSS/SVG, hand-drawn illustration) are proven to come out tacky, so B (code) and the divergence board are delegated to vetted design skills or real image sources.

## Key features

- **Diverge → select → converge loop** — N genuinely distinct concept directions (you choose the count; default 3–4), not recolors of one idea.
- **Visual divergence board** — candidates are *shown*, not described in text, so the mood actually comes across (the "visual companion" pattern).
- **Three sequential, independent deliverables** on convergence:
  - **A — Design Concept Brief**: concept name, mood narrative, color palette (hex), typography mood, motion/texture (named easing curves), layout principles, references in words.
  - **B — Frontend Code**: implements A, actually renders without errors, uses A's exact hex/typography.
  - **C — Image-Generation Prompt**: composition + lighting + texture + atmosphere, drop-in ready (optional soundscape only on opt-in).
- **Human-first, cheap-first checkpoints** — the first render goes to a person *before* any expensive machine verification (a lesson from a run that wasted an hour re-verifying pixels while missing a font fallback a human caught in 2 minutes).
- **Strict role separation** — Planner, Generator, and Evaluator are separate `Agent` calls that communicate only through files; the Generator reads only `spec.md` (zero prior context) to avoid self-evaluation bias.
- **Cliché blocking by contrast, not banned words** — slop is caught with side-by-side "sticks to anyone vs sticks to this mood only" example pairs.
- **Model-aware tiers** — detects the current model and proposes a **Simplified** or **Full** tier; harness components are designed to be removed one at a time as models get stronger.

## How it works

```
mood words ─▶ DIVERGE (N worlds, shown visually) ─▶ ┌─ user picks one
                                                     └─ or re-steer ↺
                          │ (selection recorded)
                          ▼
   Planner ──spec.md──▶ Generator ──A,B,C──▶  [EARLY human check on B]
                            ▲                          │
                            └──── critique.md ───── Evaluator (verifiable proxies only)
```

| Role | Prompt | Output |
|------|--------|--------|
| Planner | `references/planner-prompt.md` | `spec.md` |
| Generator | `references/generator-prompt.md` | A, B, C + `generator_report.md` |
| Evaluator | `references/evaluator-prompt.md` | `critique.md` |

The Evaluator is graded against a 4-criterion rubric — **C1 Fidelity (2×)**, **C2 Originality & Depth (2×)**, **C3 Translatability (1×)**, **C4 Divergence Quality (1×)** — weighting the axes Claude is weakest at without pressure.

## Repository structure

```
mood-to-design-concept-harness/
├── SKILL.md                              # Skill entry: frontmatter + orchestrator flow, gates, principles
└── references/
    ├── planner-prompt.md                 # Self-contained Planner agent prompt
    ├── generator-prompt.md               # Self-contained Generator agent prompt
    ├── evaluator-prompt.md               # Self-contained Evaluator agent prompt
    ├── rubric.md                         # 4-criterion weighted scoring rubric
    ├── evaluator-calibration.md          # Few-shot 1/3/5 score anchors (prevents score drift)
    └── cliche-contrast-examples.md       # Slop vs distinctive contrast pairs (cliché blocking)
```

## Tech stack

- **Claude Code skill format** — Markdown `SKILL.md` (YAML frontmatter `name` + `description`) plus a `references/` folder loaded on demand.
- **Harness pattern** — Planner / Generator / Evaluator multi-agent loop dispatched via the Claude Code `Agent` tool, communicating only through files.
- No runtime dependencies — the skill is a prompt/instruction package; the only "engine" is the Claude model running it.

## Installation

This is a personal Claude Code skill. Place (or symlink) the folder into your skills directory:

```bash
# Symlink this folder into Claude Code's skills directory
ln -s "$(pwd)/mood-to-design-concept-harness" ~/.claude/skills/mood-to-design-concept-harness
```

Then start (or reload) Claude Code. The skill activates automatically when your request matches its triggers.

## Usage

Just describe a mood and ask to turn it into a design concept. Example triggers:

- "Turn this mood into a design concept" / "design concept from a vibe"
- "atmosphere to design brief" / "diverge converge design concept"
- (KO) "무드를 디자인 컨셉으로", "분위기 컨셉 잡아줘", "이 느낌으로 디자인"

The skill will:
1. Bind an available design skill as the B engine + propose a model tier (you confirm).
2. Ask how many directions to compare, then show a **visual divergence board**.
3. **Stop and wait** for you to pick a direction (or re-steer).
4. Converge into A → B, show you the first render **early**, then produce C.
5. Run a single Evaluator pass on verifiable proxies and deliver A · B · C.

> Related skills: for a single surface with a human in the loop, see `mood-to-design-checkpoints`; for a whole web service concept, see `webservice-design-concept`.

## Credits

Harness principles adapted from Anthropic's *"Harness Design for Long-Running Application Development"* (Prithvi Rajasekaran, 2026) and *"Building Effective Agents."*

## License

Released under the [MIT License](./LICENSE).
