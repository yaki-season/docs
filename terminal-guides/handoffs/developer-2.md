# 개발자 2 직전 작업 기록

- 마지막 갱신: `2026-08-01`
- 현재 담당: 작업 011 `v1.1.0` 완료,
  작업 003 `v1.5.0`의 다음 승인 아트 대기와 작업 005 `v1.1.0` 공개 shell 검증 마감
- 현재 알려진 checkpoint: `PR-SHOP-KEY@R1-B1` exact S0 KEY binding 완료,
  `ST-S0-BRAZIER` primary/companion 계약 v1 응답 완료, Developer 1
  `slotViews().nextAction` 전용 D1 행동 UI 소비 완료
- 입력 대기: Artist 1 음식 없는 tray·후속 그릴 finalizer, Artist 2
  `ST-S0-BRAZIER`·`PR-CHARCOAL-IGNITION` 제작/승인 finalizer, Artist 3
  `ST-DRINK-BEER-TIER-1` 후속 승인 handoff
- 현재 PM 지시 결과: KEY promotion·화로 계약·조기 뒤집기 안내 교정을 완료했다.
  유효 finalizer 없는 후속 아트 promotion은 시작하지 않음
- contract: `app/src/assets/s0D1ArtBindingContract.js` / `v1.1.0`,
  `app/src/assets/s0ExteriorBackgroundBindingContract.js` / `v3.0.0`(작업 009)
- harness: `app/src/art-recomposition-harness.html` / query `componentId`, `mode=placeholder|approved`;
  `app/src/s0-exterior-background-harness.html` / query `stateId`, `mode=placeholder|approved`
- shell entry: `app/src/public-shell.html` / `http://localhost:8777/src/public-shell.html`
- save UI: 작업 008 공개 `createSaveFilePort`만 소비, 검증→확인→backup-1→active 교체
- D4: `d4-preview/preview` 전용, 경고 확인, active·backup·recovery source 진입 전후 동일
- 검증: assets 12, references 36, 전체 Vitest 43파일·316개,
  S0 exact binding/harness Chromium FHD/720 30개와 D1 행동·키보드 제공·영업 회귀 18개 통과
- 전체 verify: 종료 코드 `0`, Playwright 199개 통과·2개 skip·1개 flaky 회복.
  flaky는 작업 범위 밖 FHD `production-drink.spec.js` 레버 hold 최초 1회 `beerSec=0`이며
  동일 실행 retry에서 통과했다.
- 다음 한 동작: 작업 003은 다음 유효 Artist finalizer를 기다린다. Artist 2 화로 두 asset은
  이번 v1 계약으로 제작하되 사용자 승인·finalizer 전에는 exact placeholder를 유지한다.
  작업 005는 별도 지시에서 branded Edge·실제 정적 host smoke를 마감한다.
- 잔여 환경 위험: 로컬에 Microsoft Edge가 없어 branded Edge E2E 미실행
- 인계 대상: Artist 2 S0 exterior 두 background 적용 완료와 Artist 3의 `BG-WORKSPACE-DRINK`
- 주의: 유효 finalizer·dry-run 영수증 없이 write 금지, manifest 수동 편집 금지

## 2026-08-01 PM 복구 checkpoint

- 최신 전역 inventory는 required `44`, binding `9`, placeholder `35`, drink placeholder `5`,
  unbound approved `0`, manifest SHA
  `3ef18fffe69b4d447a00c1ecb99bdaf22c7e6a0848f242e3a580acf60bab13c7`다.
- 현재 쓰기 작업은 없다. 사용자 승인→Artist finalizer가 끝난 exact handoff만 받아
  dry-run→receipt write→exact binding을 순차 실행한다.
- 아래 `BG-WORKSPACE-DRINK` placeholder `34→33`은 승격 당시의 역사 증거며 현재 전역
  inventory 값으로 사용하지 않는다.

## 작업 011 KEY promotion·화로 계약·nextAction UI checkpoint

- epic:
  `docs/epic/developer-2/011_S0_KEY_승격_화로_계약과_D1_nextAction_UI.md`
  / `v1.1.0` / 완료
