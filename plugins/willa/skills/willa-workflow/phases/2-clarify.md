# Phase 2 · Clarify

Discovery 에서 찾은 **UX 이슈** 를 기반으로, **어느 이슈를 우선 해결할지 + 어떻게 접근할지** 확인.

## ⚠ 무조건 질문 (매 실행 필수)

**이 단계는 건너뛸 수 없다.** 이전 실행에서 같은 질문에 답했더라도 **다시 묻는다**:
- 매 실행마다 사용자 의도가 달라질 수 있음
- 이전 답을 추정하면 willa 의 자율성 상실
- **4개 고정 질문 모두 호출** · 순서 · 옵션 모두 표준화

**스킵 금지**. AskUserQuestion 호출 없이 Phase 3 진입 = 품질 게이트 실패.

## 4개 고정 질문 (표준 · 매 실행 동일)

### Q1. 타겟 사용자

```
header: "타겟"
multiSelect: false
옵션:
  1. 일반 치과의 (진단 중심)
     설명: 하루 여러 환자 · 각 스터디 깊이가 아님 · 흐름 직관 아이콘 중요
  2. 임플란트/구강외과 전문의
     설명: 기능 깊이 + 정밀도 · 탭 항목 · 단축키 · 플로우 최적화 우선
  3. 영상 판독의 (진단)
     설명: MPR/Panorama/3D VR 중심 · 슬라이스 스캔 · 측정 · 보고서
  4. 범용 (타깃 안 좁힘)
     설명: 3직군 공통 사용자 설계 · 보편성 우선
```

### Q2. 해결할 이슈 (multiSelect)

```
header: "이슈"
multiSelect: true
옵션: Discovery 의 Top 5 이슈를 그대로 제시 (동적 생성)

예시 (Discovery 결과에 따라 다름):
  1. 숨겨진 9개 탭 부활 (TMJ · 3D Ceph · 투명교정 · 보철 등)
  2. 탭 왕복 클릭 감소 (MPR↔PANO↔IMPLANT 왕복)
  3. OTF 값 탭 간 누수 (프리셋 잘못 적용)
  4. 공통 도구 탭마다 재구현 (측정/WW-WL/세그)
  5. AI 세그/자동화 기능 숨김

+ 고정 옵션:
  - willa 판단 (니알아서) — willa 가 Top 3 자율 선택
```

### Q3. 변경 범위

```
header: "범위"
multiSelect: false
옵션:
  1. 작은 수정 (색 · 아이콘 · 레이블만)
     설명: 구조 유지 · 디테일 보강 · 안전 · 빠름
  2. 일부 영역 (1-2 구역 교체)
     설명: 예: Inspector 만 · ToolRail 만 · 나머지 유지
  3. 전체 레이아웃 재설계
     설명: 5개 구역 전부 재배치 · archetype 교체 가능
  4. willa 재량 · 자유도 최대
     설명: willa 가 범위 스스로 판단
```

### Q4. Archetype 방향

```
header: "archetype"
multiSelect: false
옵션:
  1. willa 판단 (이전 plan 발전 + 새 변이)
     설명: 이전 plan-latest.md 읽고 좋은 것 keep · 약점 교체 · 새 요소 추가
  2. 시드 8개 중 특정 지정
     설명: Chrome (1) · Floating (2) · Timeline (3) · Landmark (4) · Split (5) · Conversation (6) · Cards (7) · Dashboard (8)
  3. 완전 새 발명 요청
     설명: 시드 무시 · willa 가 새 archetype 창조 · 가장 혁신적
  4. 이전과 유사한 변이만 (보수)
     설명: 이전 archetype 유지 · 세부만 변형 · 안정 우선
```

## 규칙

- **4개 질문 모두 필수 호출** (multiSelect 는 Q2 만)
- Q1-Q4 순서 고정 · 한 번에 최대 4개 AskUserQuestion
- **Q2 옵션은 Discovery 결과 기반 동적 생성** · Top 5 이슈 + "willa 판단"
- 답변 후 1줄 요약:
  ```
  정리: 일반 치과의 · [숨겨진 탭 + 왕복 감소 + 공통 도구 통합] · 전체 재설계 · Conversation archetype
  ```

## 결정 로그

Plan 문서 Context 섹션에 기록:

```markdown
## Context
- 타깃 (Q1): 일반 치과의
- 선택 이슈 (Q2): [숨겨진 탭 부활, 왕복 클릭 감소]
- 변경 범위 (Q3): 전체 레이아웃 재설계
- Archetype 방향 (Q4): Conversation-First (시드 6) + threads 변이
```

## 분기

- 4개 답변 완료 → Plan 단계
- "중단/그만" → `.willa/draft-*.md` 저장 · 종료
- Q2 가 "willa 판단" → willa 가 Discovery Top 3 자율 선택 · Plan 에 이유 기록
- Q4 가 "시드 지정" → Plan 에서 해당 archetype 으로 시작 · 변이 여부는 willa 판단
