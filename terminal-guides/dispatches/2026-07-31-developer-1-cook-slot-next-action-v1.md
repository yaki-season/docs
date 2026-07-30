# Developer 1 → Developer 2 그릴 `nextAction` 계약

- 계약 버전: `v1.0.0`
- 전달일: `2026-07-31`
- 소비 작업: Developer 2 작업 011
- 도메인 정본: `app/src/render/cookStations.js`
- 기준: `GPL-004 v1.39.0`, Developer 3 작업 011 `v1.0.2`

## 공개 필드

`createCookStations().slotViews(now)[slotIndex]`는 기존 필드를 바꾸지 않고 다음 필드를 추가한다.

```text
nextAction: 'none' | 'wait' | 'flip' | 'retrieve'
```

같은 문자열은 `COOK_SLOT_NEXT_ACTION`으로 export한다.

| 상태 | `nextAction` |
|---|---|
| `status='empty'` | `none` |
| `status='staged'` | `wait` |
| `flipping=true` 또는 `inputLocked=true` | `wait` |
| `contactFace=null`인 공중·그릴 밖 상태 | `wait` |
| 앞·뒤 누적 판정 중 하나라도 `under` | `flip` |
| 앞·뒤 누적 판정이 모두 `under`가 아님 | `retrieve` |

`clickSlot()`과 `slotViews()`는 위 판정 함수 하나를 함께 사용한다. Developer 2는
`contactFace`, 현재 보이는 면 또는 단일 면 `doneness`로 뒤집기와 회수를 다시 추론하지 않는다.
`retrieve`는 두 면이 모두 `under`를 벗어났다는 뜻이며 두 면이 반드시 모두 `perfect`라는 뜻은 아니다.

## 0.3초 잠금 경계

- 뒤집기를 시작하면 `flip.completeAt=now+300`이고 그 공중 회전 구간 동안 입력이 잠긴다.
- 공중 회전 중에는 `contactFace=null`, `flipping=true`, `inputLocked=true`,
  `nextAction='wait'`다.
- 정확히 0.3초가 끝나면 새 접촉면으로 복귀하고 별도의 추가 0.3초 잠금은 시작하지 않는다.
- 앞면 3초 뒤 뒤집은 정확한 종료 시점은
  `frontElapsedSec=3`, `backElapsedSec=0`, `contactFace='back'`,
  `flipping=false`, `inputLocked=false`, `nextAction='flip'`이다.
- 앞·뒤가 각각 8초이면 `nextAction='retrieve'`이고 같은 시점의 `clickSlot()`은
  최종 누적 시간을 분류해 `Perfect` 회수를 반환한다.

이 경계가 `2026-07-30-developer-1-free-flip-customer-serving-handoff.md`의
“재접촉 뒤 추가 입력 잠금” 문장을 대체한다.

## 저장·UI 경계

- `nextAction`은 호출 시 계산하는 view projection이며 snapshot 또는 캠페인 저장 payload에 넣지 않는다.
- `frontElapsedSec`, `backElapsedSec`, `contactFace`, `orientationFaceDown`,
  `inputLockedUntil`, snapshot `stateVersion=1`은 그대로다.
- 이 변경은 Developer 2 DOM·CSS와 아트·manifest를 수정하지 않았다.

## 검증

- `npm test -- --run tests/unit/cookStations.test.js` → `16/16`
- `npm test -- --run tests/unit/d1FlipCustomerServingRegression.contract.test.js tests/unit/cookStations.test.js`
  → `21/21`
- `npm test` → `43파일·316/316`
