# Developer 3 → Developer 1: D1 정적 release artifact 공개 부팅

## Production import와 URL

```js
import {
  D1_BUSINESS_DAY_RELEASE_DEFINITION_URL,
  loadD1BusinessDayReleaseDefinition,
} from '../application/ports/d1BusinessDayDefinition.js';

const consumed = await loadD1BusinessDayReleaseDefinition({
  url: D1_BUSINESS_DAY_RELEASE_DEFINITION_URL,
});
```

공개 browser가 읽는 유일한 파일은
`app/content/releases/d1-business-day-definition.v1.json`이고 URL은
`/content/releases/d1-business-day-definition.v1.json`이다. 이 파일은 정적 서버에서 HTTP 200과
`application/json`으로 제공된다. production runtime에는 `tests/fixtures/` fetch 또는 코드 기본값
fallback을 추가하지 않는다.

## 갱신·drift 명령

```sh
node tools/content/build-d1-release-definition.mjs --check
node tools/content/build-d1-release-definition.mjs --write
```

`--check`은 현재 정본 builder 출력과 artifact의 byte를 비교하고 다르면 최상위 필드를 출력하며
파일을 변경하지 않는다. `--write`만 artifact를 갱신한다. generator는 build-time에만
`day-d1.json`, `early-campaign.json`, `types.json`, 검증된 `d1-full-day.json` fixture와
`buildD1PublicRuntimeContract`·`buildD1ReleaseDefinition`을 사용한다.

## 공개 검증 분리

- 기존 `tests/e2e/d1-release-definition.js`의 `routeD1ReleaseDefinition()` 테스트는 builder 결과의
  단위적 결정성 검증으로 유지한다.
- `tests/e2e/d1-release-static-smoke.spec.js`의 성공 smoke는 `page.route`나 fixture 응답 주입 없이
  실제 static artifact를 GET하고 D1을 부팅한다.
- FHD/720 모두 6석, artifact totals `4명/4주문/9항목`, first wave `REGULAR_TSUKIOKA`,
  `D1-OFFICE-A/B`, `D1-SOLO-A`, beer `6`, negima `3`, cleanup `3000`을 확인한다.
- 별도 404·`schemaVersion=2` 테스트는 D1 시작 실패를 유지하고 session·campaign 저장/진행을 만들지
  않는 것을 확인한다.

## 현재 artifact 식별

- SHA-256: `b1535f2f6b5a7f3fde6f32eb4cb016007df5ad790d760f92456ab70098507941`
- schema version: `1`
- source day: `d1`

## 금지

route 주입은 production 대체 경로가 아니다. 실제 artifact의 404·버전 오류를 development fixture나
과거 하드코딩으로 복구하지 말고 `D1_DEFINITION_LOAD_FAILED` 또는
`D1_DEFINITION_VERSION_UNSUPPORTED`의 명시적 시작 실패 상태를 보존한다.