- A — KEY:
  - finalizer handoff SHA:
    `ba6cd227d7041e45df8d98e7c29dff3d4362d4608c5304390a559478f6d8c40f`
  - runtime raster SHA:
    `a8a22c3beaa6a4ffa1b45dad6eb4438eda729d325740ad9bcb265d3363229eeb`
  - runtime URL: `/assets/core/s0/prologue/pr-shop-key-placed-r1-b1.png`
  - `dry-run → 30분 receipt 독립 검증 → 동일 receipt 명시적 write` 완료
  - `S0-STATE-KEY`만 exact `PR-SHOP-KEY@R1-B1`을 소비하며 exact ID가 없으면
    승인 배경이 있어도 `개발 중` placeholder를 유지
- B — 화로:
  - machine contract: `app/src/assets/s0D1ArtBindingContract.js` / `v1.1.0`
  - Artist 2 handoff:
    `docs/terminal-guides/dispatches/2026-07-31-developer-2-s0-brazier-binding-handoff-v1.0.0.md`
  - source master `CM-PROLOGUE-INHERITANCE-R1`
  - primary `ST-S0-BRAZIER`: architecture z20, FHD `(648,376,624,432)`,
    720 `(432,251,416,288)`, visible charcoal·ignition pixel 소유 금지
  - companion `PR-CHARCOAL-IGNITION`: vfx z50, FHD child `(736,408,448,224)`,
    720 `(491,272,299,149)`, 모든 visible charcoal·ember·glow·flame·smoke·ash·spark 소유
  - 최대 두 exact layer, cross-pixel·중복 렌더 금지, 누락 exact layer별 placeholder
  - 이미지·runtime asset·manifest는 생성하거나 등록하지 않음
- C — 행동 UI:
  - Developer 1 입력:
    `docs/terminal-guides/dispatches/2026-07-31-developer-1-cook-slot-next-action-v1.md`
  - Developer 2 완료 응답:
    `docs/terminal-guides/dispatches/2026-07-31-developer-2-next-action-ui-consumption.md`
  - `none→대기`, `wait→회전/입력 잠금 대기`, `flip→뒤집기`,
    `retrieve→회수`; 접촉면 기반 행동 추측 제거
  - 조기 뒤집기 뒤 `flip/뒤집기`, 양면 완료 뒤에만 `retrieve/회수` 검증
- 최신 검증:
  - assets `12`, references `36`, 전체 Vitest `43파일·316/316`
  - S0 FHD/720 `30/30`, D1 FHD/720 `18/18`
- 소유 경계:
  - Developer 1의 cook domain·`slotViews()` 구현은 수정하지 않음
  - Artist source·review와 `runtimeRegistrationAllowed`는 수정하지 않음
  - manifest는 KEY promotion 도구만 원자적으로 갱신

## 작업 010 D1 면별 누적·손님 중심 제공 UI checkpoint

- epic: `docs/epic/developer-2/010_D1_면별_누적_표시와_손님_중심_제공_UX.md`
  / `v1.2.0` / 완료
- domain 입력: Developer 1 `slotViews()`의 `contactFace`, `frontElapsedSec`,
  `backElapsedSec`, `flipping`, `flipProgress`; business-day 선택 손님 `serve-item`
- UI port: `app/src/application/businessDay/d1BusinessDayUiPort.js`
  - 좌석별 `remainingItems`, `remainingOrderLabel`, `acceptedMenuIds`
  - 선택 메뉴와 좌석의 남은 주문 일치를 판정하는 `canServeD1MenuToSeat`
- 화면:
  - `app/src/d1-game.html`, `d1-game.css`, `d1-game.js`
  - 그릴 6칸 compact DOM은 앞/뒤 모양 아이콘과 텍스트, 양면 누적 초,
    `뒤집기|회수`, 조기 뒤집기 누적 보존, 0.3초 공중 `양면 정지`를 표시
  - 공용 완성품 카드 선택 뒤 일치하는 모든 좌석을 제공 가능으로 표시하며,
    특정 손님·첫 order line을 자동 선택하지 않음
  - 좌석 6개를 지속 DOM button으로 유지해 마우스·Enter/Space 제공 가능
  - `D1_GUIDED_ORDER_SEQUENCE` UI 오류·문구·기대 제거, 네기마 선제 제공 허용
- 검증:
  - `tests/integration/d1BusinessDayUiPort.test.js`
  - `tests/e2e/d1-face-serving-ux.spec.js`
  - 작업 010·기존 D1·전체 영업 FHD/720 `16/16`
  - 전체 Vitest `308/308`, 전체 verify 종료 코드 `0`
