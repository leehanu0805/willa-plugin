---
name: willa-workflow
description: /willa 명령 실행 시 사용하는 7단계 워크플로우. Will3D UI를 분석·질문·기획·적용·검증·자가치유·감사. willa 가 스스로 방향성을 판단하며 각 실행마다 다른 결과를 낼 수 있다.
---

# Willa Workflow

`/willa` 호출 시 이 스킬 발동. **7단계** 순차 (6은 선택).

## 핵심 원칙

### 도메인 불변 (CBCT 뷰어/플래너)
- Will3D 는 치과 CBCT 3D 뷰어/플래너
- 뷰포트가 주인공, 다크 뷰포트, 3축 MPR + 3D VR, 드래그 중심 인터랙션
- **저널 리더/대시보드/소셜 피드 같은 재해석 금지**

### 문제 해결이 먼저
- Discovery 에서 **구체 UX 이슈** 찾기
- Clarify 에서 **이슈 우선순위** 결정
- Plan 은 **이슈별 해결책** (aesthetic 은 해결책의 시각 표현일 뿐)

### 자유도는 "어떻게" 에만
- CBCT 본질 유지한 범위 내에서 aesthetic · 패널 배치 · 도구 형태 자유
- `patterns/` 는 영감 · 강제 아님
- `design-system.md` 는 한 예시 · 새 방향 자유
- 기존 will3d-ui(5173) / willa-preview(5175) 결과물에 얽매이지 않음

## 단계 요약

```
1. Discovery      → phases/1-discovery.md     (UI/UX 파악 · 개수 제한 없음)
   ↓
2. Clarify        → phases/2-clarify.md       (4개 고정 질문 · 무조건 필수)
   ↓
3. Plan           → phases/3-plan.md          (후보 유연 · 사용자 선택 · 토큰 구체화)
   ↓
4. Scaffold       → phases/4-scaffold.md      (willa-preview 수정)
   ↓
4.5 Investigate  ⭐ → phases/4.5-investigate.md (품질 조사 + 사용자 확인 · 게이트)
   ↓
4.75 Self-Heal ⭐ → phases/4.75-self-heal.md   (자동 · 무제한 · Plan 정렬 + 조작성 개선)
   ↓
5. Serve          → phases/5-serve.md          (완성본 보고)
   ↓
6. Polish (선택)  → phases/6-polish.md          (.willa/audit-*.md)
```

Figma 로 옮기려면 `/figwilla` (html.to.design 수동 가이드) 사용. willa 워크플로우 외부.

## frontend-design 사용

| 단계 | 호출 | 목적 |
|------|------|------|
| 3. Plan | 토큰 검증 · 신규 컴포넌트 설계 · 톤 변경 시 | 미적 방향 상담 (필수) |
| 4. Scaffold | 신규 컴포넌트 생성 시 | 이번 plan 방향성에 맞춘 코드 생성 |
| 6. Polish | 감사 필요 시 | 일관성 체크 (특정 시스템 강제 아님) |

호출 시 **이번 plan 의 방향성** 만 전달. 고정 디자인 시스템 강제 안 함.

## 진입 조건

- `/willa` / `/willa:willa` → 전체 재검토 (6은 선택 제안)
- `/willa polish` → Phase 6 단독
- `/figwilla` → Figma 이전 수동 가이드 (html.to.design) · willa 외부 명령

## 중단 조건

각 단계에서 AskUserQuestion에 "중단/그만" → 즉시 종료. `.willa/draft-*.md` 저장.

## 산출물 위치

```
C:\code\Will3D\.willa\
├── plan-{timestamp}.md
├── plan-latest.md
├── placement-*.md        (/newilla 결과)
└── audit-*.md            (/willa polish 결과)
```

## 패턴 디렉토리 (영감용)

`patterns/` 문서는 과거 Will3D UI 탐색 중 등장한 패턴 기록. 참고만.

- **layout-archetypes** ⭐ — 8개 레이아웃 원형 · Plan 단계 다양성 강제 기반
- phase-navigation
- tool-rail
- inspector-tabs
- command-palette
- anatomy-navigator
- guide-overlay
- context-menu
- design-system (Reading Room 예시)
- docs-explorer

**강제 없음** 단, **layout-archetypes 는 Plan 단계에서 필수 참조**. 이전과 같은 archetype 연속 사용 금지 (전체 재설계 시).

## 다양성 원칙

willa 는 매 실행 **다른 구조** 를 탐색해야 사용자에게 의미있는 대안 제공.
- Plan 단계에서 `.willa/plan-latest.md` 읽어 이전 archetype 확인
- 시드 8개는 출발점 · willa 자유 변이·조합·발명
- Plan 상단에 4개 항목 기록 (가져온 것 · 바꾼 것 · 추가한 것 · 기각 후보)

## 무조건 질문 원칙 ⚠

**Phase 2 Clarify 는 매 실행 필수**. 스킵 불가:

- 이전 실행 답변 추정 금지
- AskUserQuestion 최소 2개 호출해야 Phase 3 진입 가능
- 사용자가 "willa 판단" 으로 답했어도 **다음 실행에서 다시 물음**
- 질문 없이 Plan 작성 = 품질 게이트 실패 → Plan 무효화

## 템플릿

`prototype/willa-preview/` 가 willa 의 출력 공간. 매 실행 여기만 수정.
기존 `prototype/will3d-ui/` (5173) 는 건드리지 않음.

## 주의사항

- Will3D C++ 본체 수정 금지
- 완료 시 `current.md` 및 `.willa/plan-latest.md` 업데이트
