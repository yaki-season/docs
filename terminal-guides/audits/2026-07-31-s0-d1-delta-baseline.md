# S0·D1 병렬 변경 delta 중간 기준선 — 2026-07-31

- 역할: `Integration QA & Release`
- 성격: 병렬 작업 중 read-only 기준선이다. 공개 통과 선언·promotion·finalizer·manifest 편집을 하지 않았다.
- 제외: D2~D5 구현·아트·정리와 다른 역할의 tracked dirty 변경.

## 저장소와 실행 중 process

| repository | branch / HEAD | dirty + untracked | 현재 path owner 범위 |
|---|---|---:|---|
| docs | `main` / `d22cb47b2207` | 68 | `epic/developer-1/**` Developer 1, `developer-2/**` Developer 2, `developer-3/**` Developer 3, `artist/000·023·025/**` 각 Artist, `qa-release/**` QA, `spec/**`·공통 dashboard는 PM/문서 정본 |
| app | `main` / `f22b546f60d8` | 156 | D1 domain/renderer/business-day 및 관련 E2E는 Developer 1, S0 binding·asset resolver·shell·promotion 도구 및 UI는 Developer 2, `content/releases/**`·D1 release contract/schema/fixture/builder는 Developer 3. `content/d5/**`·D5 fixture는 범위 밖으로 변경하지 않았다. |
| art-workspace | `main` / `b6ada20b8182` | 256 | `review/artist-000/**` Artist 1, `artist-023/**` Artist 2, `artist-025/**` Artist 3; 기존 `artist-010/**`은 승인 evidence 보존 영역 |

PID가 있는 local capture/static server `8010`, `8011`, `8012`, `8777`가 실행 중이었다. Vitest,
Playwright, promotion, finalizer process는 기준선과 targeted 검증 시작 시점에 없었다. targeted 검증 뒤
`app/test-results/.last-run.json` 1 file·4 KiB(SHA-256 `91bf4caf4a6f10d368dcfc0ea4e5f73221429f7dc68ea54eb12d2a4987d9c9a3`)가
생겼으나, ignored·자동 재생성·현재 handoff 무참조·live test 부재를 다시 확인한 뒤 정확한 `app/test-results/`
경로만 삭제했다. 다음 Playwright 실행으로 복구되며 receipt/staging은 보존했다.

## Runtime·promotion 기준선

| 항목 | 이전 QA cleanup 기준 | 현재 중간 기준선 | 판정 |
|---|---|---|---|
| manifest SHA-256 | `417d2be0584e620450ebd90e0e58b31dca49881b5f86884a06d5380c124c4b57` | `3ef18fffe69b4d447a00c1ecb99bdaf22c7e6a0848f242e3a580acf60bab13c7` | 병렬 변경 delta, QA가 수정하지 않음 |
| manifest asset | 9 | 12 | `assets:validate` 통과 |
| required / approved+bound runtime | 42 / 9 | 42 / 9 | runtime contract valid, semanticOwner conflict `0` |
| placeholder | 33 | 33 | 공개 통과 불가 |
| drink placeholder | 3 | 3 | 공개 통과 불가 |
| unbound approved ID | 0 | 0 | 통과 |
| promotion receipt / staging | 7 files / 28 KiB, staging 0 | 동일 | KEEP |
| runtime-handoff | 12 | 12 | 목록은 이 감사의 read-only inventory에 보존 |

placeholder missing ID는 `artist-1.d1-assembly-grill-food-shader` 15개와
`artist-3.d1-drink-service-cleanup-customer-settlement` 18개다. 상세 ID는 runtime audit 출력과
`reportD1RuntimeAssetReadiness()` 결과를 기준으로 하며, unbound approved ID는 없다.

## Read-only validation

| 검증 | PASS / FAIL / SKIP | 재현 명령 | 소유자 | 판정 |
|---|---:|---|---|---|
| runtime assets | 1 / 0 / 0 | `cd app && npm run assets:validate` | Developer 2 / QA | manifest 12개 통과 |
| reference images | 1 / 0 / 0 | `cd app && npm run visual:references:validate` | QA | 36장 통과 |
| binding·semanticOwner | 1 / 0 / 0 | `reportD1RuntimeAssetReadiness(manifest)` | Developer 2 / QA | contract valid, conflict 0 |
| art pipeline | 0 / 29 / 0 | `cd art-workspace && node pipeline/validate-pipeline.mjs` | metadata owners / QA | 아래 기존 오류와 동일 |
| Developer 1·3 unit bundle | 30 / 0 / 0 | `cd app && npx vitest run tests/unit/cookStations.test.js tests/unit/d1BusinessDay.test.js tests/unit/d1FlipCustomerServingRegression.contract.test.js --reporter=verbose` | Developer 1, Developer 3 | nextAction·양면 누적·저장·선택 손님 통과 |
| Developer 2 FHD/720 UI | 6 / 0 / 0 | `cd app && npx playwright test tests/e2e/d1-face-serving-ux.spec.js --project=chromium-1280x720 --project=chromium-1920x1080` | Developer 2 | 조기 뒤집기 표시·양면 정지·선택 손님 제공 통과 |
| 전체 verify / Chrome·Edge / static host journey | 0 / 0 / 1 | `cd app && npm run verify` | QA | 병렬 중간 기준선에서는 미실행 |

