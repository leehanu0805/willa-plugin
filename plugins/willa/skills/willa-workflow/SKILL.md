---
name: willa-workflow
description: /willa 명령 실행 시 사용하는 6단계 워크플로우. Will3D UI를 분석·질문·기획·적용·검증한다. willa 가 스스로 방향성을 판단하며 각 실행마다 다른 결과를 낼 수 있다.
---

# Willa Workflow

`/willa` 호출 시 이 스킬 발동. **6단계** 순차 (6은 선택).

## 핵심 원칙 · willa는 자유롭다

- 매 실행마다 **다른 aesthetic/구조를 선택**할 수 있음
- `patterns/` 는 영감 레퍼런스 · 강제 아님
- `design-system.md` 는 **이전에 탐색된 한 가지 예시** · 새 방향 자유
- 기존 결과물(will3d-ui 5173 · willa-preview 5175)에 얽매이지 않음
- 치과 CT 도메인 전문가용이라는 맥락만 지킴

## 단계 요약

```
1. Discovery    → phases/1-discovery.md    (C++ 코드 읽기)
   ↓
2. Clarify      → phases/2-clarify.md      (AskUserQuestion 4개)
   ↓
3. Plan         → phases/3-plan.md         (.willa/plan-*.md · 자유 형식)
   ↓
4. Scaffold     → phases/4-scaffold.md     (willa-preview 수정)
   ↓
5. Serve        → phases/5-serve.md        (localhost:5175 확인)
   ↓
6. Polish (선택) → phases/6-polish.md      (.willa/audit-*.md)
```

## frontend-design 사용

| 단계 | 호출 | 목적 |
|------|------|------|
| 4. Scaffold | 신규 컴포넌트 생성 시 | 이번 plan 방향성에 맞춘 코드 생성 |
| 6. Polish | 감사 필요 시 | 일관성 체크 (특정 시스템 강제 아님) |

호출 시 **이번 plan 의 방향성** 만 전달. 고정 디자인 시스템 강제 안 함.

## 진입 조건

- `/willa` / `/willa:willa` → 전체 재검토
- `/willa polish` → Phase 6 단독

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

- phase-navigation
- tool-rail
- inspector-tabs
- command-palette
- anatomy-navigator
- guide-overlay
- context-menu
- design-system (Reading Room 예시)
- docs-explorer

**강제 없음**. 이번 실행에 맞지 않으면 무시하고 새 패턴 제안해도 OK.

## 템플릿

`prototype/willa-preview/` 가 willa 의 출력 공간. 매 실행 여기만 수정.
기존 `prototype/will3d-ui/` (5173) 는 건드리지 않음.

## 주의사항

- Will3D C++ 본체 수정 금지
- 완료 시 `current.md` 및 `.willa/plan-latest.md` 업데이트
