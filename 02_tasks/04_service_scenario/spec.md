# TASK SPEC: 04_service_scenario

## 목적
주요 사용자 유형별 핵심 서비스 시나리오를 작성하여
기능 목록을 실제 사용 흐름으로 전환한다.
이 결과는 IA, 화면 흐름, 와이어프레임의 직접 근거가 된다.

## INPUT
- output/{run_id}/02_user_analysis.json
- output/{run_id}/03_features.json

## OUTPUT
- output/{run_id}/04_scenarios.json

## FORMAT
JSON

## REQUIRED FIELDS

```
{
  "scenarios": [
    {
      "id": "SC-001",
      "title": "시나리오 제목 (예: 상담사가 AI 어드바이저로 채무자 상담을 준비하는 과정)",
      "user_type": "UT-001",
      "trigger": "시나리오가 시작되는 조건/상황",
      "goal": "사용자가 이 시나리오에서 달성하려는 목표",
      "steps": [
        {
          "step_no": 1,
          "action": "사용자 행동",
          "system_response": "시스템이 제공하는 것",
          "pain_moment": "이 단계에서 발생할 수 있는 어려움 (없으면 null)",
          "feature_used": ["F-001"]
        }
      ],
      "expected_outcome": "시나리오 완료 시 기대 결과",
      "alternative_flows": ["예외 상황 또는 대안 경로 설명"],
      "priority": "Critical/High/Medium",
      "wireframe_target": true
    }
  ],
  "scenario_coverage": {
    "user_type_coverage": {
      "UT-001": ["SC-001", "SC-002"],
      "UT-002": ["SC-003"]
    },
    "feature_coverage": {
      "F-001": ["SC-001"],
      "F-002": ["SC-001", "SC-003"]
    }
  },
  "priority_wireframe_scenarios": ["SC-001", "SC-002"]
}
```

## QUALITY RULES
- 시나리오는 사용자의 감정과 맥락이 느껴지도록 구체적으로 작성한다
- steps는 최소 3단계 이상, 각 단계에서 시스템이 무엇을 제공하는지 명시한다
- pain_moment는 현실적인 어려움을 솔직하게 기술한다 (긍정적인 시나리오만 쓰지 않는다)
- wireframe_target: true인 시나리오는 핵심 화면 도출의 근거가 된다
- 모든 Critical 기능은 최소 1개 이상의 시나리오에 포함되어야 한다

## FAIL CONDITION
- scenarios 3개 미만
- steps 없는 시나리오 존재
- wireframe_target: true인 시나리오 없음
- JSON 파싱 불가

## VALIDATION RULE
- 모든 user_type이 실제 UT ID를 참조하는지 확인
- 모든 feature_used가 실제 F ID를 참조하는지 확인
- priority_wireframe_scenarios에 최소 3개 이상 포함
