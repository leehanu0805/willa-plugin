---
name: selfheal
description: willa-preview 자가 진단 + 개선. /selfheal N 으로 N 회 반복. 매 반복마다 현 UI를 스캔하고 이슈 3-5개 찾아 즉시 수정.
triggers:
  - /selfheal
  - /selfheal <N>
scope: willa-workflow
---

# /selfheal — willa-preview 자가 치유

## 사용법

```
/selfheal         # 1회 (기본)
/selfheal 3       # 3회 반복
/selfheal 5       # 5회 반복
```

## 동작

매 반복마다:

1. **스캔** — `prototype/willa-preview/` 코드/레이아웃 읽고 현재 UX/구현 이슈 식별
   - 시각 일관성 (색/여백/폰트)
   - 상호작용 결함 (키보드/호버/포커스)
   - 접근성 (ARIA/키 네비/대비)
   - 컴포넌트 누락 (empty state/loading/error)
   - 성능 (불필요 재렌더/큰 번들)

2. **우선순위** — Tier 1 (사용성 큰 영향) → Tier 2 (완성도) → Tier 3 (polish)

3. **수정** — Tier 1 중심 3-5개 즉시 적용. 매 수정 후 TypeScript 체크.

4. **보고** — "Iter N: 이슈 5개 찾음, 4개 수정, 1개 다음 반복으로 미룸"

## 중단 조건

- N회 완료 → 종료
- 더 이상 유의미한 이슈 없음 → 조기 종료
- TypeScript 에러 3회 연속 실패 → 중단

## 범위 · 금지

- `prototype/willa-preview/src/**` 만 수정
- Will3D C++ 본체 · will3d-ui · willa-docs 절대 건드리지 않음
- CBCT 도메인 본질 유지 (뷰포트 주인공 · 다크 뷰포트 · 3축 MPR)
- 사용자에게 질문 하지 않음 (자율 진행)

## 출력

각 반복 보고 + 최종 종합:

```
✅ SelfHeal 완료 — N 회

Iter 1: 찾음 5 · 수정 4
  - {issue 1 → fix}
  - ...
Iter 2: 찾음 3 · 수정 3
  - ...
Iter 3: 찾음 2 · 수정 2

총 수정: M 개
남은 과제: .willa/selfheal-defer.md 저장 (있으면)
```
