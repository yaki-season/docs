# Artist 1 직전 작업 기록

- 마지막 갱신: `2026-08-01`
- 현재 담당: Artist 작업 000의 D1 조립·고정 6칸 그릴 model/shader 및 소비 화면 검수
- semanticOwner: `Artist 1 / D1-ASSEMBLY-GRILL-FOOD-SHADER`
- 수정 금지: `review/artist-023/`, `review/artist-024/`, `review/artist-025/`; D2·D3 작업 026은 시작하지 않음

## 2026-08-01 v8 pipeline 정합 checkpoint

- runId: `ART1-D1-V8-PROVENANCE-20260801-R1`,
  `ART1-D1-V8-REPORT-BRIDGE-20260801-R2`.
- provenance `14`건과 v8 profile report `6`건을 기존 승인 evidence로 정합해 validator를
  error `23→3`, report `19→25`, provenance `38`로 개선했다. non-metadata artifact·승인 의미
  변경은 `0`이다.
- 남은 exact blocker: `assembly/r1` stable ID/whole-screen 의미, `finished-tray/r1` 격리
  review board 부재, `grill/master/r3` whole-screen 승인과 v8 single-asset profile 충돌.
- 현재 사용자 후보는 여전히 `MDL-NEGIMA-GRILL-COOKING-SECOND-FACE R2`며
  `pending-user-review`, runtime 불가능이다.

## 확정 입력

- `CM-GRILL-STATION-QUEUED-SELECTION R3`: canonical FHD 카메라·원근·구도·조명·색조 master.
  SHA-256 `b029f99434ee4a66036e3cdd0dd5125af7ce89028daa29ca39a219630973fb2c`.
- `CM-GRILL-STATION-EMPTY-BASE R1`: 사용자 승인. SHA-256
  `ed989dcbbddcb2ff1014f38a6c93512989b553149fb16c4654edb522d2b41d1a`.
- `ST-GRILL-FINISHED-TRAY R6`: 사용자 승인된 빈 대나무 makisu. SHA-256
  `51abf390cbf31d40b50a7d99f08a5d37a2da70df6cf94d405788538bad7d9184`, FHD alphaBBox
  `(x=1534, y=123, width=266, height=354)`. 음식·꼬치·UI는 포함하지 않으며
  `runtimeRegistrationAllowed=false`를 유지한다.
- `MDL-NEGIMA-GRILL-RAW R1`: 사용자 승인. approved GLB 3종과 nearest albedo를 그대로
  재사용하는 476 triangle Three.js composition이다. local `+Y` `flipPivot`은
  첫 입력 `0→π`, 두 번째 입력 `π→2π`를 따른다.

## 완료 tray revision 이력

- R2는 사용자가 반려했고 binary는 삭제됐으며 사유·해시는
  `art-workspace/review/artist-000/d1-cooking/grill/finished-tray/REJECTIONS.md`에만 남아 있다.
- R3·R4·R5는 사용자 피드백으로 superseded된 중간 revision이다. 재사용하지 않는다.
- R6만 현재 사용자 승인본이다. R6를 축소·이동·재생성하거나 finalizer/manifest에 올리지 않는다.

## Developer 1·2 전달 사항

`docs/terminal-guides/handoffs/artist-1-r6-footprint-to-developers.md`에 전달했다.
기존 개발 reserved bounds FHD `(1534,130,218,342)`와 R6 실제 alphaBBox
`(1534,123,266,354)`의 차이를 보고한다. runtime 배치·DOM 비침범 계약 변경 여부는
Developer 1·2가 결정한다. 이 확인은 raw 네기마 소비 검수의 시작을 막지 않지만,
완료 tray finalizer는 확인 전까지 금지다.

## 현재 단일 후보와 다음 gate

- R1·R2는 각각 사용자 피드백으로 superseded됐다. R1은 292px legacy slot 사각형에 맞춰 짧았고,
  R2는 같은 폭에서 위아래 길이를 더 늘리라는 다음 피드백을 받았다. 재사용하지 않는다.
- 현재 단일 후보: 없음. `MDL-NEGIMA-GRILL-COOKING-FIRST-FACE R1`의 station 소비 화면 재조립 R1은
  사용자 승인됐다.
  경로: `art-workspace/review/artist-000/d1-cooking/grill/recomposition/cooking-first-face-station-consumption/r1/`.
  FHD와 1280×720에서 한 꼬치·앞면 4초·뒷면 0초·`orientationFaceDown=front`,
  `contactFace=front` 상태를 검수·승인했다.
