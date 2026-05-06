# TASK SPEC: 03_feature_definition

## 목적
RFP 기능 요구사항을 사용자 유형 및 페인포인트와 연결하여 재정의하고,
UX 관점의 기능 우선순위를 도출한다.

## INPUT
- output/{run_id}/01_rfp_structured.json
- output/{run_id}/02_user_analysis.json

## OUTPUT
- output/{run_id}/03_features.json

## FORMAT
JSON

## REQUIRED FIELDS

```
{
  "feature_groups": [
    {
      "id": "FG-001",
      "name": "기능 그룹명 (예: AI 상담 지원)",
      "description": "그룹 설명",
      "target_users": ["UT-001", "UT-002"]
    }
  ],
  "features": [
    {
      "id": "F-001",
      "name": "기능명",
      "description": "기능 설명 (사용자 관점으로 기술)",
      "group_id": "FG-001",
      "target_users": ["UT-001"],
      "painpoint_addressed": ["PP-UT001-01"],
      "needs_addressed": ["ND-UT001-01"],
      "rfp_refs": ["SFR-001"],
      "priority": "Critical/High/Medium/Low",
      "priority_rationale": "우선순위 판단 근거",
      "complexity": "High/Medium/Low",
      "dependencies": ["F-002"],
      "notes": "추가 고려사항"
    }
  ],
  "priority_summary": {
    "critical": ["F-001"],
    "high": ["F-002"],
    "medium": ["F-003"],
    "low": ["F-004"]
  },
  "unaddressed_painpoints": [
    {
      "painpoint_id": "PP-UT001-02",
      "reason": "RFP 범위 외이거나 기능으로 해결되지 않는 이유"
    }
  ]
}
```

## PRIORITY 기준
- Critical: 핵심 업무 수행에 필수, 없으면 시스템 사용 불가
- High: 주요 페인포인트 해결, 사용자 만족도에 직접 영향
- Medium: 효율 향상에 기여하나 없어도 업무 가능
- Low: 있으면 좋지만 후순위 가능

## QUALITY RULES
- features는 RFP의 SFR과 1:1 매핑이 아니어도 된다 (사용자 관점으로 재정의)
- 하나의 SFR이 여러 feature로 분리되거나, 여러 SFR이 하나의 feature로 묶일 수 있다
- painpoint_addressed는 반드시 02_user_analysis의 실제 PP ID를 참조한다
- priority_rationale은 "기능이 중요해서"가 아닌 사용자 임팩트 기반으로 기술한다
- unaddressed_painpoints는 해결되지 않는 페인포인트를 명시적으로 기록한다

## FAIL CONDITION
- features 배열 비어있음
- painpoint_addressed가 존재하지 않는 PP ID를 참조
- priority 필드 누락
- JSON 파싱 불가

## VALIDATION RULE
- 모든 rfp_refs가 01_rfp_structured의 실제 기능 ID를 참조하는지 확인
- 모든 target_users가 02_user_analysis의 실제 UT ID를 참조하는지 확인
- Critical 기능이 최소 3개 이상인지 확인
