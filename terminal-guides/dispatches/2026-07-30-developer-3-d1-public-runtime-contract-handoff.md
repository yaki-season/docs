# Developer 3 → Developer 1: D1 공개 runtime contract 인계

## Import

`app/src/content/d1PublicRuntimeContract.js`에서 `buildD1PublicRuntimeContract`를 import하고,
기존 content loader가 만든 `{ days, orders }` bundle을 넘긴다.

```js
import { buildD1PublicRuntimeContract } from '../content/d1PublicRuntimeContract.js';

const d1Contract = buildD1PublicRuntimeContract(contentBundle);
```

## Runtime config 구조

- `runtime.spawnIntervalSec`: 정본 `day-d1.json`의 입장 간격.
- `runtime.economy`: `basePrice`, `qualityMultGood`, `qualityMultLow`, `tipBase` 원본.
- `runtime.expected`: 계획 주문에서 계산한 `customers`, `orders`, `items`.
- `runtime.firstOrder`: `D1-ORDER-001`의 보존된 항목 순서·수량·양념.
- `runtime.grill`: 고정 6칸, 업그레이드 비활성, 3개 staged 뒤 같은 시각 시작 계약.
- `settlementExample`: `calculateD1PublicSettlement(runtime.economy, ['good', 'low'])`가 계산한 Good 1건+Low 1건 정산.

## Fixture

- 정상: `D1-PUBLIC-RUNTIME-VALID-BASELINE`, `D1-PUBLIC-RUNTIME-VALID-GOOD-LOW-SETTLEMENT`
- 오류: `D1-PUBLIC-RUNTIME-ERR-LEGACY-SPAWN-12`, `D1-PUBLIC-RUNTIME-ERR-LEGACY-BASE-100`, `D1-PUBLIC-RUNTIME-ERR-LEGACY-SETTLEMENT-250`, `D1-PUBLIC-RUNTIME-ERR-FIRST-ORDER`, `D1-PUBLIC-RUNTIME-ERR-GRILL-SLOTS`, `D1-PUBLIC-RUNTIME-ERR-STAGGERED-START`, `D1-PUBLIC-RUNTIME-ERR-DUAL-FACE-TICK`

## E2E 적용

- `production-groups`: 과거 spawn `12` 상수 대신 `d1Contract.runtime.spawnIntervalSec`.
- `production-growth`: 과거 기본가 `100` 또는 직접 day JSON 참조 대신 `d1Contract.runtime.economy.basePrice`; 10% 업그레이드 반올림 결과는 계약 밖 PM 결정으로 유지.
- `production-settlement`: 과거 합계 `250` 또는 개별 수식 복사 대신 `d1Contract.settlementExample` 또는 `calculateD1PublicSettlement`.

정산 근거는 Good 매출 `basePrice × qualityMultGood`, Low 매출
`basePrice × qualityMultLow`, 팁 `tipBase × 2`, 합계는 두 매출과 팁의 합이다. 현재 값은
contract builder가 계산하며 E2E에 숫자를 복사하지 않는다.
