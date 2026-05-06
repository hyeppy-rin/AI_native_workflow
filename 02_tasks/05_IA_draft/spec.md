# TASK SPEC: 05_IA_draft

## 목적
기능 정의와 서비스 시나리오를 기반으로 시스템의 정보구조(IA)를 설계한다.
사용자 유형별 접근 권한과 메뉴 구조를 명확히 정의한다.

## INPUT
- output/{run_id}/03_features.json
- output/{run_id}/04_scenarios.json

## OUTPUT
- output/{run_id}/05_IA.json
- output/{run_id}/05_IA.md (문서용 정리본)

## FORMAT
JSON + Markdown

## REQUIRED FIELDS (JSON)

```
{
  "system_name": "시스템명",
  "ia_version": "v1.0",
  "menus": [
    {
      "id": "M-001",
      "label": "메뉴명",
      "depth": 1,
      "parent_id": null,
      "path": "/dashboard",
      "target_users": ["UT-001", "UT-002"],
      "access_control": "ALL/ROLE_BASED/ADMIN_ONLY",
      "linked_features": ["F-001"],
      "linked_scenarios": ["SC-001"],
      "screen_count": 1,
      "description": "이 메뉴에서 할 수 있는 것",
      "children": ["M-001-01", "M-001-02"]
    }
  ],
  "navigation_rules": [
    {
      "rule_id": "NR-001",
      "description": "규칙 설명",
      "affected_menus": ["M-001"],
      "condition": "조건 (예: 관리자 권한 보유 시)"
    }
  ],
  "user_menu_map": {
    "UT-001": ["M-001", "M-002"],
    "UT-002": ["M-001", "M-003"]
  },
  "ia_summary": {
    "total_menus": 0,
    "depth_1_count": 0,
    "depth_2_count": 0,
    "depth_3_count": 0,
    "total_screens": 0
  }
}
```

## Markdown 출력 형식 (05_IA.md)
- 시스템 개요 1~2줄
- 메뉴 트리 (들여쓰기로 depth 표현)
- 사용자 유형별 접근 가능 메뉴 표
- 주요 네비게이션 규칙 목록

## QUALITY RULES
- depth는 최대 3단계로 제한한다
- 모든 Critical/High 기능은 반드시 메뉴와 연결된다
- target_users는 02_user_analysis의 UT ID를 참조한다
- 사용자 유형별 메뉴 접근 범위가 명확히 구분되어야 한다
- linked_scenarios로 IA와 시나리오의 연결성을 유지한다

## FAIL CONDITION
- menus 배열 비어있음
- depth 1 메뉴 없음
- navigation_rules 없음
- user_menu_map 없음
- JSON 파싱 불가

## VALIDATION RULE
- 모든 linked_features가 실제 F ID를 참조하는지 확인
- 모든 linked_scenarios가 실제 SC ID를 참조하는지 확인
- Critical 기능 전부 최소 1개 메뉴에 연결되어 있는지 확인
- 05_IA.md 파일이 함께 생성되었는지 확인
