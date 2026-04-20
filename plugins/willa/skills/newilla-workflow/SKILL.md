---
name: newilla-workflow
description: /newilla 명령 실행 시 사용하는 5단계 워크플로우. 먼저 "재설계 vs 추가" 분기를 묻고, 답에 따라 전체 리디자인(/willa 위임) 또는 기존 UI에 배치 추가로 갈림.
---

# Newilla Workflow

`/newilla "<새 기능명>"` 가 호출되면 이 스킬이 발동.

## 5단계 (분기형)

```
1. Context        → 현재 UI 구조 + 최근 plan 로드
    ↓
2. Branch Choice  → AskUserQuestion: 재설계 vs 추가 (핵심 분기)
    ├─ A. 갈아엎기 → /willa 워크플로우 위임 (새 기능을 토픽으로)
    └─ B. 추가만  → 3-5단계 진행
           ↓
        3. Classify     → 빈도/사용자/타입/Phase 질문
           ↓
        4. Recommend    → Top 3 배치 후보 제시
           ↓
        5. Patch        → 선택 후 placement-*.md 저장 + (옵션) 코드
```

---

## Step 1 · Context

다음 파일 읽기:

- `C:\code\Will3D\.willa\plan-latest.md` (있으면)
- `C:\code\Will3D\prototype\willa-preview\src\lib\constants.ts` (Phase/Domain/Tools 정의)
- `C:\code\Will3D\prototype\willa-preview\src\lib\medical-data.ts` (샘플 데이터 구조)

현재 UI의 **"이미 있는 자리들"** 목록화:
- Phase 4개 → 각각 어떤 UI 쓰는지
- 각 Phase의 Domain
- Tool Rail 그룹 4개 + 총 42 도구
- Inspector 탭 3개 + 각 탭 섹션
- Command Palette 카테고리 4개
- Context Menu 항목

---

## Step 2 · Branch Choice (핵심 분기)

새 기능을 들고 왔을 때 **가장 먼저** 물어야 할 질문:

```
AskUserQuestion(
  question: "'{새 기능명}' 을(를) 어떻게 반영할까요?",
  options: [
    {
      label: "🔁 갈아엎기 · 이 기능 중심으로 전체 재설계",
      description: "새 기능이 핵심이 되도록 Phase/Domain/Tool Rail/Inspector 전반을 재검토. /willa 워크플로우가 이어서 실행됨."
    },
    {
      label: "➕ 추가만 · 기존 디자인 유지하고 이 기능을 어디에 둘지만 결정 (Recommended)",
      description: "현재 UI 구조 그대로 두고, 새 기능이 들어갈 가장 적합한 자리만 찾음. 최소 변경."
    },
    {
      label: "🤔 일단 보류 · 기획 문서에만 기록",
      description: "아직 구현/배치 안 함. .willa/deferred-*.md 로 저장해두고 나중에 /newilla 재실행."
    },
  ]
)
```

### 분기 처리

**A. 갈아엎기 선택 시**:
- `/willa "{새 기능명}"` 워크플로우로 **위임**
- 본 newilla 워크플로우는 여기서 종료
- 새 기능이 plan 전체의 **중심 주제**가 되어 Discovery → Clarify → Plan → Scaffold → Serve 진행

**B. 추가만 선택 시**:
- Step 3 Classify 로 진행
- 기존 구조를 건드리지 않는 것이 원칙
- 불가피하게 구조 건드려야 한다면 Step 4 Recommend 에서 trade-off 명시

**C. 보류 선택 시**:
- `C:\code\Will3D\.willa\deferred-{slug}.md` 에 간단한 메모 저장
- 내용: 기능명 · 날짜 · (유저 자유 메모) · "나중에 /newilla 재실행" 안내
- 종료

---

## Step 3 · Classify

(B 선택 시만 진행)

`AskUserQuestion` 으로 4개 질문:

### Q1. 사용 빈도
- 일상적 (진료마다 씀) → Tool Rail Pinned / Phase 직접 배치 후보
- 가끔 (특정 케이스) → Tool Rail 그룹 / Inspector 섹션 후보
- 특수 (월 1회 이하) → Command Palette / Context Menu 후보

### Q2. 주 사용자
- 모든 의사
- 임플란트 전문의만
- 교정 전문의만
- 영상 판독의만

### Q3. 기능 타입
- **Tool**: 클릭 후 드래그/입력으로 데이터 생성 (측정, 그리기 등)
- **Panel**: 상시 정보 표시 (수치, 상태, 리스트)
- **Modal/Sheet**: 복잡한 제어 (설정, 마법사)
- **Action**: 1회 실행 (캡처, 내보내기)

### Q4. 연관 Phase
- LOAD
- EXAMINE
- PLAN
- DELIVER
- 여러 Phase 공통

