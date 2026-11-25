# 레포·디렉터리 구조

- 작성일: 2026-11-25 (멀티 레포 구조로 개정)
- 결정: 4개 레포 운영, Streamlit 미사용 — 2026-11-25 팀 결정
- 빈 디렉터리는 `.gitkeep` 으로만 추적하며, 빈 Python 모듈 파일은 만들지 않았습니다.

## 1. 로컬 작업공간

```text
growthlens/                      # 작업공간 (git 아님)
├── growth-lens/                 # 메인 (Public)
├── growth-lens-data/            # 데이터 파이프라인 (Private)
├── growth-lens-backend/         # 백엔드·AI (Private)
└── growth-lens-frontend/        # 프론트엔드 (Public)
```

## 2. growth-lens (메인, Public) — 문서 전용

```text
growth-lens/
├── README.md / PROJECT_PROPOSAL.md / CONTRIBUTING.md / AGENTS.md / CLAUDE.md
├── .gitignore
└── docs/
    ├── planning/        기획안 v1.2(보존), 범위, R&R, 로드맵, 리스크, 백로그
    ├── data/            출처, 데이터 사전, 타당성 체크리스트
    ├── architecture/    시스템, 레포·디렉터리, 모듈 인터페이스
    ├── ai/              AI 아키텍처, LLM·RAG 전략, 평가 계획
    ├── meetings/        회의록 (YYYY-MM-DD_주제.md)
    ├── weekly/          주간일지 (YYYY-Www.md)
    ├── reports/         모듈별 방법론·결과 보고서
    └── experiments/     (미생성) 실험 설계·평가 문서
```

## 3. growth-lens-data (Private) — 수집·전처리·분석

```text
growth-lens-data/
├── README.md / AGENTS.md / CLAUDE.md / .gitignore / .env.example / pyproject.toml
├── config/              카테고리·키워드 사전, 파라미터 (비밀값 금지)
├── data/                ※ sample/ 외 전부 git 제외
│   ├── raw/ · interim/ · processed/ · sample/
├── notebooks/m01_timing · m02_promotion · m03_opportunity · m04_revenue
├── src/
│   ├── common/          설정 로딩, 로깅, 표준 출력 스키마
│   ├── ingestion/       출처별 수집기 (네이버 공통 수집기 포함)
│   ├── preprocessing/   정제·정규화
│   ├── database/        DB 연결 (DB 결정 후)
│   └── modules/m01_timing · m02_promotion · m03_opportunity · m04_revenue
├── sql/schema · views · analysis
├── models/              학습 모델 (git 제외)
├── tests/unit · integration · data_quality
└── outputs/figures · metrics · reports   (git 제외)
```

## 4. growth-lens-backend (Private) — API·AI·리포트

```text
growth-lens-backend/
├── README.md / AGENTS.md / CLAUDE.md / .gitignore / .env.example
├── src/
│   ├── ai/llm · rag · prompts · retrieval · evaluation · guardrails
│   └── reporting/       월간 리포트 조립
├── data/knowledge/      RAG 지식 문서 승인본 (git 제외)
└── tests/ai/            AI 평가셋·테스트
```
API 서버 코드 위치·프레임워크는 미정입니다(결정 후 `src/api/` 등 추가).

## 5. growth-lens-frontend (Public) — 화면

```text
growth-lens-frontend/
├── README.md / AGENTS.md / CLAUDE.md / .gitignore / .env.example
└── src/pages · components · assets
```
프레임워크 미정(Streamlit 미사용). 프레임워크 확정 시 해당 관례에 맞게 구조를 조정합니다.

## 6. 소유권

| 경로 | 1차 소유 |
|---|---|
| `growth-lens-data/src/modules/m01_timing`, `notebooks/m01_timing` | 김한승 |
| `growth-lens-data/src/modules/m02_promotion`, `notebooks/m02_promotion` | 지혜인 |
| `growth-lens-data/src/modules/m03_opportunity`, `notebooks/m03_opportunity` | 김민지 |
| `growth-lens-data/src/modules/m04_revenue`, `notebooks/m04_revenue` | 이선진 |
| `growth-lens-data/src/ingestion` 네이버 공통 수집기 | 김민지 (제안) |
| `growth-lens-data/src/common` | 김한승 |
| `growth-lens-backend/` 전체 | 김한승 |
| `growth-lens-frontend/` 골격 | 김한승 |
| 모듈별 화면 | 미정 — 모듈 담당자가 FE 코드를 직접 작성할지, 차트 사양·데이터만 제공할지 결정 필요 |
| `growth-lens/docs/` | 전원 (문서별 담당) |
