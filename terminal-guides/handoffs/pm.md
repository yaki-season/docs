# 기획/PM/오케스트레이터 run ledger

- 마지막 갱신: `2026-07-31`
- 담당 역할: `기획/PM/오케스트레이터`
- 기준 dashboard: `docs/epic/작업_현황.md v2.1.141`
- 현재 stage epic:
  - `pm-orchestration/001` S0 `v1.0.0` 진행 중
  - `pm-orchestration/002` D1 `v1.0.0` 진행 중
  - `pm-orchestration/003` D2 `v1.0.0` 대기
  - `pm-orchestration/004` D3/D4 `v1.0.0` 대기

## 저장소 체크포인트

| 저장소 | branch | HEAD | 담당 dirty·untracked 경로 | 다른 역할 변경 주의 |
|---|---|---|---|---|
| `docs` | `main` | `1fc074e` | PM stage epic·가이드·dashboard·QA/Dev3 상태 동기화 변경 | commit 전 다른 역할의 docs writer와 겹치지 않게 한다. |
| `app` | `main` | `c0b1bd4` | 없음 | PM은 직접 구현하지 않는다. |
| `art-workspace` | `main` | `36b0164` | 없음 | 사용자 승인 전 후보·metadata를 수정하지 않는다. |

현재 docs 변경은 아직 원격에 전달되지 않았다. 다른 컴퓨터에서는 commit/push 또는 승인된 파일
전송 전 복구할 수 없다.

## 완료한 관리 동작

- PM/오케스트레이터의 단일 부팅 프롬프트와 역할 subagent 자동 인계 loop를
  `terminal-guides/PM.md`에 고정했다.
- S0·D1 진행, D2·D3/D4 대기 stage 태스크를 `epic/pm-orchestration/`에 만들었다.
- 사람 화면 테스트 표준 양식과 stage별 필수 `PASS` gate를 만들었다.
- Developer 3 작업 011을 현재 HEAD 계약 `5/5`로 재검증하고 `v1.0.3` 완료 처리했다.
- QA 작업 001은 사람 화면 gate 수신이 필요해 `v1.2.0`·`변경 확인 필요`로 전환했다.

## 현재 사용자 승인 큐

승인 요청은 한 건씩 올린다. 독립 제작 lane은 승인 대기 중에도 계속한다.

1. S0 / Artist 2 — `ST-S0-BRAZIER cold R1`
   - asset:
     `art-workspace/review/artist-023/s0-prologue/ignite/st-s0-brazier/cold/r1/assets/st-s0-brazier-cold-r1.png`
   - SHA-256:
     `df043cca7d3672e36792e3db6705a88a83a89a4fd916021f34c023605998bcbb`
   - 상태: `pending-user-review`
   - 승인 뒤: FHD/720 실제 소비 화면 제작·2차 승인
2. D1 / Artist 1 — `MDL-NEGIMA-GRILL-COOKING-SECOND-FACE R2` station consumption
   - 경로:
     `art-workspace/review/artist-000/d1-cooking/grill/recomposition/cooking-second-face-station-consumption/r2/`
   - 상태: `pending-user-review`
   - 승인 뒤: 다음 proper 상태 전 shader/state integration
3. D1 / Artist 3 — `ST-DRINK-BEER-TIER-1 R2`
   - FHD:
     `art-workspace/review/artist-025/st-drink-beer-tier-1/r2/review/recomposition-st-drink-beer-tier-1-fhd-r2.png`
   - 720:
     `art-workspace/review/artist-025/st-drink-beer-tier-1/r2/review/recomposition-st-drink-beer-tier-1-hd-r2.png`
   - 상태: `user-review-pending; not-approved; not-runtime-eligible`
   - 승인 뒤: completion→provenance→lossless optimization→finalizer→Developer 2 promotion

## 역할 run queue

