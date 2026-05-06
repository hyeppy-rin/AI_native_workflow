# TASK SPEC: 01_rfp_structuring

## 목적
RFP 원문을 후속 UX 분석이 가능한 구조화된 JSON으로 변환한다.
NotebookLM MCP로 전체 RFP 브리핑을 먼저 생성하고,
Claude가 브리핑 + 원문 파일을 종합하여 JSON과 context.md를 생성한다.

## INPUT
- 01_input/rfp.md (NotebookLM 소스용 — 전체 합본)
- 01_input/1.추진방향_사업범위.md
- 01_input/2.현황_및_당면과제.md
- 01_input/3.상세요구사항_기능_인터페이스_컨설팅.md
- 01_input/reference/ (참고 PDF, 매뉴얼 등 — NotebookLM 소스용)

## OUTPUT
- 03_output/{run_id}/01_nlm_briefing.md (RFP 원문 브리핑 보고서)
- 03_output/{run_id}/01_ref_briefing.md (참고 자료 브리핑 보고서 — 있을 경우 생성)
- 03_output/{run_id}/01_rfp_structured.json
- context.md (프로젝트 루트에 생성)

## FORMAT
JSON + Markdown

---

## 01_rfp_structured.json REQUIRED FIELDS

```
{
  "project_name": "string",
  "project_overview": {
    "background": "string - 사업 추진 배경",
    "objectives": ["string - 사업 목표"],
    "scope": "string - 사업 범위 요약",
    "duration": "string - 사업 기간 (미명시 시 '미명시'로 표기)",
    "budget": "string - 예산 (미명시 시 '미명시'로 표기)"
  },
  "as_is": {
    "current_systems": ["string - 현재 운영 중인 시스템"],
    "current_problems": ["string - 현황의 문제점/한계"],
    "organizational_context": "string - 조직 규모, 부서 구조 등",
    "work_context": {
      "core_work_process": "string - 핵심 업무 처리 방식 및 흐름",
      "information_access": "string - 정보 검색/접근 현황 및 불편",
      "pain_indicators": ["string - 업무 비효율의 구체적 증거 (RFP 원문 기반)"]
    }
  },
  "to_be": {
    "vision": "string - 목표 상태",
    "key_changes": ["string - 주요 변화 내용"]
  },
  "stakeholders": [
    {
      "name": "string",
      "role": "string",
      "involvement": "string - 발주처/수행사/사용자/수혜자/연계기관"
    }
  ],
  "functional_requirements": [
    {
      "id": "string - 원문 ID 유지",
      "category": "string",
      "title": "string",
      "description": "string",
      "source_file": "string"
    }
  ],
  "non_functional_requirements": [
    {
      "id": "string - 원문 ID 또는 NFR-001 형식으로 부여",
      "category": "string - 성능/보안/인프라/가용성/호환성 등",
      "description": "string"
    }
  ],
  "constraints": [
    {
      "id": "string - 원문 ID 또는 CON-001 형식으로 부여",
      "type": "string - 법규/기술/일정/예산/운영환경 등",
      "description": "string"
    }
  ],
  "uncertain_items": [
    {
      "item": "string",
      "reason": "string",
      "clarification_needed": "string"
    }
  ]
}
```

---

## context.md 구성

```
# Project Override

## 도메인
- 조직명:
- 서비스 설명:
- 핵심 업무 맥락:

## 사용자 유형
- (유형명): (한 줄 설명)
- 예: 콜센터 상담사, 채무자, 내부 직원 등 RFP에서 식별된 실제 명칭 사용

## 도메인 용어 사전
- (용어): (정의) — RFP에서 반복 등장하는 도메인 특화 용어

## 태스크 수행 시 주의사항
- 이 프로젝트에서 특별히 고려해야 할 사항
- 예: 폐쇄망 환경, 특정 법규, 민감 데이터 처리 등
```

---

## QUALITY RULES
- NLM 브리핑(RFP/참고)은 전체 맥락 파악용이며, 세부 데이터(기능 요구사항 ID 등)는 원문에서 직접 추출한다
- 참고 자료(reference/)가 있을 경우, 해당 내용을 분석하여 비기능 요구사항이나 제약사항 탐색에 적극 활용한다
- 원문에 없는 내용은 추가하지 않는다
- non_functional_requirements와 constraints는 명시적 항목이 없어도 본문 맥락에서 적극 탐색한다
- work_context는 02_stakeholder_analysis 페인포인트 도출의 핵심 근거가 되므로 최대한 구체적으로 작성한다
- context.md의 용어 사전은 RFP에서 반복 등장하는 도메인 용어만 포함한다

## FAIL CONDITION
- NotebookLM 브리핑 생성 실패 시 Phase 1 재시도 후 진행
- 01_rfp_structured.json 파싱 불가
- functional_requirements 비어있음
- non_functional_requirements 비어있음
- constraints 비어있음
- as_is.work_context 누락
- context.md 미생성

## VALIDATION RULE
- 01_nlm_briefing.md 생성 확인
- non_functional_requirements 최소 3개 이상
- constraints 최소 3개 이상
- stakeholders 최소 3개 이상 (사용자/수혜자 포함)
- as_is.work_context.pain_indicators 최소 2개 이상
- context.md 4개 섹션 모두 작성되었는지 확인
