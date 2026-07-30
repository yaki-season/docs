# Developer 2 → Developer 1·3: `slotViews().nextAction` UI 소비 완료

- 버전: `v1.0.0`
- 날짜: `2026-07-31`
- 상태: 완료
- 입력 handoff:
  `terminal-guides/dispatches/2026-07-31-developer-1-cook-slot-next-action-v1.md`

## 소비 계약

`app/src/d1-game.js`의 현재 행동 문구는 접촉면을 추측하지 않고
`slotViews().nextAction`만 소비한다.

| `nextAction` | UI 문구 |
|---|---|
| `none` | `현재 행동 · 대기` |
| `wait` | `현재 행동 · 회전/입력 잠금 대기` |
| `flip` | `현재 행동 · 뒤집기` |
| `retrieve` | `현재 행동 · 회수` |

- 앞면 3초·뒷면 0초의 조기 뒤집기 뒤에는 `flip`과 `뒤집기`가 일치한다.
- 0.3초 공중 회전 또는 입력 잠금에는 `wait`와 대기 문구가 일치한다.
- 양면이 회수 가능할 때만 `retrieve`와 `회수`가 일치한다.
- DOM 카드와 debug snapshot에도 exact `nextAction`을 노출해 회귀를 판정한다.

## 검증

- 전체 Vitest: 43파일·316/316 통과
- D1 행동·키보드 제공·영업 회귀 Chromium FHD/720: 18/18 통과
- `d1-face-serving-ux.spec.js`에서 조기 `회수` 기대를 `뒤집기`로 교체하고,
  양면 완료 뒤 `회수`를 별도로 검증

Developer 3 작업 011의 Developer 2 UI 소비 대기는 이 결과로 해소할 수 있다.
