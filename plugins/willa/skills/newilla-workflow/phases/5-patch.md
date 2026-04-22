# Step 5 · Patch

선택된 후보의 반영 범위를 묻고 실행. AskUserQuestion 필수.

## 반영 범위 질문

```
AskUserQuestion(
  question: "선택한 배치를 어떻게 기록할까요?",
  options: [
    {
      label: "📄 문서만 · 5173 목업 그대로 (Recommended)",
      description: ".willa/placement-*.md 에만 기록 · 코드 변경 없음"
    },
    {
      label: "🎨 문서 + 5173 목업 반영",
      description: "placement 저장 + prototype/willa-preview/ 실제 수정 · HMR 즉시 반영"
    },
    {
      label: "🎨📖 문서 + 5173 + 5174 docs 반영",
      description: "위 + willa-docs 페이지에도 설명 추가"
    },
  ]
)
```

## 공통 · placement 문서 (항상 저장)

`.willa/placement-{slug}.md`:

```markdown
# Placement · {기능명}
- 날짜: {YYYY-MM-DD HH:mm}
- 분류: 빈도/사용자/타입/Phase/상태범위
- 선택된 후보: {A/B/C/D}
- 반영 범위: 문서만 / 5173 / 5173+5174
- 근거
- 구현 변경 (반영한 경우)
- 관련 annotation
- 테스트 포인트
```

## "5173 반영" 시 수정 대상

`prototype/willa-preview/` 내:

| 배치 아키타입 | 수정 파일 |
|------------|---------|
| P1 Inspector 탭 | inspector.tsx 탭 목록 + 해당 tab 컴포넌트 |
| P2 BottomDock | status-bar.tsx (Dock 역할) |
| P3 ToolRail | constants.ts `RAIL_TOOLS` |
| P4 TopBar | top-bar.tsx |
| P5 Command Palette | constants.ts `COMMANDS` + command-menu.tsx |
| P6 Context Menu | viewport.tsx `ViewportContextMenu` |
| Guide overlay | guide-overlay.tsx `ANNOTATIONS` |

각 수정 후: `npx tsc --noEmit` 통과 + `curl http://localhost:5173/` 200 확인.

## "5174 docs 추가 반영" 시

`prototype/willa-docs/` 내:
- 새 도구 → `src/pages/Tools.tsx` `TOOL_DESC`
- 새 도메인 → `src/pages/Domains.tsx` `DOMAIN_DESC`
- 새 Phase 섹션 → `src/pages/PhaseDetail.tsx` `PHASE_PROSE`
- 새 단축키 → `src/pages/Shortcuts.tsx` `GROUPS`

## 종료 메시지

```
✅ Newilla 완료 — {기능명}

📂 저장
   .willa/placement-{slug}.md

반영 범위: {선택}
{반영 시: 변경 파일 목록}

{미반영: "5173/5174 목업 유지"}
{반영: "localhost:5173 새로고침 확인"}

💡 Figma 이동은 /figwilla (html.to.design 가이드)
```
