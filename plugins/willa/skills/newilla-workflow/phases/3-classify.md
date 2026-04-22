# Step 3 · Classify

(Step 2 에서 B 추가만 선택 시) 새 기능을 5축으로 분류. AskUserQuestion 5개.

## Q1. 사용 빈도
- **일상적** (진료마다) → P3 ToolRail Pinned / P4 TopBar 후보
- **가끔** (특정 케이스) → P1 Inspector 탭 / P3 ToolRail drawer 후보
- **특수** (월 1회 이하) → P5 Command Palette / P6 Context Menu 후보

## Q2. 주 사용자
- 모든 의사
- 임플란트 전문의만
- 교정 전문의만
- 영상 판독의만

## Q3. 기능 타입
- **Tool** — 클릭/드래그로 데이터 생성 (측정 · 그리기)
- **Panel** — 상시 정보 표시 (수치 · 상태 · 리스트)
- **Modal/Sheet** — 복잡 제어 (설정 · 마법사) · ⚠ 뷰포트 차단 · 최후 수단
- **Action** — 1회 실행 (캡처 · 내보내기)

## Q4. 연관 Phase
- LOAD · EXAMINE · PLAN · DELIVER · 여러 Phase 공통

## Q5. 상태 유지 범위
- **환자별** (임플란트 · 측정 · 주석) → Inspector 후보
- **세션별** (대화 · Undo · 최근) → Inspector 탭 or 사이드 후보
- **전역** (프리셋 · 테마) → TopBar / Palette / Settings 후보

**Q5 가 결정적**: 데이터 저장 위치 + 어느 범위에서 유지돼야 하는지 → 배치 자연스럽게 따라옴.
