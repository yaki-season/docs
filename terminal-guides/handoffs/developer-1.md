# 개발자 1 직전 작업 기록

## 2026-08-01 현재 우선 계약

- runId: `DEV1-D1-011-20260801-R1`; 단일 작업은 Developer 1 작업 011 `v1.2.1`.
- app 기준은 `main@20b8ee8` + Developer 3/012 dirty handoff다. release artifact SHA-256은
  `fd4c9d9f3a4f189056da4dbfde940c7ad72604276137ca31a253e4d33422d073`, 전체 Vitest `358/358`.
- D1 첫 주문은 생맥주 1·네기마 2, 시작 2칸·최대 8칸 명성 해금, placement 2/[1,2]와 두 번째 배치 뒤 동시 시작이다.
- 제공은 D1 튜토리얼을 포함해 주문 line 순서를 강제하지 않고 먼저 준비된 주문 항목부터 허용한다. 가이드는 완성품 카드→대상 손님→수량 버튼의 실제 입력 대상을 테두리·형태·문구로 시각화한다.
- 두 꼬치 안내는 준비 시각 우선(동률 낮은 슬롯), 단계 목록·현재 단계·다음 행동은 상시 유지하며 손·pulse·시범만 줄인다. 3회 무효 입력의 현재 단계 자동 완료는 도움 토글과 무관한 no-failure 복구다.
- 아래 2026-07-31 고정 6칸·첫 3개 기록은 과거 구현 증거이며 현재 구현 입력으로 사용하지 않는다.

- 마지막 갱신: `2026-07-31`
- 현재 담당: dashboard의 개발자 1 진행 중 epic 전체
- app/docs 기준: `app main@f22b546`, `docs main@d22cb47`; 공유 dirty worktree에서 Developer 1
  소유 파일만 증분 수정
- 현재 알려진 checkpoint: `slotViews()`가 `clickSlot()`과 같은 도메인 판정으로
  `nextAction='none'|'wait'|'flip'|'retrieve'`를 제공한다. 앞 3초 뒤 0.3초 공중 회전 종료의
  뒷면 0초 상태는 `flip`, 양면 8초 상태는 `retrieve`다
- 입력 수신: `/content/releases/d1-business-day-definition.v1.json`,
  SHA-256 `b1535f2f6b5a7f3fde6f32eb4cb016007df5ad790d760f92456ab70098507941`;
  builder drift check 일치, HTTP `200 application/json`
- 알려진 독립 실패: 없음. 최신 `npm test`는 `43파일·316/316` 통과
- 현재 PM 지시: Developer 2가 접촉면으로 행동을 추측하지 않도록 그릴 다음 행동의 도메인 정본을
  제공하고, Artist 3에 기존 좌석 actor geometry를 읽기 전용 versioned handoff로 전달
- 다음 한 동작: Developer 2 작업 011은 `nextAction`을 직접 소비해 조기 뒤집기 안내를 교정하고,
  Artist 3 작업 025는 별도 actor layout v1에서 geometry만 소비한다. 작업 010·D2/D3 신규 기능은
  시작하지 않음
- 주의: app에는 다른 개발자 presentation·asset binding 미커밋 파일이 함께 존재할 수 있음

## 개발자 2·Artist 1 소비 계약

- 단일 원본: `app/src/config/d1GrillLayout.js`
- 슬롯: `grillSlot0~5`; hit rect의 x는 `0.285/0.365/0.445/0.525/0.605/0.685`,
  공통 `y=0.42, width=0.065, height=0.30`. 이 raycast 계약은 R3 적용 전과 같다
- raw R3 visual rect는 공통 `y=0.23796296296296296`,
  `width=0.06822916666666666`, `height=0.4777777777777778`; x는
  `0.2838541666666667/0.3638541666666667/0.4438541666666667/
  0.5238541666666666/0.6038541666666667/0.6838541666666667`
- lane anchor는 공통 `y=0.47685185185185186`; x는
  `0.3175/0.3975/0.4775/0.5575/0.6375/0.7175`. raw·cooking·flipped·proper·burnt는
  슬롯별 같은 visualRect·anchor·source model transform(`horizontalScale=1.45`,
  `verticalScale=1.18`, root rotation `{-0.035,0.055,0}`)을 공유
- 첫 3개 동시 시작: 1·2번째 배치는 `status=staged`, `contactFace=null`,
  `frontElapsedSec=backElapsedSec=0`; 3번째 배치의 같은 `now`로 slot 0~2가
  `status=front`, `orientationFaceDown=contactFace=front`가 된다
