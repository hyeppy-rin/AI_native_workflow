# TASK SPEC: 07_screen_detail

## 목적
화면 목록과 시나리오를 기반으로 각 화면의 내부 구성을 상세하게 정의한다.
이 결과물은 Figma 와이어프레임 제작의 직접적인 설계도가 된다.

## INPUT
- output/{run_id}/06_screen_flow.json (wireframe_required: true 화면 대상)
- output/{run_id}/04_scenarios.json (사용 맥락 참조)
- context.md (도메인 용어 참조)

## OUTPUT
- output/{run_id}/07_screen_detail.json

## FORMAT
JSON

## REQUIRED FIELDS

```
{
  "screens": [
    {
      "id": "SCR-001",
      "name": "화면명",
      "layout": {
        "structure": "string - 레이아웃 구조 설명 (예: 좌우 2단 분할, 상단 헤더 + 하단 콘텐츠)",
        "grid": "string - 그리드 기준 (예: 12컬럼, 좌측 3:우측 9)",
        "scroll": "string - 스크롤 방식 (예: 전체 스크롤 없음, 우측 패널만 스크롤)"
      },
      "regions": [
        {
          "id": "R-001",
          "name": "영역명 (예: 헤더, 좌측 패널, 콘텐츠 영역)",
          "position": "string - 위치 (예: 상단 고정, 좌측 20%)",
          "components": [
            {
              "id": "C-001",
              "name": "컴포넌트명",
              "type": "string - Button/Input/Table/Card/Chart/Badge/Modal/List/Text/Icon 등",
              "purpose": "string - 이 컴포넌트가 하는 역할",
              "content": "string - 실제 표시될 텍스트, 레이블, 컬럼명 등 (Lorem ipsum 금지)",
              "state": {
                "default": "string - 기본 상태",
                "loading": "string - 로딩 상태 (해당 시)",
                "empty": "string - 빈 상태 (해당 시)",
                "error": "string - 에러 상태 (해당 시)"
              },
              "interaction": "string - 클릭/호버/입력 시 동작"
            }
          ]
        }
      ],
      "user_flow_context": "string - 이 화면이 시나리오에서 어떤 순간에 등장하는지",
      "design_notes": ["string - 와이어프레임 제작 시 특별히 고려할 사항"]
    }
  ]
}
```

## QUALITY RULES
- wireframe_required: true인 화면만 작성한다
- regions는 실제 화면을 구역으로 나눈 것이다 (헤더/네비/콘텐츠/푸터 등)
- components의 content는 반드시 실제 업무 텍스트로 작성한다 (Lorem ipsum 절대 금지)
- 모든 주요 컴포넌트의 빈화면/로딩/에러 상태를 정의한다
- design_notes는 Figma 작업자가 판단하기 어려운 UX 의도를 명시한다
- context.md의 도메인 용어를 content 작성 시 반영한다

## FAIL CONDITION
- screens 배열 비어있음
- wireframe_required: true 화면 중 누락된 화면 존재
- content에 Lorem ipsum 사용
- regions 없는 화면 존재
- JSON 파싱 불가

## VALIDATION RULE
- 모든 screen ID가 06_screen_flow의 wireframe_required: true 화면과 일치하는지 확인
- 모든 화면에 최소 2개 이상의 region이 존재하는지 확인
- 모든 컴포넌트에 content 필드가 작성되었는지 확인
