# Phase 3 · Plan

Discovery 에서 찾은 이슈 + Clarify 에서 고른 우선순위 → **이슈별 구체 해결책 + 시각 디테일**.
`C:\code\Will3D\.willa\plan-{YYYYMMDD-HHmm}.md` 저장.

## 핵심 원칙

- **Will3D 는 CBCT 뷰어/플래너** · 이 도메인에서 벗어난 재해석 금지
- 모든 제안은 **Discovery 의 구체 이슈**에서 출발 · 근거 있음
- **디자인 품질 ≠ 장식** — 기능성 높이는 시각 위계 · 상태 다양성 · 일관된 토큰
- `patterns/*.md` 는 영감 레퍼런스 · 강제 아님
- **다양성 필수** — 매 실행 다른 구조. `patterns/layout-archetypes.md` 8개 원형 중 이전과 다른 것 선택

## ⭐ Archetype 선택 (다양성 + 발전)

Plan 작성 시작 전 필수 절차.

### 핵심 프레이밍

- **이전 plan 은 피하기 위한 것이 아니라 이어받기 위한 것**
- 좋았던 요소는 keep · 약점은 fix · 새 아이디어 추가
- **8개 시드는 출발점** — willa 가 변이/조합/발명 자유

### 프로세스

1. **이전 plan 읽기** (존재 시 `.willa/plan-latest.md`)
   - 어떤 archetype 이었나?
   - 뭐가 잘 동작했나? (keep 할 요소)
   - 뭐가 약점이었나? (fix/replace)
   - 기각된 후보는?

2. **후보 생성 · 의료 뷰어 표준 5형 내에서**
   - 업계 표준 5형 (`patterns/layout-archetypes.md`):
     1. Classic Chrome (5-pane)
     2. Compact Chrome (4-pane)
     3. Viewport-First (뷰포트 80%+)
     4. Comparison Split (Pre/Post)
     5. Multi-Planar Grid (고밀도)
   - 변이는 **축 A-G** 에서 (도구그룹핑 · Inspector · 네비 · 단축키 · 밀도 · 모션 · 아이콘)
   - 이전과 같은 5형이어도 OK · 변이 축이 의미있게 다르면 됨
   - 가능한 만큼 생성 (1개 명확하면 1개도 허용 · 숫자 강제 없음)

   **⚠ 절대 금지**:
   - 카드 덱 드래그 (환자 데이터 혼동)
   - Focus/Zen mode (판독 환경 아님)
   - Radial/제스처 전용 (숨은 기능)
   - 대화창 주요 인터랙션 (의료 도구 아님)
   - Floating 남발 (예측성 파괴)
   - 뷰포트 50% 미만 (비의료 대시보드화)

3. **선택은 사용자 · AskUserQuestion 필수 (후보 2개 이상일 때)**
   - willa 가 임의 선택 금지 · 생성된 후보들을 옵션으로 제시
   - 각 옵션: 이름 + 한줄 설명 (시드/변이/발명 · 주요 특징)
   - "willa 재량" 옵션도 포함 (사용자 판단 보류 시)
   - 후보 1개만 있으면 질문 스킵 · 그것으로 진행
   - 사용자 선택 결과가 최종 archetype

4. **Plan 문서 상단 기록** (모두 필수):

   ```
   > **Archetype**: {이름} — {직접 시드 · 변이 · 발명}
   > **이전 plan 에서 가져온 것**: {keep 요소 2-3개}
   > **이전 plan 에서 바꾼 것**: {교체/제거 이유 1-2개}
   > **이번에 추가한 것**: {새 요소 + 이유 1-2개}
   > **기각한 후보**: {최소 2개 · 왜 이번엔 아닌지}
   ```

### 품질 게이트

다음 중 하나라도 해당하면 Scaffold **차단**:
- 이전 archetype 과 사실상 동일 (이름만 다르거나 색만 다름)
- 기각 후보 없음 (후보 1개만 만들고 바로 채택)
- "이전 plan 에서 가져온 것/바꾼 것" 기록 누락

### 왜 이 게이트가 필요한가

willa 는 매 실행 **다른 관점·배치** 탐색 · 사용자에게 대안 제공이 목적.
시드 8개에 갇힐 필요 없음 · **변이와 발명이 주류**, 시드는 참고.

## ⭐ 디자인 품질 기준 (필수 준수)

이전 willa 출력의 약점을 방지하기 위한 체크리스트. Plan 단계에서 **모두 명시**해야 Scaffold 진입.

**참조 (선택적 심화)**: `references/_INDEX.md` 의 Plan 섹션 참고
- archetype 배치 근거가 애매하면 → `references/laws-of-ux.md` 의 Fitts/Hick/Miller/Jakob
- 토큰 팔레트/타이포 선택 근거 → `references/typography-color.md` + `references/token-specification.md`
- 모션 duration/easing 선택 근거 → `references/motion-timing.md`
- 의료 SW 톤(차분/신뢰) 표현 → `references/emotional-design.md`
- 의사 의사결정 편향(anchoring · confirmation) 대응 → `references/cognitive-biases.md`

