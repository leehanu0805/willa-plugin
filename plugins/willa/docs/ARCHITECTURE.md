# Willa Plugin Architecture

## 왜 플러그인인가 (MCP가 아닌)

초기 논의에서 MCP 서버 / 템플릿 / 플러그인 중 고민했다. 결론은 **Claude Code 플러그인**:

| 형태 | 장점 | 단점 | 판정 |
|------|------|------|------|
| MCP 서버 | 언어 중립 · 다른 클라이언트 재사용 | 서버 프로세스 관리 · stdio/HTTP 브릿지 오버헤드 | ❌ 오버엔지니어 |
| 독립 CLI | 전역 설치 쉬움 | Claude Code 통합 어색 · AskUserQuestion 못 씀 | ❌ |
| Claude Code 플러그인 | 슬래시 커맨드 + 스킬 + AskUserQuestion 자연 통합 | Claude Code 전용 | ✅ 적합 |

이 플러그인의 **핵심 가치는 대화형 기획**이다 — AskUserQuestion · 읽기 도구 · 파일 쓰기가 하나의 세션에서 자연스럽게 이어져야 함. 이는 Claude Code 플러그인 형태에서만 가능하다.

## 컴포넌트 관계

```
사용자 타이핑 `/willa`
    ↓
commands/willa.md  ← 슬래시 커맨드 정의 · 트리거 · scope
    ↓
skills/willa-workflow/SKILL.md  ← 워크플로우 진입점
    ↓
phases/1-discovery.md → phases/2-clarify.md → ... → phases/5-serve.md
    ↓                    ↓                           ↓
읽기 (Will3D 코드)    AskUserQuestion             Bash / Write / Edit
    ↓                    ↓                           ↓
        patterns/*.md  ← 재사용 UI 패턴 레퍼런스
    ↓
산출: C:\code\Will3D\.willa\plan-*.md
      C:\code\Will3D\prototype\will3d-ui\ 코드 변경
      localhost:5173 갱신
```

## 스킬 동작 모델

Claude Code 플러그인의 스킬은 **마크다운 문서**이다. Claude가 스킬 본문을 읽고 그 지시를 따른다.

### willa-workflow 진입

`/willa` → `commands/willa.md` 의 `scope: willa-workflow` 필드를 참조 →
`skills/willa-workflow/SKILL.md` 를 로드 → 그 안의 "진입 조건" 섹션 따라 Phase 1부터 순차 진행.

각 Phase 문서(`phases/*.md`) 는 **Claude에게 주는 체크리스트** 형태.
한 Phase의 "분기" 섹션이 다음 Phase나 종료를 결정.

### newilla-workflow 진입

`/newilla "X"` → `commands/newilla.md` → `skills/newilla-workflow/SKILL.md` →
4 Step 순차 (Context → Classify → Recommend → Patch).

## 데이터 흐름

### /willa 실행 시 파일 I/O

```
입력 (Read):
  C:\code\Will3D\.willa\plan-latest.md (있으면)
  C:\code\Will3D\.ouroboros\context\current.md
  C:\code\Will3D\prototype\will3d-ui\src\lib\constants.ts
  C:\code\Will3D\prototype\will3d-ui\src\lib\medical-data.ts
  C:\code\Will3D\prototype\will3d-ui\src\components\*.tsx

사용자 입력 (AskUserQuestion):
  targetUser / level / density / modes

출력 (Write / Edit):
  C:\code\Will3D\.willa\plan-{timestamp}.md
  C:\code\Will3D\.willa\plan-latest.md
  C:\code\Will3D\prototype\will3d-ui\src\lib\constants.ts (수정)
  C:\code\Will3D\prototype\will3d-ui\src\components\*.tsx (수정)

부수 효과 (Bash):
  npx tsc --noEmit (검증)
  curl http://localhost:5173/ (확인)
```

### /newilla 실행 시 파일 I/O

```
입력:
  .willa\plan-latest.md
  prototype\will3d-ui\src\lib\constants.ts

사용자 입력:
  freq / user / type / phase

출력:
  .willa\placement-{slug}.md
  (옵션) prototype\will3d-ui\src\lib\constants.ts (수정)
  (옵션) prototype\will3d-ui\src\components\guide-overlay.tsx (annotation 추가)
```

## 확장 계획

- **/willa review** — 기존 plan의 패턴 준수 여부 자동 검증
- **/willa export-figma** — 패턴 문서를 Figma API 로 밀어넣기
- **/willa a11y** — 접근성(키보드 · 스크린리더 · 색 대비) 감사
- **/willa perf** — 컴포넌트 렌더 비용 측정 제안
- Skills 공유 메커니즘 — 다른 치과 SW 프로젝트로 확장 시 중복 부분 재사용

## 한계

- Windows 전제 (경로 구분자 `\`). WSL/Mac은 별도 검증 필요
- Will3D 저장소 구조가 변하면 phase 문서 수정 필요
- prototype 템플릿을 직접 복사하지 않고 **현재 `will3d-ui/` 를 그대로 수정**하는 방식 — 단일 환경 가정
- MCP 서버가 아니므로 Claude Code 외 다른 AI 도구에서 사용 불가
