# RFP 분석 파이프라인 실행 가이드 v3.4

## 실행 한 줄 요약
이 파일을 읽고 02_tasks/를 00부터 순서대로 실행한다.

---

## INPUT 정의
- 01_input/rfp.md: Task 01의 NotebookLM 소스용 (전체 합본)
- 01_input/ 하위 나머지 .md 파일들: rfp.md를 제외한 모든 파일을 순서대로 읽어 분석 소스로 활용
  (파일명·수량은 RFP마다 상이하므로 실행 시 동적으로 탐색)

---

## run_id 규칙
- 형식: `YYYYMMDD_HHmm` (예: `20260421_1430`)
- 모든 output은 `03_output/{run_id}/` 하위에 저장한다.

---

## 실행 순서

| 순서 | 폴더 | Input | Output | 도구 |
|------|------|-------|--------|------|
| 00 | `02_tasks/00_research` | 01_input/ (rfp.md 제외) | `00_research.json` + `00_research.md` | NotebookLM MCP (선택적) |
| 01 | `02_tasks/01_rfp_structuring` | 01_input/rfp.md (NLM) + 1~3.md (Claude) | `01_nlm_briefing.md` + `01_rfp_structured.json` + `context.md` | NotebookLM MCP + Antigravity |
| 02 | `02_tasks/02_stakeholder_analysis` | 01 output | `02_user_analysis.json` | Antigravity |
| 03 | `02_tasks/03_feature_definition` | 01+02 output | `03_features.json` | Antigravity |
| 04 | `02_tasks/04_service_scenario` | 02+03 output | `04_scenarios.json` | Antigravity |
| 05 | `02_tasks/05_IA_draft` | 03+04 output | `05_IA.json` + `05_IA.md` | Antigravity |
| 06 | `02_tasks/06_screen_flow` | 05+04 output | `06_screen_flow.json` | Antigravity |
| 07 | `02_tasks/07_screen_detail` | 06+04 output + context.md | `07_screen_detail.json` | Antigravity |
| 08 | `02_tasks/08_wireframe_generation` | 07 output + 04 output | `08_wireframes.html` + `08_wireframe_log.json` | Claude Code |
| 08P | `02_tasks/08_wireframe_generation` | 00~05 output (Task 05 완료 직후 실행 가능) | `08_prototype.html` | Claude Code |
| 10 | `02_tasks/10_proposal_strategy` | 01+02+03+04+00 output + context.md | `10_proposal_strategy.json` + `10_proposal_strategy.md` | Antigravity |
| 09 | `02_tasks/09_screen_document` | 05+06+07+08+10 output | `09_screen_spec.md` + `09_proposal_draft.md` | Antigravity |

---

## 각 Task 실행 규칙
1. Task 00은 선택적 실행이다. 실행 여부를 사용자에게 먼저 확인한다.
2. Task 01은 2단계로 실행한다.
   - Phase 1: NotebookLM MCP로 rfp.md 분석 → 01_nlm_briefing.md 생성
   - Phase 2: Claude가 브리핑 + 원문 파일(1~3.md) 종합 → JSON + context.md 생성
3. Task 02부터는 context.md를 먼저 읽고 컨텍스트로 유지한 뒤 실행한다.
4. 해당 task 폴더의 spec.md → skill.md 순으로 읽고 실행한다.
5. spec의 FAIL CONDITION 발생 시 즉시 중단하고 `z_logs/{run_id}.md`에 기록한다.
6. 이전 task output이 없으면 실행하지 않는다.
7. 각 task 완료 후 VALIDATION RULE을 자체 검증한 뒤 다음 task로 진행한다.
8. 각 task 완료 시 `z_logs/{run_id}.md`에 태스크명/소요시간/문제사항/개입여부를 기록한다.
9. Task 10은 반드시 09 실행 전에 수행한다.

---

## 도구 사용 기준
| 상황 | 사용 도구 |
|------|----------|
| JSON/MD 분석 및 생성 | Antigravity |
| RFP 전체 브리핑 생성 (task 01 Phase 1) | NotebookLM MCP |
| 데스크리서치 (task 00) | NotebookLM MCP |
| 와이어프레임 생성 (task 08) | Claude Code |
| 코드 생성 필요 시 | Claude Code |

---

## Task 08 실행 방법

### 08-A. 정적 HTML 와이어프레임 (Task 07 완료 후)
Task 08은 Claude Code에게 아래를 그대로 지시한다:

