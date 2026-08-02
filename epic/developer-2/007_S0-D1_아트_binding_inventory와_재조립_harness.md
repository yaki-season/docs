# 개발자 2 - 007 S0·D1 아트 binding inventory와 재조립 harness

- 상태: `완료`
- 담당자: `개발자 2`

> ⚠ 2026-08-02 S0 점화 계약 갱신: S0 프롤로그 상호작용은 `S0-KEY-SELECT → S0-GATE-OPEN`의 2클릭(KEY→GATE)이고 숯 점화는 story dialogue로 처리한다. 세 번째 상태 `S0-STATE-CHARCOAL / ignite / S0-CHARCOAL-IGNITE`와 그 art `ST-S0-BRAZIER`/`PR-CHARCOAL-IGNITION`은 deprecated이며 더 이상 live S0 art로 요구되지 않는다. 아래 inventory·harness 기록은 당시 3상태 기준이다(app 재정합은 별도 Dev2 작업).

## 참조 spec

- `spec/system/SYS-002_2.5D_스테이션_렌더링과_레이어_계약.md` - `SYS-002` - `v3.0.0`
- `spec/ui/UI-002_전체_게임_화면_입력_접근성.md` - `UI-002` - `v5.25.0`
- `spec/ui/UI-003_전체_게임_상세_화면_설계.md` - `UI-003` - `v1.2.0`
- `spec/art/ART-003_런타임_아트_에셋_목록과_제작_계약.md` - `ART-003` - `v5.8.0`
- `epic/artist/023_S0_프롤로그_정식_아트.md` - `Artist 2 / 023` - `v1.1.0`
- `epic/artist/025_D1_드링크_서빙_정리_엑스트라_정산_정식_아트.md` - `Artist 3 / 025` - `v1.0.0`

## 목표

완료된 S0~D3 임시 UI와 진행 중 D1 영업 화면에서 Artist 2·3이 추측 없이 제작할 수 있도록
`screenId/stateId/componentId/requiredAssetId/stateVariant/bounds/layer` 계약과 FHD/720 재조립
harness를 제공한다.

## 범위

### 포함

- S0 상태 (현행 계약은 아래 두 상태; 세 번째는 deprecated):
  - `S0-STATE-KEY / exterior-key / S0-KEY-SELECT`
  - `S0-STATE-GATE / gate-open / S0-GATE-OPEN`
  - ~~`S0-STATE-CHARCOAL / ignite / S0-CHARCOAL-IGNITE`~~ (deprecated 2026-08-02 · 점화는 story dialogue로 대체, live art 미요구)
- S0의 `SCR-STORY-PROLOGUE`, 각 component의 required asset·variant·FHD/720
  `visualBounds`·`interactionBounds`·layer/z-order와 cover/crop 규칙
- D1 드링크·서빙·정리·엑스트라·정산의 현재 runtime binding과 placeholder inventory
- `BG-WORKSPACE-DRINK`, `ST-DRINK-BEER-TIER-1`, `MDL-BEER-GLASS`,
  `MDL-BEER-LEVER`부터 시작하는 Artist 3 stable ID binding
- 개발자 1 작업 009가 제공하는 gameplay state inventory와 runtime resolver의 양방향 누락 대조
- 승인 asset을 넣기 전·후 동일 상태를 FHD/720으로 캡처하는 재조립 harness

### 제외

- 아트 생성·수정·승인 — Artist 2·3 책임
- S0 대사·클릭 상태 흐름 변경 — 완료된 개발자 2 작업 004의 계약 유지 (현행 KEY→GATE 2클릭+점화 대사)
- D1 영업·정산 계산·저장 — 개발자 1 작업 009
- manifest 승격·영수증 — 개발자 2 작업 003

## 작업 절차

