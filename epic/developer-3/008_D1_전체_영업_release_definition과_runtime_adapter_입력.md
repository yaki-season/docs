# 개발자 3 - 008 D1 전체 영업 release definition과 runtime adapter 입력

- 문서 버전: `v1.0.2`
- 최종 변경일: `2026-07-30`
- 상태: `완료`
- 담당자: `개발자 3`

## 참조 spec

- `spec/data/DAT-001_콘텐츠와_밸런스_데이터_계약.md` - `DAT-001` - `v5.23.0`
- `spec/gameplay/GPL-004_조리_스테이션과_품질_서빙.md` - `GPL-004` - `v1.38.0`

## 목표

검증된 D1 전체 영업 fixture와 D1 정본 콘텐츠의 누락 정책을 versioned release definition으로
고정해, 개발자 1이 상수나 개발 fixture 추측 없이 runtime adapter 입력을 만들 수 있게 한다.

## 범위

### 포함

- D1 정본 order·customer source를 runtime wave로 결정적으로 투영
- 저장 가능한 엑스트라 runtime ID·group ID·선행 주문 완료 조건
- release definition schema, 정상/오류 fixture와 정본 교차 검증
- 개발자 1 import 경로와 완전한 필드 표 handoff

### 제외

- D5 및 D2~D3 데이터
- `game.js`, domain runtime, UI, 아트, manifest 수정
- 새 밸런스 수치 또는 `basePrice=3`의 10% 반올림 규칙 변경

## 완료 기준

- [x] release definition이 검증된 D1 fixture의 세션·좌석·시간·wave·가격·상한을 정본 콘텐츠와 함께 제공한다.
- [x] 주문·수량·가격·시간이 정본 또는 fixture와 어긋나는 오류 fixture를 거부한다.
- [x] 개발자 1이 builder의 결과를 바로 runtime adapter 입력으로 쓸 수 있다.
- [x] 전용 schema/fixture 테스트와 전체 Vitest가 통과한다.

## 구현 및 검증 결과

- app 구현 위치: `src/content/d1ReleaseDefinition.js`, `src/content/validateD1ReleaseDefinition.js`, `content/schema/d1-release-definition.schema.json`, `tests/fixtures/d1-release-definition/`, `tests/unit/d1ReleaseDefinition.test.js`
- 구현 기준 spec 버전: `DAT-001 v5.23.0`, `GPL-004 v1.38.0`
- 구현 기준 태스크 버전: `v1.0.2`
- 검증 방법: JSON schema·정본 교차 검증·전용 fixture·전체 Vitest
- 검증 결과: 전용 4 테스트 통과, 전체 Vitest 40 파일·293 테스트 통과
- 남은 위험: 없음

## 변경 이력

| 이전 버전 | 새 버전 | 날짜 | 변경 유형 | 근거 spec 버전 | 변경 내용 | 재작업 영향 |
|---|---|---|---|---|---|---|
| 없음 | `v1.0.0` | `2026-07-30` | 최초 생성·착수 | `DAT-001 v5.23.0`, `GPL-004 v1.38.0` | D1 전체 영업 release definition과 runtime adapter 입력 작업 생성 | 개발자 1의 definition adapter 소비 handoff 필요 |
| `v1.0.0` | `v1.0.1` | `2026-07-30` | 구현 완료 | `DAT-001 v5.23.0`, `GPL-004 v1.38.0` | versioned release definition builder·Ajv schema·정상/오류 fixture·개발자 1 field handoff를 추가 | 개발자 1이 runtime adapter 입력으로 직접 소비 |
| `v1.0.1` | `v1.0.2` | `2026-07-30` | totals 승격 | `DAT-001 v5.23.0`, `GPL-004 v1.38.0` | release definition에 작업 007 contract의 `customers=4`, `orders=4`, `items=9` totals를 명시적으로 추가 | runtime adapter가 합계를 별도 추론할 필요 없음 |
