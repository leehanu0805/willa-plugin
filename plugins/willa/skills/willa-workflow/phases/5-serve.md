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

🆕 출력: http://localhost:5175/
   • MOCKUP · WIREFRAME · GUIDE 토글

📝 주요 변경
   • {Plan의 변경 사항 3-5줄 bullet}

📂 파일
   • 기획: .willa/plan-latest.md
   • 수정된 파일: prototype/willa-preview/src/**

🔜 다음
   • 새 기능 배치: /newilla "기능명"
   • 미세 조정: /willa refactor
   • 디자인 감사: /willa polish
```

## current.md 업데이트

`.ouroboros/context/current.md` 의 완료 작업에 추가:

```
| Willa UI 기획 · {topic} | Feature | {today} |
```

## 에러 복구

- 5175 기동 실패 → `cd willa-preview && npm install` 시도
- port 충돌 → `vite.config.ts` strictPort true 이므로 실패 명확
- TypeScript 에러 → Scaffold 단계 복귀
