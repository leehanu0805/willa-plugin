---
name: newilla-workflow
description: /newilla 명령 실행 시 사용하는 5단계 워크플로우. "재설계 vs 추가 vs 보류" 분기로 시작 · 추가 시 5축 분류 → 배치 Top 3 → 반영.
---

# Newilla Workflow

`/newilla "<새 기능명>"` 호출 시 이 스킬 발동. **5단계 분기형**.

## 단계

```
1. Context        → phases/1-context.md   (현재 UI 구조 + plan-latest 로드)
   ↓
2. Branch Choice  → phases/2-branch.md    (재설계 · 추가 · 보류 분기)
   ├─ A 갈아엎기 → /willa 위임 · 종료
   └─ B 추가만  → Step 3 진행
          ↓
       3. Classify   → phases/3-classify.md (빈도/사용자/타입/Phase/상태범위)
          ↓
       4. Recommend  → phases/4-recommend.md (Top 3 배치 + 트레이드오프)
          ↓
       5. Patch      → phases/5-patch.md    (반영 범위 + 저장)
```

## 진입 조건

- `/newilla "{기능명}"` 또는 `/willa:newilla "{기능명}"`
- 자연어 설명도 허용: `/newilla "Airway 분석 도구 추가"`

## 중단 조건

- Step 2 에서 "보류" 선택 → `deferred-*.md` 저장 · 종료
- 각 단계 AskUserQuestion 에 "중단/그만" → 즉시 종료

## 산출물

- `.willa/placement-{slug}.md` — 배치 결정 기록
- `.willa/deferred-{slug}.md` — 보류 시

## 참조 패턴

- **patterns/feature-placement.md** ⭐ — 8개 배치 아키타입 P1-P8 · 5축 매칭 표 · UX 법칙 · 금지 기본값 · Step 4 필수 참조

## 주의

- Will3D C++ 본체 절대 수정 금지
- `prototype/will3d-ui/` 는 건드리지 않음 (willa 영역 아님)
- 종료 메시지에 `/figwilla` 안내 필수 (Figma 이전 수동 가이드)
