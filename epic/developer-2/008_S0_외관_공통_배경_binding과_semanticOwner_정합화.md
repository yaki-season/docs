# 개발자 2 - 008 S0 외관 상태별 배경 binding과 semanticOwner 정합화

- 상태: `완료`
- 담당자: `개발자 2`

> ⚠ 2026-08-02 S0 점화 계약 갱신: S0 프롤로그 상호작용은 `S0-KEY-SELECT → S0-GATE-OPEN`의 2클릭(KEY→GATE)이고 숯 점화는 story dialogue로 처리한다. 아래 "S0 3상태·3클릭" 기술은 당시(구형 CHARCOAL/ignite 포함) 기준이며, 세 번째 상태와 `S0-CHARCOAL-IGNITE`는 deprecated다(app 재정합은 별도 Dev2 작업). 이 작업의 배경 binding·semanticOwner 결과는 그대로 유효하다.

## 참조 spec

- `spec/system/SYS-002_2.5D_스테이션_렌더링과_레이어_계약.md` - `SYS-002` - `v3.0.0`
- `spec/ui/UI-002_전체_게임_화면_입력_접근성.md` - `UI-002` - `v5.25.0`
- `spec/ui/UI-003_전체_게임_상세_화면_설계.md` - `UI-003` - `v1.2.0`
- `spec/art/ART-003_런타임_아트_에셋_목록과_제작_계약.md` - `ART-003` - `v5.9.0`
- `epic/developer-2/007_S0-D1_아트_binding_inventory와_재조립_harness.md` - `Developer 2 / 007` - `v1.1.0` (`완료`, 읽기 전용 선행 계약)
- `epic/artist/023_S0_프롤로그_정식_아트.md` - `Artist 2 / 023` - `v1.1.0`

## 목표

완료된 작업 007의 S0 inventory(당시 3상태·3클릭; 세 번째 CHARCOAL/ignite 상태는 2026-08-02 deprecated)를 변경하지 않고, KEY는 중앙 대문이 완전히 닫힌
`BG-EXTERIOR-S0-CLOSED`, GATE는 동일 외관 카메라에서 중앙 개구부가 열리고 빈 실내가 보이는
`BG-EXTERIOR-S0-GATE-OPEN`을 소비하도록 상태별 versioned background 계약을 제공한다. 동시에
Artist metadata·binding contract·runtime inventory의 `semanticOwner`를 fully-qualified machine
ID로 대조해 잘못된 owner의 promotion을 dry-run 이전에 차단한다.

## 범위

### 포함

- `BG-EXTERIOR-S0-CLOSED`와 `BG-EXTERIOR-S0-GATE-OPEN` 각각의 component·state
  variant·source master·FHD/720 bounds·layer·DOM safe rect·body count와 상태별 소비 계약
- 배경을 별도 phase로 세지 않는 state-specific layer 모델과 구형 4-phase 복원 차단
- 승인 `PR-SHOP-GATE-S0/open R6`을 GATE 화면 제작 입력으로 사용하되 닫힌 배경 overlay와
  닫힌 문 픽셀 잔존을 금지하는 full-frame recomposition 정책
- placeholder/approved 동일 camera·DOM FHD/720 재조립 harness
- `artist-1.d1-assembly-grill-food-shader`,
  `artist-2.s0-prologue-story`,
  `artist-3.d1-drink-service-cleanup-customer-settlement` machine owner ID
- Artist profile metadata, binding inventory, D1 runtime inventory의 stable asset ID별 owner
  일치 검사와 promotion preflight 차단

### 제외

- 완료된 작업 007 문서·contract·harness 수정
- S0 클릭 gameplay·대사·campaign 상태 변경 (현행 KEY→GATE 2클릭+점화 대사)
- `game.js`, renderer, 영업 로직 — Developer 1 소유
- `content/schema`·balance data — Developer 3 소유
- Artist source·review binary·provenance 수정
- finalizer handoff 없는 promotion, manifest 수동 편집, `runtimeRegistrationAllowed` 수동 변경

## 작업 절차

1. S0 exterior state-background contract `v2.0.0`을 작업 007 계약의 카메라·viewport와
   호환되게 고정한다.
2. KEY/GATE가 서로 다른 exact background ID를 소비하되 S0 interaction inventory는 계속
   세 행인지 자동 검증한다.
3. 별도 semantic harness에서 placeholder와 exact approved ID를 같은 camera·DOM으로
   FHD/720 캡처한다.
4. D1 runtime inventory의 축약 owner를 fully-qualified machine ID로 교체하고 stable asset
   의미·manifest ID는 유지한다.
5. promotion 입력의 Artist profile metadata owner를 binding/runtime inventory와 대조해
   누락·축약·불일치를 write 이전에 거부한다.
6. assets/reference/Vitest/FHD·720 E2E를 실행하고 Artist 2에게 복사 가능한 계약을 인계한다.

## 의존성과 인계 조건

- 선행 작업: 완료된 Developer 2 작업 007 `v1.1.0`
- 병행 작업: Developer 2 작업 003, Artist 2 작업 023, 통합 QA·릴리스 작업 001
- 후속 작업: Artist 2의 `BG-EXTERIOR-S0-CLOSED` 승인 뒤
  `BG-EXTERIOR-S0-GATE-OPEN` 후보와 finalizer, Developer 2 작업 006
