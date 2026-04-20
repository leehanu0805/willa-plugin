# Pattern · Anatomy Navigator

## 요약
도메인별(의료/생체/건축 등) **시각적 구조물을 SVG로 그리고** 각 부위를 클릭해 가시성 토글. Inspector 안의 전용 카드 형태. 좌 SVG + 우 레전드 리스트 동기화.

## Will3D 사례

9개 해부 구조 (Cranium · Maxilla · Mandible · Upper teeth · Lower teeth · Nerve · Sinus · Airway · Soft tissue) — 두개골 측면도 SVG.

## 구성

```
┌─ Anatomy Navigator ────────────────┐
│ ┌──────────┐ 🤎 Soft tissue    👁  │
│ │   ◯      │ ⚪ Cranium        👁  │
│ │  [SVG]   │ 🟡 Maxilla        👁  │
│ │   ▓▓▓    │ 🟫 Mandible       👁  │
│ │  teeth   │ 🔷 Upper teeth    👁  │
│ └──────────┘ 🔵 Lower teeth    👁  │
│              🟠 Nerve           👁  │
│              🟣 Sinus           👁  │
│              🔷 Airway          👁❌│
├────────────────────────────────────┤
│ 모두 보기 · 모두 숨기기 · 치아만    │
└────────────────────────────────────┘
```

## 핵심 특징

- **양방향 동기화** — SVG 클릭 ↔ 레전드 클릭 모두 hover/active 상태 공유
- **숨김 상태** — 투명도 8% + 점선 아웃라인 (위치 참조 가능)
- **Active 상태** — 진한 fill + 실선
- **Hover tooltip** — SVG 내부 좌상단에 부위 이름 박스
- **Quick presets** 하단 — 자주 쓰는 조합 (의료: "치아만", "뼈만" 등)

## 구현 포인트

```tsx
type RegionKey = 'skull' | 'maxilla' | ... ;
const [visible, setVisible] = useState<Record<RegionKey, boolean>>({...});
const [hover, setHover] = useState<RegionKey | null>(null);
```

SVG path는 단순화된 해부 실루엣. 정확한 해부학 그림 아님 — 식별 가능한 수준이면 OK.

## 다른 도메인 응용

- 금융 대시보드 → 포트폴리오 구조도 (주식/채권/현금 등 섹션)
- 건축 CAD → 건물 층별 네비게이션
- 생물 교육 → 세포/장기 구조도
- 차량 → 엔진/서스펜션/차체 토글

## Anti-patterns

- 너무 사실적인 해부도 → 복잡해서 오히려 읽기 어려움
- 색상 부재 → 카테고리 구분 안 됨. **각 부위 고유 색** 필수
- SVG만 있고 레전드 없음 → 스크린리더 접근성 0
- hover 상태 전환 너무 느림 (300ms+) → 정보 탐색에 방해. 100-150ms 권장
