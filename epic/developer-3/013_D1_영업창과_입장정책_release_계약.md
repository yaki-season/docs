# 개발자 3 - 013 D1 영업창과 입장 정책 release 계약

- 상태: `완료`
- 담당자: `개발자 3`
- 우선순위: `P0`
- 현재 milestone에 필요한 이유: 영업 시간·13초 입장·조기 종료가 문서와 런타임 사이에서 다시 어긋나지 않게 정적 계약으로 고정한다.
- 하지 않으면 막히는 stage gate: D1 release artifact와 실제 영업 동작의 동일성

## 참조 spec

- `spec/data/DAT-001_콘텐츠와_밸런스_데이터_계약.md`
- `spec/gameplay/GPL-002_30일_캠페인과_영업일_루프.md`
- `spec/qa/QA-002_전체_게임_기능_성능_출시_검증.md`

## 목표

D1 정본 데이터와 release definition에 17:30~02:30 영업창, 빈 매장 다음 도착 13초 상한, 마지막 손님 완료 후 자동 종료 정책을 명시하고 drift 검증으로 보호한다.

## 범위

### 포함

- `day-d1.json` 영업창·입장 정책 정본화
- D1 release definition schema·생성기·application port 정합
- 정적 release artifact 재생성과 drift 검사
- 정상·오류 fixture와 계약 단위 테스트

### 제외

- D2~D3 영업 시간과 입장 간격 재설계
- 실제 손님 상태 머신 구현

## 완료 기준

- [x] D1 데이터에 17:30~02:30·자정 넘김이 명시된다.
- [x] 입장 정책에 `maxEmptyWaitSeconds=13`, 마지막 손님 자동 종료가 명시된다.
- [x] release schema·port가 필수 필드를 검증한다.
- [x] 생성된 정적 artifact와 정본 사이 drift가 없다.

## 구현 및 검증 결과

- app 구현 위치: `content/campaign/day-d1.json`, `content/schema/day.schema.json`, `src/content/d1ReleaseDefinition.js`, `content/schema/d1-release-definition.schema.json`, `src/application/ports/d1BusinessDayDefinition.js`, `content/releases/d1-business-day-definition.v1.json`
- 검증 방법: content schema·release definition 단위 테스트와 `d1:release:check`
- 검증 결과: 새 필수 계약과 정적 artifact를 정합했고 release drift 및 관련 단위·통합 검증을 통과했다.
- 남은 위험: D2~D3는 기존 정책을 유지하며 별도 결정 전 D1 값을 자동 전파하지 않는다.
- 완료일: `2026-08-10`
