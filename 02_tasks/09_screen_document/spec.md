# TASK SPEC: 08_screen_design_document

## 목적
전체 파이프라인 산출물을 종합하여 화면설계서와 제안서 초안을 생성한다.

## INPUT
- output/{run_id}/05_IA.json + 05_IA.md
- output/{run_id}/06_screen_flow.json
- output/{run_id}/07_wireframe_log.json

## OUTPUT
- output/{run_id}/08_screen_spec.md (화면설계서 - PPT 변환용)
- output/{run_id}/08_proposal_draft.md (제안서 초안)

## FORMAT
Markdown

---

## 08_screen_spec.md 구성

```
# 화면설계서: {project_name}
## 문서 정보 (버전, 작성일, run_id)

## 1. 시스템 개요
- 시스템 목적 및 범위
- 사용자 유형 요약

## 2. 정보구조 (IA)
- 메뉴 트리 (05_IA.md 내용 인용)
- 사용자별 접근 권한 표

## 3. 화면 목록
- 전체 화면 목록 표 (ID, 화면명, 타입, 대상사용자, 와이어프레임 여부)

## 4. 화면별 상세 정의
각 wireframe_required: true 화면에 대해:
- 화면 ID / 화면명
- 화면 목적
- 대상 사용자
- 진입 경로 / 이탈 경로
- 주요 컴포넌트 목록
- Figma 링크 (07_wireframe_log에서 참조)
- 연관 시나리오

## 5. 화면 전환 흐름
- 시나리오별 화면 전환 흐름 다이어그램 (텍스트 기반)
```

---

## 08_proposal_draft.md 구성

```
# 제안서 초안: {project_name}

## 1. 사업 개요
## 2. 현황 분석 및 문제점 (AS-IS)
## 3. 제안 방향 및 목표 (TO-BE)
## 4. 사용자 분석 결과
## 5. 핵심 서비스 시나리오
## 6. 시스템 구성 (IA 기반)
## 7. 주요 화면 소개 (와이어프레임 링크 포함)
## 8. 기대 효과
## 9. 추진 일정
```

## QUALITY RULES
- 화면설계서는 개발자/디자이너가 바로 참고할 수 있는 수준으로 작성
- 제안서 초안은 발주처 보고용으로 작성 (전문 용어 설명 포함)
- 두 문서 모두 이전 task output을 충실히 반영해야 함

## FAIL CONDITION
- 두 파일 중 하나라도 미생성
- 화면 목록 누락
- Figma 링크 없이 와이어프레임 참조

## VALIDATION RULE
- 08_screen_spec.md의 화면 목록이 06_screen_flow의 전체 화면 수와 일치하는지 확인
- 08_proposal_draft.md의 모든 섹션이 작성되었는지 확인
