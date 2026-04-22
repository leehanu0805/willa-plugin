# willa references · Phase 매핑 인덱스

`ux-ui-mastery` 플러그인에서 **방법론 + 미감 업그레이드**에 해당하는 10개 파일 흡수.
Will3D 속성 매칭용(data-dense · industry-patterns 등)은 의도적으로 제외 — willa 는 Will3D 를 이미 안다.

모든 파일은 영어. 필요한 항목만 발췌해 willa 출력에 사용한다.
**통째로 읽지 말 것** — 섹션 목차 훑고 해당 섹션만 참조.

---

## 방법론 (UX 프로세스 강화)

| 파일 | 어느 Phase 에서 | 쓸 때 |
|------|-------------|------|
| `nng-heuristics.md` | **4.5 Investigate** | 10 heuristic 체크리스트로 품질 조사 |
| `critique-methodology.md` | **6 Polish** | Liz Lerman 4-step 비평 프로토콜 · frontend-design 상담 질문 설계 |
| `laws-of-ux.md` | **3 Plan** · **newilla Recommend** | Fitts/Hick/Miller/Jakob 등 archetype 배치 근거 강화 |
| `cognitive-biases.md` | **3 Plan** | 의사 의사결정에서 Anchoring/Confirmation bias 등 대응 설계 |

## 미감 (디자인 품질 상향)

| 파일 | 어느 Phase 에서 | 쓸 때 |
|------|-------------|------|
| `typography-color.md` | **3 Plan** 토큰 섹션 | 팔레트 · 타이포 위계 구체값 고를 때 |
| `token-specification.md` | **3 Plan** 토큰 섹션 | 토큰 구조/네이밍/계층 설계 |
| `motion-timing.md` | **3 Plan** 모션 섹션 · **Scaffold** | duration/easing 값 선택 근거 |
| `emotional-design.md` | **3 Plan** | 의료 SW 에 적합한 차분함/신뢰 톤 표현 |

## 상태/문구 (Scaffold 완성도)

| 파일 | 어느 Phase 에서 | 쓸 때 |
|------|-------------|------|
| `states-empty-error.md` | **4 Scaffold** | 로딩/빈/에러 상태 패턴 (세그 진행 · 데이터 미로드) |
| `microcopy.md` | **4 Scaffold** · **3 Plan** 라벨 | 버튼/툴팁/안내 문구 패턴 (영어 원본 · 프로젝트 로케일에 맞춰 번역 또는 영어 그대로) |

---

## 사용 규칙

1. **인용만 · 복붙 금지** — Will3D 컨텍스트에 맞게 해석해서 출력. 영어 블록 통째로 토해내지 말 것
2. **출력 언어는 사용자 따라감** — 한국어·영어·혼용 모두 OK. 다국어 UI 설계 시 i18n 키 구조 고려
3. **섹션 단위 참조** — 파일 전문 읽지 말고 `grep` 또는 Read offset/limit 으로 관련 섹션만
4. **과적용 금지** — "Laws of UX 25개 다 언급" 금지. plan 문서에는 **해당 결정 1-2개 근거** 만
5. **토큰 값은 Will3D 하이브리드 테마에 맞춤** — 뷰포트는 dark(DICOM 표준 고정) · chrome 은 bright 선호. `typography-color.md` 예제 팔레트는 출발점, 그대로 쓰지 말 것

---

## 왜 이 10개만인가

**제외된 것들**:
- `mobile-*` · `ios/android` — Will3D 데스크톱
- `cross-cultural-i18n` — 필요 시 별도 감사 (다국어 확장 시점에 참조)
- `ambient-calm-zero-ui` · `ai-spatial-voice` — 도메인 부적합
- `ux-research-methods` — willa Discovery 는 코드 기반 · 사용자 인터뷰 안 함
- `data-dense-interfaces` · `data-visualization` · `industry-patterns` — Will3D 속성 매칭 (willa 이미 앎)
- `wcag-aria-patterns` — 의료 데스크톱 a11y 는 필요시 별도 감사 ( `/willa:accessibility-check` 등 외부 플러그인 활용 )
- `component-patterns-code/react-cookbook` — 71K 단어 · 너무 무거움. Scaffold 시 `context7` MCP 로 shadcn/radix 문서 직조회가 효율적
- `figma-design-tool-workflows` — 우리 `/figwilla` 경로로 충분
