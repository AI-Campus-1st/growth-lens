# 초기 백로그 (팀원별)

- 작성일: 2026-11-25 · 상태: **[제안]** — GitHub Issue로 등록 전 팀 검토
- 우선순위: P0(이번 주 필수) · P1(W1~W2) · P2(stretch)

## 김한승 — M01 / PM / 플랫폼 / AI
| 우선 | 작업 | 산출물 |
|---|---|---|
| P0 | 킥오프 진행, 역할·범위·일정 합의 기록 | `docs/meetings/2026-11-25_kickoff.md` |
| P0 | GitHub 원격 저장소·이슈 라벨·브랜치 규칙 설정 (승인 후) | 저장소 |
| P0 | 12/1 기획 발표 자료 | 발표 자료 |
| P0 | 네이버 DataLab 키 발급, M01 후보 키워드/카테고리 정의 | 키워드 목록 |
| P1 | M01: STL 계절성 + 검색↔쇼핑 클릭 CCF 분석 | 노트북, 표준 출력 |
| P1 | DB·앱 프레임워크 결정 기록 | ADR 문서 |
| P1 | 표준 출력 계약 검증 테스트 | `tests/integration` |
| P1 | 플랫폼 골격 (홈 + 모듈 페이지 라우팅 + 출처 라벨 컴포넌트) | `app/` |
| P1 | 비-RAG AI 브리핑 베이스라인 + 평가셋 구조 | `src/ai`, `tests/ai` |
| P2 | M01 예측·변화점 탐지·외부 요인 | — |
| P2 | RAG 비교 실험 | 평가 보고 |

## 지혜인 — M02 Promotion Intelligence
| 우선 | 작업 | 산출물 |
|---|---|---|
| P0 | Criteo Uplift 데이터 다운로드·라이선스·스키마·크기 확인 | 체크리스트 갱신 |
| P0 | treatment/control 비율, 전환율 기초 통계 (randomization check) | 노트북 |
| P1 | ATE 추정 (전환·방문) + 신뢰구간 | 노트북 |
| P1 | S-Learner, T-Learner + 독립 holdout 평가 (AUUC, Qini, Uplift@K) | 표준 출력 |
| P1 | 전환 확률 기반 vs 업리프트 기반 타게팅 정책 비교 | 결과표 |
| P1 | M02 화면 컴포넌트 | `app/components` |
| P2 | 고급 업리프트 모델 (승인 후 라이브러리 도입) | — |

## 김민지 — M03 Market Opportunity
| 우선 | 작업 | 산출물 |
|---|---|---|
| P0 | 쇼핑인사이트 카테고리·세부 분류(성별·연령·기기) 수집 가능 범위 확인 | 체크리스트 갱신 |
| P0 | **공급 경쟁 데이터(상품수·판매자수·가격) 확보 경로 검증** | 범위 A/B 판단 근거 |
| P0 | 검색광고 키워드 도구 API 접근 및 제공 필드 확인 | 체크리스트 갱신 |
| P1 | 네이버 DataLab 공통 수집기 (M01과 공유) | `src/ingestion` |
| P1 | DETECT: MoM/YoY, 급상승 탐지 | 노트북 |
| P1 | DIAGNOSE: Opportunity Score v0 정의·민감도 검토 | 지표 정의 문서 |
| P1 | RECOMMEND: 추천 적격 조건(미충족 시 추천 없음) | 표준 출력 |
| P1 | M03 화면 컴포넌트 | `app/components` |

## 이선진 — M04 Revenue Analysis
| 우선 | 작업 | 산출물 |
|---|---|---|
| P0 | The Complete Journey 로딩 경로 검증(전체 transactions 포함), 라이선스 확인 | 체크리스트 갱신 |
| P0 | 테이블 스키마·기간·행 수 확인, 데이터 사전 작성 | `data_dictionary.md` |
| P1 | 상품별 매출·기여도(파레토), 주/월 추이 SQL | `sql/analysis` |
| P1 | 할인·쿠폰 금액 분석, 프로모션 기간 성과 비교(기술적 비교) | 노트북 |
| P1 | M04 화면 컴포넌트 | `app/components` |
| P2 | 사용자 입력 원가 기반 이익 시나리오 (simulated 라벨) | — |

## 공통 (전원)
- P0: `.env.example` 기준 개인 `.env` 구성, Python 버전 합의
- P1: 모듈별 AI 평가 질문 10개 내외 + 정답 근거 작성 (수량은 합의)
- P1: 모듈 방법론·한계 문서 (`docs/reports/`)
