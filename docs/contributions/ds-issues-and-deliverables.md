# DS 담당 이슈와 개인 작업물

Watchback 팀 프로젝트에서 ShinDuck에게 배정된 DS 이슈 6건을 개인 기여 중심으로 재정리한 문서입니다. 원본 이슈는 Pexy99가 등록했으며, 이슈 담당자와 코드·문서 작성자를 구분합니다. 이 문서는 2026-09-21 확인 내용을 바탕으로 새로 작성한 포트폴리오용 요약입니다. 원본 이슈·PR 링크는 팀 저장소 접근 권한이 필요하지만 아래 작업물은 이 개인 저장소에서 볼 수 있습니다.

## 이슈 → PR → 작업물

| 담당 이슈 | 배정 | 연결 PR | 개인 저장소 작업물 | 원본 이슈 상태 |
| --- | --- | --- | --- | --- |
| [#65 DS-01 Silver 분석 입력 계약·품질 기준](https://github.com/dt4-proj3-team3/watchback/issues/65) | ShinDuck | [#131](https://github.com/dt4-proj3-team3/watchback/pull/131) | [Silver 입력 계약](../contracts/silver/silver-ds-analysis-input-contract.md) | Open |
| [#66 DS-01 후보 결과 스키마와 근거 필드](https://github.com/dt4-proj3-team3/watchback/issues/66) | ShinDuck | [#131](https://github.com/dt4-proj3-team3/watchback/pull/131) | [Gold 출력 계약](../contracts/gold/gold-rereview-candidate-output-contract.md) | Open |
| [#68 DS-02 EPSS 시계열·KEV 후보 신호 분석](https://github.com/dt4-proj3-team3/watchback/issues/68) | ShinDuck·Justmagic46 공동 | [#129](https://github.com/dt4-proj3-team3/watchback/pull/129) | [분석 보고서](../analysis/risk-signal-change-analysis.md), [분석 노트북](../../fabric/workspace/analysis/ds02_risk_change_analysis.ipynb) | Open, 체크리스트 5/5 완료 표시 |
| [#73 DS-04 재검토 후보 정책·선정](https://github.com/dt4-proj3-team3/watchback/issues/73) | ShinDuck·Justmagic46 공동 | 하위 #74·#75로 연결 | [후보 선정 정책](../contracts/gold/candidate-selection-policy.md) | Open, 하위 이슈 1/2 완료 |
| [#74 DS-04 포함·제외와 우선순위 정책](https://github.com/dt4-proj3-team3/watchback/issues/74) | ShinDuck | [#116](https://github.com/dt4-proj3-team3/watchback/pull/116) | [후보 선정 정책](../contracts/gold/candidate-selection-policy.md) | Closed / Completed |
| [#75 DS-04 후보 생성 Pipeline 구현](https://github.com/dt4-proj3-team3/watchback/issues/75) | ShinDuck·Justmagic46 공동 | [#140](https://github.com/dt4-proj3-team3/watchback/pull/140), Justmagic46 작성 | 본인 정책·계약과의 협업 관계만 기록. 팀원 구현 코드 제외 | Open |

상태와 체크리스트는 원본에서 확인한 기록이며, 이번 이관 중 재검증한 실행 결과를 뜻하지 않습니다. #129와 #131은 미병합 상태이며, 이관한 #131 계약 문서는 초안입니다. 이슈 등록 이력 자체를 본인 단독 작성 성과로 주장하지 않습니다.

## DS-01: 분석 입력·출력 계약 — #65, #66

### #65 Silver 분석 입력 계약·품질 기준

위험 변화 분석에 필요한 입력 구조와 분석 가능한 데이터의 조건을 정의하는 작업입니다. CVE·GHSA·Package 연결 키, 점수와 상태 필드의 타입·기준 시점, 필수값·중복·이력 연속성·최소 관찰 기간을 계약으로 정리했습니다.

- 개인 작업물: [Silver 분석 입력 계약](../contracts/silver/silver-ds-analysis-input-contract.md)
- 작성 근거: ShinDuck의 PR #131, 이관 커밋 `28b7e9cc190de1bceb4f0b247057ebe9512c1684`
- 완료 기준의 범위: 구조 정의에 더해 실제 Silver 샘플로 품질 조건을 확인하는 것까지 포함합니다.
- 현재 한계: 이슈 완료 기준은 미체크 상태입니다. 계약 초안 작성과 실제 샘플 검증 완료를 구분하며, 원천 데이터나 내부 연결 정보는 이관하지 않았습니다.

### #66 후보 결과 스키마와 근거 필드

DS 결과를 DE와 Web이 해석할 수 있도록 후보·배치·근거의 출력 구조를 정의하는 작업입니다. 후보 식별 키, 선정 여부, 우선순위, 기준 시점, 정책 버전·Batch ID, 신호의 이전·현재 값과 출처를 담는 구조를 정리했습니다.

- 개인 작업물: [Gold 후보·근거 출력 계약](../contracts/gold/gold-rereview-candidate-output-contract.md)
- 작성 근거: ShinDuck의 PR #131, #65와 같은 이관 커밋
- 완료 기준의 범위: 선정·제외 근거를 사람이 이해할 수 있고, 고객 Alert·저장소 정보 없이 전역 후보를 표현할 수 있어야 합니다.
- 현재 한계: 초안 상태이며 이슈 완료 기준은 미체크입니다. 실제 Gold 출력과 정책의 정합성은 추가 검증이 필요합니다.

## DS-02: EPSS·KEV 후보 신호 분석 — #68

EPSS 점수·percentile의 분포와 시계열 변화, 관찰 기간과 임계값에 따른 후보 수, KEV 동반 특성을 비교하는 작업입니다. 공동 배정 이슈이지만 이 저장소에는 ShinDuck 작성 PR #129의 보고서와 노트북만 포함했습니다.

- [분석 보고서](../analysis/risk-signal-change-analysis.md): D-30 기준, 상승 배율과 절대 증가량의 조합, KEV overlap·precision-like·lift 해석
- [분석 노트북](../../fabric/workspace/analysis/ds02_risk_change_analysis.ipynb): EPSS·KEV·GHSA 신호를 비교하는 PySpark 분석
- 이관 커밋: `7158cbf1bc93bb016a3a505f7f005f9306141481`
- 원본 기록: 분포·변화량, 기간·임계값 비교, 결측·중복·이상치와 한계, KEV 동반성과 후보 영향의 체크리스트 5개가 체크돼 있습니다. 이슈와 PR은 여전히 Open입니다.
- 해석 범위: 문서의 분석 수치는 원본 기록이며 재산출하지 않았습니다. KEV 동반성은 실제 공격 탐지 정확도가 아닙니다. 노트북 실행 출력과 메타데이터는 제거했고 Fabric 전체 실행은 하지 않았습니다.

## DS-04: 후보 정책과 구현의 연결 — #73, #74, #75

### #73 상위 작업: 재검토 후보 정책·선정

정책 정의와 반복 실행 구현을 묶는 상위 이슈입니다. 동일 입력·정책 버전에서 같은 결과를 만들고, 후보별 우선순위와 선정·제외 근거를 남기는 것이 목표입니다. 하위 #74는 완료, #75는 Open이므로 전체 Pipeline 완료로 표기하지 않습니다.

### #74 개인 작성 작업: 복합 신호 포함·제외와 우선순위 정책

- 개인 작업물: [후보 선정 정책](../contracts/gold/candidate-selection-policy.md)
- 작성 근거: ShinDuck의 PR #116 병합 및 본인 후속 수정, 이관 커밋 `6cdfd7c75a3f488a3a49f3749e8bf1dd6df6b5a7`
- 설계 범위: 포함·제외 조건과 처리 순서, KEV 예외, 우선순위 필드·동률 처리, 정책 버전과 후보별 근거 생성
- 핵심 설계: Gate → Trigger → 축별 점수 → P0~P3 → 근거 생성. 동일 축 MAX·축 간 SUM으로 중복 가산을 조절하며 철회 GHSA를 먼저 제외합니다.
- 원본 상태: 체크리스트 4개 완료, 2026-08-21 Closed / Completed

### #75 공동 담당 작업: 후보 생성 Pipeline

정책을 Notebook/Job에 적용하고 후보·근거·배치 결과를 반복 생성하는 구현 작업입니다. 입력 기준 시점, 정책 버전·Batch ID, 동일 입력의 재실행 일관성, DS 출력 계약 충족이 완료 기준입니다.

원본에 연결된 PR #140은 Justmagic46 작성이며 커밋 `bd23e02aed658b52ff8d717908712d3d1e001b12` 하나로 구성됩니다. 추가된 파일은 `fabric/workspace/analysis/ds04_gold_rereview_candidate_validation_v3.ipynb`입니다. 공동 담당이라는 이유로 해당 구현을 개인 작성물로 복사하지 않았습니다. PR 본문의 실행·무결성 검증 결과도 본인 성과로 옮기지 않았습니다.

본인 정책·입출력 계약과 팀원의 후보 생성 구현이 연결되는 협업 맥락을 보여주기 위해 이 항목을 남겼습니다. #75에는 후속 백필 이슈 #141과 PR #142도 언급돼 있으나 이번 개인 작성물 이관 대상에는 포함하지 않았습니다.

## 이관 및 검증 범위

본인 PR 목록에서 확인된 3건(#116·#129·#131)에 연결된 개인 작업물 5개는 이미 이 저장소에 있습니다. 이번에는 중복 복사 없이 담당 이슈 6건의 요약과 작업물 연결을 추가했습니다. 원본 이슈를 팀 저장소에서 이동·삭제하거나 완료 처리하지 않았습니다. 팀원 코드, 원천 데이터, 자격증명, 내부 운영 자료는 추가하지 않았습니다.

[포트폴리오 README로 돌아가기](../../README.md)
