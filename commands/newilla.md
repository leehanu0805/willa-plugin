---
name: newilla
description: 새 기능을 Will3D UI의 어디에 배치할지 결정 — 기존 구조(Phase · Domain · Tool Rail · Inspector · Command Palette · Context Menu)와 비교해 Top 3 배치 후보를 추천.
triggers:
  - /newilla <새 기능명>
  - /newilla "<자연어 설명>"
scope: newilla-workflow
---

# /newilla — 새 기능 배치 결정

## 사용자가 입력하는 형태

```
/newilla "Airway 분석"
/newilla "임플란트 시뮬레이션 비교"
/newilla "AI 자동 각도 최적화"
```

## 목표

사용자가 제안한 **새 기능**을 받아서:
1. 현재 Will3D UI 구조(Phase / Domain / Tool Rail / Inspector / Command Palette / Context Menu)를 파악
2. 새 기능의 성격(빈도·타입·컨텍스트)을 질문으로 정규화
3. 가장 적합한 **배치 후보 Top 3**를 추천
4. 사용자 선택 시 `.willa/placement-{slug}.md` 저장 + (선택) 실제 prototype 코드 반영

## 실행 원칙

1. **기존 패턴 우선** — 새 UI 패턴 만들기 전에 이미 있는 자리에 넣을 수 있는지 먼저 확인.
2. **빈도가 결정한다** — 일상(80% 사용) → Tool Rail Pinned / Phase 직접 배치. 가끔 → Tool Rail 그룹 / Inspector 섹션. 특수 → Command Palette / Context Menu.
3. **타입이 자리를 결정한다** — Tool(클릭 후 드래그) vs Panel(상시 정보) vs Modal(깊은 제어) vs Action(1회 실행).
4. **Phase 적합성 확인** — LOAD/EXAMINE/PLAN/DELIVER 중 언제 쓰이는지.

## 워크플로우

skills/newilla-workflow/SKILL.md 참조. **5단계 분기형**:

| 단계 | 내용 |
|------|------|
| 1. Context | `.willa/plan-*.md` 최신본 + `prototype/will3d-ui/src/lib/constants.ts` 읽기 |
| **2. Branch Choice** ⭐ | AskUserQuestion: **🔁 갈아엎기 / ➕ 추가만 / 🤔 보류** |
| 3. Classify (B만) | AskUserQuestion: 빈도 · 사용자 · 타입 · 연관 Phase |
| 4. Recommend (B만) | Top 3 후보(각각 위치 + 근거 + 구현 방법) 제시 |
| 5. Patch (B만) | 선택된 후보를 `.willa/placement-{slug}.md` 저장 · (옵션) constants.ts 추가 · guide-overlay에 annotation 자동 추가 |

### 분기 상세

**🔁 갈아엎기** — 새 기능 중심으로 전체 재설계. `/willa "{기능명}"` 워크플로우로 위임. `localhost:5173` 건드림.

**➕ 추가만** (추천) — 기존 UI 그대로 두고 자리만 찾음. Step 3~5 진행. **Step 5 에서 반영 범위를 한 번 더 선택**:
- 📄 **문서만** (기본) — `localhost:5173` **건드리지 않음** · `placement-*.md` 만 저장
- 🎨 **5173 반영** — prototype/will3d-ui 수정 · HMR로 즉시 반영
- 🎨📖 **5173 + 5174 반영** — 위 + willa-docs 에도 설명 추가

**🤔 보류** — `.willa/deferred-{slug}.md` 만 저장. **5173/5174 건드리지 않음**.

### 정리 · 어떤 선택에서 localhost가 바뀌나

| 선택 | 5173 목업 | 5174 docs | 문서 |
|------|----------|-----------|------|
| A. 갈아엎기 | ✅ 바뀜 | (선택적) | ✅ plan-*.md |
| B1. 추가만 + 문서만 | ❌ 그대로 | ❌ 그대로 | ✅ placement-*.md |
| B2. 추가만 + 5173 | ✅ 바뀜 | ❌ 그대로 | ✅ placement-*.md |
| B3. 추가만 + 5173+5174 | ✅ 바뀜 | ✅ 바뀜 | ✅ placement-*.md |
| C. 보류 | ❌ 그대로 | ❌ 그대로 | ✅ deferred-*.md |

## Top 3 후보 예시 포맷

```
후보 A: Tool Rail → Measure 그룹
  - 근거: 측정-유사 도구 (드래그로 데이터 얻음)
  - 구현: RAIL_TOOLS 배열에 { key: 'airway', group: 'measure', ... } 추가
  - 트레이드오프: 측정 그룹이 이미 10개. 드로어에 들어감.

후보 B: EXAMINE Phase → 도메인 탭에 'Airway' 신설
  - 근거: 특화된 분석 모드 (뷰포트 레이아웃 달라짐)
  - 구현: DOMAINS.examine 에 추가 + SubNav 탭 +1
  - 트레이드오프: 도메인 6개가 되어 탭 overflow 가능성.

후보 C: Inspector → 4번째 탭 'Airway'
  - 근거: 수치/측정값 + 토글 조합
  - 구현: Inspector 탭 목록에 추가 · 기본 구조(AI card 포함)
  - 트레이드오프: Inspector 탭 밀도 증가.
```

## 주의

- 새 기능을 **반드시 어딘가에 넣어야 한다는 압박**을 받지 않는다. 억지 배치가 나쁜 UX의 원인.
- 선택지 중에 "넣지 않음 (기획에만 기록, UI 미반영)" 옵션을 항상 제공한다.
