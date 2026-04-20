# Pattern · Inspector (3-tab)

## 요약
우측 340px 패널. 3개 탭(**속성 / Segmentation / 기록**)으로 컨텍스트 분할. **선택 객체**와 관련된 속성만 표시. 접을 수 있음 (Alt+I).

## 탭 구성

### 1. 속성 (Properties)
- 선택된 임플란트 Hero 카드 (제조사 + 직경×길이 + 6-spec grid)
- `More details` 접이식 (Surface, Material, Apex 좌표)
- Clearance 5줄 (색 코딩: safe/warn/danger)
- Bone density 3-level bar
- 경고 배지 (안전 여유 < 임계)
- Window/Level 섹션 (WW/WL 슬라이더 + 프리셋 4개)
- Segmentation summary
- Measurements 목록

### 2. Segmentation
- **AI 상태 카드** (클릭 → AI Panel Sheet 오픈)
- **Anatomy Navigator** (SVG 해부도 + 가시성 토글)
- Layers (눈/자물쇠)
- Teeth FDI 상/하악 30개 칩

### 3. 기록 (History)
- 타임라인 spine + 아이콘별 분류 (create/edit/measure/ai/system)
- 시간 · 작업자 · 액션 · 타겟

## 핵심 특징

- **flex-1 min-h-0 + ScrollArea** — 스크롤 동작 보장
- 탭 전환 시 현재 Phase 라벨 작게 표시
- Header 우측 Pin 아이콘 + Close 버튼
- Inspector 접힌 상태: 우측 32px 세로 버튼만 남음

## 구현 포인트

```tsx
type Tab = 'properties' | 'segmentation' | 'history';

<aside className="flex h-full w-full flex-col border-l ...">
  <div>헤더 + 접기 버튼</div>
  <div>탭 3개</div>
  <ScrollArea className="flex-1 min-h-0">
    {tab === 'properties' && <PropertiesTab />}
    {tab === 'segmentation' && <SegmentationTab onOpenAI={...} />}
    {tab === 'history' && <HistoryTab />}
  </ScrollArea>
</aside>
```

## Anti-patterns

- 속성탭에 Workspace/Domain 스위처 두기 → SubNav과 중복 (별도 패턴 "domain-nav" 참고)
- 탭 4개 이상 → 탭 라벨 잘림. 3탭이 심미적 최대
- Selection 없이 임플란트 정보 항상 표시 → 논리 모순. 선택 없으면 비어있거나 다음 행동 안내
- 각 섹션에 과도한 구분선 → 그리드감 빽빽