- 소유 경계: Developer 1 domain·renderer·저장 schema와 아트·manifest는 수정하지 않음

## Artist 2 S0 background promotion·GATE 단일 visual checkpoint

- contract: `app/src/assets/s0ExteriorBackgroundBindingContract.js` / `v3.0.0`
- 실제 S0 소비: `app/src/s0-d3.js`
- 현재 handoff: 두 finalizer `runtime-handoff.json` 검증 완료
- promotion: `BG-EXTERIOR-S0-CLOSED@R2-B1`, `BG-EXTERIOR-S0-GATE-OPEN@R1-B1`
  각각 별도 `dry-run → 30분 receipt 검증 → 동일 receipt write → exact binding` 완료
- CLOSED runtime: `/assets/core/s0/prologue/bg-exterior-s0-closed-r2-b1.png`,
  SHA `504bc88e874a53e6a69e7dcfaad11cc949b654166d17815cae15a9948e67be21`
- GATE runtime: `/assets/core/s0/prologue/bg-exterior-s0-gate-open-r1-b1.png`,
  SHA `c97f4c435a2d5ab78e8603879dfbad79a5a4569963d939b9ae354aefb42d6f6a`
- GATE runtime visual: `BG-EXTERIOR-S0-GATE-OPEN` 한 장
- `PR-SHOP-GATE-S0 R6`: background production provenance + interaction meaning/bounds only
  - FHD interaction `(720,224,480,624)`
  - 720 interaction `(480,149,320,416)`
  - runtime visual layer `false`, 별도 promotion `false`
- 자동 판정: open outline `1`/duplicate `0`, closed residual `0`, body `0`,
  action DOM intersection `0`, PR 미승격 상태에서 exact GATE background 정상 표시
- 승인 exact ID 적용 뒤 KEY/GATE `개발 중` placeholder 제거 완료
- 검증: assets 11, references 36, 전체 Vitest 42파일·300개,
  Chromium FHD/720 contract·실제 S0·공개 전환 34개 통과
- `PR-SHOP-GATE-S0` manifest entry 0, receipt는 각 write 성공 뒤 소비됨

## BG-WORKSPACE-DRINK promotion checkpoint

| 항목 | 현재 | handoff 적용 성공 기준 |
|---|---:|---:|
| finalizer handoff | `BG-WORKSPACE-DRINK@R2-B1` 적용 완료 | SHA·schema·승인·FHD/720·최적화 통과 |
| 전체 placeholder | `33` | `34→33` 충족 |
| drink placeholder | `3` | `4→3` 충족 |
| `data-missing-asset-ids` | 두 D1 진입점에서 해당 ID 제거 | 충족 |
| `unboundApprovedIds` | `[]` | 충족 |
| contract audit | `valid=true` | 충족 |

runtime URL은 `/assets/core/drink/bg-workspace-drink-r2-b1.png`, SHA는
`a30b2d44635ba52eef5d5461d4ea9b86ea80a2ab12e47f5377b58279dddeafce`다.
receipt는 write 성공 뒤 소비됐다. manifest·runtimeRegistrationAllowed는 수동 변경하지 않았다.

## Developer 1·Artist 1 그릴 UI 비침범 handoff

- contract: `app/src/config/grillUiLayout.js` / `GRILL_UI_LAYOUT_CONTRACT_VERSION v1.1.0`
- CSS: `app/src/grill-ui-layout.css` / `SCR-SVC-GRILL` active 전용
- harness: `app/src/grill-ui-harness.html`
- approved input:
  `art-workspace/review/artist-000/d1-cooking/grill/finished-tray/r6/assets/st-grill-finished-tray-fhd-r6.png`
  / SHA `51abf390cbf31d40b50a7d99f08a5d37a2da70df6cf94d405788538bad7d9184`
- 검증: assets 9, references 36, Vitest 42파일·299개, grill UI contract 10개,
  기본 Chromium FHD/720 grill UI 4개 통과
- 전체 verify에서 과거 4개 실패는 재현되지 않았다. 실행 중 병렬 변경 2건은 최신 파일의
  FHD/720 영향 테스트 4/4 재실행으로 해소했다.

