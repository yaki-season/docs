# Artist 2 직전 작업 기록

- 마지막 갱신: `2026-07-30`
- 담당 역할: `Artist 2 / S0-PROLOGUE-STORY`
- 작성한 터미널: 현재 Artist 2 복구 세션

## 저장소 체크포인트

| 저장소 | branch | HEAD | 담당 dirty·untracked 경로 | 다른 역할 변경 주의 |
|---|---|---|---|---|
| `docs` | `main` | `d22cb47` | `terminal-guides/`의 미추적 handoff·dispatch, 그 밖의 다수 변경 | `epic/`, `spec/`의 기존 변경을 수정하지 않음 |
| `app` | `main` | `f22b546` | 없음 | D1·S0-D3 구현 변경 다수, 읽기 전용 |
| `art-workspace` | `main` | `b6ada20` | `review/artist-023/s0-prologue/exterior-key/preflight/topology-contract.md` | `review/artist-000/`, `review/artist-024/`, `review/artist-025/` 수정 금지 |

## 작업 체크포인트

- 현재 담당 epic·버전: Artist 2 작업 `023 v1.1.0` (`진행 중`)
- 마지막 완료 동작: R1은 `YS-HANDCRAFTED-NIGHT-v1`의 픽셀 요구를 위반한 painterly 결과여서
  사용자 검토 전 철회했다. `ART-002 v3.6.1`의 2D 픽셀 이미지·굵은 픽셀 군집·계단 윤곽·제한 팔레트·
  무안티앨리어싱 기준과 01 shop exterior의 무드만 다시 적용해 `BG-EXTERIOR-S0-CLOSED / closed` R2
  한 장으로 교체했다. R2는 1920×1080 opaque RGB이고 중앙 대문이 완전히 닫혀 실내·열린 구멍·R6
  입력이 없다. KEY exact ID·camera·full-frame bounds·DOM safe rect는 FHD/720 approved harness에서
  통과했다. runtimeRegistrationAllowed는 `false`다.
- 마지막 완료 동작: 사용자가 pixel closed R2와 FHD/720 재조립을 승인했다. 같은
  `S0-EXTERIOR-FIXED-V1` 카메라·파사드·골목·픽셀 팔레트를 보존하고, 승인 R6의 양개문 열림 기하를
  input으로 사용한 `BG-EXTERIOR-S0-GATE-OPEN / gate-open-empty-interior` R1 한 장만 만들었다.
  중앙 개구부는 비어 있고 차가운 실내를 보이며 closed background의 문 pixel이 남지 않는다.
  GATE exact ID·camera·full-frame bounds·DOM safe rect는 FHD/720 approved harness에서 통과했다.
  runtimeRegistrationAllowed는 `false`다.
- 승인 상태: pixel closed R2 및 FHD/720 재조립, pixel GATE-OPEN R1 및 FHD/720 재조립은 모두
  사용자 승인됨. R1 painterly closed background는 사용자 검토 전 style violation으로 철회됐다.
- 현재 단일 작업 또는 단일 후보: 없음. 두 state-specific exterior background가 승인됐으며,
  실제 key/gate 전체 화면 finalizer 범위 확인을 기다린다.
- 현재 후보 SHA: isolated FHD `c97f4c435a2d5ab78e8603879dfbad79a5a4569963d939b9ae354aefb42d6f6a`;
  harness FHD `1f3d1a5a973a5eb9b675e517c93b441b2d085c86e40d74d6eca5b4ac3e2c7ed6`;
  harness 720 `824527e6e493e1fe80a0917f721eb48943aedc137e2c3b32623ea8c9b9c37ca1`.
