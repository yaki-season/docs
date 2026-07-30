# Developer 1 → Developer 2 자유 뒤집기·손님 중심 제공 handoff

- 날짜: `2026-07-30`
- 구현 기준: `GPL-004 v1.39.0`, `GPL-003 v4.12.0`
- 소비 작업: Developer 2 작업 010
- domain 원본:
  - `app/src/render/cookStations.js`
  - `app/src/domain/businessDay/d1BusinessDay.js`
- UI·CSS·아트·manifest·저장 schema는 이 인계에서 변경하지 않았다.

## 그릴 view contract

- `slotViews(now)`의 기존 필드는 유지한다:
  `status`, `contactFace`, `orientationFaceDown`, `frontElapsedSec`, `backElapsedSec`,
  `flipping`, `flipProgress`, `visualRotationRad`, `inputLocked`.
- `active`이고 `flipping=false`, `inputLocked=false`인 slot은 익힘 단계와 무관하게 뒤집을 수 있다.
  `beginFlip()`과 `clickSlot()`은 더 이상 `not-ready`를 반환하지 않는다.
- `clickSlot()`은 두 면 중 하나라도 `under`면 반대 면으로 0.3초 회전을 시작해
  `{ok:true, flipping:true, flipped:true, fromFace, targetFace, completeAt}`를 반환한다.
- 두 면이 모두 `under`를 벗어난 뒤 `clickSlot()`을 호출하면 회수한다. 회수 결과의
  `quality.frontResult`·`quality.backResult`는 그 순간의 최종
  `frontElapsedSec`·`backElapsedSec`를 다시 분류한 값이다. 중간 뒤집기 결과는 고정하지 않는다.
- 회전 중에는 `reason=flipping`, 재접촉 뒤 0.3초 잠금 중에는 `reason=input-locked`다.
  이 두 구간은 기존처럼 중복 입력을 적용하지 않는다.
- Developer 2는 현재 접촉면이 `front|back`이라는 이유로 `뒤집기|회수`를 결정하면 안 된다.
  두 면의 누적 판정 중 하나라도 `under`면 `뒤집기`, 둘 다 준비됐으면 `회수`로 표시한다.
- 저장·복구와 offGrill 재투입 뒤에도 위 view의 양면 누적·방향·접촉면을 그대로 소비한다.
  snapshot version과 저장 schema는 변경하지 않았다.

## 손님 중심 제공 contract

- `SERVE_ITEM` intent 입력은 기존과 같다:
  `{intentId, customerId|seatId, menu|menuId, quality}`.
- 선택한 `customerId`의 현재 주문에 해당 메뉴의 미제공 수량이 있으면 주문 line 순서,
  `order.guided`, 다른 손님의 입장·주문 접수 순서와 무관하게 한 항목을 적용한다.
- D1 첫 주문은 네기마를 생맥주보다 먼저 제공할 수 있다. 먼저 주문한 손님 A를 건너뛰고
  나중에 주문한 손님 B에게 먼저 제공할 수도 있다.
- domain은 더 이상 `guided-order-sequence`를 반환하지 않는다. Developer 2 작업 010은
  `D1_GUIDED_ORDER_SEQUENCE` 오류·문구·테스트 기대를 제거하되, 이전 enum 제거 여부는
  UI port 호환성 범위에서 결정한다.
- 계속 거부하는 입력:
  존재하지 않는 손님, 제공 대기 상태가 아닌 손님, 선택한 손님 주문과 다른 메뉴,
  이미 수량을 모두 채운 메뉴, 유효하지 않은 품질. 같은 `eventId` 재입력은 기존처럼
  `{applied:false, duplicate:true}`이며 판매·수량·회복을 중복 반영하지 않는다.

## 검증 증빙

- Developer 1 타깃 단위·통합: `28/28`
- Developer 3 작업 011 독립 계약: `4/4`
- 전체 Vitest: `43파일·308/308`
- Chromium FHD/720 전체 영업·영업 중 복구·프로덕션 조리/서빙: `18/18`
- golden 유지: 방문 4·주문 4·항목 9·정리 4, 매출 36·팁 8·합계 44·명성 12,
  settlement ledger 1, 저장·새로고침 뒤 `d2/pre-open`
