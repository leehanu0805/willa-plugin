# Step 4 · Recommend

Classify 답변 기반 **Top 3 배치 후보** 생성. AskUserQuestion 필수.

## 필수 참조
`patterns/feature-placement.md` — 8개 아키타입 P1-P8 · 확장 매칭 표 · 공통 기능 권장 배치 · UX 법칙 · 금지 기본값.

## 간이 매칭 표 (상세는 patterns/feature-placement.md)

| 빈도 × 타입 × 상태범위 | 추천 |
|----------------------|------|
| 일상 × Tool × 환자별 | P3 ToolRail Pinned |
| 일상 × Panel × 환자별 | P1 Inspector 탭 최상단 |
| 일상 × Action × 전역 | P2 BottomDock · P4 TopBar |
| 가끔 × Panel × 세션별 | P1 Inspector 탭 · P7 슬라이드 |
| 가끔 × Modal × 전역 | P4 TopBar + 모달 · P5 Palette |
| 특수 × Action × 전역 | P5 Palette 전용 |
| Phase 특화 | 해당 Phase SubNav 내 도메인 |

## 규칙

- **Top 3 필수** · 1개만 제시 금지 · 후보 간 트레이드오프 비교
- **P1-P8 내에서만** · 임의 위치 창안 금지
- **기본값 금지** — 모달/Floating/새 탭 검토 없이 자동 선택 금지
- **"넣지 않음" 옵션 항상 포함** — 억지 배치 방지

## 출력 포맷

```
### 후보 A · [위치]
근거: {왜 여기}
구현: {어떤 파일에 어떻게}
트레이드오프: {잃는 것}

### 후보 B · [위치]
...

### 후보 C · [위치]
...

### 후보 D · 넣지 않음
기획에만 기록 · `.willa/deferred-*.md` 저장
```