- 승인 상태: topology R2·isolated key R1·FHD/720 key 재조립·isolated gate R5·R6·R6 FHD/720 계약 재조립은 모두 사용자 승인됨. R1은 반려, R2·R3는 후속 revision으로 대체, R4는 alpha 차폐 실패로 폐기했고 R5 FHD/720 재조립은 R6 높이 revision으로 대체했다. 실제 외관을 포함한 조립은 background contract 입력을 기다린다.
- blocker 상태: 해소. 오류가 있던 shared closed contract `v1.0.0`은 폐기됐고 Developer 2 작업
  008의 state-specific background contract `v2.0.0`만 유효하다.

  | 필드 | KEY | GATE |
  |---|---|---|
  | state | `S0-STATE-KEY / exterior-key / S0-KEY-SELECT` | `S0-STATE-GATE / gate-open / S0-GATE-OPEN` |
  | `componentId` | `prologue.exterior.background` | `prologue.exterior.background` |
  | `requiredAssetId` | `BG-EXTERIOR-S0-CLOSED` | `BG-EXTERIOR-S0-GATE-OPEN` |
  | `stateVariant` | `closed` | `gate-open-empty-interior` |
  | 시각 의미 | 중앙 대문 완전 닫힘 | 같은 점포·골목에서 중앙 개구부 열림, 빈 실내가 보임 |
  | `sourceMasterId` | `CM-PROLOGUE-INHERITANCE-R1` | `CM-PROLOGUE-INHERITANCE-R1` |
  | `camera` | `S0-EXTERIOR-FIXED-V1`, fixed 16:9 contain | 왼쪽과 동일 |
  | FHD `visualBounds` | `(0,0,1920,1080)` | 왼쪽과 동일 |
  | 720 `visualBounds` | `(0,0,1280,720)` | 왼쪽과 동일 |
  | `layer/zOrder` | `background / 0` | `background / 0` |
  | DOM safe FHD / 720 | `(128,936,1664,104)` / `(85,624,1109,69)` | 왼쪽과 동일 |
  | body | `0` | `0` |
  | 제작 입력 | 닫힌 외관 정본 | 승인 `PR-SHOP-GATE-S0 / open R6` |
- 알려진 실패·재현 명령: 없음. 수정 contract·owner·작업 007 회귀 Vitest 12개와
  FHD/720 state-specific harness 8개가 통과했다.
- 마지막 검증 명령: app `/src/s0-exterior-background-harness.html?stateId=S0-STATE-GATE&mode=approved`에
  R1 GATE data URL을 주입한 Playwright FHD/720 capture, PNG 규격·SHA-256 확인.
- 마지막 검증 결과: FHD/720 모두 approved mode·contract `v2.0.0`·requiredAssetId exact
  `BG-EXTERIOR-S0-GATE-OPEN`·`S0-EXTERIOR-FIXED-V1`·`SCR-STORY-PROLOGUE:S0-STATE-GATE`·body `0`을
  유지했고 full-frame visual/DOM safe rect를 통과했다. CLOSED ID는 forbidden으로 확인했으며,
  manifest 등록·runtime promotion은 실행하지 않았다.
- Wave 1 금지: background 승인 전 key/gate 실제 exterior 합성·최적화·finalizer·runtime handoff를 하지 않는다. `topology-registry.json`, `ASSET-CATALOG.md`, app contract/harness/manifest, dashboard를 수정하지 않는다. 숯·화로·아키 초상은 exterior cycle 완료 전 시작하지 않는다. 모든 `runtimeRegistrationAllowed`는 `false`를 유지한다.
- 수정 인계 후 후보 순서: (1) `BG-EXTERIOR-S0-CLOSED` 배경 한 장만 첫 후보로 제작·사용자 승인
  요청한다. (2) 그 승인 뒤에만 같은 `S0-EXTERIOR-FIXED-V1` 카메라의
  `BG-EXTERIOR-S0-GATE-OPEN` 소비 화면 한 장을 두 번째 후보로 제작한다. 승인된
  `PR-SHOP-GATE-S0 / open` R6을 열린 대문 입력으로 재사용하며 새 대문을 만들지 않는다.
  단, closed 배경 위 단순 overlay는 금지하며 닫힌 문 픽셀이 하나라도 남지 않는 full-frame
  background 재조립으로 제출한다.
- 다음 한 동작: approved background 소비 규칙과 finalizer 범위를 별도로 확인한다. 실제 key/gate
  전체 화면 finalizer·최적화·runtime handoff 및 숯·화로·아키 작업은 그 확인 전까지 하지 않는다.
- 수정 금지·읽기 전용 경로: `review/artist-000/`, `review/artist-024/`, `review/artist-025/`, 공유 `CM-PROLOGUE-INHERITANCE-R1` master.
- 다른 컴퓨터 이동 시 아직 전달되지 않은 파일: 승인 대기 topology contract는 `art-workspace/review/artist-023/.../topology-contract.md` 한 파일이며, Git/LFS 또는 승인된 보안 전송 없이는 전달되지 않는다고 가정한다.

