# 디렉터리 구조

- 작성일: 2026-11-25
- 빈 디렉터리는 `.gitkeep` 으로만 추적하며, 빈 Python 모듈 파일은 만들지 않았습니다.

```text
growthlens/
├── README.md / PROJECT_PROPOSAL.md / AGENTS.md / CLAUDE.md / CONTRIBUTING.md
├── .gitignore / .env.example / pyproject.toml
├── docs/
│   ├── planning/       기획안, 범위, R&R, 로드맵, 리스크, 백로그
│   ├── meetings/       회의록 (YYYY-MM-DD_주제.md)
│   ├── data/           출처, 데이터 사전, 타당성 체크리스트
│   ├── architecture/   시스템, 디렉터리, 모듈 인터페이스
│   ├── ai/             AI 아키텍처, LLM·RAG 전략, 평가 계획
│   ├── reports/        모듈별 방법론·결과 보고서 (승인본 → RAG 후보)
│   └── experiments/    (미생성) 실험 설계·평가 문서 — 첫 실험 설계 시 생성
├── config/             카테고리·키워드 사전, 파라미터 (비밀값 금지)
├── data/               ※ sample/ 외 전부 git 제외
│   ├── raw/            원본 그대로
│   ├── interim/        정제 중간
│   ├── processed/      분석 입력
│   ├── sample/         재배포 허용된 소량 샘플만
│   └── knowledge/      RAG 문서 원본 (승인본)
├── notebooks/m01_timing · m02_promotion · m03_opportunity · m04_revenue
├── src/
│   ├── common/         설정 로딩, 로깅, 표준 출력 스키마
│   ├── ingestion/      출처별 수집기 (네이버 공통 수집기 포함)
│   ├── preprocessing/  정제·정규화
│   ├── database/       DB 연결 (DB 결정 후)
│   ├── modules/m01_timing · m02_promotion · m03_opportunity · m04_revenue
│   ├── ai/llm · rag · prompts · retrieval · evaluation · guardrails
│   └── reporting/      월간 리포트 조립
├── sql/schema · views · analysis
├── models/             학습 모델 (git 제외)
├── app/pages · components · assets
├── tests/unit · integration · data_quality · ai
├── outputs/figures · metrics · reports   (git 제외)
└── experiments/        (미생성·위치 미정) 실험·평가 코드 — 현재는 src/ai/evaluation, tests/ai 로 계획
```

## 소유권
| 경로 | 1차 소유 |
|---|---|
| `src/modules/m01_timing`, `notebooks/m01_timing` | 김한승 |
| `src/modules/m02_promotion`, `notebooks/m02_promotion` | 지혜인 |
| `src/modules/m03_opportunity`, `notebooks/m03_opportunity` | 김민지 |
| `src/modules/m04_revenue`, `notebooks/m04_revenue` | 이선진 |
| `src/ingestion` 네이버 공통 수집기 | 김민지 (제안) |
| `src/ai`, `src/reporting`, `app/` 골격, `src/common` | 김한승 |
| `app/components/<module>_*` | 각 모듈 담당 |
