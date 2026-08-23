# 개발자 1 - 014 app/src 기능 동등 유지보수 리팩토링

- 상태: `완료`
- 담당자: `개발자 1`
- 우선순위: `P1`
- 현재 milestone에 필요한 이유: 현재 기능을 확장하기 전에 `app/src`의 중복과 책임 혼재를 줄여 이후 변경의 회귀 위험과 탐색 비용을 낮춘다.
- 하지 않으면 막히는 stage gate: 신규 메뉴·영업일 확장 시 기존 D1~D3 기능을 안전하게 유지하는 회귀 검증과 코드 변경 독립성

## 참조 spec

- `spec/system/SYS-004_app_프로젝트와_소스코드_아키텍처.md` - `SYS-004` - git `604ad29`
- `spec/qa/QA-002_전체_게임_기능_성능_출시_검증.md` - `QA-002` - git `832c9b6`

## 목표

`app/src`에서 동작 변경 위험이 낮고 자동 검증으로 보호되는 모듈부터 중복·불필요 구문·책임 혼재를 정리한다. 공개 API, 상태 전이, 저장 데이터, DOM 계약, 렌더 결과와 사용자 조작은 리팩토링 전과 동일하게 유지한다.

## 범위

### 포함

- 자동 테스트가 직접 보호하는 `src/application`, `src/domain`, `src/infrastructure`, `src/render` 모듈의 작은 책임 분리와 중복 제거
- 의미 없는 중간 변수·명시적 `undefined` 반환·중복 조건과 같은 불필요 구문 제거
- 동작 동등성을 고정하는 기존 테스트 재사용과 필요한 경계 회귀 테스트 보강
- 전체 Vitest와 프로덕션 빌드 검증

### 제외

- 게임 규칙, 밸런스, 화면 배치, 입력 방식, 타이밍과 에셋 변경
- 공개 export 이름·호출 규약, 저장 스키마·키, DOM selector와 CSS class 변경
- 대형 진입점의 일괄 재작성, 프레임워크·외부 의존성 추가, TypeScript 전환
- 사용자 작업인 `app/.gitignore` 수정

## 작업 절차

1. 현재 `app` 기준선의 전체 Vitest와 프로덕션 빌드를 실행하고 결과를 기록한다.
2. 모듈 크기, 호출 관계, 중복과 불필요 구문을 확인해 테스트로 동작을 대조할 수 있는 범위를 선정한다.
3. 공개 계약을 보존하면서 작은 helper·상수·책임 단위로 추출하고 불필요 구문을 제거한다.
4. 타깃 테스트, 전체 Vitest, 프로덕션 빌드를 다시 실행해 기준선과 비교한다.
5. 구현 위치, 검증 결과와 남은 위험을 기록하고 완료 상태로 전환한다.

## 병렬·순차 계획

- 다른 역할과 병렬 가능한 범위: 아트 제작과 문서 기획은 `app/src`를 수정하지 않는 한 병렬 가능하다.
- 반드시 순차로 진행할 범위: 기준선 검증 → 리팩토링 → 타깃·전체 회귀 → 결과 기록 순으로 진행한다.
- 동시 writer를 금지할 경로·stable ID·finalizer: 이번 작업에서 선정한 `app/src` 모듈과 직접 대응하는 테스트 파일은 단일 writer가 수정한다.

## 사용자 승인 gate

- 승인 대상: 없음. 사용자가 기능 동등 리팩토링을 직접 지시했다.
- 승인 전 금지: 게임 규칙·화면·콘텐츠·에셋·공개 계약 변경
- 승인 뒤 자동 인계: 자동 검증을 통과하면 작업 결과를 기록한다.

## 사람 화면 테스트

- 필수 경로: 화면·입력·렌더 구성을 변경하지 않는 범위이므로 자동 테스트와 빌드를 필수 gate로 사용한다.
- 환경: Node.js, Vitest, Vite 프로덕션 빌드
- 진단 결과 또는 명시적 필수 gate인 경우의 `PASS` 증거: 리팩토링 전후 전체 테스트 개수·결과와 빌드 성공 여부를 비교한다.

## 의존성과 인계 조건

- 선행 작업: 완료된 개발자 1 작업 008·010·013의 런타임·영업일 기준선
- 후속 작업: 신규 메뉴와 후속 영업일 확장 작업
- 다른 개발자에게 제공할 입력·출력: 동작이 보존된 정리된 모듈 경계와 회귀 테스트
- 아트 작업인 경우 `semanticOwner`·review namespace·읽기 전용 공유 source: 해당 없음

## 완료 기준

- [x] 리팩토링 전 전체 Vitest와 프로덕션 빌드 기준선이 기록되어 있다.
- [x] 선정 모듈의 중복·불필요 구문·책임 혼재가 줄고 공개 계약과 동작은 유지된다.
- [x] 게임 규칙·저장 형식·DOM·렌더 결과·에셋과 사용자 작업 파일에 의도하지 않은 변경이 없다.
- [x] 타깃 테스트와 전체 Vitest가 통과하고 테스트 수가 의도 없이 감소하지 않는다.
- [x] Vite 프로덕션 빌드가 통과한다.

## 구현 및 검증 결과

- app 구현 위치: `src/application/businessDay/d1BusinessDayUiPort.js`,
  `d1BusinessDayUiContract.js`, `d1BusinessDayViewModel.js`,
  `d1BusinessDayBrowserSession.js`, `src/render/customerArtFrameSelector.js`,
  `d1OfficeCustomerArt.js`, `d1SoloCustomerArt.js`
- 테스트 위치: `tests/integration/d1BusinessDayBrowserSession.test.js`,
  `tests/unit/d1SoloCustomerArt.test.js`
- 구현 기준 spec·태스크: docs 기준 `15afc0e`, `SYS-004@604ad29`,
  `QA-002@832c9b6`, 본 태스크; app 기준 `387c995`
- 검증 방법: 리팩토링 전·후 `npm test`, `npm run build`; 타깃 Vitest;
  `npm run verify`; 전체 Playwright와 변경 전 app `387c995` 임시 기준선의 동일 실패 재현
- 검증 결과:
  - 기준선 Vitest 69개 파일·490개 테스트와 Vite 빌드 통과
  - 변경 후 타깃 4개 파일·13개 테스트, 전체 Vitest 70개 파일·494개 테스트 통과
  - Vite 프로덕션 빌드 통과
  - D1 release drift 일치, D1 core 82개 테스트와 Chromium FHD/720 14개 E2E 통과
  - 전체 Playwright는 261개 통과·20개 의도적 skip이며, 장시간 진단의 실패 4건은
    격리 재실행 또는 변경 전 기준선 비교로 신규 회귀가 아님을 확인했다.
- 남은 위험: 전체 진단 선행 에셋 검증은 현행 manifest·payload 불일치 56건으로 중단된다.
  D3 토치 1920×1080 테스트의 coverage `0.8` 실패는 변경 전 `387c995`에서도 동일하며,
  실제 경과 시간 기반 뒤집기 테스트는 Playwright 설정에 이미 기록된 SwiftShader 타이밍
  민감성이 남아 있다. 대형 `src/d1-game.js`와 저장 트랜잭션은 기능 변경 위험을 피하려고
  이번 범위에서 제외했다.
- 완료일: `2026-08-24`
