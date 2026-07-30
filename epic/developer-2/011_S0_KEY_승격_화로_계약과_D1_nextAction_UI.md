# 개발자 2 - 011 S0 KEY 승격·화로 계약과 D1 nextAction UI

- 문서 버전: `v1.1.0`
- 최종 변경일: `2026-07-31`
- 상태: `완료`
- 담당자: `개발자 2`

## 참조 spec

- `spec/art/ART-003_런타임_아트_에셋_목록과_제작_계약.md` - `ART-003` - `v5.9.0`
- `spec/ui/UI-003_전체_게임_상세_화면_설계.md` - `UI-003` - `v1.2.0`
- `spec/gameplay/GPL-004_조리_스테이션과_품질_서빙.md` - `GPL-004` - `v1.39.0`
- `epic/developer-2/007_S0-D1_아트_binding_inventory와_재조립_harness.md` - 선행 inventory - `v1.1.0`
- `epic/developer-2/009_S0_상태별_배경_promotion과_GATE_단일_visual_정합화.md` - 선행 S0 promotion - `v1.1.0`
- `epic/developer-2/010_D1_면별_누적_표시와_손님_중심_제공_UX.md` - 선행 UI - `v1.2.0`

## 목표

유효 Artist 2 finalizer를 통해 `PR-SHOP-KEY@R1-B1`을 원자적으로 승격하고 exact KEY
binding을 완료한다. `ST-S0-BRAZIER`와 `PR-CHARCOAL-IGNITION`의 픽셀·bounds·layer 소유를
추측 없이 versioned handoff로 확정한다. Developer 1이 `slotViews().nextAction`을 공개한 뒤에는
완료된 작업 010을 다시 열지 않고 현재 행동 UI가 그 공개 필드만 소비하도록 교정한다.

## 범위

### 포함

- `PR-SHOP-KEY` finalizer SHA·evidence·bundle 검증, dry-run·receipt·원자적 write
- `PR-SHOP-KEY@R1-B1` exact resolver/inventory/S0 KEY binding과 FHD/720 placeholder 제거
- 화로 primary/companion의 source master, pixel split, child bounds, layer, no-double-render handoff
- `slotViews().nextAction` 공개 뒤 `d1-game` 행동 문구와 Playwright 기대 교정
- 관련 asset/unit/FHD/720 D1·S0 회귀

### 제외

- Artist source·review binary·이미지 생성
- manifest 수동 편집, `runtimeRegistrationAllowed` 수동 변경, 영수증 없는 write
- Developer 1 소유 `slotViews()` domain 구현 또는 nextAction 추측
- 완료된 작업 009·010 직접 수정

## 작업 절차

1. `PR-SHOP-KEY` handoff·evidence·bundle·manifest entry와 handoff SHA를 검증한다.
2. dry-run이 발급한 30분 일회성 receipt를 검증하고 같은 receipt로 write한다.
3. KEY exact binding과 placeholder 제거를 단위·FHD/720로 검증한다.
4. Artist 2 요청서에 source master, primary/companion pixel split, child bounds,
   layer/zOrder, component/variant, no-double-render 규칙을 versioned handoff로 응답한다.
5. Developer 1 handoff에서 `slotViews().nextAction`을 확인한 뒤에만 행동 문구를 교정한다.
6. S0·D1 타깃과 전체 관련 회귀를 실행하고 적용·대기 결과를 인계한다.

## 의존성과 인계 조건

- 선행 작업: Developer 2 작업 007·009·010
- Artist 2 입력:
  `review/artist-023/s0-prologue/exterior-key/pr-shop-key/placed/r1/metadata/runtime-handoff.json`,
  `review/artist-023/s0-prologue/ignite/st-s0-brazier/preflight/DEVELOPER-2-CONTRACT-REQUEST-v1.0.0.md`
- Developer 2 출력: KEY runtime URL·SHA·binding, brazier versioned contract, FHD/720 검증 결과
- Developer 1 입력 수신:
  `terminal-guides/dispatches/2026-07-31-developer-1-cook-slot-next-action-v1.md`;
  `slotViews().nextAction`의 `none|wait|flip|retrieve` 의미를 그대로 소비

## 완료 기준

