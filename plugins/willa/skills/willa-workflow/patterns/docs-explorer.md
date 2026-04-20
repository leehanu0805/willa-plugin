# Pattern · Docs Explorer (다른 localhost)

## 요약
UI의 **모든 요소(Phase · Domain · Tool · Inspector · 패턴)를 구체적으로 설명**하는 독립 문서 사이트. 메인 프로토타입(`localhost:5173`)과 **별도 포트**(`localhost:5174`)에서 실행. 인수인계 · 교육 · 디자인 리뷰 · 신규 개발자 온보딩 용도.

## 왜 별도 사이트인가

- 메인 프로토타입은 **의사가 실제 쓸 UI** — 군더더기 없어야 함
- 문서는 **개발자/관리자가 보는 것** — 서로 방해하면 안 됨
- localhost:5173 이 실제 목업 · localhost:5174 가 문서
- 둘 다 동시에 띄워놓고 비교 가능

## 위치

```
C:\code\Will3D\prototype\
├── will3d-ui/        (메인 목업 · localhost:5173)
│   └── src/
│       └── lib/      (constants.ts · medical-data.ts)
└── willa-docs/       (문서 · localhost:5174)     ← 이 패턴
    └── src/
        └── (will3d-ui/src/lib에서 import — 단일 정보원)
```

## 구성 페이지

```
/                     홈 · 검색 · 카테고리 타일 6개
/phases               4-Phase 개요 + 각각 상세 페이지 링크
/phases/:key          개별 Phase 상세 (UI 구조 · 도메인 · 스크린샷)
/domains              Phase × Domain 매트릭스
/domains/:key         개별 Domain 상세 (용도 · 언제 씀)
/tools                42개 도구 전체 카탈로그 (필터: 그룹/빈도)
/tools/:key           개별 도구 상세 (단축키 · 사용 예)
/inspector            Inspector 3-탭 각각의 섹션 해설
/patterns             7개 UI 패턴 문서 렌더링 (willa-plugin/patterns/*.md)
/shortcuts            전체 키보드 단축키 표
/glossary             의료 용어 + IA 용어 (FDI · MPR · OTF · Phase 등)
/changelog            .willa/plan-*.md 목록 · 각 변경 사항
```

## 콘텐츠 기준

각 페이지는 다음 포함:

1. **"이게 뭔가?"** — 1-2문장 설명
2. **"어디에 쓰나?"** — 실제 의사 사용 시나리오
3. **"어떻게 접근?"** — 클릭 경로 · 단축키 · 커맨드 팔레트 키워드
4. **"연관 기능"** — 이 도구와 함께 쓰는 도구들
5. **"스크린샷/다이어그램"** — 해당 영역만 하이라이트한 시각 자료
6. **"기술 참조"** — 실제 Will3D C++ 클래스명 (있으면)

## 데이터 소스

단일 정보원(single source of truth) 원칙:

```ts
// willa-docs/src/lib/data.ts
export { PHASES, DOMAINS, RAIL_TOOLS, COMMANDS } from '../../will3d-ui/src/lib/constants';
export { PATIENT, STUDY, SELECTED_IMPLANT, ... } from '../../will3d-ui/src/lib/medical-data';
```

`willa-docs`는 `will3d-ui`의 데이터를 **import만** 한다. 복사 금지.

## 디자인 시스템

`patterns/design-system.md` 의 Reading Room 그대로 사용. 같은 CSS 변수 · 같은 Geist 폰트. 문서 사이트라서 **typography가 더 중요**:

- Body text `font-size: 14.5px · line-height: 1.65`
- H1 24px · H2 18px · H3 14px uppercase tracking-wider
- Prose 에는 `max-width: 680px` (가독성)
- 코드 블록 Geist Mono + warm bg

## 기술 스택

- Vite + React + TypeScript (will3d-ui와 동일)
- `react-router-dom` (페이지 네비게이션)
- `@mdx-js/rollup` (마크다운 페이지 렌더링 — patterns/*.md 직접 로드)
- shadcn/ui 일부 재사용 (button · command · scroll-area · separator)

## 실행

```
cd C:/code/Will3D/prototype/willa-docs
npm install
npm run dev  # → localhost:5174
```

두 개 동시 실행:
- Terminal A: `cd prototype/will3d-ui && npm run dev` (포트 5173)
- Terminal B: `cd prototype/willa-docs && npm run dev` (포트 5174)

`vite.config.ts` 에 `server.port: 5174` 하드코딩.

## Anti-patterns

- ❌ 메인 앱과 같은 포트 공유 → 서로 방해
- ❌ 데이터 중복 (will3d-ui 와 willa-docs 양쪽에 TOOLS 배열) → 동기화 지옥
- ❌ 정적 HTML — Vite 사용해서 HMR 혜택 유지
- ❌ 화려한 애니메이션 · 문서 사이트는 **조용하고 읽기 좋아야**
