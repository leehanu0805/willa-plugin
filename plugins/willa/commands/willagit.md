---
name: willagit
description: 로컬 prototype 폴더(들)를 GitHub(leehanu0805)에 자동 push. gh CLI 없어도 PAT 모드로 동작 (REST API). 다중선택은 monorepo 묶음, 단일은 독립 repo. README · .gitignore 자동 생성, 이름 충돌 시 자동 접미사.
triggers:
  - /willagit
  - /willa:willagit
scope: willa-workflow
---

# /willagit — prototype 자동 GitHub 업로드

## 사용자가 입력하는 형태

```
/willagit             # prototype/* 자동 스캔 → 다중선택 메뉴 → README 자동 → push
```

인자 없음이 기본. 모든 결정은 명령 안에서 질문으로 받는다.

## 목적

`prototype/*` 의 vite 프로토타입을 사용자(leehanu0805)의 GitHub 계정으로 자동 업로드한다.
clone · install · dev 가이드(README) 까지 함께 만들어 다른 곳에서도 즉시 띄울 수 있게 한다.

- **단일 선택** → 선택한 폴더가 그대로 repo. 예: `leehanu0805/will3d-ui`
- **다중 선택** → 하나의 monorepo 로 묶음. 예: `leehanu0805/will3d-prototypes`

## 인증 모드 (둘 중 하나면 동작)

| 모드 | 우선순위 | 조건 | 사용 도구 |
|------|---------|------|----------|
| **gh CLI** | 1순위 | `gh` 명령 + `gh auth status` 통과 | `gh repo create`, `gh api` |
| **PAT** | 2순위 (fallback) | `~/.willagit/token` 파일 또는 `$GITHUB_TOKEN` env | `curl` + GitHub REST API |

둘 다 없으면 → Step 1 에서 **PAT 발급 가이드 출력 후 종료** (추측 push 금지).

## 동작 (8단계)

### 1. 환경 점검 (분기)

#### 1-A. gh CLI 시도
- `command -v gh` → 있으면
- `gh auth status` → 통과하면
- `gh api user --jq .login` 결과가 `leehanu0805` → **gh 모드 확정**, Step 2 로

#### 1-B. PAT 시도
- gh 없거나 인증 안 되어 있으면 PAT 찾기:
  1. `$GITHUB_TOKEN` env 변수
  2. `~/.willagit/token` 파일 (한 줄, 토큰만)
- 토큰 발견 시 검증:
  ```bash
  curl -sfH "Authorization: Bearer $TOKEN" https://api.github.com/user | jq -r .login
  ```
  - 결과가 `leehanu0805` → **PAT 모드 확정**, Step 2 로
  - 401 / 403 → "토큰 만료/무효" 안내 후 종료
  - login 다름 → 경고 후 사용자 확인

#### 1-C. 둘 다 실패 → 가이드 출력 + 종료

```
🔑 /willagit — 인증 필요

옵션 A: GitHub CLI 설치 (제일 간편)
  winget install --id GitHub.cli
  gh auth login --web

옵션 B: Personal Access Token 발급 (gh 안 깔고 가능)
  1. https://github.com/settings/tokens/new?scopes=repo&description=willagit
     → "Generate token" 클릭
  2. 토큰 저장 (둘 중 하나):
     bash:
       mkdir -p ~/.willagit && echo "ghp_xxxxx" > ~/.willagit/token && chmod 600 ~/.willagit/token
     PowerShell:
       New-Item -ItemType Directory -Force ~/.willagit | Out-Null
       Set-Content ~/.willagit/token "ghp_xxxxx" -NoNewline
     또는 env:
       export GITHUB_TOKEN=ghp_xxxxx
  3. /willagit 다시 실행

⚠ 토큰 권한(scope)은 'repo' (전체) 만 필요. 다른 scope 주지 말 것.
```

### 2. prototype 자동 스캔
- `prototype/*/vite.config.*` glob
- 각 폴더에서 `server.port` 추출 (없으면 5173)
- 결과: `[{name, path, port, hasGit, hasRemote}]` 표 출력

