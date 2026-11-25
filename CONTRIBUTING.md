# 협업 가이드 (Git & GitHub)

> 제안 상태 — 킥오프에서 팀 합의 후 확정합니다. 원격 저장소의 브랜치 보호 설정은 승인 없이 변경하지 않습니다.

## 0. 레포별 작업 위치

| 레포 | 공개 | 주 작업 브랜치 |
|---|---|---|
| [growth-lens](https://github.com/AI-Campus-1st/growth-lens) | Public | `docs/*` (기획·주간일지·회의록) |
| [growth-lens-data](https://github.com/AI-Campus-1st/growth-lens-data) | Private | `feature/m01-*` ~ `feature/m04-*`, `fix/*` |
| [growth-lens-backend](https://github.com/AI-Campus-1st/growth-lens-backend) | Private | `feature/ai-*`, `feature/platform-*`, `fix/*` |
| [growth-lens-frontend](https://github.com/AI-Campus-1st/growth-lens-frontend) | Public | `feature/platform-*`, `fix/*` |

- 모든 레포에 `main`(안정)·`develop`(통합) 브랜치를 둡니다.
- 이 문서가 **전 레포 공통 규칙**입니다. 코드 레포의 README는 이 문서를 링크합니다.
- 레포를 넘는 변경(예: 표준 출력 스키마 변경 → backend 영향)은 메인 레포 이슈로 먼저 합의합니다.
- **Public 레포에는 API 키, 데이터, 내부 분석 원본을 절대 올리지 않습니다.**

## 1. 브랜치 전략

| 브랜치 | 용도 | 병합 대상 |
|---|---|---|
| `main` | 발표·배포 가능한 안정 버전 | — |
| `develop` | 통합 브랜치 | `main` (마일스톤마다) |
| `feature/m01-*` | Marketing Timing (김한승) | `develop` |
| `feature/m02-*` | Promotion Intelligence (지혜인) | `develop` |
| `feature/m03-*` | Market Opportunity (김민지) | `develop` |
| `feature/m04-*` | Revenue Analysis (이선진) | `develop` |
| `feature/ai-*` | AI 계층 (김한승) | `develop` |
| `feature/platform-*` | 플랫폼/앱 (김한승) | `develop` |
| `docs/*` | 문서 | `develop` |
| `fix/*` | 버그 수정 | `develop` (긴급 시 `main`) |

예: `feature/m03-opportunity-score`, `docs/data-feasibility`

## 2. 작업 흐름 (Issue 기반)
1. GitHub Issue 생성 (모듈 라벨: `m01`~`m04`, `ai`, `platform`, `docs`, `data`)
2. `develop` 에서 브랜치 생성 — 브랜치명에 이슈 번호 포함 권장 (`feature/m01-12-stl`)
3. 작은 단위로 커밋
4. PR 생성 → 최소 1인 리뷰 → `develop` 병합 (Squash merge 권장)
5. PR 본문: 목적 / 변경 내용 / 검증 방법 / 데이터 출처·한계 / 관련 이슈(`Closes #12`)

### 작업 명령 예시
`develop` 브랜치와 원격 저장소가 생성된 이후 사용합니다. (2026-11-25 현재 로컬 `git init`만 완료, 원격·`develop` 없음)

```bash
git fetch origin
git switch develop
git pull --ff-only origin develop
git switch -c feature/m03-data-poc

# 작업 및 검증 후
git status
git diff
git add <검토한-파일>
git commit -m "feat(m03): 데이터 출처 검증"
git push -u origin feature/m03-data-poc
```

## 3. 커밋 메시지

```
<type>(<scope>): <요약>

feat(m01): STL 분해 기반 계절성 분석 추가
feat(m02): T-Learner 업리프트 모델 학습 파이프라인
feat(m03): Opportunity Score 산식 v0 구현
feat(m04): 주간 매출 추이 집계 SQL
feat(ai): 근거 인용 브리핑 프롬프트 초안
feat(platform): 홈 대시보드 화면 골격
docs: 데이터 타당성 체크리스트 갱신
fix: 쇼핑인사이트 날짜 파싱 오류 수정
test: Opportunity Score 경계값 테스트
refactor: 공통 로깅 유틸 분리
```

## 4. 리뷰 체크리스트
- [ ] `.env`, API 키, 원천 데이터, 대용량 파일이 포함되지 않았는가
- [ ] 결과에 출처·시장·기간·result_type·한계가 표기되었는가
- [ ] 모듈 출력이 `docs/architecture/module_interfaces.md` 계약을 따르는가
- [ ] 테스트가 있고 통과하는가 (없으면 이유 명시)
- [ ] 관련 없는 파일을 수정하지 않았는가

## 5. 보안 — 비밀값·데이터 유출 방지 (필수)

### 5-1. 클론 직후 1회: 커밋 차단 훅 활성화
모든 레포에 같은 `.githooks/pre-commit`이 있습니다. **클론할 때마다 각 레포에서 1회** 실행하세요.
```bash
git config core.hooksPath .githooks
```
커밋 직전에 다음을 검사하고, 걸리면 커밋을 막습니다. (외부 라이브러리 불필요)
- 금지 파일명: `.env`, `*.env`, `*.pem`, `*.key`, `credentials*.json`, `*service-account*.json` 등 (`.env.example` 제외)
- 추가된 줄의 키 패턴: GitHub 토큰, `sk-`(LLM API 키), AWS·Google 키, 개인키 블록
- `SECRET`/`TOKEN`/`PASSWORD`/`API_KEY` 등에 16자 이상 값 할당, 접속 문자열 안의 비밀번호
- 5MB 초과 파일 (`GL_MAX_FILE_KB`로 조정)

`git commit --no-verify`(훅 우회)는 팀 합의 없이 사용하지 않습니다.

### 5-2. 규칙
| 항목 | 규칙 |
|---|---|
| 실제 키 | 각 레포의 `.env`에만 저장. 메신저·이슈·PR 본문에도 붙여넣지 않음 |
| `.env.example` | `your_...` placeholder만. 새 변수를 추가하면 함께 갱신 |
| Public 레포 | `growth-lens`, `growth-lens-frontend`에는 데이터·키·내부 분석 원본 금지. 프론트엔드 환경변수는 브라우저에 노출됨 |
| 데이터 | 원천·가공 데이터, 모델, 벡터 인덱스는 커밋 금지. `data/sample/`은 라이선스 확인 후 소량만 |
| 노트북 | 커밋 전 출력 셀 정리 (출력에 데이터·키가 남을 수 있음) |
| `.gitignore` | 공통 비밀값 블록은 4개 레포 동일. 수정 시 전 레포 함께 변경 |

### 5-3. 비밀값을 커밋·push했을 때
1. **즉시 해당 키를 폐기·재발급**합니다(네이버·KOSIS·LLM 콘솔, GitHub 토큰 설정). 커밋을 지워도 이미 복제·캐시됐을 수 있으므로 **폐기가 최우선**입니다.
2. 팀 채널에 공유합니다(키 값 자체는 공유하지 않음).
3. 이력 정리가 필요하면 PM과 상의 후 진행합니다. 강제 push는 팀원 로컬 이력에 영향을 줍니다.

### 5-4. GitHub 설정 (레포 관리자)
- Settings → Advanced Security → **Secret scanning·Push protection** 활성화 권장
  - Public 레포는 무료로 사용 가능한 것으로 알려져 있습니다. Private 레포는 유료 플랜이 필요할 수 있으니 확인이 필요합니다.

## 6. 노트북 규칙
- 커밋 전 출력 셀 정리(대용량 출력·개인정보 제거)
- 파일명: `NN_설명.ipynb` (예: `01_eda_search_trend.ipynb`)
- 충돌 방지를 위해 각자 모듈 폴더에서만 작업
