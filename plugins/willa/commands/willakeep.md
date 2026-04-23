---
name: willakeep
description: willa-sandbox(5175) 결과물을 willa-preview(5173 · 사용자 KEEP 페이지)로 이관. 이전 73 상태는 자동 스냅샷 저장 · 사용자가 "원복해줘" 하면 즉시 복원 가능.
triggers:
  - /willakeep
  - /willa:keep
scope: willa-workflow
---

# /willakeep — willa 결과 킵 (75 → 73 이관)

## 사용법

```
/willakeep              # 75 의 현재 상태를 73으로 이관 + 이전 73 자동 스냅샷
/willakeep "레이블"     # 선택적 메모와 함께 스냅샷 태깅 (예: /willakeep "v1 프리셋")
```

## 목적

willa 가 5175 (sandbox) 에서 실험한 결과가 **마음에 들면** 73 (KEEP · 고정 작품) 으로 확정 이관.
73 의 기존 상태는 **자동 스냅샷** 으로 보존 · 사용자가 말만 하면 원복.

---

## 동작 (5단계)

### 1. 사전 확인
- 5175 dev server 응답 (200) 체크 · 실패 시 `cd prototype/willa-sandbox && npm run dev` 안내 후 abort
- 5173 존재 확인 · 없으면 신규 이관 모드 (스냅샷 스킵)

### 2. 73 자동 스냅샷
`.willa/keep-history/{YYYYMMDD-HHmm}/` 폴더 생성 후 복사:
- `prototype/willa-preview/src/` 전체
- `prototype/willa-preview/index.html`
- `prototype/willa-preview/package.json`
- `prototype/willa-preview/vite.config.ts`
- `prototype/willa-preview/components.json`
- `prototype/willa-preview/tsconfig.json`

제외: `node_modules/` · `.git/` · `dist/`

### 3. 75 → 73 이관
`prototype/willa-sandbox/src/` → `prototype/willa-preview/src/` 덮어쓰기.
**포트 관련 파일 제외**:
- `package.json` 의 `"dev"` script 는 73 의 것 유지 (포트 5173)
- `vite.config.ts` 의 `server.port` 는 73 의 것 유지

### 4. 스냅샷 인덱스 업데이트
`.willa/keep-history/_index.md` 에 줄 추가:

```markdown
| 2026-04-22 23:45 | 레이블 | 스냅샷 전 archetype | 복구 명령 |
|------------------|--------|-------------------|----------|
| 2026-04-22 23:45 | v1 프리셋 | Contextual Zones | 사용자가 "원복 v1" 또는 "원복 23:45" |
```

### 5. 보고

```
✅ Willa Keep 완료

📸 자동 스냅샷
   .willa/keep-history/20260422-2345/
   레이블: {사용자 입력 or "no label"}
   이전 archetype: {.willa/plan-latest.md 의 archetype}

🎯 73 새 상태
   75 의 Contextual Zones 결과물 이관됨
   포트 설정은 73 것 유지 (5173)

⏪ 원복 방법
   "원복해줘" · "원복 v1" · "원복 23:45" 중 어느 것이든 말하면 willa 가 즉시 복원
   또는 .willa/keep-history/_index.md 에서 timestamp 선택
```

---

## 원복 프로토콜 (사용자 발화 감지)

사용자가 다음 중 하나를 말하면 자동 처리:
- "원복해줘" / "keep 원복" → **가장 최근 스냅샷** 복원
- "원복 {레이블}" → 해당 레이블 스냅샷
- "원복 {HH:MM}" 또는 "원복 {timestamp}" → 정확 매칭

### 복원 동작
1. `.willa/keep-history/{선택}/` 의 파일들을 `prototype/willa-preview/` 로 복사
2. 포트 파일은 그대로 유지 (복원 대상에서 제외 · 일관성)
3. TypeScript check (`npx tsc --noEmit`) 통과 확인
4. HMR 반영 확인 (curl 5173)
5. 보고:
   ```
   ⏪ 원복 완료 — {timestamp} · {레이블}
      73 복원됨 · 5173 새로고침으로 확인
   ```

### 복원 후 변경 사항
- 직전 "원복 전 상태" 도 자동 스냅샷 (또 다른 keep-history 엔트리)
- 즉 원복 자체가 또 하나의 keep 이므로 **re-do** 가능

---

## 스냅샷 수명

- 최근 **20개** 자동 유지 · 초과 시 오래된 것부터 `.willa/keep-history/_archive/` 로 이동
- 사용자가 `/willakeep --list` 로 전체 스냅샷 목록 확인
- `/willakeep --prune {days}` 로 N 일 이상 된 것 정리 (선택)

## 범위 · 금지

- `prototype/willa-preview/` 와 `.willa/keep-history/` 만 건드림
- `prototype/willa-sandbox/` 는 **읽기만** 함 (75 는 그대로 유지)
- Will3D C++ 본체 절대 미터치
- `package.json` 전체 덮어쓰기 금지 (dev script · name 등은 73 것 보존)
