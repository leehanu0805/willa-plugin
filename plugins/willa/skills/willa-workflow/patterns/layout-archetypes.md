# Layout Archetypes · CBCT 뷰어 표준 변이

willa 가 참고할 **의료 영상 뷰어 표준 레이아웃** 풀. 산업 표준이 이유 있어서 정착된 형태 · 크게 벗어나면 도구 의미 죽임.

## 핵심 원칙 (바꿀 수 없음)

- **뷰포트 = 주인공** · 최소 60%, 권장 75%+
- **Dark theme** · DICOM 표준
- **드래그/스크롤 중심 인터랙션** · 클릭 < 드래그
- **도구 접근 예측 가능** · 같은 도구는 항상 같은 위치
- **데이터 무결성** · 실수 제스처로 환자 데이터 섞이지 않음

**다양성은 구조 패러다임 변경이 아니라 "변이 축" 에서 발휘**:
도구 그룹핑 · 패널 우선순위 · 단축키 체계 · 시각 밀도 · 기능 위치 · 아이콘 스타일 · 모션 · 접근성

## 업계 표준 5형 (의료 영상 뷰어 공인 패턴)

### 1. Classic Chrome · 5-pane

```
┌─ TopBar · 환자정보 · 메뉴 ──────────────────┐
├─ SubNav · 탭/도메인 ───────────────────────┤
│ Rail │        Viewport         │ Inspector │
├─ Bottom Dock · 공통도구 · 상태 ─────────────┤
```

- RadiAnt · Horos · 3Shape Implant Studio 공통
- 학습 비용 최저 · 모든 도구 상시 표시
- **단점**: 뷰포트 점유율 60-70% · 시각 소음

### 2. Compact Chrome · 4-pane

```
┌─ TopBar · 환자 · 탭 ────────────────────────┐
│ Rail │        Viewport         │ Inspector │
├─ Status Strip · 얇은 바 ─────────────────────┤
```

- Osirix · OnDemand3D · Dolphin Imaging
- Classic 에서 SubNav 제거 or Bottom Dock 제거
- 뷰포트 70-78% · 단축키 의존도 ↑

### 3. Viewport-First

```
┌─ 48w Rail │      Viewport (fill)      │ Inspector (collapsible) ─┐
├─ 24h Status ────────────────────────────────────────────────────┤
```

- Carestream · Planmeca Romexis · OsiriX MD MAX
- 판독 전용 모드 · Inspector 기본 접힘
- 뷰포트 80-85% · Pro user 타깃

### 4. Comparison Split

```
┌─ TopBar ────────────────────────────────────┐
│  Viewport A    │    Viewport B              │
│  (현재)        │    (참조 · Pre/Post · 다른 환자) │
├─ 공통 도구 · 동시 조작 sync ─────────────────┤
```

- 교정 · follow-up · 쿼드 판독
- 항상 2 viewport · sync cursor 기본
- **단점**: 단일 진단에 과함

### 5. Multi-Planar Grid

```
┌────────┬────────┬────────┐
│ Axial  │ Sagittal│ Coronal│
├────────┼────────┼────────┤
│ 3D VR  │ Panoram│ Xsec   │
├────────┴────────┴────────┤
│ Status + 공통 도구 strip │
```

- GE AW Server · Siemens Syngo · 영상의학과 관제실
- 고밀도 정보 · 6-9 뷰 동시
- 일반 치과엔 과하나 판독의 · 응급실 선호

## 변이 축 (Variation Axes)

**실제 다양성은 여기서 발휘**. 5형 중 하나를 고른 후 아래 축에서 선택.

### A. 도구 그룹핑
- 기능별 (Navigate/View/Measure/Annotate)
- Phase별 (Load/Exam/Plan/Deliver)
- 객체별 (대상 선택 시만 나타남)
- 빈도별 (Pinned/Recent/All)

### B. Inspector 컨텐츠
- 고정 섹션 4-5개 (WW/WL · Segmentation · Clipping · Measurements)
- 컨텍스트 교체 (선택 객체별 자동 갱신)
- 3 tabs (속성/Segmentation/기록)
- 접었다 폈다 (멀티레벨)

### C. 탭 / 네비게이션
- 좌측 세로 탭 (현재 Will3D · Qt 스타일)
- 상단 가로 탭
- Phase stepper (단계 개념)
- Domain chip (서브 카테고리)
- Breadcrumb

### D. 단축키 체계
- 숫자 1-4 Phase 전환
- WASD 뷰 회전
- M/A/T 측정 도구
- Ctrl+K palette
- Shift+scroll slice stepping

### E. 시각 밀도
- Dense (Pro · 의대병원 · 판독실)
- Balanced (일반 치과 · 클리닉)
- Generous (교육용 · 환자 상담)

### F. 모션/반응성
- Instant (100ms 이내 · 판독 우선)
- Polished (180-240ms · 일반)
- Showcase (280-400ms · 환자 앞 시연)

### G. 아이콘 스타일
- 의료 전용 아이콘 (해부 · 장비)
- 일반 UI (lucide-react)
- 커스텀 + 일반 혼합

## 선택 프로세스

1. **5형 중 하나 선택** (패러다임)
2. **변이 축 A-G 에서 이번 plan 의 특징 결정**
3. 이전 plan 과 **같은 5형이어도 OK** · 변이 축이 의미있게 다르면 됨

예:
- 이전 plan: Classic Chrome + 기능별 그룹 + Balanced 밀도
- 이번 plan: Classic Chrome + **객체별 그룹** + **Dense 밀도** · 같은 5형이지만 의사 경험 크게 다름

## 금지 사항

- 뷰포트 점유율 50% 미만 (비의료 대시보드화)
- 제스처 전용 (숨은 기능 발견 불가)
- 카드/덱 메타포 (드래그로 데이터 혼동)
- 대화창 주요 인터랙션 (의료 도구 아님)
- Floating 패널 남발 (예측성 파괴)
- 뷰포트까지 밝게 처리 (CT 판독 불가 · chrome 은 bright 허용)

## 이전 willa 실수 사례

| 버전 | archetype | 문제 |
|------|----------|------|
| v0.5 | Thread Workspace | 대화창이 뷰포트 50% 침범 · 환자 데이터 혼동 위험 |
| v0.6 | Layered Workspace (pressure-based) | 숨김/표시 예측 불가 · UI 튀어나옴 |
| Card Deck (기각) | - | 드래그로 뷰 혼동 · 의료 도구 아님 |
| Focus Mode (기각) | - | Zen mode 는 판독의 환경 아님 |

**교훈**: CBCT 뷰어는 **예측 가능성 > 혁신** · 산업 표준 5형 중 선택 · 변이는 축에서.
