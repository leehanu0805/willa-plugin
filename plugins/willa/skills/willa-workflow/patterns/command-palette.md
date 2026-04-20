# Pattern · Spotlight Command Palette

## 요약
`Ctrl+K` 로 열리는 중앙 모달. macOS Spotlight + Raycast 감성. **좌 검색/결과 + 우 Preview** 2-column 레이아웃. 카테고리별 그룹핑.

## 구성

```
┌─────────────────────────────────────────────┐
│ 🔍  검색어…                            ESC    │  Input (15px 큰 폰트)
├───────────────────────────────┬─────────────┤
│ PHASES (teal)                  │  🎯(64px)   │
│   → Load                       │  카테고리   │
│     Examine                    │             │
│ ─                              │  타이틀     │
│ WORKSPACE (보라)                │  path       │
│   Implant       ⌘I             │             │
│ ─                              │  설명       │
│ TOOLS (녹색)                    │  단축키     │
│   Ruler         M              │  태그들     │
│ ─                              │             │
│ ACTIONS (주황)                  │  실행 ⏎    │
│   캡처          ⌘⇧C            │             │
├────────────────────────────────┴─────────────┤
│ ↑↓ 이동 · ⏎ 실행 · ⎋ 닫기   4 cat · Spotlight │
└──────────────────────────────────────────────┘
```

## 핵심 특징

- 4개 카테고리: `phase` / `domain` / `tool` / `action` — 각각 고유 색
- **Preview pane (280px)** 선택 항목 상세 (아이콘 · 타이틀 · 설명 · 단축키 · 태그 청크)
- **키보드 풀 지원**: ↑↓ 이동 · Enter 실행 · Esc 닫기
- **fuzzy match** 간단 구현 (공백 split AND 검색)
- 모달 크기 720×72vh 중앙 배치, backdrop-blur-md

## 구현 포인트

```tsx
// Command 타입
interface Command {
  id: string; label: string;
  group: 'phase' | 'domain' | 'tool' | 'action';
  path: string; icon: LucideIcon;
  shortcut?: string;
  keywords: string[];
}

// 키보드 전역 핸들러 (App.tsx)
if ((e.ctrlKey || e.metaKey) && e.key.toLowerCase() === 'k') {
  setPaletteOpen((o) => !o);
}
```

## Anti-patterns

- shadcn CommandDialog 기본값만 쓰면 Spotlight 감 없음 — Preview pane + 카테고리 컬러 필요
- 카테고리 5개 이상 → 우측 Preview와 균형 무너짐. 4개가 이상적
- 검색 결과 높이 고정 안 하면 모달이 너무 커짐 → max-h 72vh 필수
- 키보드 ↑↓ 시 선택 항목이 스크롤 영역 밖 → `scrollIntoView({ block: 'nearest' })` 필수
