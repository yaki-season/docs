# Artist 1 → Developer 1·2: ST-GRILL-FINISHED-TRAY R6 footprint 차이

- 전달일: `2026-07-30`
- 전달자: `Artist 1 / D1-ASSEMBLY-GRILL-FOOD-SHADER`
- 대상: Developer 1, Developer 2
- 목적: 승인 R6의 실제 픽셀 footprint를 알리고 runtime 배치·DOM 비침범 계약의 변경 여부를
  개발 소유자가 판단하게 한다. 이 문서는 runtime 등록 또는 finalizer 허가가 아니다.

## 변하지 않는 승인 자산

| 항목 | 값 |
| --- | --- |
| stable ID | `ST-GRILL-FINISHED-TRAY` |
| 승인 revision | `R6` |
| asset | `art-workspace/review/artist-000/d1-cooking/grill/finished-tray/r6/assets/st-grill-finished-tray-fhd-r6.png` |
| SHA-256 | `51abf390cbf31d40b50a7d99f08a5d37a2da70df6cf94d405788538bad7d9184` |
| 의미 | 빈 대나무 makisu resting mat; 음식·꼬치·DOM UI 없음 |
| runtimeRegistrationAllowed | `false` |

## footprint 차이

| 구분 | FHD 값 |
| --- | --- |
| 기존 Developer 1 visual/reserved bounds | `x=1534, y=130, width=218, height=342` |
| 승인 R6 실제 alphaBBox | `x=1534, y=123, width=266, height=354` |
| 변화 | 좌측 x 유지, 위로 7px·오른쪽으로 48px·아래로 5px 확장 |

R6 픽셀을 축소·이동·재생성하지 않는다. runtime 배치와 DOM reserved/safe area를 바꿀지,
hit target을 어떻게 유지할지는 Developer 1·2 소유 판단이다. 이 확인은
`MDL-NEGIMA-GRILL-RAW` station 소비 검수 시작을 막지 않는다. 단, `grill.finished`
finalizer·runtime handoff·manifest 등록은 두 개발자의 계약 확인 전까지 금지한다.
