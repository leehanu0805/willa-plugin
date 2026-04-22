# Phase 6 · Polish (Design Audit)

Serve 이후 **선택적** 디자인 품질 감사 패스. 사용자가 `/willa polish` 또는 5단계 완료 시 "디자인 리뷰하시겠어요?" 질문에 yes 응답 시 진입.

## 목적

기획과 패턴은 맞았어도 **실제 구현에 미세한 디자인 결함**이 있을 수 있다:

- 스페이싱이 다른 섹션과 불일치
- 폰트 가중치가 계층감 약함
- 컬러 값이 토큰 대신 하드코딩됨
- hover state 빠짐
- 경계선 두께 불일치

Phase 6는 이를 스캔하고 수정 제안.

## 절차

### Step 0 · 비평 프로토콜 (선택 · 심화)

복잡한 plan 이면 `references/critique-methodology.md` 의 **Liz Lerman 4-step** 구조로 감사:

1. **Statements of Meaning** — 무엇이 효과적이었는지 서술
2. **Artist as Questioner** — willa(작성자) 가 불확실한 부분 질문
3. **Neutral Questions** — 감사자가 판단 없는 질문
4. **Opinion Time** — "X 에 대한 의견 들어도 될까?" 허락 받고 의견

이 프로토콜로 frontend-design 호출 프롬프트 설계. 무턱대고 "이것저것 고쳐" 금지.

### Step 1 · 자동 감사 (frontend-design + 패턴)

`Skill` 도구로 `frontend-design:frontend-design` 호출:

```
Skill(
  skill: "frontend-design:frontend-design",
  args: "Will3D UI Reading Room 디자인 시스템 준수 감사.
          대상: {방금 Scaffold에서 수정된 컴포넌트 목록}
          기준: patterns/design-system.md
          항목:
          1. 컬러 토큰 vs 하드코딩 색
          2. 타이포 스케일 일관성
          3. 스페이싱 리듬
          4. hover/focus 상태 완전성
          5. anti-patterns (glow/pulse/이모지) 존재 여부
          6. 뷰포트 chrome 규칙 (방향 마커/scale bar 등)
          출력: 발견된 이슈를 리스트로 · 심각도(critical/minor/nit)"
)
```

### Step 2 · 이슈 분류

감사 결과를 **심각도별** 분류:

- **Critical** — 디자인 시스템 정면 위반 (즉시 수정)
- **Minor** — 일관성 문제 (선택적 수정)
- **Nit** — 주관적 개선점 (기록만)

### Step 3 · 수정 제안 제시

사용자에게:

```
🎨 디자인 감사 완료

Critical (3):
- AI Panel 헤더에 하드코딩 색 #4fa8c9 → var(--w3-accent)로 교체
- Inspector 탭 밑줄 높이 0.5px → 일관성 위해 h-0.5 통일
- Anatomy legend에 hover 상태 누락

Minor (5):
- Phase stepper 간격이 다른 영역 대비 4px 좁음
- Measurement 행 line-height 1.3 → 1.45
- ...

Nit (2):
- Pinned 섹션 배경 tint 더 진하게 해도 좋음
- ...

수정 진행? [Critical만 / 전부 / 건너뛰기]
```

### Step 4 · 적용

사용자 선택에 따라:
- **Critical만** — 3건 자동 수정 + TypeScript 체크
- **전부** — Critical + Minor 모두 수정 (Nit은 기록만)
- **건너뛰기** — `.willa/audit-{timestamp}.md`에 이슈만 기록하고 종료

### Step 5 · 검증

수정 후:
- `npx tsc --noEmit` 통과
- 시각적 확인 (localhost:5173 새로고침 안내)
- HMR 정상 반영 확인

## 저장 파일

```
C:\code\Will3D\.willa\audit-{timestamp}.md
```

내용:
- 감사 시점 · 대상 컴포넌트
- 발견 이슈 (critical/minor/nit)
- 수정 여부
- 사용자 결정

## 언제 건너뛰어야 하나

- `/willa refresh` 실행 시 (이미 검증된 plan 재생성)
- 변경된 컴포넌트가 5개 이하 + 모두 데이터만 변경
- 사용자가 명시적으로 skip 요청

## Anti-patterns

- ❌ 매번 `frontend-design` 호출 → 토큰 낭비 + 과도 변경
- ❌ Nit까지 전부 자동 수정 → 사용자 의도 외 변경 발생
- ❌ 감사 없이 바로 수정 → 어떤 기준에 맞추는지 불투명
