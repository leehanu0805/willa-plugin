# Phase 3 · Plan

Discovery 이슈 + Clarify 답변 → **구체 해결책 문서**. `.willa/plan-{YYYYMMDD-HHmm}.md` 저장 + `plan-latest.md` 갱신.

## 원칙

- Will3D 는 CBCT 뷰어/플래너 — 도메인 이탈 금지
- 모든 변경은 Discovery 의 구체 이슈에서 출발
- 매 실행마다 **이전 plan 과 의미있게 다른 archetype** (다양성 필수)
- 이전 plan 의 좋은 요소는 **keep** · 약점만 **fix** · 새 요소 **add**

---

## 1. Archetype 선택

### 절차
1. **이전 plan 읽기** (`.willa/plan-latest.md` 있으면) — archetype · keep · 약점 · 기각 후보 파악
2. **후보 생성** — `patterns/layout-archetypes.md` 의 5형 내에서 변이 + 조합 + 발명
3. **후보 2개 이상이면 AskUserQuestion 필수** · 1개면 그것으로 진행
4. frontmatter + 본문 상단 4항목 기록 (§5 템플릿 준수)

### 금지 (도메인 이탈)
카드 덱 · Focus/Zen · Radial · 대화창 주 인터랙션 · Floating 남발 · 뷰포트 50% 미만.

---

## 2. 디자인 토큰 (구체값 필수)

**추상어 금지** ("modern/clean/깔끔" ❌). 모든 섹션에 **hex · px · ms** 구체값 명시.

- A. 팔레트 · B. 타이포 · C. 스페이싱/라디우스/섀도우 → `references/token-specification.md` · `references/typography-color.md`
- D. 시각 위계 3단계 (Primary/Secondary/Tertiary)
- E. 4-state (default/hover/active/disabled) · 모든 interactive 요소
- F. 모션: instant 80ms · quick 140ms · slow 220ms spring · 상세 `references/motion-timing.md`
- G. **하이브리드 테마**: 뷰포트 dark 고정 (DICOM) · chrome bright 선호 · 풀 light reject

팔레트 예시 (출발점 · 그대로 쓰지 말 것):
```
--viewport-bg   #0A0908   dark 고정
--bg            #F7F6F3   chrome bright
--accent        #1F6D92
```

---

## 3. frontend-design 실호출 (의무)

**Plan 작성 중 반드시** `frontend-design` skill 을 **실제로 호출**한다. 지침만 남기고 호출 안 하면 품질 게이트 실패.

### 호출 조건 (하나라도 해당시 필수)
- 새 archetype 제안
- 팔레트 또는 타이포 변경
- 신규 컴포넌트 패턴 (기존 patterns/ 에 없는)
- 이전 plan 대비 미적 방향 재설정

### 호출 방법 (그대로 실행)
```
Skill("frontend-design:frontend-design",
  "Will3D CBCT 뷰어의 {archetype 이름} 방향성 검토. \
   팔레트: {hex 값 3-5개 나열}. \
   타이포: {Display/Body/Mono 스택}. \
   레이아웃 비율: {뷰포트/chrome}. \
   이전 archetype: {이름}. 차별화 포인트: {1-2줄}. \
   평가: (1) 시각 일관성 (2) 의료 SW 적합성 (3) 이전 plan 대비 구분 가능성. \
   개선 제안 3가지, 300 단어 이내.")
```

### 기록 의무
Skill 응답을 Plan 본문 "§6. 디자인 원칙 적용" 섹션에 **원문 발췌 + 채택/기각 판단** 으로 인용. frontmatter `design_consult` 필드에 호출 시각 + 채택 개수 기록.

### 게이트
Plan 에 Skill 호출 결과 인용이 없으면 = Scaffold 차단.

---

## 4. 추가 참조 (선택)

`references/_INDEX.md` Plan 섹션:
- 배치 근거 → `laws-of-ux.md` (Fitts/Hick/Miller/Jakob)
- 편향 대응 → `cognitive-biases.md`
- 차분/신뢰 톤 → `emotional-design.md`

**규칙**: 원문 복붙 금지 · 1-2줄 인용으로만 · 출력 언어는 사용자 따라감.

---

## 5. Plan 문서 템플릿 (frontmatter 필수)

