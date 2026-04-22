---
name: selfheal
description: willa-preview 자가 진단 + 개선. 기본 무제한 반복 (스마트 stop) · /selfheal N 으로 N 회 제한.
triggers:
  - /selfheal
  - /selfheal <N>
  - /selfheal max
scope: willa-workflow
---

# /selfheal — willa-preview 자가 치유 (무제한 모드 기본)

## 사용법

```
/selfheal         # 무제한 · 스마트 stop 조건 만족시 자동 종료 (기본)
/selfheal 3       # 3회 제한
/selfheal max     # /selfheal 와 동일 (명시적 무제한)
/selfheal 1       # 1회만 (최소)
```

## 동작

매 반복마다:

1. **스캔** — `prototype/willa-preview/` 코드/레이아웃 읽고 UX/구현 이슈 식별
   - 시각 일관성 (색/여백/폰트)
   - 상호작용 결함 (키보드/호버/포커스)
   - 접근성 (ARIA/키 네비/대비)
   - 컴포넌트 누락 (empty/loading/error state)
   - 성능 (불필요 재렌더/큰 번들)

2. **우선순위** — Tier 1 (사용성 큰 영향) → Tier 2 (완성도) → Tier 3 (polish)

3. **수정** — Tier 1 중심 3-5개 즉시 적용. 매 수정 후 TypeScript 체크.

4. **보고** — "Iter N: 이슈 X 찾음, Y 수정, Z 미룸"

## 스마트 Stop 조건 (무제한 모드)

아래 중 **하나라도** 해당하면 자동 종료:

| 조건 | 설명 |
|------|------|
| **1. 무이슈** | 새 이슈 0개 발견 → 품질 안정 |
| **2. 수확 체감** | 3회 연속 Tier 1 이슈 0개 (Tier 2/3 만 잔존) |
| **3. 정체** | 같은 이슈가 2회 연속 반복 등장 (수정 무효) |
| **4. TypeScript 실패** | TS 에러 3회 연속 |
| **5. 하드 캡** | 기본 15회 초과 · `/selfheal 20` 등으로 상향 가능 |
| **6. 사용자 개입** | 다음 프롬프트로 사용자가 뭐든 입력하면 즉시 중단 |

## 중단 보고

```
🛑 SelfHeal 중단 — 조건: {1-6 중 어느 것}
   • 완료 반복: N
   • 총 수정: M
   • 남은 이슈: K개 (.willa/selfheal-defer.md 저장)
```

## 범위 · 금지

- `prototype/willa-preview/src/**` 만 수정
- Will3D C++ 본체 · will3d-ui · willa-docs 절대 건드리지 않음
- CBCT 도메인 본질 유지 (뷰포트 주인공 · 다크 뷰포트 · 3축 MPR)
- 사용자에게 질문 하지 않음 (자율 진행) · 단 위 6번 조건(사용자 개입)은 예외

## 출력 형식 (매 반복)

```
Iter N · 스캔 완료
  Tier 1: {이슈 수} · Tier 2: {수} · Tier 3: {수}

  수정 (Tier 1 중심):
  - {file:line} {issue → fix}
  - ...

  미룸: {이슈명 N개} → 다음 반복
```

## 최종 보고

```
✅ SelfHeal 완료 — N 회 (stop: {조건})

Iter 1: 찾음 X · 수정 Y
Iter 2: 찾음 X · 수정 Y
...

총 수정: M
남은 과제: .willa/selfheal-defer.md (있으면)
품질 개선: {before/after 자가 점수}
```

## willa 플로우 통합

`/willa` 메인 사이클 Phase 5 Serve 종료 메시지에 자동 제안:

> "자가 힐 돌릴래? `/selfheal` (무제한 · 스마트 stop) · `/selfheal 5` (5회 제한)"

사용자 선택. 기본값은 사용자 판단.
