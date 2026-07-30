# Artist 1 → Developer 1: MDL-NEGIMA-GRILL-RAW station consumption R3 footprint

- 전달일: `2026-07-30`
- 상태: 사용자 승인 `MDL-NEGIMA-GRILL-RAW` station consumption R3
- 전달자: `Artist 1 / D1-ASSEMBLY-GRILL-FOOD-SHADER`
- 목적: 한 lane에 한 꼬치가 충분한 길이로 보이도록 사용자 승인된 raw R3의 실제 footprint를
  알려 runtime visual/hit/reserved contract의 변경 여부를 Developer 1이 결정하게 한다.

## 승인 입력·출력

| 항목 | 값 |
| --- | --- |
| stable ID | `MDL-NEGIMA-GRILL-RAW` (새 ID 없음) |
| review | `raw-station-consumption/r3` |
| FHD review SHA | `f13b3808b56c9462854c863c6b8843206aebfbbf2bf5669b1fb86d5bbc5a1579` |
| FHD alphaBBox | `x=545, y=257, width=131, height=516` |
| 720 alphaBBox | `x=363, y=171, width=88, height=344` |
| lane anchor FHD | `x=609.6, y=515` |
| 6-slot safe bounds FHD | `x=500, y=210, width=960, height=580` |
| raw model contract | 476 triangles; local `+Y`; first flip `0→π`, second `π→2π`; elapsed 양면 0; `contactFace=null` |

기존 slot0 visualRect FHD `(570.24,469.8,78.72,291.6)`는 이 승인본보다 작다. R3는 고정 6칸
그릴의 한 lane에 한 꼬치가 충분한 길이로 보여야 한다는 사용자 지시로 이 기존 rect를 벗어난다.

Developer 1은 실제 runtime의 visualRect·hitRect·reserved bounds를 변경할지 결정하고, DOM/UI를
수정할 필요가 있으면 Developer 2와 조율한다. Artist는 이 문서로 runtime 배치, hit 판정, manifest,
finalizer를 변경하지 않는다. `runtimeRegistrationAllowed=false`를 유지하며 최종 소비 화면 승인 전
runtime handoff·finalizer·manifest 등록은 금지다.
