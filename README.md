# Watchback — 개인 기여 포트폴리오

누적된 Dependabot 취약점 알림에 EPSS·KEV·GHSA 외부 위험 신호를 결합해 다시 검토할 후보와 근거를 제시하는 팀 프로젝트입니다. **이 저장소는 Microsoft Data School 팀 프로젝트에서 ShinDuck의 데이터 분석·정책·계약, 공동 담당 파이프라인 참고본과 프로젝트 이해를 위한 팀 공통 문서를 정리한 개인 포트폴리오**입니다. 전체 서비스나 팀원 구현을 개인 성과로 주장하지 않습니다.

## 어떤 프로젝트인가요?

Watchback은 해결되지 않고 쌓인 Dependabot Alert를 관찰하다가 **위험이나 패치 가능성이 달라진 항목을 다시 검토할 수 있도록 후보와 근거를 제공하는 서비스**입니다. DE가 GHSA·EPSS·KEV 외부 데이터를 수집·정제하고, DS가 위험 변화 후보를 생성하면, Web이 조직의 실제 Alert와 연결해 검토·조치 화면을 제공합니다.

![Watchback 팀 공통 아키텍처](docs/architecture/arch-overview.png)

위 그림은 팀 공통 설계 자료입니다. 전체 서비스의 설계 범위를 보여주며 개인 구현 범위나 기능 완료를 의미하지 않습니다.

## 프로젝트 이해를 위한 읽는 순서

| 순서 | 팀 공통 문서 | 알 수 있는 내용 |
| --- | --- | --- |
| 1 | [PRD](docs/product/prd.md) | 문제·사용자·MVP·성공 지표와 제약 |
| 2 | [기술 아키텍처](docs/architecture/technical-architecture.md) · [구조도 안내](docs/architecture/README.md) | 구성요소·데이터 흐름·DE/DS/Web 책임 경계 |
| 3 | [제품 백로그](docs/product/product-backlog.md) | Epic·유저스토리·수용 기준·우선순위 |
| 4 | [Sprint 1 범위](docs/product/sp1-backlog.md) | 목표·팀 간 의존성·작업 구분 |
| 5 | [DS 백로그](docs/product/sp1-ds-backlog.md) · [DE](docs/product/sp1-de-backlog.md) · [Web](docs/product/sp1-web-backlog.md) | 각 파트의 세부 작업과 DS가 맡은 위치 |
| 6 | [개인 DS 기여](docs/contributions/ds-issues-and-deliverables.md) | 담당 이슈·실제 개인 작업물·연결 PR |

`docs/product`와 `docs/architecture`는 **팀 공통 참고 자료**입니다. 원본 출처를 문서마다 표시했으며 본인의 단독 작성물로 주장하지 않습니다. 원본의 계획·KPI·완료 기준은 당시 목표이고 실제 달성 여부는 별도입니다. 2026-09-21에 본문과 문서 간 링크를 확인해 이관했습니다.

## 담당 범위

- EPSS D-1/D-7/D-30/D-90 변화량 및 임계값별 후보 수 비교
- KEV 등재와 EPSS·GHSA 보조 신호의 overlap, precision-like, lift 분석
- Gate → Trigger → 축별 점수 → 우선순위 → 근거 생성 정책 설계
- Silver 분석 입력과 Gold 후보·근거 출력 계약 정의

기술: Python, PySpark, Spark SQL, Microsoft Fabric/Synapse PySpark Notebook, Markdown 데이터 계약. 분석 신호는 FIRST EPSS, CISA KEV, GitHub Advisory(GHSA)입니다.

## DS 담당 이슈와 작업물

[DS 이슈별 기여 상세](docs/contributions/ds-issues-and-deliverables.md)에서 담당 이슈 6건의 목적·완료 기준·상태와 개인 작업물을 함께 볼 수 있습니다.

| 이슈 | 담당 작업 | 개인 작업물 |
| --- | --- | --- |
| #65 · DS-01 | Silver 입력·품질 기준 | [입력 계약](docs/contracts/silver/silver-ds-analysis-input-contract.md) |
| #66 · DS-01 | 후보 결과·근거 필드 | [출력 계약](docs/contracts/gold/gold-rereview-candidate-output-contract.md) |
| #68 · DS-02 | EPSS·KEV 후보 신호 분석 | [보고서](docs/analysis/risk-signal-change-analysis.md) · [노트북](fabric/workspace/analysis/ds02_risk_change_analysis.ipynb) |
| #73 · DS-04 | 정책·선정 상위 작업, 공동 담당 | 하위 #74 정책과 #75 구현의 연결 |
| #74 · DS-04 | 포함·제외·우선순위 정책 | [후보 선정 정책](docs/contracts/gold/candidate-selection-policy.md) |
| #75 · DS-04 | 후보 생성 Pipeline, 공동 담당 | [Pipeline 노트북](fabric/workspace/analysis/ds04_gold_rereview_candidate_validation_v3.ipynb), 원본 작성 Justmagic46 |

#74는 완료, 나머지 이슈는 Open입니다(2026-09-21 확인). 본인 작성 작업물과 Justmagic46 작성 Pipeline 참고본을 구분해 표시합니다.

## 먼저 볼 자료

| 자료 | 내용 | 원본 상태 |
| --- | --- | --- |
| [분석 보고서](docs/analysis/risk-signal-change-analysis.md) | D-30 기준 채택 근거, 임계값 비교, KEV 동반성 | PR #129 미병합 |
| [분석 노트북](fabric/workspace/analysis/ds02_risk_change_analysis.ipynb) | EPSS·KEV·GHSA 분석을 구성하는 코드 | PR #129 미병합 |
| [후보 선정 정책](docs/contracts/gold/candidate-selection-policy.md) | 제외 조건, 점수, P0~P3, 근거 보존 | PR #116 병합 및 본인 후속 수정 |
| [Silver 입력 계약](docs/contracts/silver/silver-ds-analysis-input-contract.md) | 필드·키·시점·품질 기준 | PR #131 초안·미병합 |
| [Gold 출력 계약](docs/contracts/gold/gold-rereview-candidate-output-contract.md) | 배치·후보·근거 스키마 | PR #131 초안·미병합 |

