# Developer 3 handoff

- 상태: `012 완료`
- 담당 epic: `developer-3/006 v1.1.0` 보류, `developer-3/007 v1.0.2` 완료, `developer-3/008 v1.0.2` 완료, `developer-3/009 v1.0.1` 완료, `developer-3/010 v1.0.1` 완료, `developer-3/011 v1.0.3` 완료, `developer-3/012 v1.0.1` 완료
- 012 완료 동작: D1 첫 주문을 생맥주 1·네기마 2, 전체 4명·4주문·8항목으로 정합하고 공개 그릴 계약을 시작 2·최대 8·명성 해금, `initialPlacementCount=2`, `initialPlacementSlots=[1,2]`, 두 번째 배치 뒤 동시 시작으로 갱신했다. production customer sequence·대사와 batch 3 오류 fixture를 포함해 PM 재실행 전체 Vitest `47파일·358/358`, release drift, diff-check가 통과했다.
- Developer 1/011 exact 입력: `content/releases/d1-business-day-definition.v1.json` SHA-256 `fd4c9d9f3a4f189056da4dbfde940c7ad72604276137ca31a253e4d33422d073`; 주문 line 순서 제공 강제는 없고 먼저 준비된 주문 항목부터 제공 가능하다.
- 마지막 완료 동작: public shell의 새 게임에서 S0 세 클릭·이야기·D1 pre-open을 거쳐 day-start checkpoint 저장 뒤 실제 `d1-game.html`로 무주입 전환했다. 저장 실패 이동 차단과 같은 namespace·campaignId·seed의 session 연속성을 Chromium FHD/720 E2E `14/14`로 검증했고, release drift check와 전체 Vitest `299/299`가 통과했다.
- 개발자 1 인계: [D1 공개 runtime contract handoff](../dispatches/2026-07-30-developer-3-d1-public-runtime-contract-handoff.md), [D1 release definition handoff](../dispatches/2026-07-30-developer-3-d1-release-definition-handoff.md), [D1 static release artifact handoff](../dispatches/2026-07-30-developer-3-d1-static-release-artifact-handoff.md)를 따른다.
- 개발자 1·2·QA 인계: [S0→실제 D1 공개 경로 handoff](../dispatches/2026-07-30-developer-3-s0-d1-public-flow-handoff.md)를 사용하고, 과거 `D1-business-placeholder` 우회 경로를 합격 증거로 사용하지 않는다.
- PM 결정: `basePrice=3 × 1.1`이 반올림돼 가격 변화가 보이지 않는 문제는 임의 수정하지 않는다.