```markdown
---
version: 1
archetype: {이름}
archetype_kind: seed | variant | invention
target: willa-preview | willa-sandbox | {경로}
previous_plan: plan-YYYYMMDD-HHmm.md | null
issues: ["이슈1", "이슈2"]
design_consult:
  called_at: YYYY-MM-DD HH:mm
  suggestions_adopted: N
  suggestions_rejected: M
files_to_modify:
  - src/components/top-bar.tsx
  - src/App.tsx
metrics:
  viewport_ratio_target: 0.8
  chrome_pixels: 132
  inspector_default: collapsed | open
  theme: hybrid | full-dark
---

# Will3D UI 기획 · {YYYY-MM-DD HH:mm} · {archetype 이름}

> Archetype: {이름} — {seed/variant/invention}
> Keep (이전 plan): {2-3개}
> Fix (교체/제거): {이유 1-2개}
> Add (신규): {이유 1-2개}
> 기각 후보: {최소 2개 + 이유}

## Context
- Q1 타깃 / Q2 이슈 / Q3 범위 / Q4 archetype

## 1. 디자인 토큰
(A-G 구체값 또는 references 인용)

## 2. 해결할 이슈 (Clarify 선택된 것만)
### Issue N · {이름}
- 증상 (C++ 근거)
- 영향
- 해결책 (시각 디테일 · 상태 · 모션)
- 측정 방법

## 3. 전체 구조 변경 (필요 시)
탭/패널 매핑 · 신규 요소. 불필요하면 생략.

## 4. 트레이드오프
포기한 대안 + 이유.

## 5. 다음 단계
수정 파일 · 작업량 · 참조 토큰/패턴.

## 6. 디자인 원칙 적용 (frontend-design 상담 결과 필수 인용)
- 뷰포트 ≥ 70% · Inspector 컨텍스트 · ToolRail 빈도+카테고리
- **frontend-design 제안 (발췌)**: {원문 2-3줄}
- **채택**: {항목 + 이유}
- **기각**: {항목 + 이유}
```

### Frontmatter 필드 규칙

| 필드 | 타입 | 필수 | 설명 |
|------|------|------|------|
| `version` | number | ✓ | 템플릿 버전 · 현재 1 |
| `archetype` | string | ✓ | 이름 |
| `archetype_kind` | enum | ✓ | seed · variant · invention |
| `target` | string | ✓ | 수정 대상 프로토타입 폴더 |
| `previous_plan` | string \| null | ✓ | 이전 plan 파일명 |
| `issues` | string[] | ✓ | Clarify Q2 선택/판정 |
| `design_consult` | object | ✓ | frontend-design 호출 기록 (called_at · adopted · rejected) |
| `files_to_modify` | string[] | ✓ | Scaffold 예상 파일 · 최소 1개 기존 컴포넌트 포함 필수 |
| `metrics` | object | ✓ | 측정 가능 지표 (뷰포트 비율 · chrome 픽셀 · 테마 등) |

### `files_to_modify` 규칙
- 전체 재설계 (Q3=3) 인데 **신규 파일만 있고 기존 수정 없음** = 게이트 실패 (baseline 위 덧씌우기 방지)
- 최소 1개 기존 Phase 컴포넌트 (`phase-*.tsx` 또는 `top-bar.tsx` 등) 수정 포함 필수

---

## 6. 품질 게이트 (Scaffold 진입 조건)

아래 하나라도 해당하면 **차단**:
- frontmatter 누락 또는 필수 필드 missing
- `design_consult` 가 null (frontend-design 실호출 없음)
- `files_to_modify` 가 신규 파일만 (전체 재설계 시 · baseline 덧씌우기)
- 토큰 추상어만 (구체값 없음)
- 4-state 중 hover/active 누락
- 스페이싱 4px grid 이탈
- 뷰포트까지 밝게 처리
- Archetype 4항목 (Keep/Fix/Add/기각) 누락
- 이전 plan 과 사실상 동일 (이름만 변경 · metrics 도 동일)
- 후보 2개 이상인데 AskUserQuestion 스킵

통과 시 사용자에게 **"Scaffold 진행?"** 단답 확인 후 Phase 4.

---

## 7. 저장

- `.willa/plan-{YYYYMMDD-HHmm}.md` (버전)
- `.willa/plan-latest.md` (최신 바로가기 · 덮어쓰기)
