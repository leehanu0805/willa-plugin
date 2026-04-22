# Step 2 · Branch Choice (핵심 분기)

새 기능을 어떻게 반영할지 **가장 먼저** 결정. AskUserQuestion 필수.

## 질문

```
AskUserQuestion(
  question: "'{새 기능명}' 을(를) 어떻게 반영할까요?",
  options: [
    {
      label: "🔁 갈아엎기 · 이 기능 중심으로 전체 재설계",
      description: "Phase/Domain/Tool Rail/Inspector 전반 재검토. /willa 워크플로우가 이어서 실행."
    },
    {
      label: "➕ 추가만 · 기존 디자인 유지하고 자리만 결정 (Recommended)",
      description: "현재 UI 구조 그대로 · 새 기능 자리만 찾음 · 최소 변경."
    },
    {
      label: "🤔 보류 · 기획만 기록",
      description: "구현/배치 안 함 · .willa/deferred-*.md 저장 · 나중에 /newilla 재실행."
    },
  ]
)
```

## 분기 처리

| 선택 | 후속 |
|------|------|
| A 갈아엎기 | `/willa "{새 기능명}"` 위임 · newilla 종료 |
| B 추가만 | Step 3 Classify 진행 · 기존 구조 건드리지 않는 것이 원칙 |
| C 보류 | `.willa/deferred-{slug}.md` 저장 (기능명 · 날짜 · 메모 · 재실행 안내) · 종료 |
