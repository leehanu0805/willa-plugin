# Phase 5 · Serve

Phase 4.75 Self-Heal 자동 루프 완료 후 **완성본 보고**. target 폴더 dev server 는 이미 HMR 로 최신 상태.

## Target 기반 확인

**포트 표준 매핑** (README "로컬호스트 포트 관례" 참조):

| 역할 | 기본 폴더 | 기본 포트 |
|------|---------|---------|
| willa 출력 (sandbox) | `prototype/willa-sandbox/` | 5175 |
| 보호 baseline (preview) | `prototype/willa-preview/` | 5173 |
| 참고 (Qt 모방) | `prototype/will3d-current/` | 5176 |
| 문서 사이트 | `prototype/willa-docs/` | 5174 |

### target 폴더의 dev server 확인

Plan frontmatter `target` 필드에서 폴더 읽고 해당 dev port 로:

```
curl -s -o /dev/null -w "%{http_code}" http://localhost:<PORT>/
```

- 200 → HMR 로 이미 반영됨. OK.
- 실패 → 기동:
  ```
  cd C:/code/Will3D/prototype/<target>
  npm run dev   # background
  ```

### 참고 서버 (수정 안 함 · 상태 확인만)

- **5176 will3d-current** — willa 결과와 비교용 Qt 모방. 미기동이면 안내만.
- **5174 willa-docs** — newilla "5174 반영" 선택 시만 업데이트 · 그 외 건드리지 않음.
- **baseline (5173 preview)** — target 이 sandbox 일 때 건드리지 않음 · 사용자 명시적 `target: willa-preview` 일 때만 수정.

## 74 docs 자동 발행 (필수 · willa 산출물 publish)

Phase 5 Serve 의 **마지막 스텝** · 5174 willa-docs 에 최신 산출물을 자동 복사.

### 복사 대상 → 위치

| 소스 (.willa/) | 대상 (prototype/willa-docs/public/willa-artifacts/) |
|----------------|---------------------------------------------------|
| `plan-latest.md` | `plan-latest.md` (덮어쓰기) |
| `plan-{timestamp}.md` (최신) | `plans/plan-{timestamp}.md` |
| `investigate-{timestamp}.md` (최신) | `investigates/investigate-{timestamp}.md` |
| `audit-*.md` (모두) | `audits/` |
| `placement-*.md` (모두) | `placements/` |
| `selfheal-defer.md` (있으면) | `selfheal-defer.md` |

### 실행

```bash
mkdir -p prototype/willa-docs/public/willa-artifacts/{plans,investigates,audits,placements}
cp .willa/plan-latest.md prototype/willa-docs/public/willa-artifacts/plan-latest.md 2>/dev/null || true
# ... 위 표대로 각각
```

실패해도 willa 플로우 중단 아님 · 경고만 출력.

### willa-docs 측 기대

willa-docs (5174) 가 이 디렉토리를 정적 파일로 서빙 · 또는 자체 페이지에서 markdown 로더로 렌더. 플러그인은 **파일 복사까지만** 책임.

willa-docs 없으면 이 스텝 자동 스킵 (`prototype/willa-docs/` 미존재 체크).

---

## 최종 TypeScript 체크

```
cd C:/code/Will3D/prototype/willa-preview && npx tsc --noEmit
```

## 보고 메시지

```
✅ Willa 완료 — {토픽 또는 "전체 재검토"}

🆕 제안: http://localhost:<TARGET_PORT>/   (target: {willa-sandbox | willa-preview | ...})

📎 참고: http://localhost:5176/
   • 현재 Will3D 소프트웨어 UI (Qt 모방)
   • willa 결과와 비교용 · willa 는 수정 안 함

📝 주요 변경
   • {Plan의 변경 사항 3-5줄 bullet}

📂 파일
   • 기획: .willa/plan-latest.md
   • 수정된 파일: prototype/<target>/src/**

자가 힐 결과 요약 (Phase 4.75)
   • 반복: N회 · 수정: M건 · Plan 정렬도: X% → Y%

🔜 다음
   • /willa:newilla "기능명" — 새 기능 배치
   • /willa:willa      — 다시 기획 (다른 archetype)
   • /selfheal         — 추가 자가 치유 (Plan 정렬 외 일반 품질)
   • /figwilla         — Figma 로 옮기기 (html.to.design)
```

## 자동 제안 (한 줄)

> "Figma 로 옮기려면 `/figwilla` · 추가 자가 개선이 필요하면 `/selfheal`."

## current.md 업데이트

`.ouroboros/context/current.md` 의 완료 작업에 추가:

```
| Willa UI 기획 · {topic} | Feature | {today} |
```

## 에러 복구

- target dev server 기동 실패 → `cd prototype/<target> && npm install` 시도
- port 충돌 → `vite.config.ts` + `package.json dev` 양쪽 포트 확인 (npm script `--port` 가 vite config 를 override)
- TypeScript 에러 → Scaffold 단계 복귀
