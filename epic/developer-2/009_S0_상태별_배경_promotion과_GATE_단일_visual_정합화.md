# 개발자 2 - 009 S0 상태별 배경 promotion과 GATE 단일 visual 정합화

- 문서 버전: `v1.1.0`
- 최종 변경일: `2026-07-30`
- 상태: `완료`
- 담당자: `개발자 2`

## 참조 spec

- `spec/system/SYS-002_2.5D_스테이션_렌더링과_레이어_계약.md` - `SYS-002` - `v3.0.0`
- `spec/ui/UI-002_전체_게임_화면_입력_접근성.md` - `UI-002` - `v5.25.0`
- `spec/ui/UI-003_전체_게임_상세_화면_설계.md` - `UI-003` - `v1.2.0`
- `spec/art/ART-003_런타임_아트_에셋_목록과_제작_계약.md` - `ART-003` - `v5.9.0`
- `epic/developer-2/007_S0-D1_아트_binding_inventory와_재조립_harness.md` - 선행 완료 - `v1.1.0`
- `epic/developer-2/008_S0_외관_공통_배경_binding과_semanticOwner_정합화.md` - 선행 완료 - `v2.0.0`
- `epic/artist/023_S0_프롤로그_정식_아트.md` - Artist 2 - `v1.1.0`

## 목표

`BG-EXTERIOR-S0-CLOSED`와 `BG-EXTERIOR-S0-GATE-OPEN`의 유효 finalizer handoff가
각각 도착하면 서로 독립된 dry-run·30분 receipt·write transaction으로 승격하고 exact
S0 gameplay state에 연결한다. GATE 화면은 열린 대문 픽셀이 포함된 background 한 장만
runtime visual로 사용하며 `PR-SHOP-GATE-S0 R6`를 중복 visual layer로 그리지 않는다.

## 범위

### 포함

- 두 background handoff의 독립 SHA·증빙·bundle·owner·manifest preflight와 promotion
- KEY/GATE exact background binding 및 상태별 placeholder 제거
- `PR-SHOP-GATE-S0 R6`의 background 제작 provenance·interaction 의미/bounds reference 유지
- GATE runtime visual 한 장, DOM/scene interaction, FHD/720 동일 camera·DOM safe 검증
- 열린 대문 이중 윤곽 0·닫힌 문 잔존 0·body part 0 계약

### 제외

- 완료된 작업 007·008 문서 수정
- `PR-SHOP-GATE-S0`의 별도 runtime promotion 또는 visual layer 등록
- Artist source·review binary·provenance 수정
- manifest 수동 편집, `runtimeRegistrationAllowed` 수동 변경, 영수증 없는 write
- S0 3상태·3클릭 gameplay 변경

## 작업 절차

1. GATE contract를 breaking `v3.0.0`으로 올리고 runtime visual ID를
   `BG-EXTERIOR-S0-GATE-OPEN` 한 개로 고정한다.
2. `PR-SHOP-GATE-S0 R6`는 FHD interaction `(720,224,480,624)`, 720
   `(480,149,320,416)`과 provenance 역할만 남기고 runtime visual/promotion을 금지한다.
3. 실제 `s0-d3.html`이 exact approved background를 manifest에서 해석하고 exact ID가
   없으면 해당 state placeholder를 유지하도록 연결한다.
4. 두 finalizer handoff가 도착할 때마다 다른 항목과 receipt를 공유하지 않고
   `dry-run → receipt identity·30분 만료 검증 → 동일 receipt --write → exact binding`을 수행한다.
5. assets/reference/unit/FHD·720 실제 S0 3클릭 회귀를 실행한다.

## 의존성과 인계 조건

- 선행 작업: Developer 2 작업 007·008 완료
- 병행 작업: Artist 2 작업 023, Developer 2 작업 003, 통합 QA·릴리스 작업 001
- 후속 작업: Developer 2 작업 006
- Artist 2 입력: 각 background의 별도 `runtime-handoff.json`, final approval·completion·
  recomposition·optimization, bundle manifest와 실제 SHA
- `PR-SHOP-GATE-S0 R6`는 두 background handoff와 별개이며 promotion 입력이 아니다.

## 완료 기준

