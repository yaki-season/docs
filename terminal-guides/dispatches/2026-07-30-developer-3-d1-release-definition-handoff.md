# Developer 3 → Developer 1: D1 release definition runtime adapter 입력

## Import

```js
import { buildD1PublicRuntimeContract } from '../content/d1PublicRuntimeContract.js';
import { buildD1ReleaseDefinition } from '../content/d1ReleaseDefinition.js';

const runtimeContract = buildD1PublicRuntimeContract(contentBundle);
const definition = buildD1ReleaseDefinition({
  bundle: contentBundle,
  developmentFixture: d1FullDayFixture,
  runtimeContract,
});
```

`definition`은 기존 `createD1BusinessDayDefinition(definition)`의 입력으로 직접 넘긴다. 별도
상수·개발 fixture의 고객/주문/가격 추측을 추가하지 않는다.

## 완전한 필드

| 필드 | 값 또는 규칙 | 정본 |
|---|---|---|
| `schemaVersion` | `1` | release definition |
| `sessionTargetMs` | `420000` | `day-d1.businessWindow.targetSessionSec` |
| `seatIds` | `seat-01`~`seat-06` | 검증 fixture |
| `timingMs` | think `4000~6000`, eat `15000`, leave `1000`, cleanup `3000`, waitRecovery `10000` | day + 검증 fixture |
| `limits` | active `2`, risk `1` | `day-d1` |
| `economy` | baseTip `2`, beer `6`, negima `3` | day + 검증 fixture |
| `totals` | customers `4`, orders `4`, items `9` | 작업 007 runtime contract |
| `waves[0]` | `0ms`, `REGULAR_TSUKIOKA`, `D1-ORDER-001` | 정본 fixed customer/order |
| `waves[1]` | `100000ms`, office 2인, `D1-ORDER-002-A/B` | 정본 source/order |
| `waves[2]` | `220000ms`, solo 1인, `D1-ORDER-003` | 정본 source/order |
| `requiresOrderCompletionIds` | wave별 order의 선행 조건을 순서 보존·중복 제거 | 정본 orders |
| `groupId` | office는 `D1-GROUP-OFFICE`; solo/regular는 `null` | type groupSize |

## 저장 가능한 runtime ID

- 고정 인물: 정본 `runtimeCustomerId`를 그대로 사용한다. 첫 손님은 `REGULAR_TSUKIOKA`다.
- `runtimeCustomerId=null` 엑스트라: `D1-${TYPE_UPPER}-${A..Z}`. 같은 wave·같은 type의 0부터
  A, B 순번을 사용한다. 현재 office는 `D1-OFFICE-A/B`, solo는 `D1-SOLO-A`다.

## 검증

- schema: `app/content/schema/d1-release-definition.schema.json`
- 정상 fixture: `D1-RELEASE-VALID-BASELINE`
- 오류 fixture: `D1-RELEASE-ERR-ORDER-QUANTITY`, `D1-RELEASE-ERR-MENU-PRICE`, `D1-RELEASE-ERR-TIMING`, `D1-RELEASE-ERR-WAVE-TIME`

정본 order·type·수량·순서·인내심·합계, day의 세션/think/eat/waitRecovery/상한,
작업 007 contract의 `4명·4주문·9항목` 및 negima 기준가를 교차 검증한다. fixture가 보존하는
leave/cleanup·wave `atMs`·beer 가격도 versioned schema와 projection equality로 고정한다.
