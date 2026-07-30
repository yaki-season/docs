# Developer 1 → Artist 3 D1 엑스트라 좌석 actor 배치 계약

- 계약 버전: `v1.0.0`
- 전달일: `2026-07-31`
- 소비 작업: Artist 3 작업 025
- 대상: `CH-EXTRA-COMMUTER-SERVICE`, 이후 같은 좌석 geometry를 쓰는
  `CH-EXTRA-SOLO-SERVICE`
- 읽기 전용 정본:
  - `app/src/config/screenLayout.js`
  - `app/src/render/productionRenderer.js`
- 기준 화면: `SCR-SVC-CUSTOMERS`, top-left 정규화 좌표

이 문서는 현재 프로덕션 좌석 geometry를 수치로 옮긴 versioned handoff다.
아트·manifest·runtime binding을 변경하거나 새 asset을 승인하지 않는다.

## 공통 배치 규칙

- 좌석 중심 x: `0.12 / 0.268 / 0.416 / 0.564 / 0.712 / 0.86`
- actor visual bounds:
  `{x: centerX-0.065, y:0.13, width:0.13, height:0.42}`
- actor 하단 중앙 anchor/pivot:
  `{x:centerX, y:0.55}`
- 유효 layout occlusion line: `y=0.55`
  - `actor.y + actor.height`와 `serve.y`가 모두 이 선에 맞는다.
  - 하체·의자·카운터는 actor raster에 굽지 않는다.
  - 실제 비정형 가림 픽셀은 full-frame `custCounter` foreground alpha가 소유한다.
    `y=0.55`를 직선형 raster mask로 오인하지 않는다.
- actor는 `LAYER_Z.actor=-6`, 카운터는 `layer='foreground'`, `order=50`으로 합성되어
  손님 상체가 카운터 뒤에 놓인다.

## 좌석별 visual bounds와 anchor

| seat | normalized bounds `(x,y,w,h)` | normalized anchor `(x,y)` | FHD bounds `(x,y,w,h)` | FHD anchor | 1280×720 bounds `(x,y,w,h)` | 720 anchor |
|---|---|---|---|---|---|---|
| `seat-01` | `(0.055,0.13,0.13,0.42)` | `(0.12,0.55)` | `(105.6,140.4,249.6,453.6)` | `(230.4,594)` | `(70.4,93.6,166.4,302.4)` | `(153.6,396)` |
| `seat-02` | `(0.203,0.13,0.13,0.42)` | `(0.268,0.55)` | `(389.76,140.4,249.6,453.6)` | `(514.56,594)` | `(259.84,93.6,166.4,302.4)` | `(343.04,396)` |
| `seat-03` | `(0.351,0.13,0.13,0.42)` | `(0.416,0.55)` | `(673.92,140.4,249.6,453.6)` | `(798.72,594)` | `(449.28,93.6,166.4,302.4)` | `(532.48,396)` |
| `seat-04` | `(0.499,0.13,0.13,0.42)` | `(0.564,0.55)` | `(958.08,140.4,249.6,453.6)` | `(1082.88,594)` | `(638.72,93.6,166.4,302.4)` | `(721.92,396)` |
| `seat-05` | `(0.647,0.13,0.13,0.42)` | `(0.712,0.55)` | `(1242.24,140.4,249.6,453.6)` | `(1367.04,594)` | `(828.16,93.6,166.4,302.4)` | `(911.36,396)` |
| `seat-06` | `(0.795,0.13,0.13,0.42)` | `(0.86,0.55)` | `(1526.4,140.4,249.6,453.6)` | `(1651.2,594)` | `(1017.6,93.6,166.4,302.4)` | `(1100.8,396)` |

FHD occlusion line은 `y=594`, 1280×720은 `y=396`이다. 모든 clip은 같은 하단 중앙
pivot·눈높이·좌석 scale을 유지한다.

## raster와 입력 분리

- actor raster는 시각 전용이다. 버튼·테이블·좌석 번호·주문서·hit 표시를 굽지 않는다.
- 입력은 별도 완전 투명 `seatServe:<seatId>` raycast mesh가 소유한다.
- hit bounds는
  `{x:centerX-0.07, y:0.13, width:0.14, height:0.52}`이며 actor visual보다 넓다.
  Artist asset 크기나 alpha를 이 hit bounds에 맞춰 늘리지 않는다.
- 말풍선 anchor는 별도 DOM 좌표 `{x:centerX,y:0.10}`다. 글자·게이지를 raster에 넣지 않는다.
- 현재 `SEAT_ACTOR_UV={u0:0.485,u1:0.655,v0:0.48,v1:0.85}`는 츠키오카 full-frame
  임시 crop이다. 새 엑스트라 atlas의 UV·frame 계약으로 재사용하는 값이 아니다.

## 범위와 남은 gate

- 이 v1은 `2026-07-31-artist-3-d1-extra-commuter-contract-request.md` 중 geometry,
  occlusion, anchor, raster/hit 분리를 답한다.
- 공통 service 16 clip 자체는 `ART-003 v5.9.0`을 따른다. 현재 runtime view에는 엑스트라별
  완전한 clip selector가 없으므로 이 문서가 state-to-clip binding을 새로 확정하지 않는다.
- 사용자 preflight 승인, clip/state binding, finalizer, manifest promotion은 각 소유 gate에 남는다.
