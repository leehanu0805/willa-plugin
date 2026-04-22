# Phase 2 · Clarify

Discovery 결과 → 사용자 의도 정규화. **4개 고정 질문 매 실행 필수** (스킵 금지 · 이전 답 추정 금지). AskUserQuestion 호출 없이 Phase 3 진입 = 품질 게이트 실패.

---

## Q1. 타깃 사용자

```
header: "타깃"
multiSelect: false
1. 일반 치과의 (진단 중심) — 환자 多 · 깊이보다 속도 · 아이콘 직관
2. 임플란트/구강외과 전문의 — 정밀도 · 탭 깊이 · 단축키 · 플로우
3. 영상 판독의 — MPR/Pano/3D VR · 측정 · 보고서
4. 범용 — 3직군 공통 · 보편성 우선
```

## Q2. 해결할 이슈 (multiSelect)

```
header: "이슈"
multiSelect: true
옵션: Discovery 관찰 Top 5 이슈 동적 생성 + 고정 옵션 "willa 판단"

빈 답변 처리: Q2 에서 아무것도 선택 안 함 = "willa 판단" 자동 해석
→ Discovery 관찰 기반 willa 가 Top 3 자율 선택 · Plan 에 이유 기록
```

## Q3. 변경 범위

```
header: "범위"
multiSelect: false
1. 작은 수정 — 색/아이콘/레이블 · 구조 유지
2. 일부 영역 — 1-2 구역 교체 (Inspector 만 · ToolRail 만)
3. 전체 레이아웃 재설계 — 구역 전부 재배치 · archetype 교체
4. willa 재량 — 범위 자율 판단
```

## Q4. Archetype 방향

```
header: "archetype"
multiSelect: false
1. willa 판단 — 이전 plan 발전 + 새 변이 (기본 권장)
2. 5형 중 지정 — Classic / Compact / Viewport-First / Comparison Split / Multi-Planar Grid
3. 완전 새 발명 — 5형 무시 · 창조
4. 이전과 유사한 변이 (보수) — 이전 archetype 유지 · 세부만 변형
```

**5형 정의**: `patterns/layout-archetypes.md` 참조 (의료 뷰어 업계 표준 5형 + 7 변이 축).

---

## 실행 규칙

- **4 질문 모두 필수 호출** · Q1-Q4 순서 고정 · 한 번에 4개 AskUserQuestion
- Q2 옵션은 **Discovery 결과 기반 동적 생성** (Top 5 이슈 + "willa 판단")
- **답변 후 1줄 요약 출력**:
  ```
  정리: 일반 치과의 · [이슈A, 이슈B] · 전체 재설계 · Viewport-First 변이
  ```

## Plan 에 기록

Plan 문서 `## Context` 섹션:
```markdown
- Q1 타깃: ...
- Q2 이슈: [...]
- Q3 범위: ...
- Q4 archetype: ...
```

## 분기

| 답변 | 후속 |
|------|------|
| 4개 완료 | Phase 3 Plan |
| "중단/그만" | `.willa/draft-*.md` 저장 · 종료 |
| Q2 빈 답 or "willa 판단" | Discovery Top 3 자율 선택 · Plan 에 이유 기록 |
| Q4 "5형 중 지정" | Plan 에서 해당 archetype 으로 시작 · 변이는 willa 판단 |
