# Phase 5 · Serve

Scaffold 완료 후 willa-preview(5175) 확인 + 최종 보고.

## 체크

### 5175 · willa-preview ⭐ (willa의 출력)

```
curl -s -o /dev/null -w "%{http_code}" http://localhost:5175/
```

- 200 → HMR 로 이미 반영됨. OK.
- 실패 → 기동:
  ```
  cd C:/code/Will3D/prototype/willa-preview
  npm run dev   # background
  ```

### 5176 · will3d-current (현재 Will3D UI 참고용)

willa 제안(5175) 과 비교할 기준. 현재 Qt 애플리케이션 UI 모방. willa 워크플로우는 **수정 안 함** — 상태 확인만:

```
curl -s -o /dev/null -w "%{http_code}" http://localhost:5176/
```

미기동이면 안내:
```
cd C:/code/Will3D/prototype/will3d-current && npm run dev
```

### 5173, 5174 는 건드리지 않음

- 5173 (`will3d-ui`) 와 5174 (`willa-docs`) 는 willa 워크플로우와 무관.
- Phase 5 에서도 상태 확인만 하거나 아예 언급 안 함.

## 최종 TypeScript 체크

```
cd C:/code/Will3D/prototype/willa-preview && npx tsc --noEmit
```

## 보고 메시지

```
✅ Willa 완료 — {토픽 또는 "전체 재검토"}

🆕 제안: http://localhost:5175/
   • MOCKUP · WIREFRAME · GUIDE 토글

📎 참고: http://localhost:5176/
   • 현재 Will3D 소프트웨어 UI (Qt 모방)
   • 5175 와 비교용 · willa 는 수정 안 함

📝 주요 변경
   • {Plan의 변경 사항 3-5줄 bullet}

📂 파일
   • 기획: .willa/plan-latest.md
   • 수정된 파일: prototype/willa-preview/src/**

🔜 다음
   • /selfheal         — 자가 개선 무제한 (스마트 stop) ⭐ 권장
   • /selfheal 5       — 5회 제한
   • /willa:newilla "기능명" — 새 기능 배치
   • /willa:willa      — 다시 기획
   • /figwilla         — Figma 로 옮기기

💡 자가 힐 먼저 돌려서 품질 safe-guard 걸기 → Figma 이전 권장
```

## 자동 제안 (항상 출력)

Serve 완료 직후 **두 줄**로 고정 안내:

> "자가 힐 돌릴래? `/selfheal` (무제한 · 스마트 stop) · `/selfheal 5` (5회 제한)"
> "Figma 로 옮기려면 `/figwilla` — html.to.design 4단계 가이드."

순서: **자가 힐 먼저 → Figma** (willa 출력 품질 올린 뒤 이관이 낫다).

## current.md 업데이트

`.ouroboros/context/current.md` 의 완료 작업에 추가:

```
| Willa UI 기획 · {topic} | Feature | {today} |
```

## 에러 복구

- 5175 기동 실패 → `cd willa-preview && npm install` 시도
- port 충돌 → `vite.config.ts` strictPort true 이므로 실패 명확
- TypeScript 에러 → Scaffold 단계 복귀
