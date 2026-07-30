# Developer 2 → Artist 2: S0 화로 primary/companion binding handoff

- handoff version: `v1.0.0`
- 작성일: `2026-07-31`
- 소비 요청:
  `art-workspace/review/artist-023/s0-prologue/ignite/st-s0-brazier/preflight/DEVELOPER-2-CONTRACT-REQUEST-v1.0.0.md`
- machine-readable contract:
  `app/src/assets/s0D1ArtBindingContract.js`
  - `S0_D1_ART_BINDING_CONTRACT_VERSION=1.1.0`
  - `S0_BRAZIER_LAYER_CONTRACT_VERSION=1.0.0`
- 상태: binding 확정. 이미지·source·runtime asset 생성 또는 등록 허가가 아님

## 상태·카메라 공통 계약

| 필드 | 확정 값 |
|---|---|
| screen/state/phase/interaction | `SCR-STORY-PROLOGUE / S0-STATE-CHARCOAL / ignite / S0-CHARCOAL-IGNITE` |
| semanticOwner | `artist-2.s0-prologue-story` |
| sourceMasterId | `CM-PROLOGUE-INHERITANCE-R1` |
| source master 사용 범위 | 프롤로그 화로의 형태·재질·원근·광원·접촉 topology를 위한 읽기 전용 시각 source. master 자체 runtime 등록·직접 crop·pixel 복사는 금지 |
| camera | `S0-BRAZIER-FIXED-V1`, fixed `16:9`, `contain` |
| bodyPartCount | `0` |
| DOM safe rect | FHD `128,936,1664,104`; 720 `85,624,1109,69` |

## Primary — `ST-S0-BRAZIER`

| 필드 | 확정 값 |
|---|---|
| componentId | `prologue.brazier-and-charcoal` |
| requiredAssetId | `ST-S0-BRAZIER` |
| stateVariant | `cold-to-ignited` |
| FHD visualBounds | `648,376,624,432` |
| 720 visualBounds | `432,251,416,288` |
| FHD interactionBounds | `752,480,416,288` |
| 720 interactionBounds | `501,320,277,192` |
| layer / zOrder | `architecture / 20` |
| 소유 pixel | 차가운 화로 몸체·테두리·손잡이·다리, 숯을 받치는 내부 구조 |
| metadata-only | 숯 contact anchor. raster에 anchor 표시·marker를 굽지 않음 |
| 금지 pixel | 보이는 숯 조각 전체, 불씨·발광·불꽃·연기·재·spark·ignition mask |

`cold-to-ignited`는 S0 component stateVariant 식별자다. Primary raster 자체는 모든 점화 단계에서
정적인 차가운 화로 구조만 유지하며 점화 상태 pixel을 소유하지 않는다.

## Companion — `PR-CHARCOAL-IGNITION`

| 필드 | 확정 값 |
|---|---|
| componentId | `prologue.ignitionVfx` |
| requiredAssetId | `PR-CHARCOAL-IGNITION` |
| stateVariant | `off-to-stable` |
| FHD child visualBounds | `736,408,448,224` |
| 720 child visualBounds | `491,272,299,149` |
| interactionBounds | `null`; parent interactionBounds를 공유하거나 child bounds로 복사하지 않음 |
| layer / zOrder | `vfx / 50` |
| 독점 pixel | 꺼짐 상태를 포함한 모든 보이는 숯 조각, 불씨·발광·불꽃·연기·재·spark·ignition mask |

Child bounds는 parent visual envelope 내부의 명시적 runtime design rect다. parent
interactionBounds와 서로 다른 값이며 interaction에서 추정하지 않는다. FHD를 정본으로 하고
720은 각 축·크기에 `2/3 contain` 후 정수 반올림한 값이다.

## Primary/companion 조립과 no-double-render

1. 실제 CHARCOAL 화면의 승인 시각 layer는 최대 두 장이다.
   - z20 `ST-S0-BRAZIER` 한 장
   - z50 `PR-CHARCOAL-IGNITION` 한 장
2. Primary에는 dark/off 숯을 포함해 보이는 숯 pixel을 넣지 않는다.
3. Companion에는 화로 몸체·테두리·손잡이·다리 pixel을 넣지 않는다.
4. 같은 숯·발광·VFX pixel을 두 asset에 중복 저장하거나 동시에 두 번 그리지 않는다.
5. Companion exact ID가 미승격이면 child bounds에 `개발 중` placeholder를 유지한다.
   Primary에 임시 숯을 bake하거나 가까운 승인 VFX를 대체 사용하지 않는다.
6. Primary exact ID가 미승격이면 parent envelope에 placeholder를 유지한다.
7. DOM action·문자·버튼·focus·진행 표시는 두 raster에 포함하지 않는다.
8. 손·팔·전신·도구·아키·노트·열쇠·대문 pixel은 두 asset 모두 금지한다.

## Artist 2 다음 단일 gate

첫 후보는 요청서대로 `ST-S0-BRAZIER / cold-to-ignited`의 차가운 화로 단독 한 장이다.
`PR-CHARCOAL-IGNITION`은 별도 사용자 승인과 후속 gate 전까지 제작·합성·runtime 등록하지 않는다.
후보 metadata는 이 handoff `v1.0.0`과 machine-readable contract 두 버전을 함께 참조한다.

## Developer 2 검증

- contract unit: source master·primary/companion ID·child bounds·layer·pixel 소유·
  interaction bounds 비재사용·no-double-render 자동 검사
- `validateS0D1ArtBindingContract()` 오류 `0`
- FHD/720 child bounds가 parent envelope 내부
- `bodyPartCount=0`
- 이 handoff 작성 과정에서 Artist 이미지·source·runtime asset·manifest 변경 없음