## 재개 시 검증

- [ ] dashboard와 epic 상태가 이 기록보다 최신인지 확인
- [ ] 세 저장소 branch·HEAD 확인
- [ ] dirty·untracked 파일 실재 확인
- [ ] `UI-003 v1.2.0`의 S0 3 phase와 작업 007 최신 버전 확인
- [x] Developer 2 state-specific background contract `v2.0.0`·harness의 실제 경로와 테스트 결과 확인
- [ ] 복구 보고 후 다음 한 동작만 시작

## Developer 2 incoming contract — v2.0.0 수정 인계

Developer 2 작업 008 `v2.0.0`이 shared closed 의미 오류를 폐기했다. 아래 두 행만 유효하며
Artist 2가 candidate metadata와 FHD/720 재조립 입력으로 그대로 사용한다.

| state 소비 | asset / component | owner / sourceMaster | variant / layer | camera | FHD | 720 | 제작 규칙 |
|---|---|---|---|---|---|---|---|
| `S0-STATE-KEY / exterior-key / S0-KEY-SELECT` | `BG-EXTERIOR-S0-CLOSED` / `prologue.exterior.background` | `artist-2.s0-prologue-story` / `CM-PROLOGUE-INHERITANCE-R1` | `closed` / background z0 | `S0-EXTERIOR-FIXED-V1`, fixed 16:9 contain | visual `0,0,1920,1080`; DOM `128,936,1664,104`; interaction `null` | visual `0,0,1280,720`; DOM `85,624,1109,69`; interaction `null` | 중앙 대문 완전 닫힘; open ID 사용 금지; body `0` |
| `S0-STATE-GATE / gate-open / S0-GATE-OPEN` | `BG-EXTERIOR-S0-GATE-OPEN` / `prologue.exterior.background` | `artist-2.s0-prologue-story` / `CM-PROLOGUE-INHERITANCE-R1` | `gate-open-empty-interior` / background z0 | KEY와 동일 | KEY와 동일 | KEY와 동일 | 같은 외관의 개구부 열림·빈 실내; `PR-SHOP-GATE-S0/open R6` 입력; closed overlay·잔존 픽셀 금지; body `0` |

검증 harness는
`/src/s0-exterior-background-harness.html?stateId=S0-STATE-KEY&mode=placeholder`이며
`S0-STATE-GATE`와 `mode=approved`도 지원한다. exact ID가 없거나 반대 상태 ID만 있으면
`개발 중` placeholder를 유지한다. Chromium FHD/720 8개가 통과했다.

## 2026-07-30 — approved S0 exterior background formal pipeline delivery

현재 app의 state-specific exterior binding은 `v3.0.0`이며, v2.0.0의 ID·camera·bounds를 유지하면서
GATE runtime visual을 background 한 장으로 제한한다. 다음 두 bundle은 finalizer가 각각 원자적으로
생성했다. Developer 2는 **각 handoff별로** dry-run → receipt → write 순서를 실행하고, Artist 2는 app
manifest·asset copy·promotion을 수행하지 않는다.

| 대상 | finalizer runtime handoff | bundle SHA (`runtime-handoff.json`) | FHD / 720 승인 evidence |
|---|---|---|---|
| `BG-EXTERIOR-S0-CLOSED R2` / `S0-STATE-KEY` | `art-workspace/review/artist-023/s0-prologue/exterior-key/bg-exterior-s0-closed/closed/r2/metadata/runtime-handoff.json` | `bc7f2777b93c13ecb83d98b3c70fafd40906ef3f7a041f2c53af1b5acd808321` | `6db56ab95d8e914d1247e6db7f28dcec78bd5cb174ecab06a85eb563c27df23f` / `ce827403c9dfc2f82706a8799dff1be4f77fbc7101fa4f0155a399e0c972de6c` |
| `BG-EXTERIOR-S0-GATE-OPEN R1` / `S0-STATE-GATE` | `art-workspace/review/artist-023/s0-prologue/gate-open/bg-exterior-s0-gate-open/gate-open/r1/metadata/runtime-handoff.json` | `928904ffdc786941f4f3ec3a5653a02bf9a6deee1c53cdfd956439a77794bb02` | `1f3d1a5a973a5eb9b675e517c93b441b2d085c86e40d74d6eca5b4ac3e2c7ed6` / `824527e6e493e1fe80a0917f721eb48943aedc137e2c3b32623ea8c9b9c37ca1` |

