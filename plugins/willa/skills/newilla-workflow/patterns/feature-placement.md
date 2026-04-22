# Pattern · Feature Placement

**`/newilla` Step 3-4 에서 참조하는 배치 사고 프레임.**
Classify 결과를 Recommend Top 3 로 변환할 때 이 문서의 3축 분류 + 8 아키타입 + 휴리스틱을 쓴다.

---

## 축 추가 · 상태 유지 범위 (Classify Q5)

기존 Q1-Q4 (빈도/사용자/타입/Phase) 에 **Q5 상태범위** 추가:

| 범위 | 예 | 배치 영향 |
|------|-----|----------|
| **환자별** (Per-patient) | 임플란트 배치 · 측정 · 주석 | Inspector (환자 컨텍스트 내) |
| **세션별** (Per-session) | 최근 탭 · Undo 스택 · AI 대화 히스토리 | Inspector 탭 or 사이드 패널 |
| **전역** (Global) | WW/WL 프리셋 · 테마 · 단축키 설정 | TopBar / Palette / Settings |

이 축이 "데이터 어디 저장?" 을 결정. 환자별이면 Inspector, 전역이면 TopBar/Settings 로 자연스럽게 갈림.

---

## 8개 배치 아키타입 (P1-P8)

Step 4 Recommend 에서 후보 3개 고를 때 이 8개 중 선택. 임의 위치 금지.

| # | 아키타입 | 적합 기능 | 트레이드오프 |
|---|---------|----------|-------------|
| **P1** | Inspector 탭 추가 | 가끔 참조 · 컨텍스트 보존 · 세션 상태 | Inspector 4탭 이상 시 혼잡 |
| **P2** | BottomDock 버튼 + 팝오버 | 자주 · 빠른 ON/OFF · 뷰포트 가림 최소 | 공간 작음 · 복잡 UI 안 들어감 |
| **P3** | ToolRail 섹션 | 상시 · 도구류 · 그룹 연관성 | Rail 길이 증가 · 그룹 재편성 필요 |
| **P4** | TopBar 액션 버튼 | 전역 · 상시 노출 · 긴급 접근 | 공간 제약 · 4개 이상 시 산만 |
| **P5** | Command Palette 항목 | 드물게 · 검색으로 찾음 · 단축키 있음 | 발견성 낮음 · 신규 사용자 못 찾음 |
| **P6** | 컨텍스트 메뉴 (우클릭) | 선택 객체 종속 · 단일 대상 작업 | 우클릭 학습 필요 · 모바일 불가 |
| **P7** | 사이드 슬라이드 패널 (비모달) | 컨텍스트 유지 + 복잡 UI 필요 | 공간 차지 · 뷰포트 일부 가림 |
| **P8** | Floating Overlay (비모달) | 임시 · 드래그 가능 | 위치 예측 어려움 · 뷰포트 일부 가림 |

⚠ **모달 다이얼로그는 아키타입에 포함 안 함** — 의료 뷰에서 뷰포트 차단은 최후 수단. 꼭 필요하면 P4+모달 조합으로 정당화.

---

## 매칭 표 (확장)

기존 SKILL.md 의 "빈도 × 타입" 표를 5축(빈도 × 타입 × 상태범위 × Phase × 사용자) 기반으로 확장:

| 빈도 | 타입 | 상태범위 | 추천 아키타입 |
|------|------|----------|-------------|
| 일상 | Tool | 환자별 | P3 ToolRail Pinned |
| 일상 | Panel | 환자별 | P1 Inspector 탭 최상단 |
| 일상 | Action | 전역 | P2 BottomDock · P4 TopBar |
| 가끔 | Tool | 환자별 | P3 ToolRail drawer |
| 가끔 | Panel | 환자별 | P1 Inspector 섹션/탭 |
| 가끔 | Panel | 세션별 | P1 Inspector 탭 · P7 슬라이드 |
| 가끔 | Modal | 전역 | P4 TopBar + 모달 · P5 Palette |
| 특수 | Tool | 환자별 | P5 Palette + P3 숨김 |
| 특수 | Action | 전역 | P5 Palette 전용 |
| Phase 특화 | 어떤 것이든 | - | 해당 Phase 전용 도메인 (SubNav) |

Phase 특화(예: DELIVER 전용 Export)는 SubNav 의 해당 Phase 내부에 배치.

---

## 공통 기능 권장 배치 (레퍼런스)

업계 표준 의료 뷰어에서 자주 나오는 기능의 기본 배치. 답 아니라 **출발점**:

| 기능 | 주 권장 | 대안 |
|------|---------|------|
| **AI 챗/어시스턴트** | P1 Inspector 탭 | P7 슬라이드 · P5 Palette 호출 |
| 알림 (Notification) | P4 TopBar 아이콘 + 드롭다운 | 지속 StatusBar |
| 검색 (Search) | P4 TopBar + P5 Palette 통합 | - |
| 설정 (Settings) | P5 Palette + 모달 | P4 TopBar 아이콘 |
| Undo/Redo | P2 BottomDock · 단축키 | P4 TopBar |
| 보고서/Export | P5 Palette + P4 TopBar | 모달 |
| 고급 측정 (체적 등) | P3 ToolRail 측정 그룹 | P1 Inspector 결과 탭 |
| Layout 프리셋 | P2 BottomDock | - |
| 캡처 (스크린샷) | P2 BottomDock · 단축키 | P4 TopBar |
| Airway/기도 분석 | P1 Inspector 탭 (세션별) | P3 ToolRail 측정 |

---

## UX 법칙 적용

**심화 참조**: willa 플러그인 내 `../../willa-workflow/references/laws-of-ux.md` 에 25+ 법칙 전문. 아래 5개는 feature-placement 에 **직접** 관련.

### Fitts' Law
자주 쓰는 것 = 큰 타겟 · 가까운 위치. **상시 기능은 BottomDock/ToolRail 고정** (화면 가장자리 = 무한대 타겟).

### Hick's Law
최상위 진입점 개수 제한:
- TopBar 액션 ≤ 5
- ToolRail 섹션 ≤ 6
- Inspector 탭 ≤ 5
- BottomDock ≤ 8

초과하면 그룹핑 또는 P5 Palette 로 이전 고려.

### 3-클릭 규칙
자주 쓰는 기능은 **3클릭 이내** 도달 가능해야 함. 4클릭 이상이면 배치 재고.

### Progressive Disclosure
고급 옵션은 기본 UI 에서 숨기고 "고급" 확장으로 노출. 신규 사용자 혼잡 감소.

### 발견성 vs 효율
- **신규 사용자** → 시각적 진입점 필요 (P1-P4)
- **전문가** → 단축키 + Palette (P5) 로 충분
- **둘 다** → 시각 진입점 + 단축키 병행 (예: TopBar 아이콘 + Alt+N)

---

## 금지 (default 기피)

`/newilla` 가 배치 결정할 때 **자동 채택 금지** 위치들:

- **기본값으로 모달/Sheet** — 인라인/컨텍스추얼(P1, P6, P7) 검토 없이 모달 선택 금지. 뷰포트 차단은 흐름 파괴.
- **기본값으로 Floating 버튼** — 우하단 파란 원 = 소비자 앱 패턴. 의료 SW 에 부적합. 필요하면 P4 TopBar 로 승격.
- **기본값으로 새 탭/Phase** — 기존 Inspector 섹션/탭 재사용 먼저 검토. 새 탭은 밀도 증가의 근거 필요.
- **후보 1개만 제시** — Top 3 필수 (SKILL.md Step 4 참조). 후보 간 트레이드오프 비교 없이 추천 금지.
- **"일단 만들고 피드백" 패턴** — 배치 고민 없이 코드부터 쓰는 반응형 작업 금지.

---

## 이번 AI 챗 사례 (역산)

만약 `/newilla "AI 챗"` 이 처음부터 호출됐다면:

```
Step 3 Classify
  Q1 빈도     : 가끔
  Q2 사용자   : 모든 의사
  Q3 타입     : Panel (대화 히스토리 + 입력)
  Q4 Phase    : 여러 Phase 공통
  Q5 상태범위 : 세션별 (대화 localStorage)

Step 4 Recommend (Top 3)
  A. P1 Inspector 탭 (4번째)
     근거: 가끔 + Panel + 세션별 = 전형적 Inspector 탭 매칭
     구현: inspector.tsx 에 'ai' 탭 추가
     트레이드오프: Inspector 4탭 혼잡 가능

  B. P7 사이드 슬라이드 패널
     근거: 복잡 UI 여지 + 컨텍스트 유지
     구현: 신규 AIPanel + ToolRail/TopBar 진입 버튼
     트레이드오프: Inspector 와 공간 경쟁

  C. P5 Command Palette 호출형
     근거: 전문가용 · Ctrl+K 로 prompt 입력
     구현: COMMANDS 배열 + 모달 프롬프트
     트레이드오프: 신규 사용자 발견 불가

Step 5 Patch
  사용자 → A 선택 → inspector.tsx 수정
```

**Sheet 만들기 → 탭 옮겨달라 → 옮기기** 왕복 없이 처음부터 바로 Inspector 탭으로 감.
