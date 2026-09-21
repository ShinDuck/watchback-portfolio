# Watchback — 개인 포트폴리오

누적된 Dependabot 취약점 알림에 EPSS·KEV·GHSA 외부 위험 신호를 결합해 다시 검토할 후보와 근거를 제시하는 팀 프로젝트입니다. **이 저장소는 Microsoft Data School 팀 프로젝트에서 ShinDuck의 데이터 분석·정책·계약, 파이프라인 참고본과 프로젝트 이해를 위한 팀 공통 문서를 정리한 개인 포트폴리오**입니다.
발표 자료: [[3차] Watchback 발표자료.pdf](https://github.com/user-attachments/files/32456220/3.Watchback.pdf)

## 프로젝트 개요 및 아키텍처

Watchback은 해결되지 않고 쌓인 Dependabot Alert를 관찰하다가 **위험이나 패치 가능성이 달라진 항목을 다시 검토할 수 있도록 후보와 근거를 제공하는 서비스**입니다.

<img width="2144" height="1203" alt="image" src="https://github.com/user-attachments/assets/f68bdb37-60f0-49d8-a433-2b845c42c236" />


## 프로젝트 이해를 위한 읽는 순서

| 순서 | 팀 공통 문서 | 알 수 있는 내용 |
| --- | --- | --- |
| 1 | [PRD](docs/product/prd.md) | 문제·사용자·MVP·성공 지표와 제약 |
| 2 | [기술 아키텍처](docs/architecture/technical-architecture.md) · [구조도 안내](docs/architecture/README.md) | 구성요소·데이터 흐름·DE/DS/Web 책임 경계 |
| 3 | [제품 백로그](docs/product/product-backlog.md) | Epic·유저스토리·수용 기준·우선순위 |
| 4 | [Sprint 1 범위](docs/product/sp1-backlog.md) | 목표·팀 간 의존성·작업 구분 |
| 5 | [DS 백로그](docs/product/sp1-ds-backlog.md) · [DE](docs/product/sp1-de-backlog.md) · [Web](docs/product/sp1-web-backlog.md) | 각 파트의 세부 작업과 DS가 맡은 위치 |
| 6 | [개인 DS 기여](docs/contributions/ds-issues-and-deliverables.md) | 담당 이슈·실제 개인 작업물·연결 PR |

`docs/product`와 `docs/architecture`는 **팀 공통 참고 자료**입니다.

## 담당 범위

- EPSS D-1/D-7/D-30/D-90 변화량 및 임계값별 후보 수 비교
- KEV 등재와 EPSS·GHSA 보조 신호의 overlap, precision-like, lift 분석
- Gate → Trigger → 축별 점수 → 우선순위 → 근거 생성 정책 설계
- Silver 분석 입력과 Gold 후보·근거 출력 계약 정의

기술: Python, PySpark, Spark SQL, Microsoft Fabric/Synapse PySpark Notebook, Markdown 데이터 계약. 분석 신호는 FIRST EPSS, CISA KEV, GitHub Advisory(GHSA)입니다.

## DS 담당 이슈와 작업물

[DS 이슈별 기여 상세](docs/contributions/ds-issues-and-deliverables.md)에서 담당 이슈 6건의 목적·완료 기준·상태와 개인 작업물을 함께 볼 수 있습니다.

|담당 작업 | 개인 작업물 |
| --- | --- |
| Silver 입력·품질 기준 | [입력 계약](docs/contracts/silver/silver-ds-analysis-input-contract.md) |
| 후보 결과·근거 필드 | [출력 계약](docs/contracts/gold/gold-rereview-candidate-output-contract.md) |
| EPSS·KEV 후보 신호 분석 | [보고서](docs/analysis/risk-signal-change-analysis.md) · [노트북](fabric/workspace/analysis/ds02_risk_change_analysis.ipynb) |
| 포함·제외·우선순위 정책 | [후보 선정 정책](docs/contracts/gold/candidate-selection-policy.md) |
| 후보 생성 Pipeline | [Pipeline 노트북](fabric/workspace/analysis/ds04_gold_rereview_candidate_validation_v3.ipynb)|


## 먼저 볼 자료

| 자료 | 내용 | 
| --- | --- | 
| [분석 보고서](docs/analysis/risk-signal-change-analysis.md) | D-30 기준 채택 근거, 임계값 비교, KEV 동반성 |
| [분석 노트북](fabric/workspace/analysis/ds02_risk_change_analysis.ipynb) | EPSS·KEV·GHSA 분석을 구성하는 코드 |
| [후보 선정 정책](docs/contracts/gold/candidate-selection-policy.md) | 제외 조건, 점수, P0~P3, 근거 보존 |
| [Silver 입력 계약](docs/contracts/silver/silver-ds-analysis-input-contract.md) | 필드·키·시점·품질 기준 |
| [Gold 출력 계약](docs/contracts/gold/gold-rereview-candidate-output-contract.md) | 배치·후보·근거 스키마 |


## 주요 분석 결과

원본 분석 문서에 기록된 2026-08-17 기준 결과입니다.

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
