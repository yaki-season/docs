# 개발자 2 - 013 D1 시각 variant inventory 완전성 gate

- 문서 버전: `v1.0.1`
- 최종 변경일: `2026-07-31`
- 상태: `진행 중`
- 담당자: `개발자 2`
- 우선순위: `P1`
- 현재 milestone에 필요한 이유: required 맥주 visual 누락 상태의 placeholder 0 false positive를 막는다.
- 하지 않으면 막히는 stage gate: D1 placeholder 0의 신뢰성과 최초 공개 PASS

## 참조 spec

- `spec/gameplay/GPL-004_조리_스테이션과_품질_서빙.md` - `GPL-004` - `v1.39.0`
- `spec/ui/UI-003_전체_게임_상세_화면_설계.md` - `UI-003` - `v1.2.0`
- `spec/art/ART-003_런타임_아트_에셋_목록과_제작_계약.md` - `ART-003` - `v5.9.0`
- `spec/qa/QA-002_전체_게임_기능_성능_출시_검증.md` - `QA-002` - `v1.1.0`
- `epic/developer-2/003_D1_승인_아트_상태별_즉시_적용.md` - runtime promotion·inventory - `v1.5.0`
- `epic/artist/025_D1_드링크_서빙_정리_엑스트라_정산_정식_아트.md` - 생산자 - `v1.1.1`
- `epic/pm-orchestration/002_D1_전체_영업일_완전_구동과_사람_플레이테스트.md` - stage gate - `v1.0.0`

## 목표

D1 공개 inventory의 `placeholder 0` 판정이 component 42개만 세고 실제 화면에 필요한 맥주
liquid·VFX·제공 visual을 빠뜨리는 false positive를 막는다. `TEX-BEER-LIQUID`,
`VFX-BEER-CORE`, `FD-BEER-SERVED`의 runtime 필요 여부와 각 소비 state를 명시적으로 판정하고
required visual variant 전수 집계를 공개 gate에 포함한다.

## 범위

### 포함

- 현재 42개 D1 component inventory와 `s0D1ArtBindingContract` visual variant의 양방향 감사
- `TEX-BEER-LIQUID`, `VFX-BEER-CORE`, `FD-BEER-SERVED`의 required/derived/불필요 판정
- required로 판정한 ID의 component/state/variant/owner·placeholder·binding 집계
- 두 D1 진입점과 FHD/720 harness의 missing ID·placeholder 보고 정합
- QA가 false positive를 재현·차단할 contract test

### 제외

- 맥주 station·glass·liquid·VFX·served 픽셀 생성
- 승인 없는 asset promotion, manifest 수동 편집
- 드링크 gameplay·조작·품질 의미 변경
- Artist 3 후보 승인 대행

## 병렬·순차 계획

- 개발자 2 작업 012와 `s0D1ArtBindingContract` 쓰기 경로가 겹칠 수 있으므로 작업 012 뒤 순차 실행한다.
- Artist 3의 사용자 승인 대기와는 독립적인 계약 감사이므로 승인 결과를 기다리지 않는다.
- inventory 계약이 green이면 Artist 3에 누락 stable ID만 정확히 인계하고 후보별 승인 cycle은 별도로 진행한다.

## 사용자 승인 gate

- required ID 판정이 현재 spec·runtime 소비를 바꾸지 않는 감사이면 사용자 시각 승인이 필요 없다.
- 기존 gameplay 의미를 제거하거나 derived visual로 대체해야 하면 PM이 사용자 의미 결정을 먼저 요청한다.
- 실제 아트 후보는 사용자 승인 뒤에만 finalizer·promotion한다.

## 사람 화면 테스트

- D1 첫 손님과 전체 영업에서 빈 잔·liquid/foam·완성·제공 상태가 placeholder 또는 누락 없이
  구별되는지 PM 작업 002의 사람 플레이테스트로 확인한다.

## 작업 절차

1. component inventory, visual variant contract와 실제 D1 render 소비를 양방향 추적한다.
2. 세 stable ID를 required/derived/불필요 중 하나로 근거와 함께 판정한다.
3. required ID를 placeholder·missing·binding·owner 집계에 포함한다.
4. 일부 ID를 제외해도 placeholder 0으로 오판하지 않는 failure fixture를 추가한다.
5. 두 D1 진입점과 FHD/720 harness, QA 보고를 같은 수치로 맞춘다.
6. Artist 3와 QA에 exact ID·state·완료 조건을 handoff한다.

## 의존성과 인계 조건

- 선행 작업: 개발자 2 작업 012 `v1.1.0` 완료
- 병렬 입력: Artist 3 작업 025, 통합 QA·릴리스 작업 001
- 생산자: 개발자 2
- 소비자: Artist 3 작업 025, QA 작업 001, PM 작업 002
- 자동 인계: 작업 012 완료 → 이 작업 활성화 → inventory green → Artist 3·QA

## 완료 기준

- [ ] 세 stable ID가 근거와 함께 required/derived/불필요로 판정된다.
- [ ] required visual variant가 placeholder·missing·binding 집계에서 누락되지 않는다.
- [ ] 일부 visual variant 누락 상태에서 placeholder 0을 반환하지 않는 테스트가 통과한다.
- [ ] 두 D1 진입점과 QA가 같은 total·bound·placeholder 수를 보고한다.
- [ ] Artist 3가 추측 없이 필요한 exact ID와 소비 state를 받는다.
- [ ] 아트·manifest·promotion은 변경하지 않는다.

## 구현 및 검증 결과

- app 구현 위치: 선행 작업 뒤 확정
- 구현 기준 spec 버전: 위 참조 spec
- 구현 기준 태스크 버전: `v1.0.1`
- 검증 방법: inventory/contract unit, 두 D1 진입점 Chromium FHD/720 harness
- 검증 결과: QA 재현을 수신하고 선행 작업 012 완료 뒤 착수
- 현재 재현: assets `12/12`, reference `36/36`, D1 placeholder `33`이지만 위 세 ID는 집계 밖이라
  현재 placeholder 0 gate가 불완전하다.
- 남은 위험: derived와 required를 근거 없이 축소해 공개 false positive를 만드는 것

## 변경 이력

| 이전 버전 | 새 버전 | 날짜 | 변경 유형 | 근거 spec 버전 | 변경 내용 | 재작업 영향 |
|---|---|---|---|---|---|---|
| 없음 | `v1.0.0` | `2026-07-31` | QA 재현 기반 후속 작업 생성 | `ART-003 v5.9.0`, `QA-002 v1.1.0` | 맥주 liquid·VFX·served visual의 inventory 누락과 placeholder 0 false positive를 독립 gate로 분리 | 개발자 2 작업 012 뒤 실행하고 Artist 3·QA에 자동 인계 |
| `v1.0.0` | `v1.0.1` | `2026-07-31` | 선행 완료·자동 착수 | 동일 | 개발자 2 작업 012 `v1.1.0` 완료 뒤 같은 역할 agent에 required visual inventory 감사를 자동 인계 | QA가 보고한 최소 미집계 3건을 exact 판정·집계해야 함 |