1. 앱의 실제 S0 interaction/state/screen/phase ID와 D1 resolver 누락 ID를 추출한다.
2. 각 state에 단일 component ID, required asset ID·variant, 두 해상도 bounds와 layer를 부여한다.
3. `UI-003`의 S0 phase와 구현 클릭이 1:1인지 자동 검증한다. (당시 3 phase·3클릭 기준; 현행 계약은 KEY→GATE 2 phase·2클릭이고 점화는 대사, 상단 갱신 참조)
4. 개발자 1 작업 009 inventory와 병합해 producer/semanticOwner별 placeholder report를 만든다.
5. 승인 전 placeholder와 승인 후 manifest asset을 같은 camera·DOM으로 캡처하는 FHD/720 harness를 만든다.
6. Artist 2·3에 versioned inventory를 인계하고 ID 변경 시 호환성 영향을 기록한다.

## 의존성과 인계 조건

- 선행 작업: 완료된 개발자 2 작업 004
- 병행 작업: 개발자 1 작업 009, 개발자 2 작업 003, Artist 2 작업 023, Artist 3 작업 025
- 후속 작업: 개발자 2 작업 006 최초 공개 gate
- 첫 인계: S0 세 상태와 D1 드링크 4개 ID. 나머지는 구현 ID가 동결되는 순서대로 증분한다.
- Artist는 inventory에 없는 ID를 만들지 않고 `unassigned`로 되돌려 보낸다.

## 완료 기준

- [x] S0 세 state의 component·required asset·variant·FHD/720 bounds·layer가 모두 고정된다.
- [x] 구형 `interior-check`·`note` 직접 클릭 phase가 S0 interaction inventory에 0개다.
- [x] D1 비조리 placeholder가 producer·state·component·asset ID별로 자동 집계된다.
- [x] 동일 stable ID의 semanticOwner가 둘 이상이면 검증이 실패한다.
- [x] FHD/720 재조립 harness가 placeholder와 승인 manifest asset 양쪽을 같은 상태로 캡처한다.
- [x] Artist 2·3이 review 경로나 파일명을 추측하지 않고 inventory만으로 handoff를 만들 수 있다.

## 구현 및 검증 결과

- app 구현 위치:
  - versioned contract: `app/src/assets/s0D1ArtBindingContract.js`
  - S0 실제 binding 관찰: `app/src/s0-d3.js`
  - 동일 camera·DOM harness: `app/src/art-recomposition-harness.html`,
    `app/src/art-recomposition-harness.js`, `app/src/art-recomposition-harness.css`
  - contract 자동 검증: `app/tests/unit/s0D1ArtBindingContract.test.js`
  - FHD/720 캡처: `app/tests/e2e/art-recomposition-harness.spec.js`
- 구현 기준 spec 버전: 위 참조 spec
- 구현 기준 태스크 버전: `v1.1.0`
- 검증 방법: schema·단위·Playwright FHD/720·manifest/placeholder 대조
- 검증 결과:
  - contract `v1.0.0`: S0 3행, D1 드링크 4행, 드링크 시각 variant 5개
  - `npm run assets:validate`: runtime asset 8개 통과
  - `npm run visual:references:validate`: 참조 이미지 36장 통과
  - 전체 Vitest: 30파일·237개 통과
  - 관련 Playwright: contract harness+S0 시나리오 FHD/720 30개 통과
  - D1 드링크 운영 Playwright: 9개 통과, FHD 홀드 1개는 1회 재시도 뒤 통과
  - 전체 `npm run verify`: assets·references·Vitest와 이번 범위 E2E는 통과했으나 개발자 1 소유
    그릴 tuner·gauge·slot reset 및 FHD drink hold의 기존 병행 회귀 7건으로 최종 종료 코드 1
- 남은 위험:
  - Artist 2·3 topology는 계속 `pending-user-review`이며 이 contract가 생성·승인을 허가하지 않는다.
  - 승인 자산 URL은 finalizer handoff 이후에만 harness와 runtime이 소비할 수 있다.
  - D1 정산 5단계의 세부 component ID는 개발자 1 작업 009의 화면 구현이 동결될 때 증분 contract로
    이어야 한다.

## versioned contract

- contract 버전: `v1.0.0`
- 논리 canvas: `1920×1080`; 720 소비는 모든 좌표를 정확히 `2/3`로 축소하고 `contain`한다.
- S0 semanticOwner: `artist-2.s0-prologue-story`
- D1 드링크 semanticOwner: `artist-3.d1-drink-service-cleanup-customer-settlement`
- 승인 URL이 없으면 harness는 가까운 승인 자산이나 구형 자산을 대신 쓰지 않고 `개발 중`
  placeholder를 유지한다.

