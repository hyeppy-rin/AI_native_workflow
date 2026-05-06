# TASK SPEC: 06_screen_flow

## 목적
IA와 서비스 시나리오를 기반으로 화면 목록을 확정하고
화면 간 전환 흐름을 정의한다.
화면 내부 구성(컴포넌트 상세)은 07_screen_detail에서 수행한다.

## INPUT
- output/{run_id}/05_IA.json
- output/{run_id}/04_scenarios.json

## OUTPUT
- output/{run_id}/06_screen_flow.json

## FORMAT
JSON

## REQUIRED FIELDS

```
{
  "screens": [
    {
      "id": "SCR-001",
      "name": "화면명",
      "description": "이 화면에서 사용자가 할 수 있는 것 (1~2줄)",
      "menu_id": "M-001",
      "target_users": ["UT-001"],
      "screen_type": "Dashboard/List/Detail/Form/Modal/Popup/Landing",
      "entry_points": ["Login 또는 이전 화면 SCR-ID"],
      "exit_points": ["다음 화면 SCR-ID"],
      "linked_features": ["F-001"],
      "linked_scenarios": ["SC-001"],
      "priority": "Critical/High/Medium/Low",
      "wireframe_required": true
    }
  ],
  "flows": [
    {
      "id": "FL-001",
      "scenario_ref": "SC-001",
      "title": "흐름 제목",
      "steps": [
        {
          "step_no": 1,
          "screen_id": "SCR-001",
          "action": "사용자 행동",
          "transition": "다음 화면으로 이동 조건/방법"
        }
      ]
    }
  ],
  "screen_summary": {
    "total": 0,
    "wireframe_required_count": 0,
    "by_type": {
      "Dashboard": 0,
      "List": 0,
      "Detail": 0,
      "Form": 0,
      "Modal": 0
    }
  }
}
```

## QUALITY RULES
- 화면 내부 컴포넌트 정의는 하지 않는다 (07_screen_detail에서 수행)
- 모든 IA 메뉴에 최소 1개 이상의 화면이 대응되어야 한다
- flows는 04_scenarios의 모든 시나리오를 커버해야 한다
- entry_points / exit_points로 화면 간 연결이 명확해야 한다
- wireframe_required는 04_scenarios의 priority_wireframe_scenarios 기준으로 결정한다

## FAIL CONDITION
- screens 배열 비어있음
- flows 없음
- wireframe_required: true인 화면 없음
- JSON 파싱 불가

## VALIDATION RULE
- 모든 menu_id가 05_IA의 실제 메뉴 ID를 참조하는지 확인
- 모든 scenario_ref가 실제 SC ID를 참조하는지 확인
- wireframe_required: true인 화면이 최소 3개 이상인지 확인
- screens에 key_components 필드가 없는지 확인 (07로 이관됨)
