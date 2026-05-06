# TASK SPEC: 08_wireframe_generation

## 목적
**08-A (정적 와이어프레임):** 화면 상세 정의서를 바탕으로 우선순위 화면의 HTML 와이어프레임을 Claude Code로 생성한다. 모든 화면을 탭으로 전환하는 단일 HTML 파일로 출력한다.

**08-B (인터랙티브 프로토타입):** Task 00~05 output을 바탕으로 IA 구조와 시나리오 흐름이 클릭 가능한 인터랙티브 프로토타입 HTML을 Claude Code로 생성한다. Task 05 완료 직후 실행 가능 (Task 06~07 대기 불필요).

## INPUT

### 08-A (정적 와이어프레임)
- 03_output/{run_id}/07_screen_detail.json (wireframe_required: true 화면 대상)
- 03_output/{run_id}/04_scenarios.json (컨텍스트 참조용)

### 08-B (인터랙티브 프로토타입)
- 03_output/{run_id}/00_research.json (선택, 없으면 스킵)
- 03_output/{run_id}/01_rfp_structured.json
- 03_output/{run_id}/02_user_analysis.json
- 03_output/{run_id}/03_features.json
- 03_output/{run_id}/04_scenarios.json
- 03_output/{run_id}/05_IA.json

## OUTPUT

### 08-A
- 03_output/{run_id}/08_wireframes.html (전체 화면 탭 전환 단일 파일)
- 03_output/{run_id}/08_wireframe_log.json (생성 결과 로그)

### 08-B
- 03_output/{run_id}/08_prototype.html (인터랙티브 프로토타입 단일 파일)
- 03_output/{run_id}/08_prototype_log.json (생성 화면 목록 및 시나리오 연결 수 기록)

## HTML 와이어프레임 구조 규칙
- 단일 HTML 파일 (08_wireframes.html)
- 상단 탭 네비게이션: 화면 ID + 화면명 (예: SCR-001 AI상담어드바이저)
- 탭 순서: wireframe_required: true 화면을 우선순위(Critical → High → Medium) 순으로 정렬
- 각 탭 내부 레이아웃: Header / Navigation / Content / Footer 구조
- 뷰포트 기준: 1440px width

## 와이어프레임 표현 수준
- 그레이스케일 (컬러 없음, 배경/텍스트/보더 모두 흑백 계열)
- 실제 텍스트 레이블 사용 (Lorem ipsum 금지)
- 이미지/아이콘 위치는 placeholder 박스로 표현
- 07_screen_detail.json의 components를 모두 포함
- 인터랙션 없음 — 탭 전환만 JS로 구현

## 08_wireframe_log.json REQUIRED FIELDS
```json
{
  "run_id": "string",
  "output_file": "08_wireframes.html",
  "generated_screens": [
    {
      "screen_id": "SCR-001",
      "screen_name": "화면명",
      "tab_index": 0,
      "status": "success/failed",
      "notes": "특이사항"
    }
  ],
  "summary": {
    "total_requested": 0,
    "success": 0,
    "failed": 0
  }
}
```

## FAIL CONDITION

### 08-A
- 08_wireframes.html 미생성
- wireframe_required 화면 중 50% 이상 누락
- 08_wireframe_log.json 미생성
- 탭 전환 동작 불가

### 08-B
- 08_prototype.html 미생성
- 05_IA.json 기반 네비게이션 구조 미구현
- 외부 라이브러리 사용
- 클릭 인터랙션 전혀 없음
- 08_prototype_log.json 미생성

## VALIDATION RULE

### 08-A
- 생성된 탭 수가 wireframe_required: true 화면 수와 일치하는지 확인
- 각 탭에 07_screen_detail의 components가 모두 포함되어 있는지 확인
- 탭 전환이 정상 동작하는지 확인

### 08-B
- 05_IA.json의 1depth 메뉴 항목이 모두 네비게이션에 표현되었는지 확인
- 04_scenarios.json의 주요 시나리오 흐름이 최소 1개 이상 클릭으로 연결되어 있는지 확인
- 02_user_analysis.json의 사용자 유형(페르소나)이 스위처 UI로 제공되는지 확인
- 화면 전환 시 URL hash 또는 JS로 상태 관리가 구현되어 있는지 확인
- 08_prototype_log.json에 생성 화면 수와 연결 시나리오 수가 기록되어 있는지 확인