---

## Step 4 · Recommend

답변 기반으로 **Top 3 배치 후보**를 생성. 매칭 점수 기반:

| 빈도 × 타입 | 추천 위치 |
|-------------|----------|
| 일상 × Tool | Tool Rail (primary + Pinned 자동 추가) |
| 일상 × Panel | Inspector 새 탭 또는 기존 탭 최상단 섹션 |
| 가끔 × Tool | Tool Rail (primary=false, drawer에서 접근) |
| 가끔 × Panel | Inspector 섹션 추가 |
| 가끔 × Modal | TopBar 아이콘 버튼 + Command Palette |
| 특수 × Tool | Command Palette + ToolRail 숨김 |
| 특수 × Action | Command Palette 전용 |
| Phase 특화 × 어떤 것이든 | 해당 Phase 내 전용 영역 |

### 출력 포맷

```
### 후보 A · [위치]
근거: {왜 여기}
구현: {어떤 파일에 어떻게}
트레이드오프: {잃는 것}

### 후보 B · [위치]
...

### 후보 C · [위치]
...

### 후보 D · 넣지 않음
기획에만 기록 · UI 미반영. `.willa/deferred-*.md` 로 저장.
```

"넣지 않음" 옵션을 항상 포함 — 억지 배치 방지.

---

## Step 5 · Patch

사용자가 선택한 후보로 진행. **먼저 반영 범위를 묻는다**:

```
AskUserQuestion(
  question: "선택한 배치를 어떻게 기록할까요?",
  options: [
    {
      label: "📄 문서만 · 5173 목업은 그대로 (Recommended)",
      description: ".willa/placement-*.md 에만 기록. prototype/willa-preview/ 코드 변경 없음. 실제 반영은 나중에 /willa 또는 수동으로."
    },
    {
      label: "🎨 문서 + 5173 목업에도 반영",
      description: "placement 저장 + prototype/willa-preview/ 의 constants.ts / guide-overlay 등 실제 수정. localhost:5173 즉시 HMR 반영."
    },
    {
      label: "🎨📖 문서 + 5173 + 5174 docs 모두 반영",
      description: "위 + willa-docs (localhost:5174) 의 Tools/Domains/Inspector 페이지에도 설명 추가."
    },
  ]
)
```

### 공통 · 문서 저장

선택과 무관하게 항상 저장:

`C:\code\Will3D\.willa\placement-{slug}.md`

```markdown
# Placement · {새 기능명}
- 날짜: {YYYY-MM-DD HH:mm}
- 빈도/사용자/타입/Phase 분류
- 선택된 후보: {A/B/C/D}
- 반영 범위: 문서만 / 5173 / 5173+5174
- 근거
- 구현 변경 사항 (반영한 경우)
- 관련 annotation (guide overlay)
- 테스트 포인트
```

### "문서만" 선택 시

- `placement-*.md` 저장으로 끝
- 5173 · 5174 둘 다 **건드리지 않음**
- 나중에 반영하려면 `/willa` 또는 수동 코드 편집

### "5173 반영" 선택 시

`prototype/willa-preview/` 수정:

- Tool Rail 추가 → `constants.ts` 의 `RAIL_TOOLS` 배열
- Domain 추가 → `constants.ts` 의 `DOMAINS` 객체
- Inspector 섹션 추가 → 해당 Tab 컴포넌트 수정
- Command Palette 커맨드 추가 → `COMMANDS` 배열
- Context Menu 항목 추가 → `viewport.tsx` 의 `ViewportContextMenu` 수정
- Guide Overlay annotation 추가 → `guide-overlay.tsx` 의 `ANNOTATIONS` 배열

각 수정 후:
- `npx tsc --noEmit` 통과 확인
- `curl http://localhost:5173/` 200 확인

### "5173 + 5174 docs 반영" 선택 시

위 5173 수정 + 추가로 `prototype/willa-docs/` 수정:

- 새 도구 → `src/pages/Tools.tsx` 의 `TOOL_DESC` 객체에 설명 추가
- 새 도메인 → `src/pages/Domains.tsx` 의 `DOMAIN_DESC` 객체에 설명 추가
- 새 Phase 섹션 → `src/pages/PhaseDetail.tsx` 의 `PHASE_PROSE` 객체에 추가
- 새 단축키 → `src/pages/Shortcuts.tsx` 의 `GROUPS` 배열에 추가

## 종료 메시지

```
✅ Newilla 완료 — {새 기능명}

📂 저장
   .willa/placement-{slug}.md

반영 범위: {선택한 옵션}
{반영한 경우 변경 파일 목록}

{반영 안 한 경우: "5173/5174 목업은 그대로 유지됩니다."}
{반영한 경우: "localhost:5173 새로고침으로 확인."}
```