### 3. 다중선택 메뉴 (AskUserQuestion)
- multiSelect: true · 한 개 질문
- 옵션 라벨: `"{port} {name}"` (예: `"5175 will3d-ui"`)
- 설명: 폴더 경로 + git 상태 (`hasGit ? "git 초기화됨" : "신규"`)
- 0개 선택 시 종료

### 4. 묶음 결정
| 선택 수 | 동작 |
|---------|------|
| 1개 | repo 이름 = 폴더 이름 그대로 (사용자 변경 가능) |
| 2개+ | monorepo 이름 묻기. 기본값 `will3d-prototypes`. staging = `prototype/.willagit-staging/{repo}/` |

### 5. 이름 충돌 검사 + 자동 변경

#### gh 모드
```bash
gh repo view leehanu0805/{name}    # 0이면 존재, 1이면 비어있음
```

#### PAT 모드
```bash
curl -s -o /dev/null -w "%{http_code}" \
  -H "Authorization: Bearer $TOKEN" \
  https://api.github.com/repos/leehanu0805/{name}
# 200 → 존재 / 404 → 사용 가능
```

존재 시 `{name}-2`, `{name}-3` ... 비어있는 첫 이름. 결정 후 사용자 보고.

### 6. README · .gitignore 자동 생성

#### 단일 repo

````markdown
# {name}

Will3D 프로토타입 — Vite + React + TypeScript

## 설치 & 실행

```bash
git clone https://github.com/leehanu0805/{name}.git
cd {name}
npm install
npm run dev    # → http://localhost:{port}
```

## 빌드

```bash
npm run build
```
````

#### Monorepo 루트

````markdown
# {repo}

Will3D 프로토타입 모음. 각 프로젝트는 독립적으로 실행 가능.

## 포함된 프로토타입

| 폴더 | 포트 | 설명 |
|------|------|------|
| prototype/will3d-ui    | 5175 | Will3D 메인 UI 기획 모형 |
| prototype/willa-preview| 5173 | willa 가 만든 결과 프리뷰 |

## 실행

```bash
git clone https://github.com/leehanu0805/{repo}.git
cd {repo}/prototype/{name}
npm install
npm run dev
```
````

#### .gitignore (없을 때만 생성)

```
node_modules/
dist/
.vite/
*.log
.DS_Store
.env.local
```

기존 README/.gitignore 가 있으면 **덮어쓰기 전 사용자 확인**.

### 7. git 초기화 + repo 생성 + push

#### 7-A. gh 모드

```bash
cd <target>
[ ! -d .git ] && git init
git checkout -B main
git add .
git commit -m "Initial commit via /willagit"
gh repo create leehanu0805/{name} --private --source=. --remote=origin --push
```

#### 7-B. PAT 모드

```bash
cd <target>
[ ! -d .git ] && git init
git checkout -B main
git add .
git commit -m "Initial commit via /willagit"

# repo 생성 (REST API)
curl -sfX POST https://api.github.com/user/repos \
  -H "Authorization: Bearer $TOKEN" \
  -H "Accept: application/vnd.github+json" \
  -d "{\"name\":\"{name}\",\"private\":true,\"auto_init\":false}"
# 응답 422 (already exists) → Step 5 로 돌아가 이름 변경 재시도

# remote 등록 (토큰 미포함 — config 안전)
git remote remove origin 2>/dev/null
git remote add origin https://github.com/leehanu0805/{name}.git

# push (토큰을 일회성 헤더로만 — config 에 안 남음)
git -c http.extraheader="Authorization: Bearer $TOKEN" push -u origin main
```

#### 다중 선택 (monorepo) — 양쪽 모드 공통

