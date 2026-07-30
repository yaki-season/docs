# Artist 3 → Developer 1 D1 commuter extra versioned contract request

- 날짜: `2026-07-31`
- 요청자 / 소비 범위: Artist 3 / 작업 025
- 대상 stable asset: `CH-EXTRA-COMMUTER-SERVICE`
- known runtime identity: `SCR-SVC-CUSTOMERS` / `D1-extra-commuter` / `customers.actor.commuter`
- 현재 상태: inventory placeholder `pending`; pixel candidate, approval, finalizer, manifest registration 없음

## 요청 입력

Artist 3은 어떤 actor geometry도 추측하지 않는다. 다음을 하나의 versioned Developer 1 contract로 제공해 달라.

1. eligible seat actor의 FHD와 1280×720 visual bounds, lower-centre anchor/pivot, scale/placement rule;
2. canonical `customerOcclusionLine`의 FHD/720 값 및 screen line / mask ownership;
3. commuter extra의 필수 D1 clip/state 목록과 state-to-clip mapping;
4. 이름 없는 commuter가 표현할 수 있는 role-level 의미(고유 이름·개인 서사 금지);
5. 클릭 규칙: raster actor는 비상호작용, transparent `seatServe:<seatId>` raycast/hit target의 소유·bounds·분리 규칙, raster에 hit/button 표현 금지.

## 고정 제약

- `ART-003`의 공통 16 clip은 art baseline이다. 이 요청의 명시적 runtime mapping 전에는 subset이나 fallback을 정하지 않는다.
- 현재 `sceneLayout`의 정규화 seat/occlusion 값은 versioned asset input이 아니므로 재사용·추론하지 않는다.
- 서빙 표현은 `공용 준비 음식 선택 → 손님 선택 → 선택한 손님의 남은 주문`만 따른다. FIFO·주문 번호·먼저 주문한 손님 강조·우선순위 화살표를 넣지 않는다.
- `CH-EXTRA-SOLO-SERVICE`, cleanup, settlement는 요청 범위 밖이다.

## Artist 3 gate

Developer 1의 versioned 입력과 사용자 preflight 승인이 모두 도착할 때까지 `CH-EXTRA-COMMUTER-SERVICE`는 zero-pixel 상태를 유지한다. 이 dispatch는 동결된 `ST-DRINK-BEER-TIER-1 R2`의 승인·pipeline·후속 drink asset을 변경하거나 선행하지 않는다.
