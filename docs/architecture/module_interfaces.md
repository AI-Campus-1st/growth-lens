# 모듈 인터페이스 (표준 출력 계약)

- 작성일: 2026-11-25 · 상태: **v0 제안** — W1에 v1 확정
- 목적: 모듈을 독립 개발·테스트하고, 앱·리포트·AI가 **동일한 형식**으로 결과를 소비하게 함

## 1. 출력 위치
```
outputs/metrics/<module_id>/<run_id>/
├── manifest.json      # 메타데이터 (아래 스키마)
├── <result_name>.csv  # 결과 테이블 (Parquet 사용 여부는 W1 결정)
└── figures/           # (선택) 정적 이미지
```

## 2. manifest.json 스키마 (제안)
```json
{
  "module_id": "m03_opportunity",
  "run_id": "2026-12-10T09-00",
  "version": "0.1.0",
  "owner": "김민지",
  "generated_at": "ISO-8601",
  "results": [
    {
      "name": "opportunity_shortlist",
      "file": "opportunity_shortlist.csv",
      "result_type": "estimated",
      "description": "월별 기회 상품군 후보",
      "metrics": ["opportunity_score"],
      "unit_notes": "ratio는 상대 지수(0~100)"
    }
  ],
  "data_sources": [
    {"source_id": "naver_datalab_shopping", "market": "KR",
     "period_start": "YYYY-MM-DD", "period_end": "YYYY-MM-DD"}
  ],
  "limitations": ["관심도는 판매량이 아님", "광고 경쟁도 ≠ 공급 경쟁"],
  "validation": {"method": "rolling backtest 등", "status": "passed|failed|not_run"}
}
```

`result_type` 값:
| 값 | 의미 | 예 |
|---|---|---|
| `observed` | 데이터에서 직접 관측·집계 | CJ 주간 매출 합계 |
| `estimated` | 통계·모델 추정 | ATE, uplift, 예측값, Opportunity Score |
| `simulated` | 가정 기반 시나리오 | 원가 입력 이익 시나리오 |
| `assumption` | 사용자 입력 가정 | 마진율 |

## 2-1. 결과 테이블 공통 컬럼 후보 (README §14와 동일, 미확정)
long-format 결과 테이블을 쓸 경우의 후보 컬럼입니다. manifest 방식과의 역할 분담은 W1에 확정합니다.

| 컬럼 | 설명 |
|---|---|
| module_id | 모듈 ID (예: `m03_opportunity`) |
| metric_id | 지표 ID |
| value | 값 |
| unit | 단위 (상대지수는 `ratio_index` 등으로 명시) |
| period_start / period_end | 관측 기간 |
| as_of | 산출 기준 시점 |
| entity_id | 상품군·상품·세그먼트 ID |
| source_id | 데이터 출처 ID |
| market | KR / US / FR |
| result_type | observed / estimated / simulated / assumption |
| method_version | 방법론·모델 버전 |
| limitations | 한계 |

## 3. 모듈별 주요 출력 (초안)
| 모듈 | 결과 | result_type | 시장 |
|---|---|---|---|
| M01 | seasonality_profile (카테고리×월 계절 지수) | estimated | KR |
| M01 | lead_lag (검색→클릭 최적 시차, 상관) | estimated | KR |
| M01 | prep_schedule (권장 준비 시작 주차) | estimated | KR |
| M01 | (stretch) forecast, anomalies | estimated | KR |
| M02 | ate_summary (처치효과·신뢰구간) | estimated | FR(Criteo) |
| M02 | uplift_eval (AUUC, Qini, Uplift@K) | estimated | FR |
| M02 | policy_comparison (전환 기반 vs 업리프트 기반) | estimated | FR |
| M03 | trend_signals (MoM, YoY, 급상승) | observed/estimated | KR |
| M03 | opportunity_scores (구성 지표 포함) | estimated | KR |
| M03 | shortlist (0~3개, 적격 조건 미충족 시 0개) | estimated | KR |
| M04 | product_revenue (매출·기여도) | observed | US(CJ) |
| M04 | sales_trend (주/월) | observed | US |
| M04 | promo_period_comparison | observed | US |
| M04 | (선택) profit_scenario | simulated | US |

## 4. 모듈 실행 인터페이스 (제안, 구현 전)
- 각 모듈은 `run(config) -> manifest 경로` 형태의 단일 진입점 제공
- 입력: `data/processed` + `config/<module>.yaml` (형식 미정)
- 모듈 간 직접 import 금지 — M04가 M02 결과를 쓸 때도 `outputs/metrics/m02_promotion/` 를 읽음

## 5. 계약 검증
- `tests/integration/` 에서 manifest 필수 필드, result_type 허용값, 출처·기간 존재 여부 검사 (구현 예정)
