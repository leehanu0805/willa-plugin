# Pattern · Context Menu (Right-click)

## 요약
뷰포트 객체(임플란트/측정/신경 등) 및 사이드 패널 항목에서 **우클릭 → 관련 액션 메뉴**. 서브메뉴로 액션 카테고리 분리. shadcn ContextMenu 사용.

## Will3D 사례

### Viewport 우클릭 메뉴

```
🎯 Implant #36 · Sagittal
────────────────
Pencil   편집                    E
Copy     복제                   ⌘D
Lock     각도 잠금
Focus    포커스              ▸ [sub]
────────────────
Ruler    여기서 거리 측정       M
Shield   신경 거리 재계산
Activity 충돌 감지
────────────────
Brain    AI                  ▸ [sub]
Layers   세그 가시성           ▸ [sub]
────────────────
Max      이 뷰 최대화           F
Camera   캡처 저장            ⌘⇧C
FileText 보고서에 추가
Cross    모든 뷰 중심 동기화
────────────────
Trash    임플란트 삭제 (빨강)   ⌫
```

### Tool Rail 도구 우클릭

```
Pin/PinOff  Pinned 추가/해제
────────────────
도구 정보              (단축키 표시)
```

## 핵심 특징

- **헤더 라벨** — 무슨 객체의 컨텍스트인지 명시 (예: `Implant #36 · Sagittal`)
- **그룹핑** `ContextMenuSeparator` 로 의미 단위 구분
- **서브메뉴** — 관련 액션 3-4개는 서브메뉴로 내림
- **단축키 표시** `ContextMenuShortcut` 으로 우측 정렬
- **위험 액션** — 삭제 등은 맨 아래 + danger 컬러 (`text-[var(--w3-danger)]`)
- 서브메뉴 컨텐츠는 180-230px 폭 유지 (너무 좁으면 텍스트 잘림)

## 구현 포인트

```tsx
import {
  ContextMenu, ContextMenuContent, ContextMenuItem,
  ContextMenuLabel, ContextMenuSeparator,
  ContextMenuSub, ContextMenuSubContent, ContextMenuSubTrigger,
  ContextMenuTrigger, ContextMenuShortcut,
} from '@/components/ui/context-menu';

<ContextMenu>
  <ContextMenuTrigger asChild>
    <div>... target UI ...</div>
  </ContextMenuTrigger>
  <ContextMenuContent className="w-64 ...">
    <ContextMenuLabel>...</ContextMenuLabel>
    <ContextMenuSeparator />
    <ContextMenuItem>편집 <ContextMenuShortcut>E</ContextMenuShortcut></ContextMenuItem>
    ...
  </ContextMenuContent>
</ContextMenu>
```

## 설치

```
npx shadcn@latest add context-menu
```

## Anti-patterns

- 메뉴 항목 10개 이상 → 서브메뉴로 정리
- 아이콘 없음 → 훑어 읽기 어려움. 각 항목 좌측에 h-3 아이콘 권장
- 단축키 없이 메뉴로만 접근 → 숙련자 속도 저하. 최소 자주 쓰는 5개는 단축키 부여
- 여러 context에서 같은 메뉴 보여주기 → 각 객체 타입별로 다른 메뉴 필요
