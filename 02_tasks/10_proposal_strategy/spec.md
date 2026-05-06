# TASK SPEC: 10_proposal_strategy

## 목적
전체 분석 결과를 종합하여 제안의 핵심 전략과 차별화 포인트를 정의한다.
"왜 우리의 접근이 맞는가"에 답할 수 있는 제안 논리를 구조화한다.

## 실행 조건
09_screen_document 이전에 수행한다.
00_research가 실행된 경우 해당 output도 input으로 활용한다.

## INPUT
- output/{run_id}/01_rfp_structured.json
- output/{run_id}/02_user_analysis.json
- output/{run_id}/03_features.json
- output/{run_id}/04_scenarios.json
- output/{run_id}/00_research.json (존재하는 경우)
- context.md

## OUTPUT
- output/{run_id}/10_proposal_strategy.json
- output/{run_id}/10_proposal_strategy.md (문서용)

## FORMAT
JSON + Markdown

## REQUIRED FIELDS

```
{
  "proposal_vision": "string - 이 제안의 핵심 비전 (한 문장)",
  "problem_framing": {
    "surface_problem": "string - 클라이언트가 인식하는 문제",
    "real_problem": "string - UX 컨설턴트가 진단한 근본 문제",
    "gap": "string - 표면 문제와 근본 문제의 차이 및 그 의미"
  },
  "strategic_approach": {
    "approach_name": "string - 우리 접근 방식의 이름/프레임",
    "rationale": "string - 왜 이 접근이 맞는가",
    "vs_conventional": "string - 일반적인 접근과 어떻게 다른가"
  },
  "differentiation_points": [
    {
      "id": "DP-001",
      "point": "string - 차별화 포인트",
      "evidence": "string - 근거 (사용자 분석/레퍼런스/RFP 기반)",
      "impact": "string - 클라이언트에게 주는 가치"
    }
  ],
  "key_risks": [
    {
      "id": "RK-001",
      "risk": "string - 예상되는 리스크",
      "mitigation": "string - 대응 방안"
    }
  ],
  "success_metrics": [
    {
      "metric": "string - 성공 지표",
      "baseline": "string - 현재 수준 (AS-IS)",
      "target": "string - 목표 수준 (TO-BE)",
      "measurement": "string - 측정 방법"
    }
  ],
  "stakeholder_messaging": [
    {
      "stakeholder": "string - 이해관계자명",
      "key_message": "string - 이 이해관계자에게 전달할 핵심 메시지",
      "concern_addressed": "string - 이 이해관계자의 우려를 어떻게 해소하는가"
    }
  ]
}
```

## QUALITY RULES
- problem_framing의 real_problem은 RFP에 명시된 것 너머를 봐야 한다
- differentiation_points는 반드시 evidence가 있어야 한다 (주장만 하지 않는다)
- success_metrics는 측정 가능한 지표여야 한다
- stakeholder_messaging은 각 이해관계자의 실제 우려를 반영해야 한다

## FAIL CONDITION
- proposal_vision 없음
- problem_framing 누락
- differentiation_points evidence 없는 항목 존재
- success_metrics 비어있음
- JSON 파싱 불가

## VALIDATION RULE
- differentiation_points 최소 3개 이상
- success_metrics 최소 2개 이상
- stakeholder_messaging이 02_user_analysis의 stakeholders를 커버하는지 확인
- 10_proposal_strategy.md 함께 생성되었는지 확인
