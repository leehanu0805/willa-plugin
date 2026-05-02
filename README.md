# willa

**Will3D 기획 · Claude Code.**

> v0.6.0 · Will3D 특화

---

## WILLA

Will3D 는 복잡한 의료 뷰어다. 14개 탭 · 수십 개 도구 · 여러 직군(일반 치과의 · 임플란트 전문의 · 판독의) 이 쓴다. 메뉴 재배치 한 번 하려고 해도:

- C++ 본체는 건드리기 무서움 (Qt · GLSL · TabMgr)
- "이거 어디 넣지?" 결정이 매번 주먹구구
- 의사 입장에서 "어디 있더라" 헤매는 기능이 반복됨
- Figma 로 옮기고 싶어도 매번 수동 정리

willa 는 **기획 → 프로토타입 React 코드 → 자가 개선 → Figma 이전** 까지 한 플러그인으로 자동화한다. 본체는 안 건드리고 `prototype/willa-preview/` 에 결과물을 만든다.

---

## 커맨드 (7개 + 1 서브모드)

| 명령 | 역할 | 언제 |
|------|------|------|
| **`/willa`** | 5175 sandbox 에 전면 UI 재기획 · 7단계 | "레이아웃 실험 · 덮어써도 OK" |
| **`/willa polish`** | Phase 6 단독 실행 · frontend-design 디자인 감사 | "기존 결과 품질만 다듬기" |
| **`/newilla`** | 새 기능 1개의 배치 결정  | "AI 챗 넣어줘" · "알림 기능 추가" |
| **`/willakeep`** ⭐ | 5175 → 5173 이관 + 73 자동 스냅샷 | "75 결과 마음에 듦 · 킵" |
| **`/willa76`** | 5176 (현재 Will3D 모방) | "Will 3D 디자인, 기능을 웹에 시각화" |
| **`/figwilla`** | 5175 을 Figma 로 이전 · html.to.design 가이드 | "디자이너에게 전달" |
| **`/willagit`** ⭐ | `prototype/*` 폴더(들)를 GitHub 에 자동 push (gh CLI 또는 PAT) · 단일=독립 repo / 다중=monorepo · README · .gitignore 자동 | "다른 곳에서도 띄우고 싶음 · 백업" |
| **`/selfheal`** | 자가 진단·개선 무제한 반복 · 스마트 stop | "추가 품질 루프" |

**원복**: `/willakeep` 후 사용자가 "원복해줘" · "원복 {레이블}" · "원복 {시간}" 말하면 즉시 복원.

---

## `/willa` 7단계 워크플로우

```
1. Discovery      UI/UX 현재 상태 관찰 (C++ · 프로토타입 모두)
   ↓
2. Clarify        4개 고정 질문 (타깃 · 이슈 · 범위 · archetype)
   ↓
3. Plan           YAML frontmatter + frontend-design 실호출 · .willa/plan-*.md 저장
   ↓
4. Scaffold       prototype/willa-preview/ 코드 수정
   ↓
4.5 Investigate   Plan vs 구현 diff · 사용자 확인 게이트
   ↓
4.75 Self-Heal    자동 · 무제한 · Plan 정렬 + 조작성 개선 · 스마트 stop
   ↓
5. Serve          완성본 보고 + localhost:5173 확인
   ↓
6. Polish         (선택) frontend-design 디자인 감사
```

**품질 게이트**: 각 단계마다 차단 조건 · 미달 시 재시도. 예: Plan 에 frontend-design 실호출 없으면 Scaffold 차단.

---

## 설치

### Step 1 — Node.js 설치 (없으면)

```bash
# macOS
brew install node

# Windows — https://nodejs.org 에서 LTS 다운로드 후 설치
# 설치 확인
node -v   # v18 이상
npm -v
```

### Step 2 — Claude Code 설치

```bash
npm install -g @anthropic-ai/claude-code

# 설치 확인
claude --version
```

> Claude Code 공식 문서: https://docs.anthropic.com/claude-code

### Step 3 — Will3D 저장소 준비

```bash
# Will3D 가 C:\code\Will3D\ (Windows) 또는 ~/Will3D (Mac) 에 있어야 함
git clone https://github.com/hdxwill/Will3D.git C:/code/Will3D   # Windows
# git clone https://github.com/hdxwill/Will3D.git ~/Will3D         # Mac
```

### Step 4 — willa + frontend-design 플러그인 설치

Claude Code 실행 후 아래 명령어를 순서대로 입력:

```
/plugin marketplace add leehanu0805/willa-plugin
/plugin install willa@willa-plugin

/plugin marketplace add claude-plugins-official/frontend-design
/plugin install frontend-design@claude-plugins-official

/reload-plugins
```

설치 완료 후 `/willa` 입력하면 바로 시작.

### 업데이트

```
/plugin marketplace update willa-plugin
/plugin update willa@willa-plugin
/plugin marketplace update frontend-design
/plugin update frontend-design@claude-plugins-official
/reload-plugins
```

---

## 로컬호스트 포트 관례 (4개 역할)

| 포트 | 폴더 | 역할 | 쓰기 권한 |
|------|------|------|----------|
| **5173** | `prototype/willa-preview/` | **KEEP · 사용자 고정 페이지** · 만족스러운 willa 결과만 이관 | `/willakeep` 으로만 |
| **5175** | `prototype/willa-sandbox/` | **willa 실험장** · /willa 기본 target · 자유 재구성 | `/willa` 자유 |
| **5174** | `prototype/willa-docs/` | **willa 산출물 자동 발행** · plan · investigate · audit 등 | Serve 끝 자동 |
| **5176** | `prototype/will3d-current/` | **Will3D 현 UI 모방** · 비교용 | `/willa76` 기동만 · 수정 ❌ |