```
03_output/{run_id}/07_screen_detail.json과 03_output/{run_id}/04_scenarios.json을 실제로 읽고 파싱하여
03_output/{run_id}/08_wireframes.html을 생성해줘.
절대 데이터를 임의로 만들지 말고 JSON에 있는 내용만 사용해.

요구사항:
- 02_tasks/08_wireframe_generation/spec.md와 skill.md를 먼저 읽고 실행
- wireframe_required: true 화면만 대상
- 단일 HTML 파일, 상단 탭 전환 구조
- 탭 순서: Critical → High → Medium 우선순위 순
- 그레이스케일, 실제 텍스트 레이블, Lorem ipsum 금지
- 외부 라이브러리 사용 금지

각 화면 와이어프레임 표현 기준:
- 07_screen_detail.json의 components 필드를 빠짐없이 모두 화면에 배치할 것
- 각 컴포넌트의 label, type, description, states, interactions 등 명세된 속성을 화면 내 주석 또는 레이블로 표기
- 컴포넌트 간 위계(header/nav/content/footer 영역 구분)를 시각적으로 구분할 것
- 각 컴포넌트 블록 하단 또는 옆에 해당 컴포넌트의 주요 속성을 작은 폰트로 표기 (예: [버튼] 텍스트: "검색" / 상태: default, disabled)
- 04_scenarios.json의 해당 화면 시나리오 흐름을 탭 내 하단 영역에 "시나리오 컨텍스트"로 요약 표기
- 완료 후 08_wireframe_log.json도 동일 폴더에 생성
```

({run_id}는 실제 폴더명으로 교체)

---

### 08-B. 인터랙티브 프로토타입 HTML (Task 05 완료 직후 병렬 실행 가능)
> ⚡ Task 05(IA Draft) 완료 시점부터 실행 가능. Task 06~07 완료를 기다리지 않아도 됨.

Task 08P는 Claude Code에게 아래를 그대로 지시한다:

```
아래 파일들을 실제로 읽고 파싱하여 03_output/{run_id}/08_prototype.html을 생성해줘.
절대 데이터를 임의로 만들지 말고 JSON에 있는 내용만 사용해.

참조 파일 (존재하는 파일만 읽을 것):
- 03_output/{run_id}/00_research.json (없으면 스킵)
- 03_output/{run_id}/01_rfp_structured.json
- 03_output/{run_id}/02_user_analysis.json
- 03_output/{run_id}/03_features.json
- 03_output/{run_id}/04_scenarios.json
- 03_output/{run_id}/05_IA.json

요구사항:
- 단일 HTML 파일 (08_prototype.html)
- 외부 라이브러리 사용 금지 (순수 HTML/CSS/JS)
- 인터랙티브 프로토타입: 클릭·호버 등 실제 화면 전환 인터랙션 구현
- 05_IA.json의 IA 구조를 기반으로 전체 화면 네비게이션 구현
- 04_scenarios.json의 주요 시나리오 흐름을 버튼/링크로 연결하여 클릭 흐름 체험 가능
- 03_features.json의 핵심 기능을 각 화면에 UI 컴포넌트로 배치
- 02_user_analysis.json의 사용자 유형별 진입점(페르소나 스위처)을 상단에 제공
- 디자인: 단색 계열, 가독성 우선 (팀 리뷰·발표용)
- 각 화면에 "현재 화면 ID + 화면명" 헤더 표시
- 완료 후 08_prototype_log.json 생성 (생성 화면 목록, 연결된 시나리오 수 기록)
```

({run_id}는 실제 폴더명으로 교체)

---

## 자동 실행: 검토용 review.html 생성
Task 09 완료 후 즉시 Claude Code에게 아래를 지시한다 (사용자 확인 불필요, 자동 실행):

```
03_output/{run_id}/ 폴더의 00~07 JSON과 10 JSON을 실제로 읽고 파싱하여
03_output/{run_id}/review.html을 생성해줘.
절대 데이터를 임의로 만들지 말고 JSON에 있는 내용만 사용해.

요구사항:
- 단일 HTML 파일
- 상단 탭 9개: 00 리서치 / 01 사업개요 / 02 사용자분석 / 03 기능정의 / 04 시나리오 / 05 IA / 06 화면흐름 / 07 화면상세 / 10 제안전략
  (00_research.json이 없으면 해당 탭은 "리서치 미수행" 안내 표시)
- 각 탭의 시각화 방식은 해당 JSON 데이터의 구조와 내용을 직접 파악한 뒤 가장 적합한 방식으로 자율 결정할 것
  (테이블, 카드, 플로우, 트리, 비교표 등 데이터 특성에 맞게 선택)
- 데이터에 있는 모든 주요 항목을 누락 없이 표현할 것 — 요약·생략 금지
- 디자인: 깔끔한 단색 계열, 팀 검토용이므로 가독성 우선
```

({run_id}는 실제 폴더명으로 교체)

---

## 실행 명령
```
run.md를 읽고 실행해
```
