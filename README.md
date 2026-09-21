# Watchback — 개인 포트폴리오

누적된 Dependabot 취약점 알림에 EPSS·KEV·GHSA 외부 위험 신호를 결합해 다시 검토할 후보와 근거를 제시하는 팀 프로젝트입니다. **이 저장소는 Microsoft Data School 팀 프로젝트에서 ShinDuck이 작성한 데이터 분석 노트북과 정책·계약 문서를 선별한 개인 포트폴리오**입니다.

## 담당 범위

- EPSS D-1/D-7/D-30/D-90 변화량 및 임계값별 후보 수 비교
- KEV 등재와 EPSS·GHSA 보조 신호의 overlap, precision-like, lift 분석
- Gate → Trigger → 축별 점수 → 우선순위 → 근거 생성 정책 설계
- Silver 분석 입력과 Gold 후보·근거 출력 계약 정의

기술: Python, PySpark, Spark SQL, Microsoft Fabric/Synapse PySpark Notebook, Markdown 데이터 계약. 분석 신호는 FIRST EPSS, CISA KEV, GitHub Advisory(GHSA)입니다.

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
팀원 애플리케이션·수집 파이프라인·인프라·운영 문서·원천 데이터·Git 이력은 제외했습니다. README는 포트폴리오용으로 새로 정리했으며 기존 작성물과 구분합니다.