| 우선순위 | 역할 | stage/epic | 현재 한 작업 | 자동 다음 인계 |
|---|---|---|---|---|
| 1 | 통합 QA·릴리스 | QA 001 `v1.2.0` | 사람 화면 gate 변경 범위 수신, 최신 assets/Vitest/placeholder 기준선 재감사 | PM이 첫 실제 테스트 카드 발행 |
| 2 | Developer 2 | S0 / 012 | `CH-AKI-STORY` portrait binding의 exact bounds/layer/DOM-safe/story-only 계약 | Artist 2 Aki 무픽셀 preflight→사용자 승인 |
| 3 | Developer 1 | D1 / 005 | Artist 3 commuter state→clip mapping과 이름 없는 엑스트라 의미 계약 | Artist 3 commuter preflight→사용자 승인 |
| 4 | Developer 2 | D1 / 013 | 작업 012 뒤 맥주 liquid·VFX·served required inventory 전수 감사 | Artist 3 exact ID·QA placeholder gate |
| 5 | Artist 2 | S0 / 023 | cold R1 사용자 승인 결과 소비 | FHD/720 소비 화면 또는 반려 범위 수정 |
| 6 | Artist 1 | D1 / 000 | cooking second-face R2 사용자 승인 결과 소비 | proper-first-face 이전 integration |
| 7 | Artist 3 | D1 / 025 | beer station R2 사용자 승인 결과 소비 | finalizer→Developer 2 promotion |
| 8 | Developer 3 | D1 / 011 | 완료. 새 runnable contract task 없음 | QA가 계약 5/5를 기준선으로 소비 |

## 임계경로와 병렬성

- 현재 병렬 가능:
  - Developer 2 `CH-AKI-STORY` 계약
  - Developer 1 commuter clip/role 계약
  - QA 사람 화면 gate 수신·최신 기준선
- 사용자 승인 결과에 종속:
  - Artist 2 cold brazier
  - Artist 1 cooking second-face
  - Artist 3 beer station
- 순차:
  - 후보 승인 → finalizer → Developer 2 dry-run/receipt/write → binding → gameplay 회귀 → QA
- 최초 공개:
  - S0 전체 사람 `PASS`와 D1 전체 영업 사람 `PASS`, S0+D1 placeholder 0이 모두 필요

## 정본 drift와 PM 결정 대기

- `GPL-002 v1.10.0`의 생맥주 선제·구형 2+1 그릴 예시는 최신 `GPL-003/004`·`DAT-001`과 충돌한다.
- `SCN-002`·`ART-002`의 고정 직장인 인물은 최신 `ART-003`의 아키·츠키오카 외 이름 없는
  엑스트라 정책과 충돌한다.
- 구현·검수는 최신 사용자 확정과 최신 gameplay/art contract를 따른다. PM은 관련 spec을 직접
  수정하지 않고 사용자에게 별도 `spec 정합 반영` 승인을 요청해야 한다.

## 마지막 검증

- 명령:
  `npx vitest run tests/unit/d1FlipCustomerServingRegression.contract.test.js --reporter=verbose`
- app HEAD: `c0b1bd4`
- 결과: test file 1개, 계약 `5/5` 통과

## 다음 PM 동작

1. QA·Developer 2·Developer 1의 독립 작업을 역할 subagent에 최대 세 lane으로 배정한다.
2. 사용자에게 승인 큐 1번 `ST-S0-BRAZIER cold R1`을 표준 승인 카드로 제시한다.
3. 승인 결과를 Artist 2에 자동 인계하고 독립 lane 완료마다 다음 consumer를 즉시 배정한다.

## 재개 시 금지

- handoff의 과거 테스트 수를 현재 green으로 간주
- 승인 대기 후보의 metadata·finalizer·runtime 상태 선행 변경
- 같은 역할·stable ID의 subagent 중복 spawn
- docs/app/art-workspace의 다른 역할 dirty 변경 초기화·정리