| recipient | 고정 art/runtime 계약 | DOM/UI 계약 | interaction 계약 | 남은 gate |
|---|---|---|---|---|
| Developer 1 | R6 alpha FHD `(1534,123,266,354)`, gameplay rect·anchor는 Developer 1 단일 원본 | reserved FHD `(1502,91,330,418)` 침범 0; status·receipt·order·help는 `x≤1454`, prepared dock 접근 유지 | slot0~5 중심과 R6 visual/reserved 표본 `elementFromPoint=#scene`; visual/hit mesh 분리 | 최신 FHD/720 전체 영업·R6 hit/miss 4/4 통과 |
| Artist 1 | R3 카메라와 승인 `ST-GRILL-FINISHED-TRAY R6` 픽셀·SHA 유지 | semantic 주문·상태·도움말·품질 정보를 raster에 굽지 않음 | art는 click 판정을 소유하지 않음 | station FHD/720 최종 소비 화면 승인·finalizer handoff |

720p는 동일 `2/3 contain` 기준으로 R6 alpha `(1022.6666667,82,177.3333333,236)`,
reserved `(1001.3333333,60.6666667,220,278.6666667)`이다. status·receipt·order·help·dock의
교차 면적은 FHD/720 모두 각각 `0px²`다. R6는 최종 소비 화면 승인·finalizer 전까지
production runtime에 등록하지 않으며, manifest·runtimeRegistrationAllowed는 수정하지 않는다.

## Artist 2·3 공통 handoff

| recipient | contract slice | semanticOwner | camera·scale | topology compatibility | 다음 gate |
|---|---|---|---|---|---|
| Artist 2 / 023 | `S0_ART_BINDING_INVENTORY` 3행 | `artist-2.s0-prologue-story` | exterior/brazier fixed 16:9, FHD→720 `2/3 contain` | `review/artist-023/.../topology-contract.md`의 `unassigned` 필드 해소; 구형 phase 0, body 0 | topology 사용자 승인 뒤 `PR-SHOP-KEY / placed` 한 후보 |
| Artist 3 / 025 | `D1_DRINK_ART_BINDING_INVENTORY` 4행 + `D1_DRINK_VISUAL_VARIANTS` 5개 | `artist-3.d1-drink-service-cleanup-customer-settlement` | `D1-DRINK-FIXED-V1`, FHD→720 `2/3 contain` | `BG-WORKSPACE-DRINK R2` runtime 적용 완료, background-only·DOM 분리·body 0 유지 | `ST-DRINK-BEER-TIER-1` 단일 후보 제작 재개 |

이 인계는 생성·승인·runtime 등록 허가가 아니다. 승인 URL이 없으면 harness는 `개발 중`
placeholder를 유지하며 구형·유사 승인 자산을 대체 사용하지 않는다.

## Artist 2 외관 상태별 background handoff

| state | contract | asset / component / variant | owner / sourceMaster | camera / bounds | 제작 입력 | 검증·금지 |
|---|---|---|---|---|---|---|
| KEY `S0-STATE-KEY/exterior-key/S0-KEY-SELECT` | `app/src/assets/s0ExteriorBackgroundBindingContract.js` `v2.0.0`; `/src/s0-exterior-background-harness.html` | `BG-EXTERIOR-S0-CLOSED` / `prologue.exterior.background` / `closed`; background z0 | `artist-2.s0-prologue-story` / `CM-PROLOGUE-INHERITANCE-R1`; body `0` | `S0-EXTERIOR-FIXED-V1`; FHD visual `0,0,1920,1080`, DOM `128,936,1664,104`; 720 visual `0,0,1280,720`, DOM `85,624,1109,69`; interaction `null` | 중앙 대문 완전 닫힘 | KEY에 open ID 금지; exact ID 없으면 placeholder |
| GATE `S0-STATE-GATE/gate-open/S0-GATE-OPEN` | 위 contract/harness | `BG-EXTERIOR-S0-GATE-OPEN` / `prologue.exterior.background` / `gate-open-empty-interior`; background z0 | KEY와 동일; body `0` | KEY와 동일 | 같은 외관에서 중앙 개구부 열림·빈 실내; 승인 `PR-SHOP-GATE-S0/open R6` 입력 | GATE에 closed ID·closed overlay·닫힌 문 잔존 픽셀 금지; FHD/720 harness 8개 통과 |