## 알려진 art pipeline 오류 29개 — 신규 회귀와 분리

이번 실행의 29개 오류는 이전에 알려진 집합과 동일하다. 신규 오류 `0`으로 분류하며, 수정하지 않는다.

- **현행 spec SHA 4개**: `review/artist-010/complete-layers/background/r3/metadata/completion-report.json`
  (ART-003, UI-003), `review/artist-010/complete-layers/customer-seat-01/r2/metadata/completion-report.json`
  (ART-003, UI-003).
- **S0 sourceMaster topology pending 2개**:
  `review/artist-023/s0-prologue/exterior-key/bg-exterior-s0-closed/closed/r2/metadata/completion-report.json`,
  `review/artist-023/s0-prologue/gate-open/bg-exterior-s0-gate-open/gate-open/r1/metadata/completion-report.json`.
- **legacy provenance schema 23개**: `review/artist-000/d1-cooking/assembly/ingredient-chicken/r1`,
  `assembly/ingredient-negi/r1`, `r2`, `r3`, `assembly/r1`, `assembly/skewer-base/r2`,
  `assembly/station/r1`, `r2`, `r3`, `assembly/workspace-background/r1`, `grill/background/r1`,
  `grill/finished-tray/r1`, `r3`, `r4`, `r5`, `r6`, `grill/master/r1`, `r2`, `r3`,
  `grill/master-derived-base/r1`, `grill/recomposition/finished-proper-six-slot/r2`, `grill/station/r2`,
  `grill/waiting-rack/r2`의 각 `metadata/provenance.json`.

## 역할별 delta 판정

| 역할 | PASS | BLOCKED / 보류 |
|---|---|---|
| Developer 1 | `slotViews().nextAction`은 3초/0초에서 `flip`, 양면 준비 후 `retrieve`; 양면 누적·snapshot/restore·선택 손님 제공 unit 30/30 통과 | 전체 공개 journey와 Artist finalizer 입력 대기 |
| Developer 2 | FHD/720에서 조기 행동은 `뒤집기`로, 양면 준비 후 `회수`로 표시되고 E2E 6/6 통과. `ST-S0-BRAZIER`는 contract/preflight만 있고 runtime 등록·image 없음 | `PR-SHOP-KEY`는 exact ID 1개·runtime-handoff 1개이나 receipt directory에 대응 receipt 0개라 single-transaction 증빙 불충분 |
| Developer 3 | 작업 011은 `진행 중`이며 Developer 2 UI blocker 해소 전에 완료 처리되지 않았다. 기존 4개+신규 nextAction 계약은 현재 unit green | Developer 2 FHD/720 handoff와 작업 011의 공식 완료 상태 갱신 전 완료 선언 금지 |
| Artist 2 | `PR-SHOP-KEY` runtime-handoff 1개, duplicate finalizer script 0. `ST-S0-BRAZIER`, `CH-AKI-STORY`는 preflight markdown만 있으며 PNG/GLB/handoff 0 | topology pending 2개와 preflight/user approval 대기 |
| Artist 3 | `ST-DRINK-BEER-TIER-1 R2`는 `user-review-pending; not-approved; not-runtime-eligible`; completion/provenance/optimization/finalizer/handoff/catalog entry 0. 엑스트라는 zero-pixel preflight 1개뿐 | 사용자 승인 전 R2 runtime 등록 금지 |

## 공개 gate

차단 항목은 placeholder `33`, art pipeline `29`, `PR-SHOP-KEY` promotion receipt 증빙 부재, 최신 전체
verify·실제 static host journey·Chrome/Chromium FHD/720 기록 부재, 로컬 Microsoft Edge 미설치다.
404, console error, actual static journey, export/import, diagnostic copy/download, D4 read-only warning은
targeted green 뒤에만 전체 공개 gate에서 확인한다.
