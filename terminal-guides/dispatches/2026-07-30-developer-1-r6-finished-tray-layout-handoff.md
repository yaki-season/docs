# Developer 1 → Developer 2 R6 완료 tray layout handoff

- 날짜: `2026-07-30`
- gameplay 단일 원본: `app/src/config/d1GrillLayout.js`
- asset: `ST-GRILL-FINISHED-TRAY@R6`
- 승인 SHA-256: `51abf390cbf31d40b50a7d99f08a5d37a2da70df6cf94d405788538bad7d9184`
- 승인 상태: `approved-by-user`
- runtime 등록: `runtimeRegistrationAllowed=false`; 이 인계는 좌표·reserved 영역 갱신용이며
  manifest나 runtime asset 등록 승인이 아니다.

## Developer 2 소비 값

- gameplay `visualRect = hitRect = reservedBounds`
  - normalized: `{x:0.7989583333333333,y:0.11388888888888889,width:0.13854166666666666,height:0.3277777777777778}`
  - FHD: `{x:1534,y:123,width:266,height:354}`
  - 1280×720: `{x:1022.6666667,y:82,width:177.3333333,height:236}`
- 완료품 전달 anchor는 변경하지 않는다.
  - normalized: `{x:0.8557291666666667,y:0.27870370370370373}`
  - FHD: `{x:1643,y:301}`
  - 1280×720: `{x:1095.3333333,y:200.6666667}`
- visual mesh와 hit mesh는 별도 객체다. 현재 두 rect의 수치는 같지만 visual을 이동·축소하거나
  hit을 축소하면 안 된다. Developer 2의 DOM 비침범 reserved 영역은 이 gameplay bounds 바깥으로
  별도 여백을 확장할 수 있다.
- `sourceRevision=6`은 완료 tray 승인본 revision이다.
  `sourceMasterRevision=3`은 배경 카메라 정본 `CM-GRILL-STATION-QUEUED-SELECTION`의 revision으로
  서로 다른 필드 의미를 유지한다.
- `grillSlot0~5` rect·hit target과 첫 3개 동시 시작 gameplay 계약은 변경하지 않았다.

## 검증

- 실제 R6 PNG SHA와 alphaBBox `1534,123,266,354` 일치
- CSS overlay의 FHD/720 visual·hit bounds와 실제 production renderer 투영 bounds 일치
- tray 중심·좌·우·상·하 외곽 raycast 성공, 우측 bounds 밖 4px miss
- slot0~5 hit mesh 유지
- 첫 3개 동시 시작, 앞면 8초→뒤집기→뒷면 8초→finished 전달→tray 클릭 FHD/720 통과
- 저장 payload/schema, Artist 파일, manifest, `runtimeAssetResolver` 무변경

Developer 2는 receipt rail 등 DOM을 위 gameplay bounds 및 자체 reserved 여백 밖으로 배치한다.
UI 충돌을 피하려고 tray bounds나 anchor를 이동하지 않는다.
