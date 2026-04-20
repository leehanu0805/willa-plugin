# Pattern · Tool Rail

## 요약
좌측 148px 세로 패널. 4개 그룹(Navigate/View/Measure/Annotate)으로 도구를 카테고라이즈. **Pinned** + **Recent** 섹션 상단 + 우측 ▶ 버튼으로 **전체 도구 drawer**(220px 추가) 확장.

## 구성 요소

```
┌─ ToolRail (148px) ──────┐▶ (expand)
│ ⭐ Pinned (3)          │
│   ★ Ruler · M          │
│   ★ W/L · W            │
│   ★ Layout             │
│ ─                       │
│ ⏱ Recent (5)            │
│   Pan · H              │
│   ...                   │
│ ─                       │
│ NAVIGATE  3/12          │
│   ◈ Select · V         │
│   ✋ Pan · H            │
│   🔍 Zoom · Z           │
│ ─                       │
│ VIEW  4/12              │
│ ─                       │
│ MEASURE  3/10           │
│ ─                       │
│ ANNOTATE  2/9           │
├─────────────────────────┤
│ ● Connected     v2.1.2  │
└─────────────────────────┘
```

확장된 drawer (220px, 펼침):
- 2-column 그리드
- 각 도구 카드: 아이콘 + 라벨 + 단축키
- Pinned 도구는 우상단 ⭐ 표시

## 핵심 특징

- **도구 우클릭** → "Pinned에 추가/해제" ContextMenu
- **도구 클릭** → 활성화 + 자동으로 Recent 갱신 (최대 5개 FIFO)
- **Primary 플래그** — 각 그룹에서 기본 노출할 대표 도구 지정
- **localStorage** 로 Pinned/Recent 저장

## 구현 포인트

```ts
// RailTool 타입에 primary 플래그 + shortcut
interface RailTool {
  key: string; label: string; icon: LucideIcon;
  group: 'navigate' | 'view' | 'measure' | 'annotate';
  shortcut?: string;
  primary?: boolean;  // compact rail 표시 여부
}
```

```tsx
// localStorage 키
const STORE_PINNED = 'w3-tool-pinned';
const STORE_RECENT = 'w3-tool-recent';
const MAX_RECENT = 5;
```

## Anti-patterns

- 모든 도구를 Primary로 → compact rail이 너무 빽빽
- Pinned 없음 → 자주 쓰는 5-10개 찾기 어려움
- 그룹 5개 이상 → Navigate/View/Measure/Annotate 4개로 충분
- Recent가 너무 긺 (10+) → 노이즈