- [x] `PR-SHOP-KEY` handoff SHA와 모든 evidence/bundle SHA가 일치한다.
- [x] dry-run receipt를 같은 handoff의 명시적 write에 한 번만 사용한다.
- [x] `PR-SHOP-KEY@R1-B1` exact binding 뒤 KEY placeholder가 FHD/720에서 제거된다.
- [x] 화로 계약에 요청된 모든 필드가 확정되고 `unassigned`가 0개다.
- [x] primary/companion 중복 pixel과 runtime 이중 렌더가 계약·테스트로 차단된다.
- [x] Developer 1 handoff 뒤 UI 행동 문구가 `nextAction`만 소비한다.
- [x] FHD/720 S0·D1, 키보드 제공과 관련 전체 회귀가 통과한다.

## 구현 및 검증 결과

- app 구현 위치:
  - `public/assets/core/s0/prologue/pr-shop-key-placed-r1-b1.png`
  - promotion 도구가 갱신한 `public/assets/manifest.json`
  - `src/assets/s0D1ArtBindingContract.js`
  - `src/s0-d3.html`, `src/s0-d3.css`, `src/s0-d3.js`
  - `src/d1-game.js`
  - `tests/integration/s0D1ArtBindingContract.test.js`
  - `tests/e2e/s0-d3-scenario.spec.js`
  - `tests/e2e/art-recomposition-harness.spec.js`
  - `tests/e2e/d1-face-serving-ux.spec.js`
- docs handoff 위치:
  - `terminal-guides/dispatches/2026-07-31-developer-2-s0-brazier-binding-handoff-v1.0.0.md`
  - `terminal-guides/dispatches/2026-07-31-developer-2-next-action-ui-consumption.md`
  - `terminal-guides/handoffs/developer-2.md`
- 구현 기준 spec 버전: `ART-003 v5.9.0`, `UI-003 v1.2.0`, `GPL-004 v1.39.0`
- 구현 기준 태스크 버전: `v1.1.0`
- 검증 방법: asset validator, binding unit, Chromium FHD/720 S0·D1 Playwright
- 검증 결과:
  - `PR-SHOP-KEY@R1-B1`: handoff SHA
    `ba6cd227d7041e45df8d98e7c29dff3d4362d4608c5304390a559478f6d8c40f`,
    runtime raster SHA
    `a8a22c3beaa6a4ffa1b45dad6eb4438eda729d325740ad9bcb265d3363229eeb`;
    dry-run·30분 receipt 검증·동일 receipt write 완료
  - `npm run assets:validate`: 12개 통과
  - `npm run visual:references:validate`: 36개 통과
  - 전체 Vitest: 43파일·316/316 통과
  - S0 exact binding·placeholder harness Chromium FHD/720: 30/30 통과
  - D1 행동·키보드 제공·영업 회귀 Chromium FHD/720: 18/18 통과
- 남은 위험:
  - `ST-S0-BRAZIER`와 `PR-CHARCOAL-IGNITION`은 이번 작업에서 계약만 확정했다.
    Artist 제작·승인·유효 finalizer 전에는 exact placeholder와 promotion 금지를 유지한다.

## 변경 이력

| 이전 버전 | 새 버전 | 날짜 | 변경 유형 | 근거 spec 버전 | 변경 내용 | 재작업 영향 |
|---|---|---|---|---|---|---|
| 없음 | `v1.0.0` | `2026-07-31` | 후속 작업 생성·진행 중 전환 | `ART-003 v5.9.0`, `UI-003 v1.2.0`, `GPL-004 v1.39.0` | 완료된 작업 009·010을 보존하고 KEY promotion·화로 binding 응답·nextAction UI 교정을 독립 후속 작업으로 분리 | C는 Developer 1 공개 handoff 전 구현하지 않음 |
| `v1.0.0` | `v1.1.0` | `2026-07-31` | 구현·검증 완료 | 동일 | KEY exact promotion·FHD/720 placeholder 제거, 화로 primary/companion v1 계약, Developer 1 `nextAction` 전용 행동 문구를 완료하고 assets 12·references 36·Vitest 316·S0 30·D1 18을 통과 | 미제작 화로 두 asset은 exact placeholder를 유지하며 유효 finalizer 뒤 별도 promotion |