두 bundle은 `artist-2.s0-prologue-story`, `CM-PROLOGUE-INHERITANCE-R1`, fixed FHD/720 bounds,
`background/z0`, `interactionBounds=null`, body `0`, DOM safe FHD `128,936,1664,104` / 720
`85,624,1109,69`를 다시 검증했다. 각 `runtimeRegistrationAllowed=true`는 candidate/completion report를
수정한 값이 아니라 finalizer의 파생 output이다.

GATE handoff에는 `PR-SHOP-GATE-S0 R6`가 bundle artifact로 없고, R6는 provenance·열린 문 기하·interaction
의미 증거로만 남는다. GATE background에는 열린 문과 개구부가 이미 한 번 포함되어 있어 R6 finalizer,
runtime handoff, manifest promotion, second overlay를 만들지 않았다.

`S0-STATE-KEY` 실제 소비 검수는 `art-workspace/review/artist-023/s0-prologue/exterior-key/consumption/closed-key/r1/KEY-CONSUMPTION-REPORT.md`에
`pending-user-review`로 준비했다. 승인된 CLOSED R2 z0와 KEY R1 z40만 harness DOM 위에 재조립했으며,
KEY finalizer는 사용자 최종 화면 승인 전까지 금지다.

`ST-S0-BRAZIER`는 `art-workspace/review/artist-023/s0-prologue/ignite/st-s0-brazier/preflight/CONTRACT.md`에
preflight만 작성했다. 이번 갱신에는 새 brazer/charcoal/Aki 픽셀이 없다.

검증: 두 bundle finalizer는 성공했다. 전체 `validate-pipeline.mjs`는 다른 역할의 기존 27개 schema/SHA
오류와, Artist 3 소유이며 수정 금지인 `CM-PROLOGUE-INHERITANCE-R1` registry의 `pending-user-review` 때문에
29개 오류를 보고한다. Artist 2 bundle의 추가 오류는 두 registry 상태 오류뿐이며, registry·catalog·app에는
수정하지 않았다.

## 2026-07-30 — KEY actual-consumption approval finalizer delivery

사용자가 CLOSED R2 z0 + KEY R1 z40의 실제 FHD/720 소비 화면을 승인했다. `PR-SHOP-KEY`는 transparent
raster이므로 complete-layer completion report가 아니라 pipeline 정본 `standalone-raster-report.json`을
profile approval으로 사용했다. finalizer가 파생한 Developer 2 입력은 다음과 같다.

| 대상 | runtime handoff | bundle SHA | primary SHA |
|---|---|---|---|
| `PR-SHOP-KEY R1` / `S0-STATE-KEY` | `art-workspace/review/artist-023/s0-prologue/exterior-key/pr-shop-key/placed/r1/metadata/runtime-handoff.json` | `ba6cd227d7041e45df8d98e7c29dff3d4362d4608c5304390a559478f6d8c40f` | `a8a22c3beaa6a4ffa1b45dad6eb4438eda729d325740ad9bcb265d3363229eeb` |

정확한 소비 증거는 `S0-EXTERIOR-FIXED-V1`, FHD visual `256,650,224,150`, interaction
`224,614,288,222`, 720 visual `171,433,149,100`, interaction `149,409,192,148`, DOM safe
FHD `128,936,1664,104` / 720 `85,624,1109,69`, body `0`을 유지한다. derived handoff만
`runtimeRegistrationAllowed=true`이고 candidate/profile/provenance는 `false`다.

Developer 2는 이 KEY handoff도 dry-run → receipt → write로 별도 promotion한다. Artist 2는 app
manifest나 asset copy를 수정하지 않았다. promotion receipt가 완료되기 전에는 `ST-S0-BRAZIER` 후보를
제작하지 않으며, 이미 작성한 preflight만 유효하다.

### PM 재검수 승인 확인

