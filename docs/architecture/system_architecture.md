# 시스템 아키텍처 (상위 설계)

- 작성일: 2026-11-25 · 상태: **[제안]** — 구현 없음

## 1. 계층 구조

```
┌─────────────────────────────────────────────────────────────┐
│ Presentation  growth-lens-frontend (프레임워크 미정)          │
│  홈 · M03 · M01 · M02 · M04 · AI Assistant · 월간 리포트        │
│  ※ 모든 화면에 출처·시장·기간·결과유형·한계 라벨               │
├─────────────────────────────────────────────────────────────┤
│ API + AI Layer  growth-lens-backend/src/ai/                   │
│  LLM(해석·서술) ← Retrieval(RAG: 문서) + Structured 결과 조회   │
│  guardrails · evaluation                                      │
├─────────────────────────────────────────────────────────────┤
│ Reporting  growth-lens-backend/src/reporting/ (월간 리포트)     │
├─────────────────────────────────────────────────────────────┤
│ Analytics  growth-lens-data/src/modules/m01~m04               │
│   → 표준 출력: growth-lens-data/outputs/metrics/<module>/       │
├─────────────────────────────────────────────────────────────┤
│ Data Layer  growth-lens-data/src/{ingestion,preprocessing,database} │
│  data/raw → data/interim → data/processed · sql/              │
└─────────────────────────────────────────────────────────────┘
        ▲ 국내: 네이버 DataLab·검색광고·KOSIS·기상청·공휴일
        ▲ 해외: Criteo Uplift · The Complete Journey
```

## 1-1. 레포 경계 (2026-11-25 결정)
| 레포 | 공개 | 책임 | 다른 레포와의 연결 |
|---|---|---|---|
| growth-lens | Public | 문서 | — |
| growth-lens-data | Private | 수집 → 전처리 → 분석 → 표준 출력 | backend가 표준 출력을 읽음 |
| growth-lens-backend | Private | API, AI 계층, 리포트 | frontend가 API 호출 |
| growth-lens-frontend | Public | 화면 | backend API만 사용 (데이터·키 직접 접근 금지) |

**data → backend 결과 전달 방식은 미정**입니다. 후보: 공유 DB 테이블 / 결과 파일(manifest) 배포 / 패키지화. DB 결정(W1)과 함께 확정합니다.

## 2. 데이터 흐름
1. **수집** (`growth-lens-data/src/ingestion`): 출처별 수집기, 원본 응답을 `growth-lens-data/data/raw/<source>/` 에 그대로 저장 + 수집 메타
2. **전처리** (`growth-lens-data/src/preprocessing`): 스키마 정규화 → `data/interim` → 분석용 `data/processed`
3. **분석** (`growth-lens-data/src/modules/*`): 모듈별 파이프라인, `growth-lens-data/sql/` 의 뷰·쿼리 활용
4. **표준 출력**: 결과 테이블 + 메타데이터(`result_type`, 출처, 기간, 한계)
5. **통합**: backend(API·AI·리포트)는 **표준 출력만** 읽고, frontend는 backend API만 호출 (모듈 내부 코드 직접 참조 금지)

## 3. 데이터 파이프라인 분리 원칙
| 파이프라인 | 출처 | 소비 모듈 | 비고 |
|---|---|---|---|
| 국내 관심도 파이프라인 | 네이버 DataLab | M01, M03 | **공통 수집기 1개** (중복 방지) |
| 국내 경쟁·시장 파이프라인 | 검색광고, KOSIS, (공급 데이터) | M03 (+M01 일부) | |
| 외부 요인 | 기상청, 공휴일 | M01 | |
| 실험 데이터 | Criteo | M02 | 해외(FR), RCT |
| 소매 거래 | Complete Journey | M04 (M02 보조) | 해외(US), 관측 |

국내·해외 파이프라인은 **조인하지 않습니다.**

## 4. 설계 결정
| ID | 결정 | 상태 | 근거 |
|---|---|---|---|
| AD-01 | 모듈은 파일 기반 표준 출력으로 통합 (초기) | 제안 | DB 결정 전에도 병렬 개발 가능, 단위 테스트 용이 |
| AD-02 | 수치 계산은 SQL/Python, LLM은 해석만 | 확정(요구사항) | 환각·계산 오류 방지 |
| AD-03 | Streamlit 미사용, FE/BE 분리 (프레임워크 미정) | 확정(2026-11-25 팀 결정) | 레포 분리 결정에 따름 |
| AD-07 | 4개 레포 운영 (메인 문서·data·backend·frontend) | 확정(2026-11-25 팀 결정) | 공개 범위 분리, 역할별 독립 개발 |
| AD-04 | 결과 유형 라벨(observed/estimated/simulated/assumption) 필수 | 확정(요구사항) | 해석 오류 방지 |
| AD-05 | 모듈 디렉터리는 기능명, 사람 이름 미사용 | 확정(요구사항) | 담당 변경에 강건 |
| AD-06 | AI 계층은 비-RAG 베이스라인 먼저 | 제안 | RAG 가치 검증 전 과투자 방지 |

## 5. 미결정 기술 스택
| 항목 | 후보(예시, 비확정) | 결정 시점 |
|---|---|---|
| 저장소/DB | 파일(Parquet/CSV) + DuckDB / PostgreSQL | W1 |
| 프론트엔드 프레임워크 | 미정 (Streamlit 제외) | W2 |
| 백엔드 API 프레임워크 | 미정 | W2 |
| data → backend 결과 전달 방식 | 공유 DB / 결과 파일 / 패키지 | W1 |
| LLM 제공사·임베딩·벡터DB·오케스트레이션 | 미정 | W2 |
| 배포 | 이전 논의에서 Docker + AWS EC2 언급 — 팀 재확인 | W1 |
| 의존성 관리 | pip / uv / poetry | 킥오프 |

> 모든 라이브러리 도입은 승인 후 진행합니다.