### S0 첫 인계

| state / phase / interaction | componentId | requiredAssetId / variant | FHD visual / interaction | 720 visual / interaction | layer / z | DOM safe rect FHD / 720 |
|---|---|---|---|---|---|---|
| `S0-STATE-KEY / exterior-key / S0-KEY-SELECT` | `prologue.key` | `PR-SHOP-KEY / placed` | `256,650,224,150 / 224,614,288,222` | `171,433,149,100 / 149,409,192,148` | `interactable / 40` | `128,936,1664,104 / 85,624,1109,69` |
| `S0-STATE-GATE / gate-open / S0-GATE-OPEN` | `prologue.gate` | `PR-SHOP-GATE-S0 / open` | `656,176,608,704 / 720,224,480,624` | `437,117,405,469 / 480,149,320,416` | `architecture / 20` | `128,936,1664,104 / 85,624,1109,69` |
| ~~`S0-STATE-CHARCOAL / ignite / S0-CHARCOAL-IGNITE`~~ (deprecated 2026-08-02 · 점화 대사 대체, live art 미요구) | `prologue.brazier-and-charcoal` | ~~`ST-S0-BRAZIER / cold-to-ignited`; companion `PR-CHARCOAL-IGNITION / off-to-stable`~~ (deprecated) | `648,376,624,432 / 752,480,416,288` | `432,251,416,288 / 501,320,277,192` | `architecture / 20`; VFX `50` | `128,936,1664,104 / 85,624,1109,69` |

### D1 드링크 첫 인계

| componentId | requiredAssetId / variant | FHD visual / interaction | 720 visual / interaction | layer / z | DOM safe rect FHD / 720 |
|---|---|---|---|---|---|
| `drink.scene` | `BG-WORKSPACE-DRINK / base-empty-workspace` | `0,0,1920,1080 / none` | `0,0,1280,720 / none` | `background / 0` | `176,104,1568,144 / 117,69,1045,96` |
| `drink.station` | `ST-DRINK-BEER-TIER-1 / single-lever-empty` | `240,288,1152,528 / none` | `160,192,768,352 / none` | `architecture / 20` | `176,104,1568,144 / 117,69,1045,96` |
| `drink.glass` | `MDL-BEER-GLASS / empty` | `792,424,224,336 / 752,392,304,400` | `528,283,149,224 / 501,261,203,267` | `interactable / 40` | `176,104,1568,144 / 117,69,1045,96` |
| `drink.lever` | `MDL-BEER-LEVER / idle` | `1152,432,176,184 / 1120,400,240,248` | `768,288,117,123 / 747,267,160,165` | `interactable / 42` | `176,104,1568,144 / 117,69,1045,96` |

드링크 `empty`, `fill-70`, `fill-100`, `overflow`, `finished`는
`MDL-BEER-GLASS@z40 + TEX-BEER-LIQUID@z44 + VFX-BEER-CORE@z50`의
`stateVariant` 조립이다. contract에는 품질·시간·제공 가능 여부 판정이 없다.

## Artist topology compatibility

| 대상 | 읽기 전용 대조 문서 | 결과 | 인계 효과 |
|---|---|---|---|
| Artist 2 / 023 | `art-workspace/review/artist-023/s0-prologue/exterior-key/preflight/topology-contract.md` | 호환 | `unassigned`였던 S0 3상태 component·variant·FHD/720 bounds·interaction·DOM safe·z-order를 contract `v1.0.0`으로 해소. 기존 `pending-user-review`·이미지 생성 금지는 유지 |
| Artist 3 / 025 | `art-workspace/review/artist-025/preflight/bg-workspace-drink-spatial-inference-topology-r1.md` | 호환 | 같은 `1920×1080` 논리 camera와 720 `2/3 contain`, body 0, background→station→model→VFX→DOM 순서를 고정. 기존 `pending-user-approval`·runtime 등록 금지는 유지 |
