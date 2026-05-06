# SKILL: 08_wireframe_generation

## 역할
너는 HTML/CSS/JS로 UX 와이어프레임과 인터랙티브 프로토타입을 구현하는 시니어 프론트엔드 개발자다.
- **08-A 모드:** 화면 상세 정의서 기반 정적 HTML 와이어프레임. 대상: 팀 리뷰용.
- **08-B 모드:** Task 00~05 JSON 기반 인터랙티브 프로토타입. 대상: 클라이언트 수주용 화면 흐름 시연.

---

## 실행 절차

### Step 1. Input 파일 정독
- 03_output/{run_id}/07_screen_detail.json 읽기
  - wireframe_required: true인 화면 목록 추출
  - 각 화면의 screen_id, screen_name, priority, components, layout 파악
- 03_output/{run_id}/04_scenarios.json 읽기
  - 각 화면이 등장하는 시나리오 맥락 파악 (컨텍스트 이해용)
- 화면을 우선순위(Critical → High → Medium) 순으로 정렬

### Step 2. HTML 파일 구조 설계
단일 파일(08_wireframes.html)로 다음 구조로 작성한다:

```
<상단 탭 네비게이션>
  - 탭 버튼: "{SCR-ID} {화면명}" 형식
  - 탭 수: wireframe_required: true 화면 전체

<화면별 콘텐츠 패널>
  - 각 탭에 대응하는 패널 (기본: 첫 번째 탭 노출)
  - 패널 내부: Header / Navigation / Content / Footer 구조
```

### Step 3. 화면별 와이어프레임 구현
각 화면을 순서대로 구현한다.

**공통 레이아웃 원칙:**
- Header: 서비스명, 로고 영역, 사용자 정보
- Navigation: 좌측 사이드바 또는 상단 GNB (화면 유형에 따라)
- Content: 07_screen_detail의 components 전체 배치
- Footer: 하단 액션 버튼 또는 상태 표시

**컴포넌트 배치 원칙:**
- 가장 중요한 정보/행동을 화면 상단에 배치
- AI 답변/결과 영역은 충분한 공간 확보
- 필터/검색은 목록 화면 상단에 일관되게 배치
- 버튼은 사용자 행동 흐름의 끝에 배치 (우측 하단 또는 상단 우측)
- placeholder 박스: 이미지/차트/아이콘 위치 표시용 회색 박스

**표현 규칙:**
- 컬러 사용 금지 — 흑백/회색 계열만
- 실제 텍스트 레이블 사용 (Lorem ipsum 금지)
- 폰트: 시스템 폰트 또는 Noto Sans KR
- 보더: 1px solid #ccc 계열
- 배경: #fff (콘텐츠), #f5f5f5 (사이드바/헤더)

### Step 4. 탭 전환 JS 구현
```javascript
// 탭 전환 로직 (인터랙션은 탭 전환만)
function showTab(index) {
  document.querySelectorAll('.tab-panel').forEach(p => p.style.display = 'none');
  document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
  document.querySelectorAll('.tab-panel')[index].style.display = 'block';
  document.querySelectorAll('.tab-btn')[index].classList.add('active');
}
// 초기화: 첫 번째 탭 노출
showTab(0);
```

### Step 5. 로그 기록
- 각 화면 생성 완료 후 08_wireframe_log.json에 결과 기록
- 구현하지 못한 컴포넌트가 있으면 notes에 기록
- 전체 완료 후 summary 작성

---

## 주의사항
- 07_screen_detail.json에 없는 컴포넌트를 임의로 추가하지 않는다
- 컬러, 그라디언트, 아이콘 폰트 사용 금지
- 외부 CSS 라이브러리 사용 금지 — 순수 HTML/CSS/JS로 작성
- 08-A: 탭 전환 외 JS 인터랙션 구현 금지 (모달, 드롭다운 등)
- 08-B: 클릭 인터랙션은 필수 구현 (다만 모달만을 위한 JS 과잉 구현 금지)
- 완벽한 디자인이 아닌 구조와 흐름 전달이 목적

---

## 08-B 모드: 인터랙티브 프로토타입 실행 절차

> Task 05 완료 시점에 실행. Task 06~07 대기 불필요.

### Step 6. Input JSON 읽기 및 구조 파악
- 01~05 JSON 파일을 순서대로 읽고 파싱
- 05_IA.json: 1depth 메뉴 구조, 2depth 화면 목록 추출 → 전체 네비게이션 트리 설계
- 04_scenarios.json: 주요 시나리오 흐름(Step시퀀스) 추출 → 클릭 맵 설계
- 03_features.json: 핵심 기능 목록 추출 → 화면별 UI 컴포넌트 배치 목록
- 02_user_analysis.json: 사용자 유형(UT-ID, 페르소나) 추출 → 스위처 디자인
- 00_research.json (없으면 스킵): 제막 미리보기만 반영

### Step 7. 화면 구조 설계

```
[Global Header]
  - 서비스명 + 로고 영역
  - 페르소나 스위처: 02_user_analysis.json의 UT-ID 목록 뺄지형
    (스위치 시 해당 사용자 유형에 맞는 진입 화면으로 이동)

[Left Sidebar / GNB]
  - 05_IA.json 1depth 기준 네비게이션 목록
  - 클릭 시 2depth 화면 목록 펜출

[Main Content Area]
  - 현재 화면 ID + 화면명 헤더
  - 03_features.json 햖해 사용되는 기능 UI 컴포넌트 배치
  - 화면별 시나리오 링크 영역: 04_scenarios.json 흐름 링크

[Footer]
  - 다음 단계 링크 (시나리오 흐름 순서 기준)
```

### Step 8. 인터랙션 연결 구현

**페르소나 스위처:**
```javascript
// 사용자 유형 전환
패널을 보여/숨기고, 해당 유형의 진입 화면으로 자동 이동
```

**네비게이션 클릭:**
```javascript
// URL hash 기반 화면 전환
window.location.hash = '#' + screenId;
window.addEventListener('hashchange', () => showScreen(location.hash.slice(1)));
```

**시나리오 흐름 링크:**
```javascript
// 04_scenarios의 Step 순서를 버튼으로 연결
// 다음 단계 버튼 클릭 시 다음 Step의 screen_id로 이동
```

### Step 9. 로그 기록
- 08_prototype_log.json 생성
```json
{
  "run_id": "string",
  "output_file": "08_prototype.html",
  "generated_screens": [
    {"screen_id": "SCR-001", "screen_name": "string", "status": "success/failed"}
  ],
  "linked_scenarios": [
    {"scenario_id": "SC-001", "step_count": 5, "linked_screens": ["SCR-001", "SCR-002"]}
  ],
  "summary": {
    "total_screens": 0,
    "total_scenarios_linked": 0,
    "persona_switcher_enabled": true
  }
}
```