규칙: 영어 원문 그대로 붙이지 말고 **Will3D 컨텍스트에 맞게 해석**. plan 문서에 "근거: Fitts's Law — 자주 쓰는 도구는 화면 가장자리(ToolRail) 에 고정" 같은 1-2줄 인용만. 출력 언어는 사용자가 쓴 언어 따름(한국어·영어·혼용 OK).

### A. 토큰 구체화 (추상 금지)

"다크 톤 / 뷰포트 중심" 같은 추상적 문구 **금지**. 구체 값 명시:

```
팔레트
  --bg         #0A0908
  --bg-panel   #181613
  --bg-elev    #211F1B
  --text       #E5E1D6
  --text-dim   #A29C8D
  --accent     #1F6D92
  --accent-hi  #2B92C4
  --warn       #C88B20
  --success    #4A9378

타이포
  Display  Inter Semi Bold / -1.5% letter-spacing / size {24,20,16}
  Body     Inter Regular   /  0%               / size {13,12,11}
  Mono     JetBrains Mono  /  0%               / size {11,10}

Spacing (4px grid)  4 · 8 · 12 · 16 · 20 · 24 · 32 · 48
Radius              4 · 6 · 8 · 12 · 999
Shadow              sm: 0 1px 2px rgba(0,0,0,.18)
                    md: 0 4px 12px rgba(0,0,0,.35)
                    lg: 0 12px 48px rgba(0,0,0,.55)
```

### B. 시각 위계 (Hierarchy)

각 화면에서 3단계 시각 위계 명시:

```
Primary    뷰포트 / 주요 조작 타겟       (가장 밝음 · 큰 면적 · 대비 강)
Secondary  도구 / 인스펙터 / 네비          (중간 밝기 · 중간 크기)
Tertiary   메타정보 / 상태바 / 보조텍스트   (어둡거나 작음)
```

### C. 상태 다양성 (States)

모든 인터랙티브 요소 **4개 상태** 설계:

```
default   기본
hover     호버 (배경 약간 밝거나 border 강조)
active    선택됨 (accent 색 · 그림자 · 굵기 변화)
disabled  비활성 (opacity 0.4 · 색 탈채)
```

### D. 공간 리듬 (Spacing rhythm)

- 같은 그룹 요소는 같은 간격 (`gap: 8`) · 그룹 간은 한 단계 큰 간격 (`gap: 16~24`)
- Section 사이에는 divider 또는 큰 여백 (`space-y-6`)
- 임의 px 값 금지 · 위 grid 토큰만 사용

### E. 폰트 위계 실사용 예

```
TopBar 환자명        : 13px Semi Bold · color text
TopBar MRN 코드      : 10px Regular · color text-dim · mono
Inspector 섹션 헤더   : 10px Semi Bold · tracking 8% · color text-mute · UPPERCASE
Inspector KV 라벨     : 12px Regular · color text-dim
Inspector KV 값       : 12px Medium  · color text
StatusBar 전체       : 10.5px Regular · mono · color text-mute
```

### F. 모션 (Motion)

기본 duration / easing 명시:

```
fast    120ms · ease-out     호버 · 포커스
normal  180ms · ease-in-out  상태 전환 · 탭 전환
slow    280ms · spring(0.35) 패널 open/close
```

### G. 하이브리드 테마 원칙 (뷰포트 dark · chrome bright 선호)

Will3D 는 CT 영상 판독 도구 · **뷰포트 영역(CT 슬라이스 · 3D VR · MPR)** 은 DICOM 표준상
**dark 고정** (밝은 배경은 CT 판독 불가 · 대비 손상). 이 제약은 도메인 상수.

그 외 **chrome(TopBar · ToolRail · Inspector · BottomDock · Command Palette · 모달 등)** 은
**bright 선호**. 모던 의료 SW(Invivo · coDiagnostiX 등) 트렌드.

팔레트 구조 예 (출발점 · 그대로 쓰지 말 것):
```
--viewport-bg    #0A0908   (뷰포트 · 다크 고정)
--viewport-fg    #E5E1D6   (CT 상 텍스트/오버레이)

--bg             #F7F6F3   (chrome 기본 · bright)
--bg-panel       #FFFFFF
--bg-elev        #FCFBF7
--text           #1A1916
--text-dim       #5C574F
--border         #E0DDD5
--accent         #1F6D92
```

풀 dark 테마도 옵션으로 허용 (사용자가 dark 선호 시). 풀 light 로 뷰포트까지 밝게 만드는 건 **reject**.

## 문서 구조

