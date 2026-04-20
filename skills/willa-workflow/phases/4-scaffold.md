# Phase 4 · Scaffold

Plan을 실제 `prototype/willa-preview/` 코드에 반영한다.

## 범위

**수정 허용**:
- `prototype/willa-preview/src/lib/constants.ts` (Phase/Domain/Tool 데이터) ← 단일 정보원
- `prototype/willa-preview/src/lib/medical-data.ts` (샘플 데이터) ← 단일 정보원
- `prototype/willa-preview/src/components/**.tsx`
- `prototype/willa-preview/src/index.css` (design tokens)
- `prototype/willa-preview/src/App.tsx`, `main.tsx`
- `prototype/willa-docs/src/**` (문서 사이트 · `patterns/docs-explorer.md` 참조)

**willa 는 오직 `prototype/willa-preview/` 만 수정한다.**

- `prototype/will3d-ui/` · `prototype/willa-docs/` 는 willa와 무관. 건드리지 않음.
- Will3D C++ 본체 절대 수정 금지 (읽기 전용).

**수정 금지**:
- `prototype/willa-preview/src/components/ui/**` (shadcn 생성 컴포넌트 — `npx shadcn add` 로만)
- `prototype/willa-preview/node_modules/**`
- `prototype/willa-preview/package-lock.json`
- Will3D 본체 (C++ 소스 전체)

## 실행 순서

1. **기존 파일 Read 필수** — 수정 전 항상 대상 파일을 Read 도구로 로드 (Will3D 규칙)
2. **데이터 먼저, UI 나중**: constants.ts / medical-data.ts 를 먼저 수정하고 컴포넌트 조정
3. **TypeScript 체크**: 각 수정 후 `npx tsc --noEmit` — 에러 0 유지
4. **HMR 의존**: dev server가 이미 떠 있으면 저장하면 자동 반영. 아니면 Serve 단계에서 기동.

## frontend-design 플러그인 호출 (신규 컴포넌트 시)

새 컴포넌트를 **처음부터 만들 때만** `Skill` 도구로 `frontend-design:frontend-design` 호출. 기존 컴포넌트 수정은 패턴 문서(`patterns/*.md`)만으로 충분.

### 호출 형식

```
Skill(
  skill: "frontend-design:frontend-design",
  args: "<Reading Room 디자인 시스템 준수 명시>\n
          <컴포넌트 역할/범위>\n
          <참조할 기존 컴포넌트>\n
          <절대 준수사항: glow/pulse/이모지 금지, mono는 숫자만>"
)
```

### 준수 원칙 (argv에 명시)

1. **Reading Room 토큰만 사용** — `patterns/design-system.md` 참조
2. **Lucide 아이콘 · 1.5~2 stroke** (이모지 금지)
3. **mono 폰트는 숫자 전용**
4. **120ms hover · 200ms panel** — 그 이상 느리거나 화려한 애니메이션 금지
5. **shadcn 컴포넌트 재사용** — `src/components/ui/` 먼저 확인
6. **warm shadow** — slate-blue 아님

### 호출 후 검증

생성된 코드를 그대로 사용하지 말고:
- 컬러 값이 `var(--w3-*)` CSS 변수로 되어 있는지
- 이모지/glow 없는지
- 토큰 외 직접 색 사용 시 `patterns/design-system.md` 와 대조
- 불일치 시 인라인 수정

## 패턴별 적용 가이드

### Phase / Domain 변경
```ts
// constants.ts
export const DOMAINS: Record<PhaseKey, Domain[]> = {
  examine: [
    { key: 'mpr',  label: 'MPR', icon: Crosshair },
    { key: 'airway', label: 'Airway', icon: Wind },  // ← 추가
  ],
};
```

### Tool 추가
```ts
// constants.ts — RAIL_TOOLS 배열에
{ key: 'hu-probe', label: 'HU probe', icon: TestTube, group: 'measure', primary: false },
```

### Inspector 탭 추가
```tsx
// inspector.tsx
type Tab = 'properties' | 'segmentation' | 'airway' | 'history';
// ... 탭 UI + 섹션 컴포넌트 추가
```

### Guide Overlay annotation 추가
```tsx
// guide-overlay.tsx — ANNOTATIONS 배열에
{
  id: 'airway', number: 21, category: 'inspector',
  title: 'Airway 분석',
  description: '...',
  pin: { top: '520px', right: '290px' },
  callout: 'left',
},
```

## 3모드 동기화

Plan의 Q4에 따라:

| 모드 | 파일 |
|------|------|
| MOCKUP | `components/*.tsx` 전체 |
| WIREFRAME | `Wireframe.tsx` + `wireframe.css` |
| GUIDE | `guide-overlay.tsx` (ANNOTATIONS 배열) |

3개 모두 선택 시: MOCKUP 먼저 → 구조 확정되면 Wireframe + Guide 순서대로.

## 검증 체크리스트

Scaffold 완료 전:

- [ ] `npx tsc --noEmit` 에러 0
- [ ] `curl http://localhost:5173/` HTTP 200 (server 떠 있으면)
- [ ] Plan의 "제안 구조" 모든 항목이 실제 코드에 반영됨
- [ ] 지우기로 한 기능이 실제로 제거됨 (구 코드 잔재 확인)
- [ ] 최근 git status 에 예상치 못한 변경 없음

## 커밋 여부

자동 커밋하지 않는다. 사용자가 확인 후 본인이 커밋하거나 `/patch` 등으로 진행.

## 분기

- 전 체크리스트 통과 → Serve 단계
- TypeScript 에러 발생 → 수정 반복. 3회 실패 시 사용자에게 보고하고 중단
- Plan과 맞지 않는 현재 코드 구조 발견 → plan 재검토 요청
