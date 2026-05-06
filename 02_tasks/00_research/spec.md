# TASK SPEC: 00_research

## 목적
RFP 분석 전에 유사 사례, 경쟁사 접근 방식, 도메인 레퍼런스를 수집하여
제안의 전략적 방향 설정과 차별화 근거를 마련한다.

## 실행 조건
선택적 실행 태스크다. run.md에서 실행 여부를 결정한다.
실행 시 01_rfp_structuring보다 먼저 수행한다.

## INPUT
- 01_input/ 폴더 내 rfp.md를 제외한 모든 파일 (도메인 파악용)
- 웹 리서치 (NotebookLM 또는 웹 검색)

## OUTPUT
- output/{run_id}/00_research.json
- output/{run_id}/00_research.md (문서용)

## FORMAT
JSON + Markdown

## REQUIRED FIELDS

```
{
  "domain_context": {
    "industry": "string - 산업/도메인",
    "key_trends": ["string - 관련 산업의 최신 트렌드"],
    "regulatory_environment": ["string - 관련 법규/정책 동향"]
  },
  "benchmarks": [
    {
      "id": "BM-001",
      "name": "string - 레퍼런스명 (기관/서비스명)",
      "type": "string - 국내유사사례/해외유사사례/경쟁사/베스트프랙티스",
      "description": "string - 어떤 사례인지",
      "relevance": "string - 이 프로젝트와의 연관성",
      "key_takeaways": ["string - 참고할 만한 점"],
      "cautions": ["string - 피해야 할 점"]
    }
  ],
  "ux_patterns": [
    {
      "pattern": "string - UX 패턴명",
      "use_case": "string - 어떤 상황에 쓰이는지",
      "relevance": "string - 이 프로젝트에서의 활용 가능성"
    }
  ],
  "differentiation_hints": [
    "string - 경쟁 제안 대비 차별화할 수 있는 방향 힌트"
  ],
  "research_limitations": [
    "string - 리서치의 한계 또는 추가 확인이 필요한 사항"
  ]
}
```

## QUALITY RULES
- 실제 존재하는 사례만 포함한다 (임의로 만들지 않는다)
- benchmarks는 최소 3개 이상, 국내/해외 각 1개 이상 포함
- differentiation_hints는 RFP의 사업 목적과 직접 연결되어야 한다
- research_limitations를 솔직하게 기록한다 (리서치 공백이 있으면 명시)

## FAIL CONDITION
- benchmarks 비어있음
- differentiation_hints 없음
- 임의로 만든 사례 포함
- JSON 파싱 불가

## VALIDATION RULE
- benchmarks 최소 3개 이상
- 국내/해외 사례 각 1개 이상 포함
- differentiation_hints 최소 2개 이상
- 00_research.md 함께 생성되었는지 확인
