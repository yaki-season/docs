# PM 실행 지시 — topology preflight와 D1 개발 cycle 동기화

- 발행일: `2026-07-30`
- 기준 dashboard: `docs/epic/작업_현황.md v2.1.88`
- 적용 대상: 개발자 1·2, Artist 1·2·3
- 목표: 개발용 placeholder 화면 구현과 아트 승인 cycle을 같은 stable ID·bounds·state 계약으로 연결한다.

## PM 판정

1. Artist 1의 `CMP-GRILL-FINISHED-PROPER-NEGIMA R1`은 검증 증거와 D1 inventory의 stable ID가
   일치하지만 아직 `pending-user-review`다. 사용자 승인 전에는 metadata 승인 전환,
   finalizer, runtime 등록, 다음 후보 제작을 하지 않는다.
2. Artist 2와 Artist 3의 topology preflight는 방향과 소유권 경계가 타당하다. 다만 두 문서 모두
   개발자 2 작업 007의 `componentId/stateVariant/bounds/layer` 입력이 비어 있고 사용자 승인도
   받지 않았으므로 이미지 생성 gate는 아직 닫혀 있다.
3. 개발자 1의 D1 도메인에는 고정 6칸·첫 3개 동시 시작이 구현됐지만 프로덕션 화면에는
   `game.js`의 `createCookStations({ slots: 1 })`, `d1-game.js`의 `{ slots: 2 }`,
   `grillSlot0/1`, 2칸 업그레이드 경로가 남아 있다.
   이 드리프트 제거가 개발자 1의 최우선 구현이다.
4. 개발자 2 작업 007은 아직 app 기준 미구현이다. Artist 2·3이 좌표와 component 의미를 추측하지
   않도록 S0 세 상태와 D1 드링크 첫 묶음의 versioned inventory·재조립 harness를 먼저 제공한다.
5. 승인 cycle은 항상 `사용자 승인 → Artist finalizer handoff → 개발자 2 dry-run/영수증/write·binding
   → 개발자 1 gameplay 회귀 → placeholder count 갱신` 순서다.

## 병렬 실행 순서

### Cycle 1 — 지금 즉시 병렬

- 개발자 1: 프로덕션 D1 그릴을 고정 6칸 factory와 실제 여섯 hit target으로 교체한다.
- 개발자 2: 작업 007의 S0 3상태+D1 드링크 첫 inventory와 FHD/720 harness를 구현한다.
- Artist 1: 현재 후보를 변경 없이 동결하고 사용자 결정을 기다린다.
- Artist 2·3: 새 이미지를 만들지 않고 개발자 2 handoff를 받을 준비만 한다.

### Cycle 2 — 개발자 2 inventory 인계 직후

- Artist 2는 `exterior-key` topology의 `unassigned` 항목만 정식 계약값으로 채우고 사용자에게
  topology 한 건을 다시 제시한다.
- Artist 3은 `BG-WORKSPACE-DRINK` topology의 component·bounds·layer를 정식 계약값으로 채우고
  사용자에게 topology 한 건을 다시 제시한다.
- 개발자 1은 고정 6칸 화면과 D1 UI port를 통해 첫 3개 동시 시작·접촉면 정지를 FHD/720에서 검증한다.

### Cycle 3 — 개별 사용자 승인 직후

- 해당 Artist만 승인 metadata·소비 화면 재조립·finalizer를 만든다.
- 개발자 2가 해당 handoff 한 건만 승격하고 inventory·resolver 양방향 감사를 통과시킨다.
- 개발자 1이 전체 영업 회귀를 실행해 실제 상태와 아트 state가 일치하는지 확인한다.

## 개발자 1 지시문

```text
PM 지시: 개발자 1 작업 005·009의 다음 단일 증분으로 프로덕션 D1 그릴의 구형 가변 1/2칸
경로를 제거하라.

1. app/src/game.js의 createCookStations({ slots: 1 })와 app/src/d1-game.js의 `{ slots: 2 }`,
   SLOT_KEYS 2개, grillSlots 업그레이드 배선을 최신 createD1CookStations의
   고정 6칸·initialBatchSize=3 계약으로 교체하라.
2. app/src/config/screenLayout.js와 app/src/config/d1Layout.js 및 대응 렌더 입력에
   grillSlot0~5 여섯 칸을 실제로 만들고 첫 3개가 모두 배치되기 전 contactFace=null,
   세 번째 배치 시 같은 now로 조리 시작되는지 검증하라.
3. 구형 “2칸 해금/업그레이드” UI와 테스트를 D1 본편에서 제거하되 SCN-001 회귀 화면의 별도
   테스트 계약은 함부로 바꾸지 마라.
4. Artist stable ID와 review 파일은 수정하지 마라. 현재 42개 runtime inventory의 ID도 의미 변경
   없이 유지하라.
5. 단위 테스트와 1920×1080·1280×720 프로덕션 E2E를 실행하고, 결과를 작업 005·009와
   handoffs/developer-1.md에 기록하라.
6. 완료 뒤 개발자 2에게 실제 6칸 rect/hit target, 첫 3개 동시 시작 state, finished tray
   state를 명시적으로 인계하라.
```

## 개발자 2 지시문