- 완료 tray: `componentId=grill.finished`, `stableAssetId=ST-GRILL-FINISHED-TRAY`,
  `sourceRevision=6`, `sourceMasterId=CM-GRILL-STATION-QUEUED-SELECTION`,
  `sourceMasterRevision=3`, 승인 SHA
  `51abf390cbf31d40b50a7d99f08a5d37a2da70df6cf94d405788538bad7d9184`.
  `visualRect=reservedBounds={x:0.7989583333333333,y:0.11388888888888889,
  width:0.13854166666666666,height:0.3277777777777778}`,
  anchor `{x:0.8557291666666667,y:0.27870370370370373}`
- hitRect는 별도 mesh·필드지만 현재는 R6 alphaBBox 전체를 클릭하도록 visualRect와 같다.
  이후 hit 확장도 visualRect/reservedBounds를 움직이면 안 된다
- FHD 소비 기준: slot visual `131×516px`, x는
  `545/698.6/852.2/1005.8/1159.4/1313`, 공통 y `257`; hit `124.8×324px`; tray
  visual/hit/reserved `{x:1534,y:123,width:266,height:354}`, anchor `{x:1643,y:301}`
- 1280×720 소비 기준: slot visual `87.3333333×344px`, x는
  `363.3333333/465.7333333/568.1333333/670.5333333/772.9333333/875.3333333`,
  공통 y `171.3333333`; 승인 raster bbox는 slot0 `363,171,88,344`; hit `83.2×216px`; tray
  visual/hit/reserved `{x:1022.6666667,y:82,width:177.3333333,height:236}`,
  anchor `{x:1095.3333333,y:200.6666667}`
- R3 연속 석쇠 runtime safe bounds는 FHD `{x:500,y:210,width:960,height:580}`이며,
  slot0~5 visual/hit이 모두 내부에 있다. 이 bounds는 asset extraction bbox가 아니라 overlay 검증용이다
- 좌표 책임: Artist 1은 full-frame background와 master-aligned grate·tray visual 자산,
  Developer 1은 slot0~5 food overlay·조리 상태와 tray 전달, Developer 2는 receipt rail을 포함한 DOM UI가
  tray reserved bounds를 침범하지 않도록 소유
- 저장 payload에는 layout/rect/anchor가 없으므로 schema/migration 없음. Artist stable ID·art-workspace·
  manifest와 `SCN-001` 회귀 경로는 이 변경에서 수정하지 않음
- Developer 2 즉시 인계:
  `terminal-guides/dispatches/2026-07-30-developer-1-r6-finished-tray-layout-handoff.md`
- Artist 1·Developer 2 raw R3 인계:
  `terminal-guides/dispatches/2026-07-30-developer-1-raw-r3-footprint-runtime-handoff.md`

## 전체 영업 화면 소비 계약

- 실제 진입: `http://localhost:8777/src/d1-game.html`
- 개발 저장을 지우고 S0 종료 직후 D1부터 다시 시험할 때만
  `http://localhost:8777/src/d1-game.html?reset=1` 사용. 초기화 뒤 주소에서 query를 제거해
  이후 새로고침은 day-start 복구를 검증함
- 조립 adapter: `app/src/application/businessDay/d1BusinessDayBrowserSession.js`
- 화면 port: `D1BusinessDayUiPort`; 화면은 `getViewModel()`과
  `ACCEPT_ORDER/SERVE_ITEM/BEGIN_CLEANUP/CANCEL_CLEANUP/SET_RISK_COUNT/LOWER_CHARCOAL/REVEAL_SETTLEMENT_STEP`
  intent만 소비
- 종단 결과: 목표 `420000ms`, 방문 `4`, 주문 `4`, 제공 항목 `9`, 정리 좌석 `4`,
  매출 `36`, 팁 `8`, 합계 `44`, 명성 `12`, settlement ledger `1`, 저장 후 `d2/pre-open`
- 새로고침: 영업 중에는 `day-start`의 D1 pre-open으로 안전 복구하고, 완료 뒤에는 저장된
  D2 pre-open·보상·원장 1건을 그대로 재개
- 화면 placeholder: 엑스트라 `CH-EXTRA-COMMUTER-SERVICE`,
  `CH-EXTRA-SOLO-SERVICE`; 마감 `ST-CHARCOAL-CORE`; 정산 `UI-ECONOMY-ICONS`.
  DOM에 `data-placeholder=development`, `data-component-id`, `data-required-asset-id`를 유지
