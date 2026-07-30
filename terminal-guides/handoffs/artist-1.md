# Artist 1 직전 작업 기록

- 마지막 갱신: `2026-07-30`
- 현재 담당: Artist 작업 000의 D1 조립·고정 6칸 그릴 model/shader 및 소비 화면 검수
- semanticOwner: `Artist 1 / D1-ASSEMBLY-GRILL-FOOD-SHADER`
- 수정 금지: `review/artist-023/`, `review/artist-024/`, `review/artist-025/`; D2·D3 작업 026은 시작하지 않음

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
- 현재 단일 후보: 없음. PM 보고
  `docs/inbox/99.change-request/2026-07-30_D1_cooking-second-face_R2_face-signal_PM_지시_초안.md`의
  최소 shader/face-signal R2 교정안 승인 전에는 `proper-first-face`로 진행하지 않는다.
- 최종 소비 화면·최적화·사용자 최종 승인 전에는 runtime handoff, finalizer, manifest와
  app runtime 등록을 만들지 않는다.
