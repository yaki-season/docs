# 기획/PM/오케스트레이터 run ledger

- 마지막 갱신: `2026-08-02`
- 담당 역할: `기획/PM/오케스트레이터`
- 기준 dashboard: `docs/epic/작업_현황.md` 실제 epic 재집계 `45 / 11 / 6 / 0 / 2 / 26`
- 현재 stage epic:
  - `pm-orchestration/001` S0 `v1.0.0` 진행 중
  - `pm-orchestration/002` D1 `v1.0.0` 진행 중
  - `pm-orchestration/003` D2 `v1.0.0` 대기
  - `pm-orchestration/004` D3/D4 `v1.0.0` 대기

## 저장소 체크포인트

| 저장소 | branch | HEAD | 담당 dirty·untracked 경로 | 다른 역할 변경 주의 |
|---|---|---|---|---|
| `docs` | `main` | `b44763fcc23b3763f90579bdf675875f3f70aa53` | PM이 이 run에서 동기화한 epic/dashboard/handoff; 소유자 미복구 untracked `terminal-guides/dispatches/2026-08-01-d1-s0-art-production-packets.md` | untracked packet은 손상·구문과 stale owner 정보가 있어 PM이 수정·삭제하지 않는다. |
| `app` | `main` | `63df61849f3bbd5e67bbbc63640febd912f7e223` | Developer 1 task 011 `src/d1/view.js` + E2E 5파일 | dirty 6파일의 SHA는 QA R3와 일치하며 commit/push 없음. |
| `art-workspace` | `agent/pipeline-v8-contract` | `c1905689130d18e7bcb2774e1aac98c57d8b5bd3` | Artist 1 provenance 20·신규 report 6, Artist 2 cold candidate report 1 | pixel·PNG·GLB·render diff 0; 승인 lane·stable ID·finalizer/promotion 동결. |

세 저장소는 모두 upstream HEAD와 일치하지만 이 run의 세 dirty lane은 commit/push되지
않아 다른 컴퓨터에서 복구할 수 없다.

## 완료한 관리 동작

- PM/오케스트레이터의 단일 부팅 프롬프트와 역할 subagent 자동 인계 loop를
  `terminal-guides/PM.md`에 고정했다.
- S0·D1 진행, D2·D3/D4 대기 stage 태스크를 `epic/pm-orchestration/`에 만들었다.
- 사람 화면 테스트 표준 양식과 stage별 필수 `PASS` gate를 만들었다.
- Developer 3 작업 011을 현재 HEAD 계약 `5/5`로 재검증하고 `v1.0.3` 완료 처리했다.
- Developer 1은 D1 익명 엑스트라 state→clip·6석 geometry 계약을 완료해 Artist 3 입력을 만들었다.
- Developer 2 작업 012는 CH-AKI-STORY exact binding 계약을 완료했고 작업 013으로 자동 인계했다.
- QA 작업 001은 사람 화면 gate를 수신해 `v1.2.1`·진행 중으로 재개했다.
- Developer 1 task 011의 시작 2칸·네기마 2개·순차 가이드를 구현하고 QA R3
  독립 회귀 GREEN으로 잠갔다. 사람 PASS는 대행하지 않았다.
- 2026-08-01 Artist 2 cold R1을 `pending-user-review`로 복구한 PM 판단은 과거 사용자
  반려를 누락한 오류였다. 2026-08-02 `rejected-by-user; do-not-resubmit`으로 복구했다.
- Artist 1 v8 provenance/report bridge를 검증해 pipeline error를 `23→3`으로 줄였고 QA R4가
  report `25`·provenance `38`·error `3`을 독립 재현했다.

## 현재 사용자 승인 큐

승인 요청은 한 건씩 올린다. 독립 제작 lane은 승인 대기 중에도 계속한다.

1. D1 / Artist 1 — `MDL-NEGIMA-GRILL-COOKING-SECOND-FACE R2` station consumption
   - 경로:
     `art-workspace/review/artist-000/d1-cooking/grill/recomposition/cooking-second-face-station-consumption/r2/`
   - 상태: `pending-user-review`
   - 승인 뒤: 다음 proper 상태 전 shader/state integration
2. D1 / Artist 3 — `ST-DRINK-BEER-TIER-1 R2`
   - FHD:
     `art-workspace/review/artist-025/st-drink-beer-tier-1/r2/review/recomposition-st-drink-beer-tier-1-fhd-r2.png`
   - 720:
     `art-workspace/review/artist-025/st-drink-beer-tier-1/r2/review/recomposition-st-drink-beer-tier-1-hd-r2.png`
   - 상태: `user-review-pending; not-approved; not-runtime-eligible`
   - 승인 뒤: completion→provenance→lossless optimization→finalizer→Developer 2 promotion

## 역할 run queue

