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

## CBCT 도구 본질 (절대 위반 금지)

willa-preview 결과물은 **여전히 CBCT 뷰어/플래너** 로 인식되어야 한다:

✅ **유지해야 함**
- 뷰포트가 화면 주인공 (최소 60%, 권장 70%+)
- 3축 MPR + 3D VR 표시 가능 (Axial/Sagittal/Coronal/3D)
- 다크 뷰포트 (DICOM 표준)
- 드래그로 슬라이스 스크롤/도구 사용
- 빠른 도구 접근 (rail · 상단 strip · 단축키 등 어느 형태든)
- 측정 · 임플란트 · 세그먼트 등 객체 선택·편집

❌ **금지 형태**
- 저널/논문 리더 (정적 문서)
- 대시보드 (카드 위주, 작은 차트)
- 소셜/타임라인 피드
- 뷰포트가 썸네일 수준으로 작아지는 레이아웃

구조 자유도는 **뷰포트 배치 / 도구 배치 / 패널 배치** 내에서 발휘. 위 본질을 깨면 plan 단계로 복귀.

**수정 금지**:
- `prototype/willa-preview/src/components/ui/**` (shadcn 생성 컴포넌트 — `npx shadcn add` 로만)
- `prototype/willa-preview/node_modules/**`
- `prototype/willa-preview/package-lock.json`
- Will3D 본체 (C++ 소스 전체)

## 실행 순서

1. **기존 파일 Read 필수** — 수정 전 항상 대상 파일을 Read 도구로 로드 (Will3D 규칙)
2. **데이터 먼저, UI 나중**: constants.ts / medical-data.ts 를 먼저 수정하고 컴포넌트 조정
3. **TypeScript 체크**: 각 수정 후 `npx tsc --noEmit` — 에러 0 유지

## 상태·문구 품질 (references 참조)

**완성도 올리는 선택적 참조** (`references/_INDEX.md` Scaffold 섹션):
- 로딩 / 빈 / 에러 / 온보딩 상태 패턴 → `references/states-empty-error.md` (세그 로딩 progress · 데이터 미로드 empty state · 신경 충돌 에러)
- 버튼/툴팁/안내 문구 패턴 → `references/microcopy.md` (영어 원본 · 프로젝트 로케일에 맞춰 번역 또는 영어 그대로 · 다국어 UI 설계 시 i18n 키 구조 고려)

규칙:
- 인라인 상태(loading/empty/error)는 의료 SW 에서 **필수** · placeholder 텍스트 "—" 한 글자로 끝내지 말 것
- microcopy 는 Will3D 도메인 용어 우선 ("슬라이스" · "세그먼트" · "신경 거리"), 패턴 파일은 스타일 참고만
4. **HMR 의존**: dev server가 이미 떠 있으면 저장하면 자동 반영. 아니면 Serve 단계에서 기동.

## frontend-design 플러그인 호출 (자유롭게)

새 컴포넌트는 `Skill("frontend-design:frontend-design")` 으로 생성. **willa가 이번 plan에서 결정한 방향성**을 argv로 전달하되, 특정 디자인 시스템 강제하지 않음.

### 호출 형식 (예시 · 강제 아님)

```
Skill(
  skill: "frontend-design:frontend-design",
  args: "이번 plan에서 택한 aesthetic: {willa의 방향성}
          컴포넌트 역할: {무엇을 만들 건지}
          제약: 치과 CT 전문가용 · shadcn/ui 재사용 가능
          그 외는 자유롭게 · distinctive design 환영"
)
```

### 호출 원칙

1. **plan의 디자인 방향성** 만 전달 · 과도한 제약 금지
2. **shadcn 컴포넌트** 는 `src/components/ui/` 재사용 (접근성 · 구조 때문)
3. 나머지 (컬러 · 타이포 · 모션 · 아이콘 스타일) 는 **willa/frontend-design 재량**
4. 생성된 결과가 plan 방향성과 맞는지만 검증 · 고정 체크리스트 없음

### Reading Room 은 한 예시일 뿐

`patterns/design-system.md` 의 Reading Room 은 **과거에 한번 탐색한 방향**.
이번에 다른 방향 선택 시 해당 패턴 문서 무시 OK. 새 aesthetic 이 plan 에 기록돼 있으면 그것이 우선.

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
