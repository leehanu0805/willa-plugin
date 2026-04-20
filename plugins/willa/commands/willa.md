---
name: willa
description: Will3D UI/UX 전면 기획 — 사용자가 기능·도구를 쉽게 찾고 조작할 수 있게 메뉴를 재배치/리디자인한다. 질문으로 의도 정규화 → 기획 문서 생성 → localhost:5173 프로토타입에 MOCKUP/WIREFRAME/GUIDE 3모드로 반영.
triggers:
  - /willa
  - /willa <토픽>
scope: willa-workflow
---

# /willa — Will3D UI 기획

## 사용자가 입력하는 형태

```
/willa             # 기본 · Will3D C++ 읽고 UI/UX 개선 기획 + 프로토타입 생성
/willa polish      # 디자인 품질 감사만 (Phase 6 단독 실행)
```

인자 없음이 기본. `willa`는 지금 있는 Will3D 전체에서 사용자가 쉽게 쓸 수 있도록 UI/UX를 수정하는 도구.

## 목표

이 명령은 **Will3D 애플리케이션의 UI/UX 기획**을 담당한다.
목표는 단 하나 — *사용자(의사)가 기능 조작·전체 조작에 어려움을 느끼지 않도록 메뉴를 재배치·리디자인*한다.

## 실행 원칙

1. **추측 금지** — 애매하면 `AskUserQuestion`. 전문가/비전문가, 타깃 페이즈, 우선순위.
2. **실제 Will3D 코드 존중** — `C:\code\Will3D\` 의 기존 탭/도구 구조를 근거로.
3. **프로토타입 우선** — 기획은 `prototype/will3d-ui/` 에 시각화로 검증한다.
4. **3-모드 출력** — MOCKUP / WIREFRAME / GUIDE 모두 갱신.
5. **되돌릴 수 있게** — 기획 문서는 `.willa/plan-{YYYYMMDD-HHmm}.md`로 버전 저장.

## 워크플로우

skills/willa-workflow/SKILL.md 를 따라 6단계 진행 (6은 선택):

| 단계 | 파일 | 산출물 |
|------|------|--------|
| 1. Discovery | phases/1-discovery.md | Will3D C++ 코드 읽고 현재 상태 파악 |
| 2. Clarify   | phases/2-clarify.md   | AskUserQuestion 3~6개 → 의도 정규화 |
| 3. Plan      | phases/3-plan.md      | `.willa/plan-*.md` 기획 문서 (디자인 방향성 포함) |
| 4. Scaffold  | phases/4-scaffold.md  | prototype/willa-preview/ 파일 생성/수정 |
| 5. Serve     | phases/5-serve.md     | localhost:5175 확인 + 요약 보고 |
| 6. Polish ⭐ | phases/6-polish.md    | (선택) frontend-design 감사 → `.willa/audit-*.md` |

## 참고 패턴

기획 시 `skills/willa-workflow/patterns/` 의 재사용 패턴 참조:

- phase-navigation — 4-Phase (LOAD/EXAMINE/PLAN/DELIVER) 네비게이션
- tool-rail — 그룹화된 도구 rail (Navigate/View/Measure/Annotate) + Recent/Pinned
- inspector-tabs — 3-탭 Inspector (속성/Segmentation/기록)
- command-palette — Spotlight 스타일 Ctrl+K
- anatomy-navigator — SVG 해부 구조 인터랙티브 가시성 토글
- guide-overlay — 번호 핀 + hover 설명
- context-menu — 우클릭 · Pin/Unpin
- **design-system** ⭐ — Reading Room 컬러/타이포/모션 토큰 (`frontend-design` 플러그인과 공용)

## 주의

- Will3D C++ 본체 코드(`GLSL_Module/` · `UIComponent/` · `TabMgr/` 등)는 **건드리지 않는다**.
- 작업 범위는 `.ouroboros/context/current.md` 의 "작업 범위" 또는 본 명령의 인자에 한정.
- 완료 시 current.md + `.willa/plan-*.md` 모두 업데이트.