- Artist 2는 `sourceMasterId=CM-PROLOGUE-INHERITANCE-R1`을 읽기 전용 분위기 source로만
  사용하고 contract에 없는 ID·bounds·owner를 추측하지 않는다.
- `BG-WORKSPACE-DRINK` handoff는 Developer 2 작업 003의 dry-run→receipt→write→binding
  순서를 유지하며 성공 기준은 전체 placeholder `34→33`, drink `4→3`이다.

## 완료 기준

- [x] KEY는 `BG-EXTERIOR-S0-CLOSED`, GATE는 `BG-EXTERIOR-S0-GATE-OPEN`을 소비한다.
- [x] 두 background의 camera·bounds·DOM safe rect는 같고 requiredAssetId는 다르다.
- [x] KEY↔GATE background 교차 대체를 금지하고 exact ID가 없으면 placeholder를 유지한다.
- [x] GATE는 R6 입력으로 full-frame 재조립하며 closed overlay·잔존 픽셀을 금지한다.
- [x] 상태별 background 추가 뒤에도 S0는 당시 3상태·3클릭이고 구형 4-phase는 0개다. (현행 계약은 KEY→GATE 2상태·2클릭+점화 대사, 상단 갱신 참조)
- [x] FHD/720에서 component·variant·visual bounds·z/layer·DOM safe·source master·body 0이 검증된다.
- [x] placeholder/approved harness가 exact approved ID만 사용하며 가까운 자산을 대체하지 않는다.
- [x] D1 runtime inventory에 축약 `artist-1`·`artist-3` owner가 0개다.
- [x] Artist metadata·binding·runtime owner가 다르면 promotion 전에 자동 실패한다.
- [x] 기존 승인 runtime 8개의 의미·stable manifest ID·현재 binding은 변하지 않는다.

## 구현 및 검증 결과

- app 구현 위치:
  - `src/assets/s0ExteriorBackgroundBindingContract.js` — state-specific background contract `v2.0.0`
  - `src/s0-exterior-background-harness.html`·`.css`·`.js` — placeholder/approved 동일
    camera·DOM static harness
  - `src/assets/artSemanticOwnerIds.js`·`artSemanticOwnerContract.js` — machine ID registry와
    stable asset별 metadata/binding/runtime 교차 감사
  - `src/assets/d1RuntimeInventory.js` — 42행 owner fully-qualified 정규화
  - `tools/assets/promote-runtime-asset.mjs` — profile approval 직후, receipt 생성 이전 owner preflight
- 구현 기준 spec 버전: 위 참조 spec
- 구현 기준 태스크 버전: `v2.0.0`
- 검증 방법: contract unit·promotion owner unit·FHD/720 Playwright·assets/reference validation
- 검증 결과:
  - `npm run assets:validate`: runtime 승인 asset 8개 통과
  - `npm run visual:references:validate`: 참조 이미지 36장 통과
  - 전체 `npm test`: 38파일·274개 통과
  - 수정 계약 타깃 Vitest: 3파일·12개 통과
  - 수정 harness Playwright: Chromium 1280×720/FHD 8개 통과
    (KEY/GATE exact ID·동일 camera/DOM/bounds·양방향 교차 대체 금지)
  - placeholder baseline: 전체 `34→34`, drink `4→4`; promotion 미실행
- 남은 위험: Artist 1·2·3의 유효 finalizer handoff가 없어 promotion은 실행하지 않았다.
  `BG-WORKSPACE-DRINK`도 R2 review까지만 존재해 전체 `34`, drink `4`, missing ID 포함 상태다.
  branded Edge와 실제 외부 static host smoke는 작업 005 잔여로 유지한다.

## Artist 2 인계 계약

| 항목 | 고정값 |
|---|---|
| contract | `app/src/assets/s0ExteriorBackgroundBindingContract.js` / `v2.0.0` |
| harness | `/src/s0-exterior-background-harness.html?stateId=S0-STATE-KEY&mode=placeholder` (`S0-STATE-GATE`·`approved` 지원) |
| KEY asset / variant | `BG-EXTERIOR-S0-CLOSED` / `closed` — 중앙 대문 완전 닫힘 |
| GATE asset / variant | `BG-EXTERIOR-S0-GATE-OPEN` / `gate-open-empty-interior` — 동일 외관의 중앙 개구부 열림·빈 실내 |
| component | 양쪽 `prologue.exterior.background` |
| owner / source | `artist-2.s0-prologue-story` / `CM-PROLOGUE-INHERITANCE-R1` |
| layer | 양쪽 `background`, `zOrder=0`, body `0` |
| camera | `S0-EXTERIOR-FIXED-V1`, fixed 16:9, `contain` |
| FHD | visual `(0,0,1920,1080)`, interaction `null`, DOM safe `(128,936,1664,104)` |
| 720 | visual `(0,0,1280,720)`, interaction `null`, DOM safe `(85,624,1109,69)` |
| GATE 제작 입력 | 승인 `PR-SHOP-GATE-S0 / open R6`; full-frame background 재조립 |
| 금지/검증 | KEY에 open·GATE에 closed 사용 금지, closed overlay·잔존 픽셀 금지, exact ID 없으면 `개발 중`, FHD/720 8개 통과 |