- 검증: 전체 Vitest `42파일·299/299`; release builder drift 일치; 일반 정적 URL 무주입
  D1 전체 영업·영업 중 복구 FHD/720 `4/4`; 정적 boot·404·버전 오류 FHD/720 `6/6`;
  조리→양면 8초 적정→finished tray 전달·tray 클릭 FHD/720 통과; R6 actual-alpha overlay와
  production raycast FHD/720 2회 반복 `8/8`; 최종 관련 Playwright 묶음 `20/20`
- 전체 영업 golden: 420,000ms, 방문 4·주문 4·항목 9·정리 4, 매출 36·팁 8·합계 44·명성 12,
  settlement ledger 1, 저장·새로고침 뒤 `d2/pre-open`, 중복 보상 없음
- 성공 경로에는 `page.route`를 사용하지 않았다. 404·지원하지 않는 version 오류 검증만
  실패 응답을 만들기 위해 route를 사용하며 fallback은 없다
- 이번 증분은 Developer 2 DOM, `src/assets`, content/schema/menus, Artist 파일·manifest·
  `runtimeAssetResolver`, 저장 payload/schema를 수정하지 않음

## 손님 직접 조작·그릴 뒤집기 계약

- 좌석 actor는 `screenLayout.SEATS[*].actor`와 `SEAT_ACTOR_UV`를 소비해 카운터 뒤 상체만 보인다.
  주문·서빙·퇴장 뒤 3초 정리는 `SEATS[*].hit`을 쓰는 별도 완전 투명
  `seatServe:<seatId>` raycast mesh로 처리한다. 보이는 테이블형 target은 없다
- 첫 네기마 3개를 대기 tray에서 그릴에 모두 올리면 같은 `now`로 slot0~2 앞면 조리가 시작된다.
  active 꼬치는 익힘 최소 시간 없이 클릭할 때마다 `0.3초` 동안 `contactFace=null`로 반대 면에
  뒤집힌다. 한 면이라도 `under`인 동안 클릭은 뒤집기이며, 양면이 모두 `under`를 벗어난 뒤의
  클릭만 최종 누적 시간을 재분류해 회수한다
- `slotViews()`가 view 전용 `flipProgress`·`visualRotationRad`를 제공하며 저장 snapshot과
  캠페인 schema에는 새 필드를 추가하지 않는다
- `SERVE_ITEM`은 선택한 `customerId`의 일치하는 미제공 line이면 `order.guided`, line 순서,
  다른 손님의 주문 접수 순서와 무관하게 적용한다. 네기마 선제공과 늦게 주문한 손님 선제공을
  허용하며, 메뉴·수량 불일치와 중복 event만 계속 차단한다
- Developer 2 작업 010 인계:
  `terminal-guides/dispatches/2026-07-30-developer-1-free-flip-customer-serving-handoff.md`
- 검증: Developer 1 타깃 단위·통합 `28/28`, Developer 3 작업 011 독립 계약 `4/4`,
  전체 Vitest `43파일·308/308`, 전체 영업·복구·프로덕션 조리/서빙 FHD/720 `18/18`,
  담당 변경 `git diff --check` 통과

## 그릴 다음 행동 view 계약 v1

- 정본: `app/src/render/cookStations.js`
- `slotViews(now)[i].nextAction`:
  - 빈 칸 `none`
  - staged·flipping·input locked·접촉면 없음 `wait`
  - 앞/뒤 중 하나라도 `under` `flip`
  - 양면 모두 `under`가 아님 `retrieve`
- `clickSlot()`과 `slotViews()`가 같은 판정 함수를 소비한다. UI는 `contactFace`나 현재 면의
  `doneness`로 행동을 재추론하지 않는다
- 뒤집기 입력 잠금은 공중 회전 0.3초와 같은 구간이다. 정확한 종료 시점에는
  `frontElapsedSec=3`, `backElapsedSec=0`, `contactFace=back`, `inputLocked=false`,
  `nextAction=flip`이다
- `nextAction`은 view projection만이며 snapshot `stateVersion=1`, 양면 누적·접촉면 필드,
  캠페인 저장 schema는 변경하지 않았다
- Developer 2 작업 011 인계:
  `terminal-guides/dispatches/2026-07-31-developer-1-cook-slot-next-action-v1.md`
- Artist 3 좌석 actor geometry 인계:
  `terminal-guides/dispatches/2026-07-31-developer-1-artist-3-extra-actor-layout-v1.md`
- 검증: `cookStations` `16/16`, Developer 3 계약 포함 `21/21`, 전체 Vitest
  `43파일·316/316`
