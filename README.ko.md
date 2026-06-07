<div align="right">

[English](./README.md) | **한국어**

</div>

# mood-to-design-concept-harness

**막연한 무드·기분·분위기**("물에 잠긴 도시 같은", "서늘하고 몽환적인")를 **쓸 수 있는 디자인 컨셉**으로 바꾸는 [Claude Code](https://claude.com/claude-code) 스킬입니다 — **Planner → Generator → Evaluator** 하네스로 동작합니다.

완성된 미감을 한 방에 뽑지 않습니다. 이 스킬의 정체성은 **발산 → 사용자 선택 → 수렴** 루프입니다: 진짜로 서로 다른 여러 개의 컨셉 "세계"를 보여주고, 사용자가 렌더링을 직접 보며 하나를 고르고, 그 다음에야 구체적 산출물로 수렴합니다.

이 하네스는 Anthropic의 *["Harness Design for Long-Running Application Development"](https://www.anthropic.com/engineering)* 원칙(역할 분리·컨텍스트 리셋·루브릭 평가·자기평가 편향 방지)을 무드→디자인 컨셉 도메인에 이식한 것입니다.

## 이 스킬이 존재하는 이유

LLM은 *"이 팔레트가 아름다운가"*, *"이 모션 리듬이 좋은가"* 를 신뢰성 있게 판단하지 못합니다 — 감각의 한계입니다. 이 사실이 설계 전체를 결정합니다:

1. **미감은 사람이 판정하고, 검증 가능한 것만 기계가 채점합니다.** 사용자가 렌더링을 보고 고르는 행위가 **load-bearing 휴먼-비주얼 체크포인트(P-1)** 입니다. Evaluator는 아름다움·리듬을 **채점하지 않고**, 검증 가능한 프록시만 채점합니다(발산 다양성·추적성·코드 렌더·프롬프트 완결성·예시 기반 클리셰).
2. **시각 craft는 직접 만들지 않고 위임합니다.** LLM이 맨손으로 짠 비주얼(생 CSS/SVG, 손그림 일러스트)은 촌스러워지는 게 검증됐기에, B(코드)와 발산 보드는 검증된 디자인 스킬이나 실제 이미지 소스에 위임합니다.

## 주요 특징

- **발산 → 선택 → 수렴 루프** — 진짜로 서로 다른 N개 컨셉 방향(개수는 사용자가 선택, 기본 3~4), 한 아이디어의 색만 바꾼 recolor가 아님.
- **비주얼 발산 보드** — 후보를 텍스트로 설명하지 않고 *실제로 보여줘서* 무드가 전달되게 함("비주얼 컴패니언" 패턴).
- 수렴 시 **세 개의 순차적·독립적 산출물**:
  - **A — 디자인 컨셉 브리프**: 컨셉 이름, 무드 내러티브, 컬러 팔레트(hex), 타이포 무드, 모션/텍스처(명명된 이징 커브), 레이아웃 원칙, 말로 묘사한 레퍼런스.
  - **B — 프론트엔드 코드**: A를 구현하고, 에러 없이 실제로 렌더링되며, A의 정확한 hex/타이포를 그대로 사용.
  - **C — 이미지 생성 프롬프트**: 구도 + 조명 + 텍스처 + 분위기를 담아 그대로 붙여넣을 수 있는 형태(사운드스케이프는 opt-in 시에만).
- **사람 먼저·싸게 먼저 체크포인트** — 비싼 기계 검증 *전에* 첫 렌더를 사람에게 보여줌(픽셀을 반복 검증하느라 1시간을 쓰면서 정작 사람이 2분에 잡은 폰트 폴백을 놓친 실행에서 얻은 교훈).
- **엄격한 역할 분리** — Planner·Generator·Evaluator가 별도 `Agent` 호출로, 파일로만 소통. Generator는 자기평가 편향을 막기 위해 `spec.md`만 읽음(사전 컨텍스트 0).
- **클리셰 차단은 금지어가 아니라 대조로** — "아무에게나 붙는가 vs 이 무드에만 붙는가"의 예시 대조쌍으로 slop을 잡음.
- **모델 인식 티어** — 현재 모델을 감지해 **Simplified** 또는 **Full** 티어를 제안. 하네스 컴포넌트는 모델이 강해질수록 하나씩 제거 가능하도록 설계됨.

## 동작 방식

```
무드 단어 ─▶ 발산 (N개 세계, 비주얼로 표시) ─▶ ┌─ 사용자가 하나 선택
                                               └─ 또는 re-steer ↺
                       │ (선택 기록)
                       ▼
   Planner ──spec.md──▶ Generator ──A,B,C──▶  [B에 대한 EARLY 휴먼 체크]
                            ▲                          │
                            └──── critique.md ───── Evaluator (검증 가능한 프록시만)
```

| 역할 | 프롬프트 | 산출물 |
|------|----------|--------|
| Planner | `references/planner-prompt.md` | `spec.md` |
| Generator | `references/generator-prompt.md` | A, B, C + `generator_report.md` |
| Evaluator | `references/evaluator-prompt.md` | `critique.md` |

Evaluator는 4기준 루브릭으로 채점합니다 — **C1 충실도(2×)**, **C2 독창성/깊이(2×)**, **C3 번역 구체성(1×)**, **C4 발산 다양성(1×)** — 압력 없이 Claude가 가장 약한 축에 가중치를 둡니다.

## 저장소 구조

```
mood-to-design-concept-harness/
├── SKILL.md                              # 스킬 진입점: frontmatter + 오케스트레이터 흐름·게이트·원리
└── references/
    ├── planner-prompt.md                 # 자기완결 Planner 에이전트 프롬프트
    ├── generator-prompt.md               # 자기완결 Generator 에이전트 프롬프트
    ├── evaluator-prompt.md               # 자기완결 Evaluator 에이전트 프롬프트
    ├── rubric.md                         # 4기준 가중 채점 루브릭
    ├── evaluator-calibration.md          # Few-shot 1/3/5 점수 앵커(score drift 방지)
    └── cliche-contrast-examples.md       # slop vs distinctive 대조쌍(클리셰 차단)
```

## 기술 스택

- **Claude Code 스킬 포맷** — Markdown `SKILL.md`(YAML frontmatter `name` + `description`)와 필요 시 로드되는 `references/` 폴더.
- **하네스 패턴** — Claude Code `Agent` 툴로 디스패치되는 Planner / Generator / Evaluator 멀티에이전트 루프, 파일로만 소통.
- 런타임 의존성 없음 — 스킬은 프롬프트/지침 패키지이며, 유일한 "엔진"은 이를 실행하는 Claude 모델.

## 설치

개인용 Claude Code 스킬입니다. 폴더를 스킬 디렉터리에 두거나 심링크합니다:

```bash
# 이 폴더를 Claude Code 스킬 디렉터리에 심링크
ln -s "$(pwd)/mood-to-design-concept-harness" ~/.claude/skills/mood-to-design-concept-harness
```

이후 Claude Code를 시작(또는 리로드)하면, 요청이 트리거에 맞을 때 스킬이 자동 활성화됩니다.

## 사용법

무드를 설명하고 디자인 컨셉으로 바꿔 달라고 요청하면 됩니다. 예시 트리거:

- "무드를 디자인 컨셉으로", "분위기 컨셉 잡아줘", "이 느낌으로 디자인", "컨셉 발산 수렴"
- (EN) "turn this mood into a design", "design concept from a vibe", "diverge converge design concept"

스킬은 다음을 수행합니다:
1. 사용 가능한 디자인 스킬을 B 엔진으로 바인딩 + 모델 티어 제안(사용자 확정).
2. 몇 개 방향을 비교할지 묻고 **비주얼 발산 보드**를 보여줌.
3. 사용자가 방향을 고를 때까지(또는 re-steer) **멈추고 대기**.
4. A → B로 수렴, 첫 렌더를 **일찍** 보여주고, 그 다음 C 생성.
5. 검증 가능한 프록시에 대해 Evaluator 1회를 돌리고 A · B · C 전달.

> 관련 스킬: 사람이 함께하는 단일 화면은 `mood-to-design-checkpoints`, 웹 서비스 전체 컨셉은 `webservice-design-concept` 참고.

## 출처

하네스 원칙은 Anthropic의 *"Harness Design for Long-Running Application Development"*(Prithvi Rajasekaran, 2026)와 *"Building Effective Agents"*를 각색했습니다.

## 라이선스

[MIT License](./LICENSE)로 배포됩니다.
