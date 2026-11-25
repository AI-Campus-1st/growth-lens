# 데이터 사전 (Data Dictionary)

- 작성일: 2026-11-25
- **아래 스키마는 공식 문서 기준 "예상" 스키마입니다. 실제 데이터 로드 후 각 담당자가 타입·단위·결측·행 수를 검증하고 상태를 갱신하세요.**

## 공통 메타 필드 (모든 가공 테이블)
| 컬럼 | 타입 | 설명 |
|---|---|---|
| source | str | 데이터 출처 ID (예: `naver_datalab_shopping`) |
| market | str | `KR` / `US` / `FR` 등 |
| collected_at | datetime | 수집 일시 |
| period_start / period_end | date | 관측 기간 |

## 1. 네이버 DataLab — 검색어 트렌드 (M01, M03) [미검증]
| 컬럼(가공 후) | 타입 | 설명 |
|---|---|---|
| keyword_group | str | 요청 시 지정한 키워드 그룹명 |
| period | date | 집계 구간 시작일 (일/주/월) |
| ratio | float | 요청 내 상대 지수 (0~100) |
| time_unit | str | date / week / month |
| request_id | str | 같은 정규화 기준을 공유하는 요청 식별자 |
| device / gender / age | str | 필터 조건 (적용 시) |

## 2. 네이버 DataLab — 쇼핑인사이트 (M01, M03) [미검증]
| 컬럼 | 타입 | 설명 |
|---|---|---|
| category_id | str | 쇼핑 분야 코드 |
| category_name | str | 분야명 |
| keyword | str | 분야 내 키워드 (키워드 단위 조회 시) |
| period | date | 집계 구간 |
| ratio | float | 클릭 상대 지수 |
| group_type / group_value | str | 성별·연령·기기 등 세부 분류 |
| request_id | str | 정규화 기준 식별자 |

## 3. 네이버 검색광고 키워드 도구 (M03) [미검증]
| 컬럼 | 타입 | 설명 |
|---|---|---|
| keyword | str | 키워드 |
| monthly_pc_qc / monthly_mobile_qc | int | 월간 검색수 (구간값 표기 가능성 확인 필요) |
| comp_idx | str | 광고 경쟁 정도 — **광고 경쟁**이며 공급 경쟁 아님 |
| 기타 | — | 실제 응답 필드 확인 후 추가 |

## 4. 공급 경쟁 지표 (M03) [미검증 — 확보 여부 미정]
| 컬럼 | 설명 |
|---|---|
| product_count | 검색 결과 상품 수 |
| seller_count | 판매처 수 |
| price_p25 / p50 / p75 | 가격 분포 |

## 5. Criteo Uplift (M02) [미검증]
| 컬럼 | 설명 |
|---|---|
| f0 ~ f11 | 익명화된 피처 |
| treatment | **무작위 배정 여부 — 주 처치 변수** |
| exposure | 실제 노출 여부 — 처치 변수로 단순 비교 금지 |
| visit | 방문 여부 |
| conversion | 전환 여부 |

## 6. The Complete Journey (M04, M02 보조) [미검증]
| 테이블 | 주요 컬럼(예상) |
|---|---|
| transactions | household_id, store_id, basket_id, product_id, quantity, sales_value, retail_disc, coupon_disc, coupon_match_disc, week, transaction_timestamp |
| products | product_id, manufacturer_id, department, brand, product_category, product_type, package_size |
| promotions | product_id, store_id, display_location, mailer_location, week |
| campaigns | campaign_id, household_id |
| campaign_descriptions | campaign_id, campaign_type, start_date, end_date |
| coupons | coupon_upc, product_id, campaign_id |
| coupon_redemptions | household_id, coupon_upc, campaign_id, redemption_date |
| demographics | household_id, age, income, home_ownership, marital_status, household_size 등 |

- 할인 컬럼의 부호(음수 표기 여부)와 순매출 계산식은 로드 후 확인 필요.

## 7. M01 외부 요인 [미검증]
| 테이블 | 컬럼 |
|---|---|
| weather_daily | date, region, avg_temp, min_temp, max_temp, precipitation |
| holidays | date, name, is_holiday |
| season_events | event_name, start_date, end_date, industry (팀 정의) |
