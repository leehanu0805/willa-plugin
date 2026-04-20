# Pattern · Phase Navigation

## 요약
사용자 워크플로우를 4-Phase (LOAD · EXAMINE · PLAN · DELIVER)로 나누고, 각 Phase는 **완전히 다른 UI**를 가진다. TopBar 중앙에 stepper 배치.

## 언제 쓰나
- 사용자가 여러 단계의 워크플로우를 순차적으로 거치는 도구
- 각 단계에서 필요한 기능/컨트롤이 본질적으로 다를 때
- 비선형 이동(왕복)을 허용해야 할 때

## 구성 요소

```
TopBar [Logo][환자][Study][← Phase stepper →][Search][Bell][Settings][WinCtl]
           │            │
           └─ 환자 컨텍스트  │
                         │
                         └─ 4-step 진행 표시, 현재 위치 하이라이트
```

Phase별 화면 구조:
- **LOAD**: 파일 업로드 + Import queue + Recent + Storage (ToolRail/Inspector 없음)
- **EXAMINE**: ToolRail + Viewport + Inspector (풀 clinical chrome)
- **PLAN**: Implant Library + 3D 대형 + 2D mini slices (전용 레이아웃)
- **DELIVER**: Outline + Rich-text Editor + Captures/Export

## 구현 포인트

```ts
// constants.ts
export type PhaseKey = 'load' | 'examine' | 'plan' | 'deliver';
export const PHASES = [
  { key: 'load', index: 1, ... },
  { key: 'examine', index: 2, ... },
  { key: 'plan', index: 3, ... },
  { key: 'deliver', index: 4, ... },
];
```

```tsx
// App.tsx
const clinicalChrome = phase === 'examine';
// phase 에 따라 center 영역 교체
{phase === 'load' && <PhaseLoad />}
{phase === 'examine' && <Viewport />}
{phase === 'plan' && <PhasePlan />}
{phase === 'deliver' && <PhaseDeliver />}
```

## Anti-patterns

- Phase 전부에 같은 UI를 두면 Phase의 의미 소실 → 단순 탭으로 축소
- 5개 이상 Phase → 사용자 인지 과부하. 4개가 상한선
- 선형 강제 (LOAD 완료 전 EXAMINE 잠금) → 실제 임상 왕복 흐름 방해. 자유 이동 허용
