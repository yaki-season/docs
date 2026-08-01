# 개발자 2 - 012 S0 아키 이야기 초상 binding 계약

- 상태: `완료`
- 담당자: `개발자 2`
- 우선순위: `P1`
- 현재 milestone에 필요한 이유: Artist 2의 S0 아키 초상 제작을 여는 exact 계약이다.
- 하지 않으면 막히는 stage gate: S0 이야기 placeholder 0과 S0 사람 화면 PASS

## 참조 spec

- `spec/scenario/SCN-002_프롤로그와_30일_캠페인_시나리오.md` - `SCN-002` - `v1.6.0`
- `spec/ui/UI-002_전체_게임_화면_입력_접근성.md` - `UI-002` - `v5.25.0`
- `spec/ui/UI-003_전체_게임_상세_화면_설계.md` - `UI-003` - `v1.2.0`
- `spec/art/ART-003_런타임_아트_에셋_목록과_제작_계약.md` - `ART-003` - `v5.9.0`
- `epic/artist/023_S0_프롤로그_정식_아트.md` - Artist 2 생산자 - `v1.1.0`
- `epic/pm-orchestration/001_S0_프롤로그_완전_구동과_사람_화면_검수.md` - stage gate - `v1.0.0`

## 목표

Artist 2가 `CH-AKI-STORY`를 화면과 파일명을 추측하지 않고 제작할 수 있도록 실제 S0 이야기
runtime에서 component/state/variant, fixed camera, FHD/720 bounds, layer/zOrder, DOM safe rect,
허용 표정과 story-only 소비 규칙을 versioned binding 계약으로 확정한다.

## 범위

### 포함

- 현재 S0 이야기 화면의 실제 DOM·logical canvas·FHD/720 배치 감사
- `CH-AKI-STORY` exact component/state/variant와 placeholder fallback
- portrait visual/hit bounds, layer/zOrder, DOM safe rect, 대사·요약·skip과 비침범 규칙
- 피로·집중·실수·안도의 표정 variant와 하나의 source master 소비 계약
- Artist 2 handoff와 계약 단위·Chromium FHD/720 harness 검증

### 제외

- 아키 픽셀·GLB·texture 생성이나 구형 `CH-OWNER-STORY` 재사용
- Artist review·metadata 수정, 사용자 시각 승인, finalizer·manifest promotion
- 영업·조립·그릴·드링크 화면에서 아키를 노출하는 기능
- S0 대사 내용·순서 변경, 오디오

## 병렬·순차 계획

- 이 계약은 Artist 2의 화로·숯 stable ID와 쓰기 경로가 달라 병렬 가능하다.
- 계약과 FHD/720 harness가 먼저 완료된 뒤 Artist 2 무픽셀 preflight→사용자 승인→단일 후보 순서로 진행한다.
- 같은 `CH-AKI-STORY` 계약과 runtime binding에는 동시에 한 writer만 둔다.

## 사용자 승인 gate

- binding 계약과 placeholder 검증에는 사용자 시각 승인이 필요 없다.
- Artist 2의 무픽셀 preflight와 실제 FHD/720 초상 소비 화면은 사용자가 별도 승인한다.
- 승인 전 finalizer·promotion과 구형 ID fallback을 금지한다.

## 사람 화면 테스트

- PM 작업 001의 S0 전체 이야기·skip·3줄 요약 테스트에서 초상과 대사/UI 비침범을 확인한다.
- 계약 작업 자체는 자동 FHD/720 증거를 만들고 사람 `PASS`는 실제 승인 asset binding 뒤 받는다.

## 작업 절차

1. S0 story runtime과 현재 placeholder를 실제 화면·테스트에서 감사한다.
2. exact `CH-AKI-STORY` binding 필드를 versioned contract로 고정한다.
3. 구형 `CH-OWNER-STORY`와 유사 asset fallback을 차단한다.
4. FHD/720 logical canvas, bounds, layer, DOM safe rect를 harness와 단위 테스트로 검증한다.
5. Artist 2에 exact 값과 허용/금지 소비 규칙을 handoff한다.

## 의존성과 인계 조건

- 선행 작업: 개발자 2 작업 004, Artist 2 작업 023의 무픽셀 preflight
- 생산자: 개발자 2
- 소비자: Artist 2 작업 023
- 자동 인계: 계약 green → Artist 2 preflight 정합 → 사용자 승인 → 초상 후보 제작
- PM/QA 소비: PM 작업 001, 통합 QA·릴리스 작업 001

## 완료 기준

- [x] 요청 필드에 `unassigned`·추측값이 0개다.
- [x] `CH-AKI-STORY` exact ID와 구형 ID 금지가 contract·test에 고정된다.
- [x] FHD/720 bounds·layer·DOM safe rect에서 대사·skip·요약 UI 교차가 없다.
- [x] story-only 소비와 영업·조리 화면 노출 금지가 검증된다.
- [x] Artist 2가 파일명·camera·variant를 추측하지 않는 versioned handoff를 받는다.
- [x] 아트·manifest·runtime promotion은 변경하지 않는다.

## 구현 및 검증 결과

- app 구현 위치:
  - `src/assets/s0AkiStoryPortraitBindingContract.js`
  - `tests/integration/s0AkiStoryPortraitBindingContract.test.js`
- docs handoff 위치: 이 태스크와 PM run ledger
- 구현 기준 spec 버전: 위 참조 spec
- 구현 기준 태스크 버전: `v1.1.0`
- 검증 방법: binding contract 단위 테스트, Chromium FHD/720 S0 story harness
- 검증 결과:
  - contract `v1.0.0`, `story.actors / CHAR-AKI / CH-AKI-STORY`,
    `CM-AKI-STORY-PORTRAIT-R1`, `S0-AKI-STORY-FIXED-V1`
  - FHD visual `(192,224,384,512)`, 720 `(128,149,256,341)`, actor `z30`, DOM `z80`
  - 표정 `fatigue/focus/mistake/relief`, 현재 아키 대사 11개 exact mapping
  - 전용 `7/7`, 관련 `21/21`, 전체 Vitest `348/348`, Chromium FHD/720 `12/12`,
    runtime asset `12` 검증 통과
  - app 파일 SHA-256
    `da5b6ada6ad69a1d92abda02960ddd02a19b5dbc8b0fde57c78413cb8c25592d`
- 남은 위험: Artist 2 무픽셀 preflight와 실제 초상 후보는 사용자 승인 전이며 manifest에 exact asset이 없다.
