# Pattern · Reading Room Design System

## 요약
Will3D의 디자인 랭귀지. **방사선과 판독실**의 분위기에서 영감 — 따뜻한 charcoal 베이스, 페이퍼 화이트 텍스트, **true black 뷰포트** (DICOM 표준), 의료 teal + radiological orange 액센트. `frontend-design` 플러그인과 함께 사용.

## 컬러 토큰

### Surface (warm ivory 계열, slate 아님)
```css
--w3-bg:         #faf8f4;   /* app 배경 */
--w3-bg-panel:   #e6e3dc;   /* 패널 (TopBar · Inspector · StatusBar) */
--w3-bg-elev:    #dbd7cf;   /* elevated · 선택된 상태 */
--w3-bg-hover:   #d2cec4;   /* hover */
--w3-bg-viewport:#0a0908;   /* CT 뷰 · 검정 유지 (DICOM 표준) */
```

### Lines (경계선)
```css
--w3-line:       rgba(60, 50, 40, 0.14);   /* 일반 hairline */
--w3-line-hi:    rgba(60, 50, 40, 0.22);   /* 강조 경계 */
--w3-line-lo:    rgba(60, 50, 40, 0.06);   /* 은은한 구분 */
```

### Text (warm near-black)
```css
--w3-text:       #1c1a17;   /* 주 텍스트 */
--w3-text-dim:   #595148;   /* 설명/라벨 */
--w3-text-mute:  #86786a;   /* 메타 정보 */
--w3-text-faint: #b0a89e;   /* placeholder */
```

### Viewport-local (뷰포트 내부 light 오버레이용)
```css
--w3-view-text:       #dcd6cc;
--w3-view-text-dim:   #8a8378;
--w3-view-text-faint: #4a453f;
```

### Accents
```css
--w3-accent:       #1f6d92;   /* 의료 teal · UI active */
--w3-accent-hov:   #2685b0;
--w3-accent-soft:  rgba(31, 109, 146, 0.08);
--w3-accent-line:  rgba(31, 109, 146, 0.32);

--w3-measure:      #c86a1a;   /* radiological orange · 측정선 */
--w3-measure-soft: rgba(200, 106, 26, 0.10);
```

### Status
```css
--w3-success: #4a9378;
--w3-warn:    #c88b20;
--w3-danger:  #b84a3d;
```

## 타이포그래피

- **Geist Variable** · 주 폰트 (깔끔한 sans-serif)
- **Geist Mono Variable** · 수치/데이터 전용 (tabular-nums)
- 기본 크기 **13px / line-height 1.45 / letter-spacing -0.003em**
- 섹션 헤더 **10.5px · uppercase · tracking 0.08em · font-semibold**

```ts
font-feature-settings: 'cv11', 'ss01', 'ss03';
```

### 사용 규칙
- **Mono는 숫자와 뷰포트 오버레이만** — mono 남발 금지
- **본문은 Geist Variable** · 한글/영문 혼용 OK
- 한글 가중치는 영문보다 살짝 무겁게 보이므로 중요 요소에만 `font-semibold`

## 스페이싱 스케일

Tailwind 기본 + `px-5 py-4` (섹션), `gap-2.5` (아이템), `mb-1.5` (라벨) 조합 사용.

## 모션

- **Hover transition**: 120ms (짧게 · 의료 SW는 게임 아님)
- **Panel slide-in**: 200ms ease-out
- **Phase 전환**: 180ms (`phase-enter` 애니메이션)
- **금지**: pulse · LED glow · 화려한 shimmer (반복 2.2s pulse는 Guide pin 전용 예외)

## 아이콘

- **Lucide-react only** — 일관성 · accessibility
- 기본 사이즈 `h-3.5 w-3.5` (14px)
- 뷰포트 컨트롤 `h-3 w-3` (12px)
- Domain/Hero 아이콘 `h-4 w-4` ~ `h-5 w-5`
- **stroke-width 1.5** (inactive) · **1.75~2** (active/primary)
- 이모지 절대 금지 (의료 SW 품위)

## 그림자

- 거의 사용 안 함
- 예외: 모달/팝업 (Spotlight · AI Panel · Context Menu)
- `0 4px 12px rgba(60, 50, 40, 0.12)` 정도의 **warm shadow** (slate 아님)

## 뷰포트 chrome

뷰포트 내부 오버레이는:
- **방향 마커** A/P/R/L/S/I · mono 10px · opacity 0.7
- **Corner brackets** 2.5×2.5px · 0.5 opacity
- **Crosshair** accent-line color · 0.32 alpha
- **Scale bar** · mono 9.5px
- **WW/WL** 우하단 · mono 9.5px · text-faint

## frontend-design 플러그인 사용 규칙

`frontend-design:frontend-design` skill 호출 시 **항상 이 디자인 시스템을 우선**으로 지시한다:

```
skill argv 예시:
"Reading Room 디자인 시스템을 따름 (see patterns/design-system.md).
새 컴포넌트 X를 생성. warm ivory bg + Geist + medical teal accent.
glow/pulse/이모지 금지. mono는 숫자만."
```

## Anti-patterns (금지)

- ❌ 차가운 slate/blue-grey (현대 SaaS 감성 — 의료 SW에 부적합)
- ❌ 이모지 아이콘 (Lucide 일관성 깨짐)
- ❌ Mono 폰트 남발 (tech cosplay 되어버림)
- ❌ Glow · pulse · LED 깜빡임 (SF 게임 UI 연상)
- ❌ 과장된 그라디언트 배경 (generic AI aesthetic)
- ❌ 이모지로 카테고리 구분 (색상+아이콘으로)
- ❌ Purple gradients on white (흔한 AI 디자인 냄새)