## 공동 담당 파이프라인 참고본

[후보 생성 Pipeline 노트북](fabric/workspace/analysis/ds04_gold_rereview_candidate_validation_v3.ipynb)은 공동 담당 이슈 #75에 연결된 [PR #140](https://github.com/dt4-proj3-team3/watchback/pull/140)의 작업물입니다. 원본 구현 작성자는 **Justmagic46**이며, ShinDuck의 단독 작성 코드로 표기하지 않습니다. 원본 커밋은 `bd23e02aed658b52ff8d717908712d3d1e001b12`입니다.

룰 평가 → 후보·근거 생성 → 30일 중복 처리 → Gold 적재·무결성 검증 흐름을 담습니다. 별도의 `rules.rule_queries_v1` 모듈과 Silver 데이터가 필요하며 이 참고본만으로 실행되지 않습니다. 실행 출력·횟수·메타데이터를 제거하고 팀 Lakehouse 이름을 예시로 치환했습니다. 코드 셀 7개의 Python 문법과 JSON 구조를 확인했으며 전체 실행은 하지 않았습니다. `WRITE_OUTPUT_TABLES=False`를 유지합니다.

## 주요 분석 결과

원본 분석 문서에 기록된 2026-08-17 기준 결과입니다. 이번 이관 과정에서 원천 데이터로 재산출하지 않았습니다.

- EPSS 최신 snapshot 360,396개 CVE, 518개 일별 snapshot 분석 범위
- 전체 EPSS 기준 D-30 Top 5% 진입 50건, 그중 7일 유지 48건
- D-30 대비 5배 이상이면서 0.05 이상 증가한 후보 64건
- GHSA·EPSS·KEV 조인 범위에서 급상승 후보 14건 중 KEV 동반 2건, 기록된 lift 33.14

전체 EPSS 후보 수와 조인 후 후보 수는 모집단이 달라 직접 혼용하지 않습니다. KEV는 실제 공격의 정답 라벨이 아니며, precision-like/lift는 동반성 지표입니다. 실제 고객 Alert 탐지 정확도나 실서비스 효과로 해석하지 않습니다.

## 설계에서 고려한 점

단순 배율 상승은 작은 기준값의 잡음을 확대하므로 절대 증가량을 함께 사용했습니다. 위험 변화(A)와 조치 가능성 변화(C)를 후보 Trigger로 두고, 현재 심각도(B)는 보정에 사용합니다. 동일 축에서는 MAX, 축 간에는 SUM으로 중복 가산을 줄이며, 철회된 GHSA는 KEV 특례보다 먼저 제외합니다.

## 실행 환경과 검증 범위

노트북은 Spark 세션과 display를 제공하는 Fabric/Synapse PySpark 환경을 전제로 합니다. EPSS 일별 파일, GHSA snapshot, silver_kev 테이블은 별도로 준비해야 합니다. 원천 데이터와 팀 Lakehouse 연결 정보는 포함하지 않습니다. 노트북의 경로와 테이블 이름을 자신의 입력에 맞춰 설정한 뒤 셀 순서대로 실행합니다. 테이블 쓰기 대신 임시 View를 생성합니다.

이관 시 JSON 구조와 코드 셀 11개의 Python 문법을 확인하고 실행 출력·실행 횟수·셀 메타데이터를 비웠습니다. Fabric 전체 실행은 하지 않았습니다.

## 출처와 이관 범위

원본 팀 저장소: [dt4-proj3-team3/watchback](https://github.com/dt4-proj3-team3/watchback) (private, 접근 권한 필요).

- 후보 정책: 본인 커밋 `6cdfd7c75a3f488a3a49f3749e8bf1dd6df6b5a7`, [PR #116](https://github.com/dt4-proj3-team3/watchback/pull/116)
- 분석 문서·노트북: 본인 커밋 `7158cbf1bc93bb016a3a505f7f005f9306141481`, [PR #129](https://github.com/dt4-proj3-team3/watchback/pull/129)
- 입력·출력 계약: 본인 커밋 `28b7e9cc190de1bceb4f0b247057ebe9512c1684`, [PR #131](https://github.com/dt4-proj3-team3/watchback/pull/131)

공동 담당 후보 생성 노트북 1개는 원본 작성자를 명시해 포함했습니다. 프로젝트 맥락을 위한 팀 공통 제품 문서 6개, 기술 아키텍처 문서와 구조도도 출처를 표시해 포함했습니다. 그 외 팀원 애플리케이션·수집 파이프라인·인프라·운영 문서·원천 데이터·Git 이력은 제외했습니다. README는 포트폴리오용으로 새로 정리했으며 기존 작성물과 구분합니다.

## 남은 한계

실제 사용자 판단 시점 대신 D-30 합성 baseline을 사용한 분석입니다. 계약 문서는 초안이며 실제 출력과 정합성 검증이 남아 있습니다. 예를 들어 Gold 문서의 EPSS Trigger 코드 표기와 패치 비교 시점 예시는 정책·구현과 추가 대조가 필요합니다. 이 저장소에 이관한 자료만으로 전체 탐지 Job과 웹 서비스가 구현·검증됐다고 주장하지 않습니다.

현재 private이며 공개 전환은 소유자의 확인 후 진행합니다.
