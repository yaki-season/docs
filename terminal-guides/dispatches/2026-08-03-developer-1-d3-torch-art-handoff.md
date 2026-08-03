# Developer 1 → Artist 1·3 D3 타레 모모·토치 아트 handoff

- 작성일: `2026-08-03`
- 발신: Developer 1 작업 010(완료)
- 수신: Artist 1 작업 026, Artist 3 작업 024
- 구현 기준: app `fbc7cd3`
- 소비 화면: `src/d1-game.html?day=d3`, `SCR-SVC-GRILL`
- 검증 기준: Vitest 381/381, Chromium D3 E2E 3/3, D4 읽기 전용 E2E 1/1

## 동결된 gameplay 상태

| stateId | 의미 | 품질 결과 | 저장 필드 |
|---|---|---|---|
| `D3-MOMO-TARE-READY` | 양면 조리 완료, 타레 전 | 회수 불가 | `tareApplied=false`, `torchState=none` |
| `D3-MOMO-TARE-APPLIED` | 타레 적용, 토치 대기 | 회수 불가 | `tareApplied=true`, `torchState=none` |
| `D3-MOMO-TORCH-ACTIVE` | 토치 홀드·좌우 스윕 중 | 회수 불가 | `torchState=active`, coverage·focus 누적 |
| `D3-MOMO-TORCH-UNDER` | 적용 범위 부족 | Good | `torchState=under`, `torchCompleted=true` |
| `D3-MOMO-TORCH-PROPER` | 고른 적정 마감 | Perfect+불향 | `torchState=proper`, `torchCompleted=true` |
| `D3-MOMO-TORCH-OVER` | 총 가열 과다 | OK | `torchState=over`, `torchCompleted=true` |
| `D3-MOMO-TORCH-FAILED` | 한 지점 집중 과열 | Fail | `torchState=failed`, `torchCompleted=true` |

상태 의미와 판정 임계값은 아트 교체로 변경하지 않는다. 음식 표면은 색만이 아니라 광택·그을림 분포·연기 강도로 구별한다.

## Artist 1 작업 026 소유

| requiredAssetId | componentId | 요구 결과 |
|---|---|---|
| `MDL-MOMO-D2-D3` | `grill.food.momo` | D1 승인 모델·카메라·pivot을 재사용한 모모 꼬치 |
| `MAT-MOMO-TARE-MASK` | `grill.food.tareSurface` | 미적용·적정·과다·탄을 파생하는 음식 표면 mask/shader recipe |
| `MDL-GRILL-TORCH` | `grill.torch` | 손·팔 없이 보이는 그릴 토치 도구 |
| `VFX-TORCH-FLAME` | `grill.torch.flame` | 점화·가동 루프, 음식·UI hit 영역 비침범 |
| `VFX-MOMO-TORCHED` | `grill.food.torchResult` | under/proper/over/failed 결과의 캐러멜화·그을림·연기 |

- 신규 음식 raster나 중복 GLB를 만들지 않고 승인 master와 shader 파생으로 제작한다.
- 현재 프로덕션 그릴의 가변 2~8칸 geometry를 소비한다. 구형 고정 6칸 전용 에셋을 새로 만들지 않는다.
- 각 revision은 `SCR-SVC-GRILL` 실제 slot 위치에서 FHD/720 한 상태 소비 화면을 제출한다.

## Artist 3 작업 024 소유

| requiredAssetId | componentId | 현재 placeholder/DOM |
|---|---|---|
| `PROP-TARE-BRUSH-D3` | `grill.tare.apply` | `#d3ApplyTare` |
| `UI-TORCH-COVERAGE-D3` | `grill.torchCoverage` | `#d3TorchCoverage`, progressbar |
| `UI-TORCH-FOCUS-WARN-D3` | `grill.torchFocusWarning` | `#d3TorchWarning` |
| `UI-TORCH-STATE-D3` | `grill.torchState` | `#d3TorchState` |
| `SFX-TORCH-LOOP` | `audio.grill.torchLoop` | 현재 무음 placeholder |

- coverage·집중 과열·완료 신호는 색 이외에 형태·텍스트·게이지 변화를 유지한다.
- 조작 화면에는 플레이어 손·팔·전신을 합성하지 않는다.
- 음식 표면 source는 Artist 1 작업 026 결과를 stable ID로만 읽고 수정하지 않는다.

## 런타임 교체 불변식

- DOM 입력·접근성 owner는 유지한다: `#d3TorchTrack`, pointer/touch hold-sweep, `Space + ←/→`.
- `data-testid`와 위 component/state ID를 바꾸지 않는다.
- 그릴↔드링크 화면 전환 및 새로고침 뒤 coverage·focus·완료 상태가 보존돼야 한다.
- 승인 전 결과는 `art-workspace/review/artist-026/` 또는 `review/artist-024/`에만 두고 runtime 승격하지 않는다.
- 최종 교체 뒤 D3 E2E 3개와 FHD/720 재조립을 재실행한다.

## 시작 조건

- stable ID handoff: 완료.
- Artist 1 작업 026 실제 제작 시작: Artist 1 작업 000 완료 뒤.
- Artist 3 작업 024 실제 제작 시작: Artist 3 작업 025 완료 뒤.
- 선행 완료 전에는 이 문서를 제작 packet으로 준비할 수 있으나 finalizer·runtime promotion은 금지한다.

