---
name: willa76
description: 5176 (will3d-current · Will3D 현 UI 모방본) 상태 확인 및 미기동 시 자동 기동. 일회성 · 이미 기동 중이면 skip.
triggers:
  - /willa76
  - /willa:76
scope: willa-workflow
---

# /willa76 — Will3D 현 UI 모방 (5176) 기동 확인

## 사용법

```
/willa76         # 5176 상태 확인 → 미기동이면 자동 기동 · 이미 기동이면 스킵
```

## 목적

Will3D 의 **현재 Qt 애플리케이션 UI** 를 React 로 모방한 참고본을 5176 에 띄워 · willa 제안본(5175) 과 비교/대조할 기준점 제공.

**willa 는 이 폴더를 절대 수정하지 않음.** 참고용만.

---

## 동작 (단순)

### 1. 5176 상태 확인
```
curl -s -o /dev/null -w "%{http_code}" http://localhost:5176/
```

- **200** → 이미 기동 중 · `"✅ will3d-current 이미 기동 중 · http://localhost:5176/"` 출력 후 종료
- **실패** → 기동 시도 (아래)

### 2. 폴더 존재 확인
- `prototype/will3d-current/` 존재? → 있으면 기동
- 없으면:
  ```
  ❌ prototype/will3d-current/ 폴더 없음.
  · Qt UI 모방본이 아직 없습니다.
  · 생성은 별도 작업 (/willa 사이클로 target: will3d-current 지정 또는 수동 init)
  ```
  안내 후 종료

### 3. 기동
```bash
cd prototype/will3d-current && npm run dev   # background
```

### 4. 응답 대기
- 최대 30초 대기 · 200 응답 확인
- 성공: `"✅ will3d-current 기동 완료 · http://localhost:5176/"`
- 실패: 에러 로그 경로 안내 + `npm install` 재시도 권장

---

## 왜 일회성?

- `will3d-current` 는 Will3D Qt UI 의 **정적 모방**. 자주 바뀌지 않음.
- 이미 기동 중이면 재기동 불필요 (프로세스 중복 방지).
- Qt 본체 UI 가 크게 바뀐 경우에만 수동 재생성 (별도 /willa 사이클).

## 범위 · 금지

- `prototype/will3d-current/` 는 **읽기/기동만** · 수정 안 함 (willa76 은 "참고 기동" 목적)
- will3d-current 의 코드 수정은 별도 /willa 사이클에서 `target: will3d-current` 명시 후만
- Will3D C++ 본체 무관
- 실패해도 willa 플로우 중단 아님 · 참고용이므로 optional

## 언제 호출되나

- `/willa` Phase 1 Discovery 중 · 5176 비교 필요 시 자동 권장
- 사용자가 "현재 Will3D UI 와 비교하려면?" 물을 때
- /willa Serve 결과에 "📎 참고: 5176" 안내 포함 (미기동이면 /willa76 제안)
