---
name: figwilla
description: willa-preview(5175) 를 Figma 로 옮기는 수동 가이드 출력. html.to.design 플러그인 경로 안내 · 자동화 없음 · 5175 상태 확인만.
triggers:
  - /figwilla
  - /willa:figwilla
scope: willa-workflow
---

# /figwilla — Figma 이전 수동 가이드

## 사용자가 입력하는 형태

```
/figwilla           # html.to.design 경로 가이드 출력
```

## 목적

`willa` 로 만든 localhost:5175 결과물을 **사용자가 직접** Figma 로 옮길 때 필요한
html.to.design 플러그인 절차를 안내한다. **자동화 안 함** — 사용자가 Figma 앱 열고
플러그인 실행하는 단계는 직접 한다.

willa 플러그인에서 Figma 이전은 이 경로가 유일. 자동 캡처(Figma MCP) 경로는 의도적으로 제거됨 —
사용자가 직접 플러그인 실행하는 편이 레이어 품질·편집 자유도 측면에서 우수.

## 동작

1. `curl http://localhost:5175/` 200 확인. 아니면 기동 안내.
2. 4개 Phase URL 목록 출력
3. Figma Desktop 앱 · 플러그인 설치 · localhost 접근 대안(Desktop Companion · localtunnel) 설명
4. 사용자는 직접 Figma 열고 플러그인 실행

## 출력 템플릿

```
🎨 /figwilla — html.to.design 경로 안내

📍 5175 상태: {200 or 기동 필요}

1. Figma 준비
   • Figma Desktop 앱 권장 (Web 은 localhost 접근 제한)
   • 새 빈 파일 생성

2. 플러그인 설치
   • https://www.figma.com/community/plugin/1159123024924461424
   • "Install" 클릭

3. 임포트 (4개 URL 순서대로)
   • http://localhost:5175/                    ← EXAMINE
   • http://localhost:5175/?phase=load         ← LOAD
   • http://localhost:5175/?phase=plan         ← PLAN
   • http://localhost:5175/?phase=deliver      ← DELIVER

   빈 캔버스 우클릭 → Plugins → html.to.design → URL 입력 → Import

4. localhost 접근 안 될 때
   • 플러그인 안에 "Desktop App" 다운로드 안내 있으면 설치
   • 또는 터미널에서:
       npx localtunnel --port 5175
     생성된 URL 을 플러그인에 입력

5. 결과 확인 포인트
   • Frame/Text/Vector 레이어 분리 잘 됐나
   • 다크 테마 · 폰트 · 아이콘 살아있나
   • ResizablePanel · Tailwind v4 CSS 변수 깨지지 않나

📌 팁
   • CT 슬라이스 뷰포트는 chrome 만 남기고 비워둔 상태 — 복잡 SVG 는 Figma 에서 노이즈
   • 4개 Phase 를 나란히 배치하면 비교 쉬움
   • 프레임 이름을 LOAD / EXAMINE / PLAN / DELIVER 로 바꿔두길 권장
```

## 이 명령이 하지 않는 것

- **Figma 파일 자동 생성** — 사용자가 Figma 앱에서 직접
- **플러그인 자동 실행** — 수동 클릭
- **5175 의 내용 변경** — 상태만 확인
- **쿼터/결제 관여** — html.to.design 무료 플랜은 플러그인측이 관리

## 제한

- 사용자가 Figma 계정 · Desktop 앱 · 플러그인 설치 모두 스스로 해야 함
- 4개 URL 임포트는 각각 수동 클릭 — 이 명령이 자동화하지 않음
