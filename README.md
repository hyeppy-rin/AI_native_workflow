# 🛡️ AI-Native UX Design Pipeline

본 프로젝트는 복잡한 RFP(제안요청서) 분석부터 와이어프레임 생성까지의 전 과정을 자동화한 **AI-Native UX 컨설팅 파이프라인**입니다.

---

## 🚀 프로젝트 개요

이 파이프라인은 파편화된 요구사항을 수집하여 구조화된 JSON 데이터로 변환하고, 이를 기반으로 시각적 프로토타입과 상세 명세서를 생성합니다. 모든 단계는 독립적인 모듈(`02_tasks`)로 구성되어 있으며, `run.md`라는 단일 진입점을 통해 오케스트레이션됩니다.

### 핵심 가치
1.  **Data-Driven Consistency**: 모든 산출물이 이전 단계의 JSON 데이터를 상속받아 정합성이 100% 보장됩니다.
2.  **Autonomous Execution**: `run.md` 지침에 따라 AI 에이전트가 자율적으로 다음 단계의 Task를 판단하고 실행합니다.
3.  **Visual Quick-Win**: Task 05(IA) 완료 시점에 즉시 인터랙티브 프로토타입(`08_prototype.html`)을 추출하여 초기 컨셉 검증이 가능합니다.

---

## 📂 폴더 구조

```text
.
├── 01_input/           # 분석 대상 RFP 및 원천 자료 (.md)
├── 02_tasks/           # 단계별 실행 지침 (00~10)
│   └── {task_name}/
│       ├── spec.md     # Task 목표 및 데이터 입출력 정의
│       └── skill.md    # 구체적인 분석 방법론 및 도구 활용 기술
├── 03_output/          # 실행 결과물 (run_id별 관리)
│   └── {run_id}/       # YYYYMMDD_HHmm 형식의 실행 결과 폴더
├── z_logs/             # 실행 로그 및 히스토리
├── run.md              # 파이프라인 전체 실행 가이드 (Entry Point)
└── README.md           # 프로젝트 가이드 (현재 파일)
```

---

## 🛠️ 준비 사항 (Prerequisites)

파이프라인 실행을 위해 아래 도구들이 환경에 설정되어 있어야 합니다.

1.  **Antigravity (Google DeepMind)**: 메인 오케스트레이션 및 데이터 분석 에이전트.
2.  **Claude Code**: HTML 와이어프레임 및 프로토타입 코드 생성용 에이전트.
3.  **NotebookLM MCP**: 대규모 문서 브리핑 및 웹 리서치 수행용 도구.

---

## 🏃‍♂️ 실행 방법 (Quick Start)

팀원분들이 이 파이프라인을 처음 사용하신다면 아래 순서를 따르세요.

1.  **Input 준비**: `01_input/` 폴더에 분석할 RFP 원문(`rfp.md`)과 참고 자료들을 업로드합니다. 원문 중 핵심 목차들은 Antigravity 내장 agent에게 별도 md 파일 분리를 요청합니다.
2.  **파이프라인 가동**: 터미널 또는 에이전트 채팅창에서 아래 명령을 입력합니다.
    ```bash
    run.md를 읽고 실행해
    ```
3.  **자율 실행 승인**: 에이전트가 `run.md`의 규칙에 따라 각 Task를 순차적으로 실행합니다. `run_id`가 생성되고 결과물이 `03_output/`에 쌓이기 시작합니다.
4.  **결과 확인**: 모든 Task가 완료되면 `03_output/{run_id}/review.html`을 열어 전체 프로세스를 한눈에 검토합니다.

---

## 📊 파이프라인 워크플로우

| 단계 | Task명 | 주요 역할 | 산출물 |
|:---:|:---|:---|:---|
| 00 | Research | 시장 트렌드 및 벤치마킹 분석 | `00_research.json` |
| 01 | RFP Structuring | 요구사항 구조화 및 컨텍스트 추출 | `01_rfp_structured.json` |
| 02 | Stakeholder | 사용자 페르소나 및 니즈 분석 | `02_user_analysis.json` |
| 03 | Feature | 서비스 기능 정의 및 우선순위 선정 | `03_features.json` |
| 04 | Scenario | 사용자 경험 시나리오 설계 | `04_scenarios.json` |
| 05 | IA Draft | 정보 구조도(IA) 설계 | `05_IA.json`, `05_IA.md` |
| 06 | Screen Flow | 화면 이동 경로 및 로직 설계 | `06_screen_flow.json` |
| 07 | Screen Detail | 컴포넌트 단위 상세 속성 정의 | `07_screen_detail.json` |
| 08 | Prototype | 인터랙티브 HTML 와이어프레임 생성 | `08_wireframes.html` |
| 10 | Strategy | UX 제안 전략 및 가치 제언 | `10_proposal_strategy.md` |
| 09 | Document | 최종 화면 명세서 및 제안 초안 작성 | `09_screen_spec.md` |

---

## 🛡️ 활용 팁

*   **Task 08P를 활용하세요**: Task 05가 끝나는 즉시 프로토타입 생성을 요청할 수 있습니다. 팀 내부 리뷰 시간을 획기적으로 줄여줍니다.
*   **review.html은 필수입니다**: 개별 JSON을 일일이 열어볼 필요 없이, `review.html` 하나로 데이터의 흐름과 논리적 비약을 검토할 수 있습니다.
*   **Fail Condition**: 실행 중 에이전트가 중단된다면 `z_logs/`를 확인하세요. 데이터 불일치나 누락이 있을 경우 장군이가 엄격하게 실행을 멈추도록 설정해두었습니다.

---
**문의:** 본 파이프라인의 커스터마이징은 언제든 요청해주세요!
