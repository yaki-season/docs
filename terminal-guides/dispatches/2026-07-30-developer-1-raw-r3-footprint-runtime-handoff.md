# Developer 1 → Artist 1·Developer 2: raw R3 footprint runtime 계약

- 전달일: `2026-07-30`
- 상태: runtime layout 적용 완료
- 승인 입력: `MDL-NEGIMA-GRILL-RAW` source R1 / station consumption review R3
- 승인 review SHA-256:
  `f13b3808b56c9462854c863c6b8843206aebfbbf2bf5669b1fb86d5bbc5a1579`
- 단일 원본: `app/src/config/d1GrillLayout.js`
- 주의: `runtimeRegistrationAllowed=false` 유지. 이 인계는 Artist 1 finalizer의 좌표 선행 조건을
  해소하지만 manifest 등록이나 runtime promotion 승인이 아니다.

## 공개 interface

- `D1_GRILL_FOOD_FOOTPRINT`
  - stable ID·source/review revision·승인 SHA·FHD/720 승인 bbox
  - slot0 `visualRect`·anchor
  - lane step과 승인 source model transform
- `D1_GRILL_FOOD_STATE_KEYS`
  - `raw`, `cooking`, `flipped`, `proper`, `burnt`
- `D1_GRILL_SLOTS[*]`
  - `visualRect`, 기존 `hitRect`, `anchor`, `stateTransforms`
- `createD1GrillObjects()`
  - 두 프로덕션 layout이 위 필드를 같은 객체 참조로 소비

`visualRect`와 `hitRect`는 별도 frozen 객체이며 Three.js도 visual mesh와 투명 raycast mesh를
별도 UUID로 만든다. R3 확대로 입력 회귀가 없어 hitRect는 변경하지 않았다.

## 최종 좌표

모든 값은 1920×1080 top-left 정규화 좌표다.

| slot | visual x | anchor x | hit x |
|---|---:|---:|---:|
| 0 | 0.2838541666666667 | 0.3175 | 0.285 |
| 1 | 0.3638541666666667 | 0.3975 | 0.365 |
| 2 | 0.4438541666666667 | 0.4775 | 0.445 |
| 3 | 0.5238541666666666 | 0.5575 | 0.525 |
| 4 | 0.6038541666666667 | 0.6375 | 0.605 |
| 5 | 0.6838541666666667 | 0.7175 | 0.685 |

- visual 공통: `y=0.23796296296296296`,
  `width=0.06822916666666666`, `height=0.4777777777777778`
- anchor 공통: `y=0.47685185185185186`
- hit 공통: `y=0.42`, `width=0.065`, `height=0.30`
- lane step: FHD `153.6px`, normalized `0.08`

FHD visual은 x `545/698.6/852.2/1005.8/1159.4/1313`, 공통
`y=257,width=131,height=516`; anchor는 x `609.6/763.2/916.8/1070.4/1224/1377.6`,
공통 y `515`다.

1280×720 visual은 x
`363.3333333/465.7333333/568.1333333/670.5333333/772.9333333/875.3333333`,
공통 `y=171.3333333,width=87.3333333,height=344`; slot0 승인 raster alphaBBox는
`363,171,88,344`다. anchor는 x `406.4/508.8/611.2/713.6/816/918.4`,
공통 y `343.3333333`다.

## 상태·충돌 계약

- raw/cooking/flipped/proper/burnt는 슬롯별 같은 `visualRect`, anchor,
  source model transform을 사용한다.
- source model transform:
  `horizontalScale=1.45`, `verticalScale=1.18`,
  `rootRotationRadians={x:-0.035,y:0.055,z:0}`.
- safe bounds FHD `500,210,960,580` 안에서 slot0 좌측 45px, slot5 우측 16px,
  상단 47px, 하단 17px 여유가 있다.
- 인접 visual 간격은 FHD 22.6px, 720p 약 15.0667px다.
- slot5 visual 오른쪽 `1444`와 R6 tray 왼쪽 `1534` 사이는 FHD 90px,
  720p 60px다.
- 프로덕션 DOM에서 여섯 hit의 중심·좌우 외곽 `elementFromPoint`는 모두 canvas다.
  Developer 2는 이 raycast 비침범을 유지하고 visual을 피하려고 slot/tray를 이동하지 않는다.
- 완료 tray R6 visual/hit/reserved와 anchor는 변경하지 않았다.

## 검증

- `npm test -- --run tests/unit/d1GrillLayout.test.js` → `9/9`
- 조리·선택 손님 제공 단위/통합 → `29/29`
- `npx playwright test tests/e2e/d1-grill-layout-overlay.spec.js` → FHD/720 `4/4`
- `npx playwright test tests/e2e/d1-game.spec.js tests/e2e/d1-face-serving-ux.spec.js`
  → FHD/720 `12/12`
- `npm test` → `43파일·311/311`

Playwright overlay 출력:

- `app/test-results/d1-grill-layout-overlay-승인-e59e6-료-tray-overlay가-좌표-계약과-일치한다-chromium-1920x1080/d1-grill-master-r3-tray-r6-overlay-1920x1080.png`
- `app/test-results/d1-grill-layout-overlay-승인-e59e6-료-tray-overlay가-좌표-계약과-일치한다-chromium-1280x720/d1-grill-master-r3-tray-r6-overlay-1280x720.png`

위 이미지는 재실행 가능한 테스트 산출물이며 Artist review/승인 metadata가 아니다.
아트 파일·승인 metadata·review·manifest·runtime asset resolver·저장 payload/schema는 수정하지 않았다.
