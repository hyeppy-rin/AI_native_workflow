# TASK SPEC: 02_stakeholder_analysis

## 목적
RFP 구조화 결과를 바탕으로 실제 시스템을 사용할 사용자 유형을 정의하고,
각 유형의 현재 페인포인트와 니즈를 도출한다.
이 결과는 이후 모든 UX 설계의 근거가 된다.

## INPUT
- output/{run_id}/01_rfp_structured.json

## OUTPUT
- output/{run_id}/02_user_analysis.json

## FORMAT
JSON

## REQUIRED FIELDS

```
{
  "analysis_basis": "분석 근거 요약",
  "stakeholders": [
    {
      "id": "SH-001",
      "name": "이해관계자명",
      "type": "발주처/수행사/운영기관 등",
      "interests": ["주요 관심사항"],
      "influence": "High/Medium/Low"
    }
  ],
  "user_types": [
    {
      "id": "UT-001",
      "name": "사용자 유형명 (예: 내부 운영자)",
      "description": "유형 설명",
      "organization": "소속 조직",
      "primary_tasks": ["주요 업무"],
      "usage_context": {
        "frequency": "사용 빈도",
        "environment": "사용 환경",
        "device": "주요 사용 기기",
        "tech_literacy": "High/Medium/Low"
      },
      "current_painpoints": [
        {
          "id": "PP-UT001-01",
          "painpoint": "현재 겪고 있는 불편/문제",
          "impact": "업무에 미치는 영향",
          "severity": "High/Medium/Low"
        }
      ],
      "needs": [
        {
          "id": "ND-UT001-01",
          "need": "사용자가 원하는 것",
          "type": "Functional/Emotional/Social",
          "linked_painpoint": "연결된 페인포인트 ID"
        }
      ],
      "goals": ["이 사용자 유형의 최종 목표"],
      "frustrations": ["현재 시스템/프로세스에서 느끼는 불만"]
    }
  ],
  "key_insights": [
    {
      "id": "INS-001",
      "insight": "UX 관점의 핵심 인사이트",
      "affected_users": ["영향받는 사용자 유형 ID"],
      "design_implication": "설계 시 고려해야 할 사항"
    }
  ],
  "user_relationship_map": "사용자 유형 간 관계 및 상호작용 설명"
}
```

## QUALITY RULES
- user_types는 RFP에서 추론 가능한 모든 사용자 유형을 포함한다 (최소 3개 이상)
- painpoints는 현재 AS-IS 상황 기반으로 도출한다 (TO-BE 기능에서 역산하지 않는다)
- needs는 painpoints에서 자연스럽게 도출되어야 한다 (기능 목록을 그대로 needs로 쓰지 않는다)
- key_insights는 단순 정리가 아닌 UX 컨설턴트 관점의 해석이어야 한다
- severity/influence는 RFP 맥락 기반으로 판단한다

## FAIL CONDITION
- user_types 3개 미만
- painpoints가 TO-BE 기능 목록의 단순 반복인 경우
- key_insights 없음
- JSON 파싱 불가

## VALIDATION RULE
- 모든 needs의 linked_painpoint가 실제 존재하는 PP ID를 참조하는지 확인
- key_insights의 affected_users가 실제 존재하는 UT ID를 참조하는지 확인
