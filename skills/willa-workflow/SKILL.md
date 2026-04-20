---
name: willa-workflow
description: /willa 명령 실행 시 사용하는 6단계 워크플로우. Will3D UI를 분석·질문·기획·적용·검증·디자인 감사하는 표준 절차. frontend-design 플러그인과 협업.
---

# Willa Workflow

`/willa` 가 호출되면 이 스킬이 발동한다. **6단계**를 순서대로 수행한다 (마지막 Polish 단계는 선택).

## 단계 요약

```
1. Discovery     → phases/1-discovery.md
    ↓
2. Clarify       → phases/2-clarify.md
    ↓
3. Plan          → phases/3-plan.md     [산출: .willa/plan-*.md]
    │            [Design Direction 섹션 포함 — patterns/design-system.md 준수]
    ↓
4. Scaffold      → phases/4-scaffold.md [수정: prototype/will3d-ui/src/**]
    │            [신규 컴포넌트는 frontend-design skill 호출]
    ↓
5. Serve         → phases/5-serve.md    [확인: localhost:5173]
    ↓
6. Polish (선택)  → phases/6-polish.md   [frontend-design 감사 · anti-patterns 점검]
                                        [산출: .willa/audit-*.md]
```

## frontend-design 플러그인 사용 요약

| 단계 | 호출 여부 | 목적 |
|------|----------|------|
| 3. Plan | ❌ 호출 안 함 | 패턴 문서만 참조 |
| 4. Scaffold | ✅ 신규 컴포넌트 생성 시만 | 초기 코드 생성 |
| 6. Polish | ✅ 감사 대상 컴포넌트 많을 때 | Reading Room 준수 점검 |

**호출 시 항상**: `patterns/design-system.md` 준수 · anti-patterns 금지 명시.

## 진입 조건

- 인자 없이 `/willa` → 전체 UI 재검토
- `/willa <토픽>` → 특정 영역 재검토
- `/willa refactor` → 기존 기획과 실제 코드 간극 정리
- `/willa refresh` → 가장 최근 plan 기반으로 prototype 재생성 (질문 생략)

## 중단 조건

각 단계에서 사용자가 AskUserQuestion에 "중단" 또는 "그만"으로 응답하면 즉시 종료. 중단 지점까지의 부분 산출물은 `.willa/draft-*.md` 로 저장.

## 산출물 위치

모든 산출물은 Will3D 루트(`C:\code\Will3D\`)의 `.willa/` 디렉토리에 저장:

```
.willa/
├── plan-20260420-2134.md         # 이번 실행 기획 문서
├── plan-latest.md → plan-...md   # symlink (최신)
└── placement-*.md                # /newilla 결과
```

## 패턴 참조

`patterns/` 디렉토리에 재사용 가능한 UI 패턴 문서들이 있다. 기획 시 참조한다:

- phase-navigation.md
- tool-rail.md
- inspector-tabs.md
- command-palette.md
- anatomy-navigator.md
- guide-overlay.md
- context-menu.md

## 템플릿

prototype/will3d-ui/ 자체가 살아있는 템플릿이다. `/willa` 는 이것을 수정하는 방식으로 작동한다. (별도 템플릿 복사 없음)

## 주의사항

- Will3D C++ 본체 수정 금지
- 작업 범위는 `current.md` 의 "작업 범위" 절에 한정
- 완료 시 `current.md` 및 `.willa/plan-latest.md` 업데이트
