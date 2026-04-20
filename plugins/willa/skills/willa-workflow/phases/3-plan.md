# Phase 3 · Plan

Discovery + Clarify 결과로 **기획 문서**를 작성. `C:\code\Will3D\.willa\plan-{YYYYMMDD-HHmm}.md` 저장.

## 핵심 원칙 · willa는 자유롭다

willa는 **템플릿을 따르지 않는다**. 매번 새로운 방향을 탐색할 권리가 있음.

- ❌ "Reading Room 준수" 같은 고정 시스템 강제 안 함
- ❌ "anti-patterns 금지" 체크리스트 강제 안 함
- ❌ 기존 will3d-ui 와 같은 결과물을 내야 한다는 의무 없음
- ✅ Clarify 답변(타깃/범위/밀도) 기반으로 **willa가 직접 판단**
- ✅ `patterns/*.md` 는 **영감 레퍼런스** · 참고용. 강제 아님.
- ✅ `design-system.md` 는 **이전에 탐색된 한 가지 예시** · 새 방향 자유롭게 제안 가능.

## 문서 구조 (가이드라인 · 엄격하지 않음)

```markdown
# Will3D UI 기획 · {YYYY-MM-DD HH:mm}

## Context
(Clarify 답변 · 현재 C++ 구조 요약)

## 문제 정의
현재 Will3D 에서 사용자가 느끼는 구체적 어려움 3-5개.

## 목표
측정 가능한 목표 2-3개.

## 제안 (자유 형식)

willa 가 그 순간 옳다고 판단하는 제안.

가능한 형태 (한 가지 골라서 or 조합):
- 구조 재편안 (Phase / Domain / Tool / Panel 재배치)
- 완전히 새로운 네비게이션 패러다임
- 전혀 다른 IA 컨셉 (타임라인 · 맵 · 대화형 · 모달 중심 등)
- 기존 구조 유지 + 밀도/계층만 조정
- 다중 대안 (A · B · C) 제시

## 디자인 방향성 (willa 재량)

제약 없음. 매 실행마다 달라도 OK.

예시 영감:
- 따뜻한 종이 (Reading Room)
- 심야 판독실 (Night Clinic)
- 브루탈리스트 타이포
- 글래스모피즘
- 레트로 터미널
- 파스텔 실크
- 인더스트리얼
- 또는 완전히 새로운 것

이 섹션은 **왜 이 방향인지** 설명이 핵심.

## 트레이드오프
선택 안 한 대안 + 이유 (선택).

## 다음 단계
Scaffold 예정 파일 · 대략 작업량.
```

## willa의 창의성 활용

Clarify Q2 = **L4 완전 재시작** 인 경우 특히:
- 기존 Will3D / will3d-ui / willa-preview 의 이전 결과물 **의식하지 말 것**
- 치과 CT 도메인 전문가용이라는 제약만 유지 · 나머지 자유
- "의사가 쉽게 쓴다" 목표만 충족하면 형식/색/구조 전부 재량

Clarify Q1/Q3 가 핵심 제약. 나머지는 willa 재량.

## frontend-design 활용

기획 단계에서는 호출 안 함. Scaffold 에서 새 컴포넌트 생성 시 호출.
호출 시 **"이번 plan의 방향성과 일치하게"** 만 지시. design-system.md 강제 안 함.

## 저장

```
.willa/plan-{timestamp}.md    (새 파일)
.willa/plan-latest.md         (덮어쓰기)
```

## 검증

plan 작성 후 **핵심 요약 2-3줄** + "Scaffold 진행할까요?" 단답형 질문.
