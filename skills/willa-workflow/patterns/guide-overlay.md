# Pattern · Guide Overlay

## 요약
기존 UI 위에 **번호가 매겨진 pin**을 배치. hover 시 callout 카드로 설명. 디자인 스펙 공유, 신입 의사 onboarding, QA 검토 등에 유용. 3-모드 토글의 "GUIDE" 모드.

## 구성

```
─────────────────────────────────────
  [앱 UI 원본]    ①    ②    ③
                  ↓    ↓    ↓
                  pin  pin  pin
                  
  ④
  pin
  └→ hover 시 callout 카드 펼침:
     ┌──────────────────┐
     │ TOPBAR   #04      │
     │ ────────────────  │
     │ 4-Phase 워크플로우 │
     │ LOAD → EXAMINE … │
     │ ────────────────  │
     │ 단축키: ⌃K         │
     └──────────────────┘
─────────────────────────────────────
```

## 핵심 특징

- **카테고리별 컬러 pin** (topbar/subnav/rail/viewport/inspector/status 6색)
- **pulse ring 애니메이션** (2.2s 반복) — 눈에 띄게
- **hover → Callout 카드** 방향별(top/bottom/left/right) 배치
- Callout 내용: 카테고리 뱃지 + #NN + 타이틀 + 설명 + 단축키 + 태그
- **잠시 숨김** 버튼 — overlay opacity 20%, hover 시 복귀
- 모드 토글 통해 MOCKUP/WIREFRAME/GUIDE 전환

## 구현 포인트

```tsx
// Annotation 타입
interface Annotation {
  id: string; number: number;
  title: string; description: string;
  shortcuts?: string[];
  category: 'topbar' | 'subnav' | 'rail' | 'viewport' | 'inspector' | 'status';
  pin: { top?: string; left?: string; right?: string; bottom?: string };
  callout: 'left' | 'right' | 'top' | 'bottom';
}
```

```css
@keyframes guide-pulse {
  0%   { transform: scale(1);   opacity: 0.35; }
  80%  { transform: scale(1.8); opacity: 0;    }
}
```

## 배치 전략

- **fixed 좌표** 사용 (top/left/right/bottom) — 컴포넌트 구조 바뀌어도 유지
- Phase별로 다른 annotation 세트 필요 시 조건부 렌더
- 20개 내외가 상한 — 더 많으면 사용자 피로

## Anti-patterns

- 정적 스크린샷 + 오버레이 → HMR 갱신 안 됨. 실제 앱 위에 렌더해야 함
- pin 위치를 동적 계산 (getBoundingClientRect) → 성능 저하 + 위치 흔들림. 정적 좌표 권장
- Callout 너무 길게 → 3-4줄이 최대. 더 필요하면 별도 링크
- 카테고리 색이 UI 본체 색과 충돌 → 구분이 어려움. 밝고 포화도 높은 색 권장
