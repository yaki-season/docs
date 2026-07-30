# Developer 3 → Developer 1·2·QA: S0에서 실제 D1 전체 영업 공개 경로

## 확정 URL 흐름

```text
/src/public-shell.html
  → 새 게임
  → /src/s0-d3.html?new=1
  → 열쇠 선택 → 대문 열기 → 숯 점화
  → S0 이야기 또는 3줄 요약
  → D1 영업 전 이야기 또는 3줄 요약
  → day-start checkpoint 저장 성공
  → /src/d1-game.html
```

정상 경로는 `page.route` 없이
`/content/releases/d1-business-day-definition.v1.json`을 HTTP 200으로 소비한다.
`campaignBridge.startDay()`가 실패하면 이동하지 않고 `campaign-error`를 표시한다.

## 공통 저장

- prefix: `yaki-season.dev2-scenario.`
- campaignId: `scenario-s0-d3`
- contentVersion: `content-s0-d3-r1`
- seed: 기존 캠페인의 값을 보존
- D1 진입 active: `checkpointType=day-start`, payload `d1/pre-open`
- D1 runtime: 같은 저장을 읽은 뒤 in-memory `d1/business` session을 시작
- D1 완료 active: `checkpointType=day-complete`, payload `d2/pre-open`

새 namespace를 추가하지 않았고 기존 active·backup-1·backup-2 transaction 정책을 그대로
사용한다.

## Developer 1 인계

실제 S0 진입에서 D1 전체 영업·정산·D2 저장까지 golden을 다시 확인한다.

```sh
npx playwright test tests/e2e/s0-d1-public-flow.spec.js \
  --grep "D1 완료 저장"
npx playwright test tests/e2e/d1-business-day.spec.js
```

첫 명령은 정적 release를 무주입으로 소비한다. production에 fixture fallback을 추가하지 않는다.

## Developer 2 인계

public shell의 새 게임 URL은 그대로 `s0-d3.html?new=1`이며, D1 pre-open 완료 뒤 실제
`d1-game.html`로 같은 탭에서 이동한다. `D1-business-placeholder`, 임시 `영업 결과 보기`,
가짜 settlement는 D1 사용자 경로에 더 이상 나타나지 않는다. D2·D3 deferred placeholder
범위와 `grillUiLayout`은 변경하지 않았다.

## QA 인계

```sh
node tools/content/build-d1-release-definition.mjs --check
npx playwright test tests/e2e/s0-d1-public-flow.spec.js
npx playwright test tests/e2e/s0-d3-scenario.spec.js tests/e2e/public-shell.spec.js
npm test
git diff --check
```

핵심 판정은 FHD/720 각각 요약·전체 대사·키보드·저장 실패·S0/D1 새로고침·기존 저장
backup·D1 완료/이어하기 7개, console error/pageerror 0, static release 200, 6석,
첫 wave `REGULAR_TSUKIOKA`, totals `4/4/9`, 완료 ledger 1건이다.
