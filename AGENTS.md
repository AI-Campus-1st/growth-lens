# AGENTS.md — AI 코딩 에이전트 작업 지침

이 저장소에서 작업하는 모든 AI 에이전트(Claude, Codex, Copilot 등)와 팀원이 따르는 규칙입니다.

## 1. 프로젝트 맥락
- GrowthLens: 네이버 스마트스토어 소규모 판매자용 커머스·마케팅 의사결정 지원 플랫폼 (4인, 5~8주)
- 모듈: M01 Timing(김한승) · M02 Promotion(지혜인) · M03 Opportunity(김민지) · M04 Revenue(이선진)
- 공통 AI 계층·플랫폼 통합·PM: 김한승
- 현재 단계와 결정 상태는 최신 기획안 `PROJECT_PROPOSAL.md`(v1.3)와 `docs/meetings/` 를 우선 확인
- `docs/planning/project_proposal_v1.2.md` 는 이전 버전(보존용)이며 기준 문서가 아님

## 2. 문서 참조 매핑
사용자 전역 규칙이 참조하는 문서 경로는 이 저장소에서 다음으로 대응됩니다.

| 전역 규칙 경로 | 이 저장소 |
|---|---|
| `docs/bible/PRD.md` | `PROJECT_PROPOSAL.md`, `docs/planning/project_scope.md` |
| `docs/bible/FUNCTIONS.md` | `docs/architecture/module_interfaces.md` |
| `docs/bible/TECHSTACK.md` | `docs/architecture/system_architecture.md` (기술 스택 대부분 미정) |
| `docs/frontend.md`, `docs/backend.md`, `workflows/` | 아직 없음 — 기술 스택 확정 후 작성 |

## 3. 필수 규칙
1. 팀 대상 문서와 보고는 **한국어**로 작성합니다.
2. **커밋·푸시·머지·배포는 명시적 승인 없이 하지 않습니다.**
3. **외부 라이브러리 설치, 의존성 변경은 승인 후에만** 합니다. (LLM 제공사, 임베딩, 벡터DB, 프레임워크는 미정)
4. 파일 삭제, 핵심 기능 변경, 관련 없는 파일 수정 금지.
5. API 키·비밀번호·개인정보를 코드나 문서에 넣지 않습니다. `.env` 사용, `.env.example` 에는 placeholder만.
6. 원천 데이터·대용량 파일·모델 산출물·벡터DB는 커밋하지 않습니다.

## 4. 분석 원칙
- 검색/쇼핑 클릭 **관심도 ≠ 실제 판매량**.
- 광고 키워드 경쟁도 **≠ 상품 공급 경쟁**.
- Criteo는 **무작위 배정(treatment)** 을 처치 변수로 사용. 노출(exposure) 여부 단순 비교 금지.
- The Complete Journey 쿠폰/캠페인은 **관측 데이터** — 식별 가정 없이 인과 효과로 주장 금지.
- 개별 고객의 실제 인과 유형(Persuadables 등)은 관측 불가 — 추정치로만 표현.
- 비용·광고비 데이터 없이 이익·ROAS 계산 금지.
- 국내·해외 데이터를 동일 판매자/상품/고객처럼 결합 금지.
- 추천 기준을 충족하는 상품군이 없으면 억지로 추천하지 않음.
- 모든 결과에 `result_type`(observed / estimated / simulated / assumption)과 출처·시장·기간·한계를 표기 (`docs/architecture/module_interfaces.md`).

## 5. 코드 규칙
- 모듈 디렉터리는 사람 이름이 아닌 기능 이름(`m01_timing` 등)을 사용.
- 모듈 간 직접 import 금지 — 표준 출력 계약(module_interfaces.md)을 통해서만 연동.
- 핵심 비즈니스 로직은 TDD 우선(테스트 작성 → 실패 확인 → 구현 → 통과 → 리팩터링).
- 테스트하지 않은 기능을 "검증됨"으로 보고하지 않음.
- 노트북(`notebooks/`)은 탐색용, 재사용 로직은 `src/` 로 이동.

## 6. 브랜치·커밋
`CONTRIBUTING.md` 참고.

## 7. 작업 요청 공통 양식
AI 코딩 도구에 작업을 요청할 때 사용합니다.

```text
Project: GrowthLens

담당자:
담당 모듈:
관련 GitHub Issue:
작업 목표:
수정 허용 범위:
활용 데이터 및 출처:
기대 산출물:
검증 방법:
완료 기준:
제외 범위:
```

## 8. 작업 완료 보고 양식
- 생성 및 수정 파일
- 주요 구현 내용
- 실행한 테스트 및 검증 결과 (실행하지 않았으면 그 사실과 이유)
- 분석 한계 및 미해결 사항
- 다음 작업 제안
- Git 상태