`2026-07-30` PM KEY 소비 화면 재검수에서 FHD/720 후보가 다시 사용자 승인됐다. 이미 같은 승인 원본과
소비 증거로 생성된 `PR-SHOP-KEY@R1-B1` handoff를 확정 promotion 대상으로 유지한다. 기존 evidence SHA를
변경하거나 동일 source revision/runtime build의 finalizer를 중복 생성하지 않았다.

## 2026-07-30 — KEY promotion 대기 / BRAZIER versioned contract 요청

- KEY promotion: **미확인**. `app/public/assets/manifest.json`, `app/assets/`,
  `app/.asset-promotion-receipts/`, Developer 2 handoff에서 `PR-SHOP-KEY@R1-B1`의 manifest entry·write
  receipt·runtime file을 찾지 못했다. 기존 handoff는 수정하지 않는다.
- BRAZIER 요청서: `art-workspace/review/artist-023/s0-prologue/ignite/st-s0-brazier/preflight/DEVELOPER-2-CONTRACT-REQUEST-v1.0.0.md`
- 미확정 항목: 3개 — `sourceMasterId`, primary/companion pixel 분리 규칙,
  `PR-CHARCOAL-IGNITION` FHD/720 child visual bounds. layer/z와 runtime component/variant는 기존 table의
  값을 재확인할 version stamp 대상으로 요청서에 포함했다.
- 판정: KEY promotion receipt와 Developer 2 versioned handoff, 사용자 preflight 승인이 모두 없으므로
  `ST-S0-BRAZIER` 제작은 **불가**. 신규 화로·숯·점화 VFX·아키 픽셀은 만들지 않았다.

## 2026-07-31 — S0 gate audit / `CH-AKI-STORY` 무픽셀 preflight

- KEY audit: `PR-SHOP-KEY@R1-B1`은 manifest·`app/assets`·promotion receipt에 없으므로 write/promotion
  **미확인**이다. existing finalizer와 handoff는 변경·중복 생성하지 않았다.
- BRAZIER audit: Developer 2의 신규 versioned BRAZIER handoff는 발견되지 않았다. 기존 unassigned 3개
  (`sourceMasterId`, primary/companion pixel split, `PR-CHARCOAL-IGNITION` child FHD/720 bounds)는 모두
  미해소이며, envelope/interaction은 변경하지 않았다.
- Aki preflight: `art-workspace/review/artist-023/s0-prologue/story/ch-aki-story/preflight/CONTRACT.md`
- Aki Developer 2 요청: `art-workspace/review/artist-023/s0-prologue/story/ch-aki-story/preflight/DEVELOPER-2-CONTRACT-REQUEST-v1.0.0.md`
- Aki는 story/settlement 전용이고 `CH-OWNER-STORY`를 재사용·runtime 등록하지 않는다. bounds·layer·DOM safe가
  아직 unassigned이므로 이미지·표정 variant·runtime handoff를 만들지 않았다.

## Developer 2 follow-up — v3.0.0 GATE 단일 visual 정정

- contract: `app/src/assets/s0ExteriorBackgroundBindingContract.js` / `v3.0.0`
- `S0-STATE-GATE` runtime visual은 `BG-EXTERIOR-S0-GATE-OPEN` 한 장뿐이다.
- background 안에 열린 대문 픽셀이 포함되므로 `PR-SHOP-GATE-S0`를 같은 화면에 다시
  그리거나 별도 runtime asset으로 finalizer/promotion하지 않는다.
- 승인 R6는 background production provenance와 interaction 의미·bounds 기준으로만 유지한다.
  FHD `(720,224,480,624)`, 720 `(480,149,320,416)`이다.
- Developer 2가 기다리는 입력은 두 개의 독립 handoff다.
  1. `BG-EXTERIOR-S0-CLOSED` finalizer `runtime-handoff.json`
  2. `BG-EXTERIOR-S0-GATE-OPEN` finalizer `runtime-handoff.json`
- 각 handoff에는 final approval·completion·recomposition·optimization·bundle manifest·실제
  SHA가 필요하다. 두 asset의 receipt와 write transaction은 공유하지 않는다.
- 현재 `review/artist-023`에는 handoff가 0개라 promotion은 실행하지 않았다.

## 2026-07-30 — Developer 2 runtime promotion·exact binding 완료

두 background는 서로 receipt를 공유하지 않고 각각
`dry-run → 30분 receipt 검증 → 동일 receipt write → exact state binding`을 완료했다.