| 역할 | runId | 현재 단일 작업 | 쓰기 경로 | 사용자 승인 대기 | 외부 입력 대기 | 다음 인계 |
|---|---|---|---|---|---|---|
| Artist 2 | `ART2-S0-BRAZIER-USER-REJECTION-20260802-R2` | cold R1 반려·재제출 금지 복구 | cold R1 candidate report | 없음 | 기존 반려 피드백·올바른 화로 방향 복구 | 방향 복구 전 새 제작 금지 |
| Developer 1 | `DEV1-D1-011-20260801-R2` | task 011 자동 회귀 GREEN | app dirty 6파일 | D1 첫 손님 사람 PASS | 정적 URL·placeholder 0 | PM→사람 화면 gate |
| Artist 1 | `ART1-D1-V8-REPORT-BRIDGE-20260801-R2` | pipeline `23→3` 완료 | Artist 1 metadata 26파일 | second-face R2; legacy 의미 결정은 별도 | finished-tray/r1 review board | 의미 승인→잔여 3건 정합→QA |
| 통합 QA·릴리스 | `QA-ART-PIPELINE-20260801-R4` | task011 GREEN·pipeline 3 RED 재현 완료 | 없음 | 없음(대행 금지) | promotion·static host | binding 뒤 gameplay 회귀→placeholder 0→사람 카드 |
| Artist 3 | 기존 역할 queue | beer station R2 후보 대기 | 없음 | beer R2 | 없음 | finalizer→Developer 2 promotion |
| Developer 2 | 기존 역할 queue | 승인 runtime handoff 대기 | 없음 | 없음 | Artist finalizer | dry-run→write→binding→Developer 1 |
| Developer 3 | 완료 lane | 실행 가능한 신규 contract task 없음 | 없음 | 없음 | 없음 | D2는 D1 사람 gate 뒤 |

## 임계경로와 병렬성

- 현재 병렬 가능:
  - Developer 2 작업 013 inventory 보완
  - 사용자 승인 큐 1건 판단
- 사용자 승인 결과에 종속:
  - Artist 2 cold brazier
  - Artist 1 cooking second-face
  - Artist 3 beer station
- 순차:
  - 후보 승인 → finalizer → Developer 2 dry-run/receipt/write → binding → gameplay 회귀 → QA
- 최초 공개:
  - S0 전체 사람 `PASS`와 D1 전체 영업 사람 `PASS`, S0+D1 placeholder 0이 모두 필요

## 정본 drift와 PM 결정 대기

- 최신 `GPL-002/003/004`, `DAT-001`, `ART-003`은 D1 첫 주문 네기마 `2`, 시작 2칸,
  명성 해금 `2→4→6→8`칸이다. `SCN-002`의 네기마 `3`개·직장인 실명, Artist
  000/025·Developer 1/009·일부 handoff의 고정 6칸/3개 문구, app state ID
  `D1-grill-six-slots`는 drift다. 이를 실제 파일 정본으로 승격하지 않는다.
- art pipeline 잔여 3건은 별도 의미/증거 blocker다:
  `assembly/r1` stable ID·whole-screen profile, `finished-tray/r1` 격리 review board 부재,
  `grill/master/r3` 승인 whole-screen master·v8 single-asset 계약 충돌.
- 현재 후보 승인 카드 1건을 먼저 묻고, 그 뒤 `spec 정합 반영`과 pipeline legacy 의미
  결정을 한 건씩 요청한다.

## 마지막 검증

- app `main@63df61849f3bbd5e67bbbc63640febd912f7e223` + dirty 6파일:
  - Developer 1 전체 Vitest `364/364`, task011 E2E `10/10`, dirty E2E `26/26`, 관련 unit `59/59`.
  - QA R3 task unit `6/6`, FHD/720 핵심·교차 E2E `24/24`, 최초 실패·retry·40404 `0`.
- runtime: assets `12/12`, references `36/36`, required `44`, binding `9`, placeholder `35`, drink
  `5`, manifest SHA `3ef18fffe69b4d447a00c1ecb99bdaf22c7e6a0848f242e3a580acf60bab13c7`.
- art `agent/pipeline-v8-contract@c1905689130d18e7bcb2774e1aac98c57d8b5bd3`: QA R4
  `YS-ASSET-PIPELINE-v8`, report `25`, provenance `38`, error `3`, diff check PASS, non-metadata diff `0`.
- 실제 정적 public URL/build ID는 없다. 따라서 Chrome·Edge 사람 화면 카드·사람 PASS는 없다.

## 다음 PM 동작

1. cold R1을 모든 승인·제작·파생 lane에서 제거한 상태를 감사한다.
2. 새 화로를 dispatch하기 전 기존 반려 피드백과 올바른 방향을 기록에서 복구한다.
3. 현재 턴에서 다른 승인 후보를 이어서 요청하지 않는다.

## 2026-08-02 반려 정합 checkpoint

- 사용자가 cold R1이 과거 강하게 반려된 산출물임을 재확정했다. PM은 이 사실을
  누락한 재승인 요청에 대해 책임을 기록했다.
- candidate report는 `rejected-by-user; do-not-resubmit`으로 정합했고 PNG는 반려
  증거로만 보존한다. FHD/720·finalizer·promotion·ignition/source 재사용은 금지다.

## 2026-08-01 종료 checkpoint

- active role subagent `0`; completed: Artist 2 reconcile, Developer 1 R1/R2, Artist 1 R1/R2,
  Integration QA R3/R4. root PM만 사용자 승인 응답을 대기한다.
- test·promotion·finalizer·receipt·static host 프로세스 검색 결과는 `0`이다.
- LFS object push 대기는 세 저장소 모두 `0`이다. `docs` 14 tracked + 기존 untracked
  packet 1, `app` tracked 6, `art-workspace` tracked 21 + untracked report 6은 모두 원격 미전달이다.
- dashboard는 실제 epic 45개를 재집계해 진행 `11`, 대기 `6`, 변경 확인 `0`, 보류 `2`,
  완료 `26`과 일치한다. 이 run은 사람 PASS나 stage 완료 상태를 올리지 않았다.

## 재개 시 금지

- handoff의 과거 테스트 수를 현재 green으로 간주
- 승인 대기 후보의 metadata·finalizer·runtime 상태 선행 변경
- 같은 역할·stable ID의 subagent 중복 spawn
- docs/app/art-workspace의 다른 역할 dirty 변경 초기화·정리