```markdown
# Will3D UI 기획 · {YYYY-MM-DD HH:mm}

## Context
- 타깃: {Q1}
- 선택된 이슈: {Q2}
- 변경 범위: {Q3}
- 디자인 톤: {Q4}

## 1. 디자인 토큰 (필수 섹션)
위 체크리스트 A-G 각각 구체 값으로 명시

## 2. 해결할 이슈 (Clarify에서 고른 것만)

### Issue 1 · {이슈명}
**증상**: {C++ 코드 근거 포함}
**영향**: 어떤 의사가 어떤 작업에서 불편한지

**제안 해결책** — 각 변경마다:
- 시각 디테일: 크기/여백/색상 (토큰 참조)
- 상태: default/hover/active/disabled 어떻게 보이는지
- 모션: 전환 시 어떻게 움직이는지

**어떻게 측정**: 클릭 수 / 시간 / 학습 난이도 등

### Issue 2 · ...
(위와 같은 형식)

## 3. 전체 구조 변경 (있는 경우만)

이슈 해결책을 종합했을 때 **구조 변경이 불가피**하면:
- 탭 → 그룹 매핑 표
- 패널 위치 변경
- 신규 요소 도입

변경 없이 이슈 해결 가능하면 이 섹션 생략.

## 4. 디자인 원칙 적용 (Will3D 특화)

- 뷰포트 ≥ 70% 화면 주인공
- Inspector 는 모드-컨텍스트 (선택 객체 기반)
- ToolRail 은 사용 빈도 (Pinned/Recent) + 카테고리 이중 구조
- 어떤 상태에서든 현재 위치(Phase/Domain) 즉시 보이게

## 5. 트레이드오프
이슈 해결 과정에서 포기한 대안 + 이유.

## 6. 다음 단계 (Scaffold)
- 수정할 파일 목록 (prototype/willa-preview/src/**)
- 작업량 추정
- Scaffold 에서 참조할 토큰 / 컴포넌트 패턴
```

## frontend-design 플러그인 상담

**미적 의사결정 의무 상담**: plan 작성 중 다음 경우에 `frontend-design` 서브에이전트 호출:

1. **토큰 선택** — 팔레트/타이포 조합 검증
2. **신규 컴포넌트 패턴** — 기존 patterns/ 에 없는 UI 요소 설계
3. **디자인 톤 변경 (Q4 "신선한 방향")** — 전체 방향성 제안 검증

상담 없이 임의 미적 결정 금지. Agent 결과는 `.willa/plan-*.md` 의 "4. 디자인 원칙 적용"
섹션 말미에 인용 (참고한 제안 · 채택 여부 · 이유).

호출 방식 예:

```
Task({
  subagent_type: "general-purpose",  // 또는 frontend-design agent
  description: "Aesthetic direction review",
  prompt: "Will3D (CBCT viewer, dark theme, 뷰포트 70%) 의 EXAMINE 화면 토큰 제안 검토.
           현재 제안: bg #0A0908 + accent #1F6D92 + warn #C88B20 ...
           의료 영상 판독에 적합한가? 대비 · 시각 피로도 · 상태 식별성 관점에서 평가 후
           개선 제안 3가지. Under 300 words."
})
```

## Aesthetic 가이드

- Q4 "기존 유지" → 기존 Will3D 느낌 유지하며 이슈 해결 · **위 토큰 구체화는 여전히 필수**
- Q4 "신선한 방향" → willa 재량으로 다른 톤 선택하되 **CBCT 도구 본질은 유지**
  - dark 필수 + 뷰포트 중심 + 인터랙션 반응성
  - 저널 리더 · 대시보드 · 소셜 피드 같은 형태 ❌
  - 팔레트/타이포/그리드 변화는 OK
  - **"신선"이라고 라이트 테마/pastel/장식적 요소 도입 금지**

## 저장

```
.willa/plan-{timestamp}.md
.willa/plan-latest.md    (덮어쓰기)
```

## 검증

plan 작성 후 요약 + **품질 체크리스트 준수 확인** + "Scaffold 진행?" 단답 질문.

체크리스트 미준수 항목이 있으면:
- 해당 항목 구체 값으로 채우거나
- "이번 제안은 이 항목 불필요" 이유 기재

## 품질 게이트

다음 중 하나라도 해당하면 Scaffold 진입 **차단** · 다시 Plan 보완:
- 토큰 구체 값 없이 추상어만 있음 ("modern", "clean", "깔끔")
- 상태 다양성 누락 (hover/active 언급 없음)
- 스페이싱 임의값 사용 (4의 배수 아닌 값 포함)
- 뷰포트까지 밝게 처리 (CT 판독 불가)
- **Archetype 기록 불완전** (가져온/바꾼/추가한/기각 4개 항목 중 누락)
- **변이 없음** — 시드 직접 사용하면서 이전과 사실상 동일
- **사용자 선택 없음** — 후보 2개 이상인데 AskUserQuestion 없이 willa 가 임의 채택

## 보충 · 다양성 + 연속성 왜 필요한가

willa 는 매 실행 **다른 관점·배치** 탐색 · 사용자에게 대안 제공이 목적.
매번 "TopBar + Rail + Inspector + Bottom Dock" 이면 5173 리빌드일 뿐.

동시에 이전 plan 의 좋은 발견은 **이어받아야** 함. 피하기만 하면 품질이 제자리걸음.

- **피하기만** ❌ "이전 Chrome 이니 Chrome 금지" → 8개 중 순환
- **발전시키기** ⭕ "이전 Chrome 에서 TopBar 는 좋았음 keep · Inspector 고정 약점 → Contextual 변이"

다양성 = **변이 + 조합 + 발명**. 숫자 순회 아님 · 사용자 대안은 무한.