| state | runtime asset | URL | SHA-256 | placeholder |
|---|---|---|---|---|
| `S0-STATE-KEY` | `BG-EXTERIOR-S0-CLOSED@R2-B1` | `/assets/core/s0/prologue/bg-exterior-s0-closed-r2-b1.png` | `504bc88e874a53e6a69e7dcfaad11cc949b654166d17815cae15a9948e67be21` | exact 승인 뒤 제거 |
| `S0-STATE-GATE` | `BG-EXTERIOR-S0-GATE-OPEN@R1-B1` | `/assets/core/s0/prologue/bg-exterior-s0-gate-open-r1-b1.png` | `c97f4c435a2d5ab78e8603879dfbad79a5a4569963d939b9ae354aefb42d6f6a` | exact 승인 뒤 제거 |

GATE는 승인 background 한 장과 DOM/scene hit target만 소비한다. `PR-SHOP-GATE-S0 R6`는
provenance·interaction 의미/bounds reference이며 manifest entry·별도 runtime visual·promotion이
모두 없다. 자동 계약은 runtime visual `1`, open outline duplicate `0`, closed residual `0`,
body part `0`, action DOM 교차 `0`을 확인했다.

검증은 runtime assets 11개, reference 36장, 전체 Vitest 42파일·300개,
Chromium 1280×720/1920×1080 S0 contract·실제 시나리오·공개 전환 34개가 통과했다.

## 2026-07-31 — Developer 2 작업 011 수신 감사 / 화로 사용자 preflight 준비

- KEY promotion: **확인 완료**. `PR-SHOP-KEY@R1-B1` manifest exact ID는 `PR-SHOP-KEY`, runtime URL은
  `/assets/core/s0/prologue/pr-shop-key-placed-r1-b1.png`, runtime SHA-256은
  `a8a22c3beaa6a4ffa1b45dad6eb4438eda729d325740ad9bcb265d3363229eeb`이다. Artist 2 finalizer handoff SHA
  `ba6cd227d7041e45df8d98e7c29dff3d4362d4608c5304390a559478f6d8c40f`와 일치한다.
- receipt: Developer 2 작업 011의 `dry-run → receipt 검증 → 동일 receipt write` 완료 기록을 수신했다.
  `app/.asset-promotion-receipts/`에는 소비 전 `PR-SHOP-KEY` receipt가 남지 않아 write 뒤 receipt 소비 규칙과
  일치한다. 기존 KEY finalizer와 runtime handoff는 수정·재생성하지 않았다.
- exact binding / placeholder: runtime은 `S0-STATE-KEY`에서만 exact `PR-SHOP-KEY`를 해석하고, 이 ID가 없으면
  승인 CLOSED background가 있어도 interaction 및 전체 visual을 `placeholder`로 남긴다. 현재 manifest exact ID와
  asset이 존재해 placeholder가 제거된다. `S0_D1_ART_BINDING_CONTRACT v1.1.0` 검증 결과는 오류 `0`이다.
- BRAZIER contract: Developer 2 `S0_BRAZIER_LAYER_CONTRACT v1.0.0`을 감사했다. 기존 미확정 3개와 수락 조건을
  포함한 6개 항목이 모두 해소됐다: `sourceMasterId`, primary/companion pixel 분리, companion FHD/720 child
  visualBounds, layer/zOrder, component/variant, no-double-render. envelope·interaction·DOM safe·body `0`도
  기존 값 그대로다. 따라서 계약 blocker는 `0`개다.
- 사용자 검수용 단일 preflight: `art-workspace/review/artist-023/s0-prologue/ignite/st-s0-brazier/preflight/USER-REVIEW-CONTRACT-v1.0.0.md`
  — 이번 검수 대상은 차가운 `ST-S0-BRAZIER` primary 한 장뿐이며, 보이는 숯·VFX·손·도구·아키는 제외한다.
- Aki: 기존 `CH-AKI-STORY` 무픽셀 preflight와 Developer 2 계약 요청만 유지한다. portrait binding은 아직 수신되지
  않았으므로 pixel·runtime 등록·구형 `CH-OWNER-STORY` 재사용은 하지 않았다.
- 다음 단일 gate: 사용자가 위 BRAZIER preflight를 승인할 때만 차가운 primary 후보 한 장을 제작한다.
