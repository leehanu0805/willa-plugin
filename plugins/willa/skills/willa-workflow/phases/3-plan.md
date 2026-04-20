# Phase 3 · Plan

Discovery + Clarify 결과로 **기획 문서**를 작성. `C:\code\Will3D\.willa\plan-{YYYYMMDD-HHmm}.md` 저장.

## 문서 구조 (표준 템플릿)

```markdown
# Will3D UI 기획 · {YYYY-MM-DD HH:mm}

## Context
- 타깃 사용자: {Q1 답변}
- 변경 수준: {Q2 답변}
- 정보 밀도: {Q3 답변}
- 출력 모드: {Q4 답변}
- 대상 토픽: {인자 또는 "전체"}

## 문제 정의
현재 UI에서 사용자가 겪는 구체적 어려움 3-5개.
각각 "왜 어려운지" 한 줄 설명.

## 목표
측정 가능한 목표 3개 이하.
예: "임플란트 배치까지 클릭 수 N → M"

## 제안 구조

### Phase 네비게이션
| Phase | 변경 사항 |
|-------|----------|
| LOAD | ... |
| EXAMINE | ... |
| PLAN | ... |
| DELIVER | ... |

### Domain 구성
Phase별 Domain 변경/추가/제거 목록.

### Tool Rail
- Pinned 기본값 조정
- Group 재편성 여부
- 추가/제거 도구

### Inspector
- 탭 구성 변경
- 섹션 우선순위 조정

### Global
- Command Palette 카테고리 변경
- Context Menu 추가 항목
- Keyboard shortcuts

## 패턴 적용
어떤 `patterns/*.md` 를 어디에 적용했는지 명시.

## 디자인 방향성 (Design Direction)

이 섹션은 **패턴 구조 위에 아름다움을 얹는 단계**. `frontend-design` 플러그인 혼용.

### Reading Room 시스템 준수 확인
- 컬러 토큰 (`patterns/design-system.md`) 그대로 사용 여부
- 타이포 스케일 일관성
- 이모지/glow/pulse 등 anti-patterns 미사용

### 이번 변경의 aesthetic 강조점
Clarify Q2 (변경 수준) + Q3 (정보 밀도)에서 도출:

| L1 튜닝   | 컬러 온도 미세조정 · 타이포 가중치만 |
| L2 배치   | 정보 계층 재정렬 · 시각 중심점 이동 |
| L3 구조   | 새로운 distinctive moment 1개 설계 (예: workflow line) |
| L4 재시작 | 완전히 새로운 aesthetic 방향 (고객/의사 요청 시) |

### frontend-design 호출 시점
**새 컴포넌트/화면 생성** 시에만 Scaffold 단계에서 호출. 기존 컴포넌트 **조정**은 패턴 문서만으로 처리.

## 트레이드오프
선택하지 않은 대안 2-3개 + 이유.

## 다음 단계
1. Scaffold 에서 적용할 파일 목록
2. 사용자 검증 포인트
3. 추후 /newilla 로 추가 예정인 기능 (있다면)
```

## 작성 원칙

1. **짧고 결정적이게** — 각 섹션 150단어 이내
2. **근거 포함** — 왜 그렇게 결정했는지 Why: 한 줄
3. **코드 파일 언급** — 어떤 파일을 어떻게 바꿀지 Scaffold에서 쓸 수 있게
4. **이전 plan 참조** — `plan-previous.md` 와의 차이점 delta 섹션 권장

## 파일 저장

```
C:\code\Will3D\.willa\
├── plan-{timestamp}.md    (새로 작성)
├── plan-latest.md         (덮어쓰기 — 항상 가장 최근)
└── plan-previous.md       (이전 latest를 여기로 rename)
```

Windows 환경이라 symlink 대신 **내용 복사** 방식 사용.

## 검증

작성 후 사용자에게 **diff 요약** 제시:

```
이번 plan 요약:
• Phase PLAN에 'Airway' 도메인 신설
• Tool Rail Measure 그룹에 Profile 추가
• Inspector 속성탭의 clearance 순서 변경 (위험도 우선)
• 기타 6개 변경

전체 plan 보기: .willa/plan-latest.md
진행할까요? (yes → Scaffold / no → 수정)
```

## 분기

- 사용자 yes → Scaffold 단계
- 사용자 no 또는 수정 요청 → AskUserQuestion으로 구체적 수정 사항 수집 후 재작성
- 변경 사항 없음 (현재 UI 유지가 최선이라 판단) → 그 이유만 plan에 기록하고 종료
