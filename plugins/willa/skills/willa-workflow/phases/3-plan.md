# Phase 3 · Plan

Discovery 이슈 + Clarify 답변 → **구체 해결책 문서**. `.willa/plan-{YYYYMMDD-HHmm}.md` 저장 + `plan-latest.md` 갱신.

## 원칙

- Will3D 는 CBCT 뷰어/플래너 — 도메인 이탈 금지
- 모든 변경은 Discovery 의 구체 이슈에서 출발
- 매 실행마다 **이전 plan 과 의미있게 다른 archetype** (다양성 필수)
- 이전 plan 의 좋은 요소는 **keep** · 약점만 **fix** · 새 요소 **add**

---

## 1. Archetype 선택

### 절차
1. **이전 plan 읽기** (`.willa/plan-latest.md` 있으면)
   - 어떤 archetype? · 좋았던 요소? · 약점? · 기각 후보?
2. **후보 생성** — `patterns/layout-archetypes.md` 의 5형 내에서 변이 + 조합 + 발명
3. **후보 2개 이상이면 AskUserQuestion 필수** · 1개면 그것으로 진행
4. **Plan 상단 기록** (4항목 전부 필수):
   ```
   > Archetype: {이름} — {시드/변이/발명}
   > Keep (이전 plan): {2-3개}
   > Fix (교체/제거): {이유 1-2개}
   > Add (신규): {이유 1-2개}
   > 기각 후보: {최소 2개 + 이유}
   ```

### 금지 (도메인 이탈)
카드 덱 · Focus/Zen · Radial · 대화창 주 인터랙션 · Floating 남발 · 뷰포트 50% 미만.

---

## 2. 디자인 토큰 (구체값 필수)

**추상어 금지** ("modern/clean/깔끔" ❌). 모든 섹션에 **hex · px · ms** 구체값 명시.

### A. 팔레트 · B. 타이포 · C. 스페이싱/라디우스/섀도우
→ 상세 형식은 `references/token-specification.md` 참조. 실제 선택 근거는 `references/typography-color.md` 의 warm/neutral/cool 스케일 섹션.

### D. 시각 위계 (3단계 필수)
Primary (뷰포트/주조작) · Secondary (도구/인스펙터) · Tertiary (상태바/메타).

### E. 상태 4-state 필수 (모든 interactive 요소)
default · hover · active · disabled.

### F. 모션
```
instant  80ms ease-out            hover/focus
quick    140ms ease-out           panel/tab
slow     220ms cubic-bezier(.3,1,.4,1)   open/close
```
상세: `references/motion-timing.md`.

### G. 하이브리드 테마
- **뷰포트 dark 고정** (DICOM 표준 · 변경 불가)
- **chrome bright 선호** · 풀 dark 옵션 허용 · 풀 light (뷰포트까지 밝게) reject

팔레트 예시 (출발점 · 그대로 쓰지 말 것):
```
--viewport-bg   #0A0908   dark 고정
--bg            #F7F6F3   chrome bright
--bg-panel      #FFFFFF
--text          #1A1916
--accent        #1F6D92
```

---

## 3. frontend-design 상담 (미적 의사결정 의무)

다음 경우 `frontend-design` 서브에이전트 호출:
- 토큰 조합 검증 · 신규 컴포넌트 설계 · 톤 변경 검증

호출 프롬프트에 **이번 plan 방향성**만 전달 (고정 시스템 강제 금지). 결과는 "디자인 원칙 적용" 섹션에 인용 (참고/채택/이유).

---

## 4. 추가 참조 (선택적 심화)

`references/_INDEX.md` 의 Plan 섹션:
- 배치 근거 애매 → `laws-of-ux.md` (Fitts/Hick/Miller/Jakob)
- 의사결정 편향 대응 → `cognitive-biases.md`
- 의료 SW 톤(차분/신뢰) → `emotional-design.md`

**사용 규칙**: 영어 원문 복붙 금지 · 1-2줄 인용으로만 · 언어는 사용자 따라감.

---

## 5. Plan 문서 템플릿

```markdown
# Will3D UI 기획 · {YYYY-MM-DD HH:mm} · {archetype 이름}

> Archetype / Keep / Fix / Add / 기각 후보 (위 4항목)

## Context
- Q1 타깃 / Q2 이슈 / Q3 범위 / Q4 archetype

## 1. 디자인 토큰
(A-G 구체값 또는 references 인용)

## 2. 해결할 이슈 (Clarify 에서 선택된 것만)
### Issue N · {이름}
- 증상 (C++ 근거)
- 영향
- 제안 해결책 (시각 디테일 · 상태 · 모션)
- 측정 방법

## 3. 전체 구조 변경 (필요 시)
탭/패널 매핑 · 신규 요소. 불필요하면 생략.

## 4. 디자인 원칙 적용
- 뷰포트 ≥ 70% · Inspector 컨텍스트 · ToolRail 빈도+카테고리 · 현재 위치 상시
- frontend-design 상담 결과 인용

## 5. 트레이드오프
포기한 대안 + 이유.

## 6. 다음 단계
수정 파일 · 작업량 · 참조 토큰/패턴.
```

---

## 6. 품질 게이트 (Scaffold 진입 조건)

아래 하나라도 해당하면 **차단 · Plan 보완 후 재시도**:
- 토큰 추상어만 (구체값 없음)
- 4-state 중 hover/active 누락
- 스페이싱 4px grid 이탈
- 뷰포트까지 밝게 처리
- Archetype 기록 4항목 (Keep/Fix/Add/기각) 중 누락
- 이전 plan 과 사실상 동일 (이름만 변경)
- 후보 2개 이상인데 AskUserQuestion 스킵

통과 시 사용자에게 **"Scaffold 진행?"** 단답 확인 후 Phase 4.

---

## 7. 저장

- `.willa/plan-{YYYYMMDD-HHmm}.md` (버전)
- `.willa/plan-latest.md` (최신 바로가기 · 덮어쓰기)
