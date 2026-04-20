# Phase 1 · Discovery

willa의 목표: **지금 있는 Will3D 기능·플로우를 사용자가 더 쉽게 쓰도록 UI/UX를 수정**.

그 위해 **딱 하나의 입력**만 읽는다:

## 읽는 것

**Will3D C++ 소스** — 지금 소프트웨어가 가지고 있는 탭·도구·플로우 전부

- `C:\code\Will3D\CTCommon\W3Enum.h` (TabType enum)
- `C:\code\Will3D\TabMgr\W3Tabmgr.cpp`
- `C:\code\Will3D\UIFrame\*.cpp`
- `C:\code\Will3D\UIComponent\view_gl_*.h`
- `C:\code\Will3D\UITools\*_task_tool.h`

## 읽지 않는 것

- `prototype/will3d-ui/` · `prototype/willa-docs/` · `prototype/willa-preview/` — 전부 무관
- `.willa/plan-*.md` — 이전 결정도 무관, 매번 fresh

## 파악할 것

- 활성 탭 / 주석된 탭 목록
- 탭별 도구 (ToolVBar)
- 우측 OTF 패널
- 뷰 컴포넌트 (MPR / Panorama / Implant 등)
- 측정/주석/세그 도구 위치
- 전체 플로우 (파일 로드 → 진단 → 계획 → 보고)

## 출력 (사용자에게 짧게)

```
Will3D 현재 상태 파악 완료:
- 활성 탭 5 · 주석된 탭 9
- 도구 ~40개
- 우측 OTF · 좌측 ToolVBar · 중앙 뷰포트 구조

다음: 어느 방향으로 개선할지 질문 드릴게요.
```

## 금지

- 사용자에게 질문 금지 (Clarify 단계에서 일괄)
- C++ 본체 수정 시도 금지
- 5분 이상 탐색 금지
