# PM 지시 초안 — D1 cooking-second-face R2 최소 face-signal 교정

## 결정 요청

`MDL-NEGIMA-GRILL-COOKING-SECOND-FACE R1`의 실제 station 소비 검수는 사용자 후보로 제출하지 않았다.
FHD·720에서 180° 뒤집기에 따른 대파 단면과 재료 표면 방향의 반전은 보이지만, 반대면이 조리 중임을
읽게 해야 하는 부분 sear가 약하다. `rotationY=PI`나 전체 픽셀 차이만으로 통과 처리하지 않는다.

검수 증빙은 아래에 있다.

- FHD: `art-workspace/review/artist-000/d1-cooking/grill/recomposition/cooking-second-face-station-consumption/r1/review-mdl-negima-grill-cooking-second-face-station-fhd-r1.png`
- 720: `art-workspace/review/artist-000/d1-cooking/grill/recomposition/cooking-second-face-station-consumption/r1/review-mdl-negima-grill-cooking-second-face-station-720-r1.png`
- 동일 위치 first/second 비교판: `art-workspace/review/artist-000/d1-cooking/grill/recomposition/cooking-second-face-station-consumption/r1/review-mdl-negima-grill-cooking-face-compare-fhd-r1.png`

## 제안하는 R2 범위

기존 `MDL-NEGIMA-GRILL-COOKING-SECOND-FACE` stable ID의 새 revision R2만 만든다.

- 476 triangles, 승인 GLB 3종, 승인 nearest albedo 파일은 byte-identical로 재사용한다.
- 새 raster·GLB·texture·atlas·bake·복제 texture는 만들지 않는다.
- `rotationY=PI`, completedFlips=1, face별 누적시간·domain ownership은 바꾸지 않는다.
- 두 번째 면의 cloned fragment shader에만, 뒤집힌 decal UV를 기준으로 한 굵고 제한된 pixel-cell sear
  cluster를 넣는다. 현 48×48 미세 noise가 720에서 묻히므로, 닭에는 2~3개의 비대칭 갈색/호박 cluster,
  대파에는 1~2개의 절제된 olive/caramel char band가 실제 720 픽셀에서 읽히도록 한다.
- `face1HeatProgress=0.52`의 총 열량 의미는 보존한다. R2의 목적은 더 익히는 것이 아니라 같은 누적시간의
  반대면에 고유한 sear 분포와 decal 방향을 보이는 것이다.

## R2 합격 조건

- CM master R3와 raw R3 공통 slot0 anchor `(609.6,515)`, scale `(1.45,1.18)`, rotation
  `(-0.035,0.055,0)`을 정확히 보존한다.
- FHD와 1280×720 실제 station 캡처에서, first/second 나란히 비교 시 대파 단면 방향과 함께 닭의
  비대칭 sear cluster 2개 이상, 대파의 국소 char mark가 눈으로 구별된다.
- UI·문자·타이머·품질 badge·손·집게·추가 음식·연기/ember는 넣지 않는다.
- 사용자 승인 전에는 proper-first-face, optimization, finalizer, runtime handoff, manifest 등록으로
  진행하지 않는다.

PM이 이 최소 R2 범위를 승인하면 Artist 1은 R2 한 후보만 제작·실제 FHD/720 소비 검수·사용자 승인 gate를
진행한다.