- [x] GATE runtime visual이 `BG-EXTERIOR-S0-GATE-OPEN` 한 장뿐이다.
- [x] `PR-SHOP-GATE-S0 R6`는 provenance·interaction reference이고 별도 visual/promotion이 금지된다.
- [x] exact GATE background만 있으면 PR 미승격 상태에서도 승인 화면이 표시된다.
- [x] KEY/GATE exact ID가 없으면 각각 placeholder를 유지한다.
- [x] FHD/720 camera·DOM safe가 동일하고 action DOM과 interaction bounds의 교차가 0이다.
- [x] 열린 대문 윤곽 layer 1·중복 0, closed residual 0, body part 0을 자동 검증한다.
- [x] `BG-EXTERIOR-S0-CLOSED` handoff를 독립 receipt transaction으로 승격·binding한다.
- [x] `BG-EXTERIOR-S0-GATE-OPEN` handoff를 독립 receipt transaction으로 승격·binding한다.
- [x] 두 background의 실제 승인 URL·SHA를 handoff와 dashboard에 기록한다.

## 구현 및 검증 결과

- app 구현 위치:
  - `src/assets/s0ExteriorBackgroundBindingContract.js` - contract `v3.0.0`, single visual policy
  - `src/s0-exterior-background-harness.js` - exact approved/placeholder와 중복 visual 0 보고
  - `src/s0-d3.html`, `.css`, `.js` - 실제 S0 exact background resolver와 DOM interaction
  - `tests/unit/s0ExteriorBackgroundBindingContract.test.js`
  - `tests/e2e/s0-exterior-background-harness.spec.js`, `s0-d3-scenario.spec.js`
- 구현 기준 spec 버전: 위 참조 spec
- 구현 기준 태스크 버전: `v1.1.0`
- 검증 방법: assets/reference, contract·owner Vitest, Chromium FHD/720 harness·실제 S0 3클릭
- 검증 결과:
  - `npm run assets:validate`: runtime asset 11개 통과
  - `npm run visual:references:validate`: reference 36장 통과
  - 전체 Vitest: 42파일·300개 통과
  - Chromium FHD/720 contract·실제 S0·공개 전환: 34개 통과
- promotion 실행 결과:
  - `BG-EXTERIOR-S0-CLOSED@R2-B1`: 독립 dry-run·30분 receipt 검증·write 완료,
    `/assets/core/s0/prologue/bg-exterior-s0-closed-r2-b1.png`,
    SHA `504bc88e874a53e6a69e7dcfaad11cc949b654166d17815cae15a9948e67be21`
  - `BG-EXTERIOR-S0-GATE-OPEN@R1-B1`: 별도 dry-run·30분 receipt 검증·write 완료,
    `/assets/core/s0/prologue/bg-exterior-s0-gate-open-r1-b1.png`,
    SHA `c97f4c435a2d5ab78e8603879dfbad79a5a4569963d939b9ae354aefb42d6f6a`
  - 두 receipt는 write 성공으로 소비됐고 `PR-SHOP-GATE-S0` manifest entry는 없다.
- 남은 위험: 없음. Artist 2 source와 `runtimeRegistrationAllowed`는 수정하지 않았다.

## 변경 이력

| 이전 버전 | 새 버전 | 날짜 | 변경 유형 | 근거 spec 버전 | 변경 내용 | 재작업 영향 |
|---|---|---|---|---|---|---|
| 없음 | `v1.0.0` | `2026-07-30` | 후속 작업 생성·구현 | `SYS-002 v3.0.0`, `UI-002 v5.25.0`, `UI-003 v1.2.0`, `ART-003 v5.9.0` | 완료 작업 008 뒤 GATE 중복 visual을 제거하고 실제 S0 exact background resolver와 독립 promotion 대기를 구현 | handoff 전 manifest는 유지하며 작업은 `진행 중` |
| `v1.0.0` | `v1.1.0` | `2026-07-30` | 독립 promotion·완료 | 동일 | CLOSED R2와 GATE-OPEN R1의 handoff·증빙·bundle SHA를 검증하고 각각 별도 30분 receipt로 write했다. exact state binding, placeholder 제거, PR 비시각 reference, FHD/720 34개 회귀를 확인했다. | runtime asset 11개, 두 background 승인 적용; 작업 완료 |