```bash
mkdir -p prototype/.willagit-staging/{repo}/prototype
# 각 선택 폴더 복사 (node_modules · dist · .git 제외)
rsync -a --exclude='node_modules' --exclude='dist' --exclude='.git' \
  prototype/{name1}/ prototype/.willagit-staging/{repo}/prototype/{name1}/
# (Windows 면 robocopy /E /XD node_modules dist .git)

cd prototype/.willagit-staging/{repo}
# 루트 README · .gitignore 생성
git init && git checkout -B main
git add . && git commit -m "Initial commit via /willagit (monorepo)"
# 이후 7-A 또는 7-B 의 repo 생성 + push 단계 동일
```

### 8. 완료 보고

```
🚀 /willagit — GitHub 업로드 완료

🔑 인증 모드: gh / PAT
📦 repo  : https://github.com/leehanu0805/{name}  (private)
🔗 clone : git clone https://github.com/leehanu0805/{name}.git

📥 단일 repo 실행
   cd {name}
   npm install
   npm run dev      # http://localhost:{port}

📥 monorepo 실행
   cd {repo}
   cd prototype/{name1} && npm install && npm run dev   # :{port1}
   cd prototype/{name2} && npm install && npm run dev   # :{port2}

✅ README · .gitignore 자동 생성됨
ℹ️  staging 폴더: prototype/.willagit-staging/{repo}/  (보존 · 수동 정리 가능)
```

## 재실행 동작

이미 origin remote 가 있는 폴더를 다시 선택하면 AskUserQuestion 으로:

```
이 폴더는 이미 leehanu0805/{기존repo} 와 연결되어 있음.
  [1] 기존 repo로 push 만 (권장)
  [2] 새 repo 로 별도 생성
  [3] 스킵
```

PAT 모드의 push 도 동일 형식 (`http.extraheader`).

## PAT 보안 메모

- 토큰을 `git remote set-url` 의 URL 에 박아 넣지 **않음** — `.git/config` 평문 노출 방지.
- `http.extraheader` 는 push 명령 한 번에만 적용 (`-c` 인라인).
- `~/.willagit/token` 파일은 `chmod 600` 권장 (Windows 는 NTFS 권한이 다르므로 위치만).
- 토큰을 절대 commit 메시지/README/로그에 echo 하지 않음.
- 만료/유출 의심 시: `https://github.com/settings/tokens` 에서 즉시 revoke.

## 이 명령이 하지 않는 것

- **public 으로 변경** — private 고정 (수동: `gh repo edit --visibility public`)
- **GitHub Pages 배포** — push 만
- **CI/CD workflow 추가** — `.github/workflows/` 생성 안 함
- **다른 계정 push** — leehanu0805 외 미지원
- **기존 repo 강제 덮어쓰기** — 충돌 시 자동 접미사
- **node_modules · dist · .git push** — .gitignore 로 제외
- **Will3D C++ 본체 push** — `prototype/*` 만 대상
- **OAuth Device Flow** — willa-plugin 이 OAuth App 등록 안 되어 있으므로 PAT 만 지원

## 제한 / 주의

- Will3D 본체(`GLSL_Module/` · `UIComponent/` · `TabMgr/` 등)는 **절대 push 대상 아님**.
- monorepo staging 폴더(`prototype/.willagit-staging/`)는 부모 git 에서 무시되도록 `.gitignore` 처리. 수동 삭제 안전.
- README 가 이미 있는 폴더는 덮어쓰기 전 사용자 확인 (skip · overwrite · merge).
- gh / PAT 둘 다 없으면 Step 1 에서 즉시 종료 — 추측 push 절대 금지.
- 첫 commit 메시지 외에 squash/rebase/force-push 는 하지 않음.
- PAT 권장 scope: `repo` (전체). 그 이상 권한은 거부 (안전성).

## willa 플로우 통합

`/willa` Phase 5 Serve 종료 시 자동 제안 (선택):

> "GitHub 에 올릴래? `/willagit` (다중선택 · README 자동 · private · gh 또는 PAT)"

`/figwilla` 와 무관 — 이 명령은 Figma 가 아니라 GitHub 전용.