### 워크플로우

```
/willa76       → 5176 (Will3D Qt 모방) 미기동 시 기동 · 참고용
```
/willa         → 5175 sandbox 에 자유 실험 (덮어써도 OK)
    ↓ 결과 마음에 들면
/willakeep     → 5175 → 5173 이관 + 이전 73 자동 스냅샷
    ↓ Serve 자동
5174 willa-docs 에 plan/audit 등 자동 발행


### `/willakeep` 안전장치

73 은 사용자의 **KEEP 페이지** (고정 작품) · 덮어쓰기 전 **자동 스냅샷**.

- 스냅샷: `.willa/keep-history/{YYYYMMDD-HHmm}/` (src + config · node_modules 제외)
- 최근 **20개** 자동 유지
- **원복 방법**: 사용자가 "원복해줘" · "원복 {레이블}" · "원복 {시간}" 말하면 즉시 복원
- 인덱스: `.willa/keep-history/_index.md` (timestamp · 레이블 · archetype)

### Plan 문서에서 target 선언
```yaml
---
target: willa-sandbox   # 기본 · willa 자유 실험
# target: willa-preview # 사용자 명시 확인 필요 (KEEP 덮어쓰기)
---
```

target 이 `willa-preview` 인데 `/willakeep` 을 거치지 않은 raw /willa 이면 → Phase 4 Scaffold 가 경고 후 차단.

### 포트 구성
각 폴더의 `vite.config.ts` + `package.json` dev script 의 port 따름. `npm run dev --port` 가 vite config 를 override 하므로 **둘 다 sync** 해야 함.

### dev 서버 기동
```
cd prototype/willa-sandbox && npm run dev       # 5175
cd prototype/willa-preview && npm run dev       # 5173
cd prototype/willa-docs && npm run dev          # 5174
/willa76                                        # 5176 자동
```

---

## 사용 예

```
# 전면 재기획
/willa

# 새 기능 배치
/willa:newilla "Airway 분석 도구"

# 결과를 Figma 로 이전
/figwilla

# 추가 자가 개선
/selfheal          # 무제한 (스마트 stop)
/selfheal 5        # 5회 제한

# 결과를 GitHub 에 push (다중선택 → monorepo)
/willagit
```

> `/willagit` 은 **gh CLI** 또는 **PAT** 둘 중 하나 필요. 둘 다 없으면 발급 가이드 출력 후 종료.
> PAT 저장 위치: `~/.willagit/token` (한 줄, scope=`repo`).

---

## 산출물

`C:\code\Will3D\.willa\` 아래:

```
plan-{timestamp}.md         /willa 사이클 결과 (YAML frontmatter + 본문)
plan-latest.md              최신 바로가기
placement-{slug}.md         /newilla 결과
deferred-{slug}.md          /newilla "보류" 결정
audit-{slug}.md             /willa polish 감사
investigate-{timestamp}.md  Phase 4.5 조사 리포트
selfheal-defer.md           Self-Heal 에서 미룬 이슈
```

Plan 은 **YAML frontmatter 필수** (v0.5.0+): archetype · target · files_to_modify · metrics · design_consult 등.

---

## 구조

```
willa-plugin/plugins/willa/
├── .claude-plugin/plugin.json
├── commands/
│   ├── willa.md              /willa
│   ├── newilla.md            /willa:newilla
│   ├── willakeep.md          /willakeep (75 → 73 이관 + 자동 스냅샷)
│   ├── willa76.md            /willa76 (5176 미기동 시 기동)
│   ├── figwilla.md           /figwilla
│   ├── willagit.md           /willagit (prototype → GitHub · gh/PAT)
│   └── selfheal.md           /selfheal
├── skills/
│   ├── willa-workflow/
│   │   ├── SKILL.md
│   │   ├── phases/           1-discovery ~ 6-polish (4.5·4.75 포함)
│   │   ├── patterns/         10개 (layout-archetypes ⭐ · docs-explorer · design-system 등)
│   │   └── references/       10개 ux-ui-mastery 흡수 (laws-of-ux · nng-heuristics 등) + _INDEX.md
│   └── newilla-workflow/
│       ├── SKILL.md
│       ├── phases/           1-context ~ 5-patch
│       └── patterns/
│           └── feature-placement.md ⭐ (8개 배치 아키타입 P1-P8)
├── docs/
│   └── ARCHITECTURE.md
└── README.md (이 문서)
```

---

## 설계 원칙

1. **Will3D 전용** — 범용 아님. 치과 CBCT 뷰어 도메인 가정.
2. **C++ 본체 미터치** — `GLSL_Module/` · `UIComponent/` · `TabMgr/` 등 절대 수정 금지.
3. **프로토타입 우선** — 기획은 `prototype/willa-preview/` 코드 변경으로 즉시 검증.
4. **추측 금지** — 매 실행 AskUserQuestion 으로 의도 정규화.
5. **하이브리드 테마** — 뷰포트 dark 고정 (DICOM 표준) · chrome bright 선호.
6. **베이스라인 보호** — `target` 명시 · 보호된 사용자 작품을 willa 가 덮어쓰지 않음.

---

## References (10개 · 방법론 + 미감)

`ux-ui-mastery` 플러그인에서 흡수한 willa 전용 레퍼런스. Plan / Investigate / Polish 에서 참조.

| 분류 | 파일 |
|------|------|
| 방법론 | laws-of-ux · nng-heuristics · critique-methodology · cognitive-biases |
| 미감 | typography-color · token-specification · motion-timing · emotional-design |
| 상태/문구 | states-empty-error · microcopy |

규칙: 영어 원문 복붙 금지 · 1-2줄 인용으로만 · 출력 언어는 사용자 따라감.

---