- 검수판: FHD `review-mdl-negima-grill-cooking-first-face-station-fhd-r1.png` SHA-256
  `54fbf825e5919a092e585d106670ed25eedd018d7b81b37eff7aa4c1e44c906c`, 720
  `review-mdl-negima-grill-cooking-first-face-station-720-r1.png` SHA-256
  `8f07b88095a9b2f5a8d2e9fd9e619b8c766a6dbe6bda946c53acf9053542a13b`다. raw R3와
  같은 alphaBBox FHD `(545,257,131,516)`, 720 `(363,171,88,344)`이며 R3 석쇠 safe
  bounds 안이다. 비교판 SHA-256은 `213596be128a48437da27088dc43793901177e716f0c5e17d2885609ffdef78e`다.
- 실제 GLB/nearest-albedo 재사용만 검증했고 새 raster·GLB·texture·stable ID는 만들지 않았다.
  `runtimeRegistrationAllowed=false`를 유지한다.
- slot0 visual contract: normalized `(0.297,0.435,0.041,0.27)`, FHD
  `(570.24,469.8,78.72,291.6)`, 720 `(380.16,313.2,52.48,194.4)`.
- `MDL-NEGIMA-GRILL-COOKING-SECOND-FACE` station consumption R1은 실제 FHD/720에서
  shared slot0 transform과 `completedFlips=1`, `rotationY=PI`, `contactFace=back`, front/back
  누적 각 4초, 476 triangles를 검증했다. 출력은
  `art-workspace/review/artist-000/d1-cooking/grill/recomposition/cooking-second-face-station-consumption/r1/`에
  남겼다. FHD SHA-256 `6c96b4e2fd0f0e72b3c0b0452576f114f27910e4d0e094765f3c278e20c1ed00`, 720
  SHA-256 `67d8a7e98f1315f9ac75cbf5e2321c7f8dceb0ff9654c49d09156ce132e4559d`, comparison SHA-256
  `8f4ec69bb85664195888ed0ecff80daa8a8480102c2aad437611547cedd4e1e5`다.
- 그러나 720에서는 대파 단면·재료 방향의 반전은 보이되, 반대면이 익는 중이라고 읽히는 부분 sear는
  약하다. 5.72% pixel difference는 형상 반전 증빙일 뿐 sear 가독성의 통과 근거가 아니다. 따라서 R1은
  `not-submitted-face-signal-insufficient`이며 사용자 후보가 아니다.
- R1의 720 sear 가독성 부족에 대한 PM R2 교정 범위는 승인됐다. R1은
  `not-submitted-face-signal-insufficient` 기록으로만 남기고 승인 후보로 재사용하지 않는다.
- 현재 단일 후보: `MDL-NEGIMA-GRILL-COOKING-SECOND-FACE R2` station consumption.
  경로: `art-workspace/review/artist-000/d1-cooking/grill/recomposition/cooking-second-face-station-consumption/r2/`.
  source state SHA-256은 `5b84a68589e3948d92e17591c1c4f12f864434ca7a8ce9c76b3c38f855c67bd5`, shader module
  SHA-256은 `a0b94cc4e8d0d34263a30eb28c33d6d543029d32e9c9260c34e120aeb32f98c9`다.
- R2는 raw R3 공통 anchor `(609.6,515)`, scale `(1.45,1.18)`, root rotation
  `(-0.035,0.055,0)`와 476 triangles를 고정한다. `completedFlips=1`, `rotationY=PI`,
  `orientationFaceDown=back`, `contactFace=back`, 앞/뒤 누적 각 4초, face1 heat 0.52다.
  imported decal 자체의 local 0–1 UV를 사용해 chicken 3개 caramel/brown cluster와 green-onion
  2개 olive/caramel band를 shader에만 추가했다. GLB·nearest albedo는 byte-identical이며 새
  raster·GLB·texture·atlas·bake는 없다.
- R2 검수판: FHD `review-mdl-negima-grill-cooking-second-face-station-fhd-r2.png` SHA-256
  `ac86c8e03f8c9bf8a91558ad4fd6b4e9fe7bcb166799588baa3400251cc3a8f7`, 720
  `review-mdl-negima-grill-cooking-second-face-station-720-r2.png` SHA-256
  `06df8021609eaccf9d8084d1a7cbcf5fe027f8f69b292d38e5b83fbac2cf3609`, first/second 비교판
  SHA-256 `f06b1cd77fbe5b2f01965919373ad8a17c427eb5f2691be77930bdfecb287ae8`다. R1 대비
  R2 screen signal은 FHD 14,891px, 720 6,609px로 검증했고 모든 alphaBBox는 raw R3 footprint와
  동일하다.
- R2는 `pending-user-review`, `runtimeRegistrationAllowed=false`다. 사용자 승인 전에는
  `proper-first-face`·optimization·finalizer·runtime handoff·manifest 등록으로 진행하지 않는다.
- 최종 소비 화면·최적화·사용자 최종 승인 전에는 runtime handoff, finalizer, manifest와
  app runtime 등록을 만들지 않는다.