```text
PM 지시: 작업 003의 승격은 보류한 채 작업 007의 첫 인계 단위를 먼저 구현하라.

1. S0의 실제 3상태만 versioned inventory로 고정하라:
   SCR-STORY-PROLOGUE +
   S0-STATE-KEY/exterior-key/S0-KEY-SELECT,
   S0-STATE-GATE/gate-open/S0-GATE-OPEN,
   S0-STATE-CHARCOAL/ignite/S0-CHARCOAL-IGNITE.
   UI-003 v1.2.0에서 삭제된 구형 4-phase 해석을 되살리지 마라.
2. 각 상태에 componentId, requiredAssetId, stateVariant, FHD/720 visualBounds,
   interactionBounds, layer/z-order, DOM safe rect를 부여하고 자동 검증하라.
3. D1 드링크 첫 묶음 BG-WORKSPACE-DRINK, ST-DRINK-BEER-TIER-1,
   MDL-BEER-GLASS, MDL-BEER-LEVER를 같은 inventory에 고정하라.
   빈 잔·70%·100%·넘침·완성은 gameplay 판정을 새로 만들지 말고 MDL-BEER-GLASS,
   TEX-BEER-LIQUID, VFX-BEER-CORE의 stateVariant/layer 소비 계약으로 표현하라.
4. placeholder와 승인 asset을 같은 카메라·DOM으로 캡처하는 FHD/720 harness를 만들고
   S0 3클릭 1:1, body-part 0, semanticOwner 단일성을 검증하라.
5. Artist 2·3의 topology 파일은 읽기 전용으로 대조하고 compatibility 결과만 인계하라.
   사용자 승인·finalizer 없는 파일을 promotion하거나 manifest를 직접 편집하지 마라.
6. 작업 007 epic과 handoffs/developer-2.md에 정확한 contract 경로·버전·테스트 결과를 기록한 뒤
   Artist 2·3에게 동시에 전달하라.
```

## Artist 1 지시문

```text
PM 지시: CMP-GRILL-FINISHED-PROPER-NEGIMA R1을 현재 상태로 동결하라.

1. 사용자 승인 전 새 생성, 승인 metadata 전환, runtimeRegistrationAllowed 변경, finalizer,
   runtime 등록, 다음 후보 제작을 하지 마라.
2. 사용자 승인 시에만 ST-GRILL-FINISHED-TRAY와
   CMP-GRILL-FINISHED-PROPER-NEGIMA를 하나의 불가분 handoff로 정리하라.
3. 승인 전부터 존재한 grill.finished.* 세부 semanticOwner 표기는 승인 전환 시
   “Artist 1 / D1-ASSEMBLY-GRILL-FOOD-SHADER” 소유권과 기계 판독 가능한 owner ID가
   서로 일치하도록 관련 metadata·catalog 전체에서 동기화하라. stable asset ID는 바꾸지 마라.
4. FHD/720 소비 화면 재조립과 finalizer 검증 뒤 개발자 2 작업 003에만 인계하라.
5. 그 다음 gate가 cooking-owned completed-food dock/handoff라면 Artist 3 소유
   MDL-SERVICE-TRAY, PR-SERVING-PLATE, 손님 제공 composition을 수정하거나 재제작하지 마라.
```

## Artist 2 지시문

```text
PM 지시: 현재 exterior-key topology는 조건부 적합이며 이미지 생성 gate는 계속 닫아라.

1. UI-003 v1.2.0과 실제 구현은 exterior-key → gate-open → ignite의 3 phase다.
   구형 4-phase 판정을 작업 근거로 사용하지 마라.
2. 개발자 2 작업 007 handoff 전에는 componentId, bounds, z-order, 클릭 좌표를 추측하지 말고
   topology-contract.md의 unassigned를 유지하라.
3. handoff를 받으면 닫힌 중앙 대문·동일 카메라 gate-open·하단 좌측 key ledge topology와
   충돌하는지 대조하고, 충돌이 없으면 계약값만 채운 revision을 사용자에게 한 건 제시하라.
4. 사용자 topology 승인 뒤에만 PR-SHOP-KEY “놓임” 단일 visible 후보를 제작하라.
5. 손·팔·전신, raster UI/text, note, charcoal, Artist 1·3 source는 포함하거나 수정하지 마라.
```

## Artist 3 지시문

```text
PM 지시: 현재 BG-WORKSPACE-DRINK topology는 조건부 적합이며 이미지 생성 gate는 계속 닫아라.

1. 개발자 2 작업 007 handoff 전에는 anchor, componentId, stateVariant, bounds, layer를 추측하지 마라.
2. handoff를 받으면 SCR-SVC-DRINK의 고정 side/down 카메라, full-frame background,
   DOM safe rect와 현재 보고서를 대조하라.
3. background에는 rear architecture와 빈 workspace 연속면만 남기고 tower, lever, glass,
   rack, tray, liquid, foam, VFX, UI, 사람·신체를 계속 제외하라.
4. 계약값을 채운 topology revision 한 건을 사용자에게 다시 제시하고, 사용자 승인 뒤에만
   body-free 1920×1080 BG-WORKSPACE-DRINK R1 후보를 생성하라.
5. 1280×720은 별도 crop을 만들지 말고 동일 logical canvas의 소비 검증으로 처리하라.
6. 다음 드링크 상태 제작은 developer inventory의 stateVariant/layer를 받은 뒤
   ST-DRINK-BEER-TIER-1 → MDL-BEER-LEVER → MDL-BEER-GLASS →
   TEX-BEER-LIQUID/VFX-BEER-CORE 순으로 한 gate씩 진행하라.
```

## 현재 사용자 결정 gate

- 결정 결과: 사용자가 Artist 1의 `CMP-GRILL-FINISHED-PROPER-NEGIMA R1`을 승인했다.
- Artist 1은 canonical owner 동기화·FHD/720 finalizer handoff 단계로 전환한다.
- Artist 2·3 topology는 개발자 2 작업 007의 실제 bounds·layer 계약이 채워진 revision을 받은 뒤
  각각 별도 사용자 승인으로 올린다.
