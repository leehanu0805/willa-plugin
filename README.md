# willa-plugin

Will3D 전용 UI/UX 기획 Claude Code 플러그인.

## 목표

Will3D (치과 CBCT 3D 뷰어)의 메뉴·도구 배치를 사용자가 쉽게 찾고 조작할 수 있게 기획한다. **기획(IA/UX) + 디자인 품질(Reading Room 시스템)**을 동시에 챙긴다. 프로토타입을 `prototype/will3d-ui/` 에 즉시 반영하고 localhost에서 MOCKUP / WIREFRAME / GUIDE 3모드로 검증.

## 의존 플러그인

`frontend-design@claude-plugins-official` — 디자인 품질 보장 (신규 컴포넌트 생성 · 디자인 감사). plugin.json `dependencies` 에 선언.

## 명령

| 명령 | 용도 |
|------|------|
| `/willa` | 전체 UI 재검토 — 질문 → 기획 → prototype 반영 (6단계) |
| `/willa "<토픽>"` | 특정 영역만 재검토 |
| `/willa refactor` | 기획과 실제 코드 간극 정리 |
| `/willa refresh` | 최근 plan 기반으로 prototype 재생성 (질문 생략) |
| `/willa polish` | 디자인 품질 감사만 실행 (Phase 6 단독) |
| `/newilla "<새 기능명>"` | 새 기능의 UI 배치 결정 (Top 3 후보 추천) |

## 설치

### 1. 로컬 설치 (현재)

```bash
# 플러그인 위치
C:\code\willa-plugin\

# Claude Code에 등록 (이미 이 저장소에 있는 경우 자동 인식)
# 또는 심볼릭 링크 / 수동 add
```

### 2. GitHub 설치 (미래)

```bash
# 저장소 clone
git clone https://github.com/leehanu0805/willa-plugin.git ~/.claude/plugins/willa

# Claude Code 재시작
/reload-plugins
```

## 사전 요구사항

- **Will3D 저장소** 가 `C:\code\Will3D\` 에 클론되어 있어야 함
- **prototype/will3d-ui/** Vite+React+Tailwind+shadcn 프로토타입이 존재
- **Node.js 18+** · **npm 9+**
- Claude Code 설치 + TooltipProvider 지원 shadcn 환경

## 구조

```
willa-plugin/
├── .claude-plugin/
│   └── plugin.json                   플러그인 manifest
├── commands/
│   ├── willa.md                      /willa 슬래시 커맨드 정의
│   └── newilla.md                    /newilla 커맨드 정의
├── skills/
│   ├── willa-workflow/
│   │   ├── SKILL.md                  5단계 워크플로우 메타
│   │   ├── phases/
│   │   │   ├── 1-discovery.md        현재 상태 파악
│   │   │   ├── 2-clarify.md          AskUserQuestion 으로 의도 정규화
│   │   │   ├── 3-plan.md             기획 문서 + 디자인 방향성
│   │   │   ├── 4-scaffold.md         prototype 코드 (frontend-design 호출)
│   │   │   ├── 5-serve.md            localhost 확인 + 보고
│   │   │   └── 6-polish.md           ⭐ (선택) 디자인 감사
│   │   └── patterns/
│   │       ├── phase-navigation.md   4-Phase 네비
│   │       ├── tool-rail.md          좌측 도구 rail
│   │       ├── inspector-tabs.md     3-탭 Inspector
│   │       ├── command-palette.md    Spotlight 스타일
│   │       ├── anatomy-navigator.md  해부 구조 시각 토글
│   │       ├── guide-overlay.md      번호 pin 가이드
│   │       ├── context-menu.md       우클릭 메뉴
│   │       └── design-system.md      ⭐ Reading Room 토큰/타이포/모션
│   └── newilla-workflow/
│       └── SKILL.md                  4단계 새 기능 배치 결정
├── docs/                             (향후 사용 사례 / 디자인 결정 로그)
└── README.md                         이 문서
```

## 산출물 위치

Will3D 저장소 루트의 `.willa/` 폴더 아래:

```
C:\code\Will3D\.willa\
├── plan-{timestamp}.md               매 /willa 실행마다 저장
├── plan-latest.md                    항상 최신 기획 사본
├── plan-previous.md                  직전 plan
├── placement-{slug}.md               /newilla 결과
└── deferred-{slug}.md                배치 보류된 기능 목록
```

## 설계 원칙

1. **Will3D 전용** — 범용 플러그인이 아님. 도메인(치과 CBCT) 가정.
2. **Prototype 우선** — 기획은 `prototype/will3d-ui/` 코드 변경으로 즉시 검증.
3. **C++ 본체 미터치** — `GLSL_Module/`, `UIComponent/`, `TabMgr/` 등 절대 수정 금지.
4. **추측 금지** — 애매하면 AskUserQuestion.
5. **되돌릴 수 있게** — 기획 문서 버전 관리, 코드도 git diff 로 확인.

## 예시 사용

```
# 전체 재검토
/willa

# Implant 플로우만
/willa "Implant 배치 플로우 개선"

# 새 기능 배치
/newilla "Airway 분석 모듈"

# 최근 기획 기준으로 prototype 재생성
/willa refresh
```

## 라이선스

MIT

## Contributing

아직 초기 버전. Will3D 프로젝트 참여자 외 외부 기여는 당분간 받지 않음.
