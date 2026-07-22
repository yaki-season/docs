# SYS-004 app 프로젝트와 소스코드 아키텍처

- 문서 버전: `v2.0.0`
- 최종 변경일: `2026-07-22`
- 상태: `초안`
- 담당 영역: `시스템`
- 근거 inbox: `inbox/0.idea/YAKI SEASON 게임 기획서.md`, `inbox/1.design/rendering-interaction.md`, `inbox/1.design/performance.md`, `inbox/1.design/2026-07-20_야키토리_및_생맥주_직접조작_UX_시나리오_초안.md`, `inbox/2.art-concept/03_main_service_gameplay_ui.png`, `inbox/2.art-concept/09_customer_waiting_and_cleanup.png`, `inbox/2.art-concept/14_responsive_ui_station_warning.png`, `inbox/99.change-request/2026-07-19_기획서_모순_검토_원본.md`
- 원본 SHA-256: 아래 근거 원본 표 참조

## 근거 원본

| 경로 | SHA-256 |
|---|---|
| `inbox/0.idea/YAKI SEASON 게임 기획서.md` | `280df6c85d16b8b17f38a65506d2bac7dfa79cb516d1412539c5b350a1500a3b` |
| `inbox/1.design/rendering-interaction.md` | `a99f0f795ab3bce19b9509a1e2e38ef57f90090ffe34b8dba945e69dd0369ec8` |
| `inbox/1.design/performance.md` | `8166c3afd6bd3d50ceeafd5683c2ade29720cbfcb878c6c28fb4f79f40ddb238` |
| `inbox/1.design/2026-07-20_야키토리_및_생맥주_직접조작_UX_시나리오_초안.md` | `9806c8abb3f0b3f067c2d25c2d0633e3bd7261bb21f8afd57f922dafd9a87f59` |
| `inbox/2.art-concept/03_main_service_gameplay_ui.png` | `ee19d6c1d5edebe17c78c5ac1322471c24dfd1be8d43c081cc08b039f6cb17a9` |
| `inbox/2.art-concept/09_customer_waiting_and_cleanup.png` | `f952c367580a656f838acce75150b6c3871755710d612aebbd23abe6e406f594` |
| `inbox/2.art-concept/14_responsive_ui_station_warning.png` | `ecfe341e2904157209b9522187fed930dd99b549c35cc358863680a71dfe7547` |
| `inbox/99.change-request/2026-07-19_기획서_모순_검토_원본.md` | `c15578ae95efe2a0e321489a34470a84a38f378e3c8ee069aef3bfb451cd26fb` |

## 현행 app 기준선

이 설계는 `app` 저장소 커밋 `970547b36ffb581b89a0c485e0136abb912fa3bf`를 현행 기준으로 삼는다.

| 대상 | 현행 역할 | SHA-256 | 설계상 처리 |
|---|---|---|---|
| `sources/main.js` | 단일 WebGL2 익힘 셰이더 스파이크 | `797c6dc894c4e70779d25459c9287c17c29cb8b17f70245fb5914cb1acdecf06` | 프로덕션 진입점으로 확장하지 않고 셰이더 이식 참고로 보존 |
| `sources/index.html` | 셰이더 수치 확인용 화면 | `12efe06f992c9ef1cdfd5fbcd667f31fb44d7e1106432fa40c04d1c7cfa601f7` | 개발 전용 시각 검증 화면으로 대체 |
| `sources/shaders/skewer.frag.glsl` | 단일 텍스처 익힘·타레 표현 | `1b5e1154b341a7690d4803077c34b7443ba1d0cb9c70dbc8d5a3567e9ef587bf` | Three.js 오브젝트용 재질로 이식하고 앞·뒷면 값을 분리 |
| `sources/shaders/skewer.vert.glsl` | 풀스크린 쿼드 정점 셰이더 | `568ec44f00be11a16c1c74ad897bc7b33e34dd8c45108d6fee637a72d28eb664` | 프로덕션 메시 정점 셰이더로 교체 |
| `art/gameplay/ASSET-MANIFEST.md` | 첫 네기마 장면 이미지 16종 | `37fbf074a9604e8d65debca27ddded730b4fc4bac9f6fdfff40fd7701f6563ca` | `ART-003`의 기계 판독 manifest로 이전 |

현재 `sources/`는 빌드 시스템, 패키지 잠금, TypeScript, Three.js 장면, 자동 테스트와 전체 게임 상태가 없는 기술 실험이다. 파일을 그대로 누적 확장하면 게임 규칙·DOM·WebGL 상태가 한 모듈에 결합되므로 정식 앱은 별도 `src/` 진입점에서 시작한다.

## 목적

30일 캠페인을 여러 개발자가 병렬 구현할 수 있도록 `app` 저장소의 기술 스택, 디렉터리, 모듈 책임, 상태·명령·이벤트, 콘텐츠·저장, 2.5D 렌더링, UI와 테스트 경계를 고정한다. 게임 규칙은 Three.js·DOM·브라우저 저장소와 분리하고, 현재 셰이더·에셋 실험은 검증된 부분만 정식 구조에 편입한다.

## 범위

### 포함

- 정적 웹 게임의 프로젝트·빌드·소스 디렉터리 설계
- 도메인, 애플리케이션, 플랫폼, Three.js, DOM UI의 의존 방향
- 게임 상태, 명령, 도메인 이벤트, 화면 투영과 단발 연출 계약
- 고정 시간 진행, 일시정지, 난수와 중복 입력 처리
- 콘텐츠 JSON, 스키마 검증, 에셋 manifest와 현지화 로딩
- 로컬 저장 키, 체크포인트, 백업과 마이그레이션
- 테스트 계층, 개발 진단 기능과 정적 배포 산출물
- 기존 WebGL2 스파이크의 이식 경계

### 제외

- 실제 애플리케이션 소스코드와 빌드 설정 작성
- 확정되지 않은 가격·확률·조리 시간·손님 수치 결정
- 계정, 서버, 원격 분석, 결제, PWA와 앱스토어 패키징
- 개발자 배정, epic, task와 일정 산정

## 상세 요구사항

상세 규칙은 기술 선택, 프로젝트 구조, 모듈 경계, 상태·이벤트, 시간, 콘텐츠·저장, UI·입력, 렌더링, 에셋과 테스트 영역을 함께 적용한다.

## 기술 선택

| 영역 | 선택 | 적용 이유와 경계 |
|---|---|---|
| 언어 | TypeScript `strict` | 상태·콘텐츠·이벤트 계약을 컴파일 단계에서 고정한다. `any`와 암시적 타입 완화를 허용하지 않는다. |
| 빌드 | Vite | 정적 웹 산출물, 개발 서버, GLSL·에셋 import와 코드 분할을 담당한다. 버전은 구현 시작 시 lockfile로 고정한다. |
| 2.5D | Three.js/WebGL2 | 단일 렌더러, 고정 카메라와 직접 조작 3D 대상에만 사용한다. |
| 화면 UI | 시맨틱 DOM + CSS 모듈 | 텍스트, 접근성, 반응형과 입력 영역을 캔버스 밖에서 관리한다. 1차 구현에는 별도 UI 프레임워크를 두지 않는다. |
| 콘텐츠 | UTF-8 JSON + JSON Schema | 기획 diff와 자동 검증을 동시에 유지한다. `candidate`와 `approved`를 로드 단계에서 분리한다. |
| 스키마 검증 | Ajv 계열 JSON Schema 검증기 | 개발·빌드에서는 전체 콘텐츠를 검증하고 운영 부팅에서는 manifest·저장 데이터만 검증한다. |
| 단위·통합 테스트 | Vitest 계열 러너 | 브라우저 없이 도메인·저장·콘텐츠를 테스트하고 가짜 시간·난수를 주입한다. |
| 브라우저 테스트 | Playwright | PC·모바일 가로 화면, 포인터, 저장 복구와 WebGL 시각 회귀를 검증한다. |
| 오디오 | Web Audio API 어댑터 | 게임 규칙에서 분리하고 사용자 제스처 뒤 지연 초기화한다. |

새 라이브러리를 추가할 때는 `도메인 규칙 대체`, `렌더 성능 예산`, `초기 gzip 5MB`에 미치는 영향을 기록한다. 전역 상태 프레임워크, 물리 엔진과 UI 컴포넌트 프레임워크는 실제 반복 복잡성이 증명되기 전 기본 의존성에 넣지 않는다.

## 목표 프로젝트 구조

```text
app/
├── index.html
├── package.json
├── package-lock.json
├── tsconfig.json
├── vite.config.ts
├── content/
│   ├── manifest.json
│   ├── schemas/
│   ├── campaign/
│   ├── recipes/
│   ├── processes/
│   ├── customers/
│   ├── progression/
│   ├── staff/
│   ├── story/
│   └── locale/ko.json
├── public/assets/
│   ├── manifest.json
│   ├── core/
│   ├── campaign/
│   └── audio/
├── art/
│   ├── source/
│   ├── review/
│   └── provenance/
├── src/
│   ├── main.ts
│   ├── app/
│   ├── core/
│   ├── domain/
│   ├── application/
│   ├── infrastructure/
│   ├── presentation/
│   └── styles/
├── tests/
│   ├── fixtures/
│   ├── integration/
│   ├── e2e/
│   ├── visual/
│   └── performance/
├── tools/
│   ├── content/
│   └── assets/
└── legacy/
    └── shader-spike/
```

### 루트 디렉터리 계약

1. `src/`는 프로덕션 TypeScript와 GLSL만 포함한다.
2. `content/`는 사람이 검토하는 게임 콘텐츠 원본과 JSON Schema를 포함하며 렌더링 코드를 포함하지 않는다.
3. `public/assets/`는 빌드에 직접 포함되는 최적화 완료 런타임 에셋만 포함한다.
4. `art/source/`는 편집 가능한 원본, `art/review/`는 승인 전 결과, `art/provenance/`는 프롬프트·라이선스·해시 기록을 보관한다.
5. `tests/fixtures/`는 D1, 러시, D30, 저장 손상과 같은 재현 가능한 상태를 보관한다.
6. `tools/`는 콘텐츠·에셋 검증과 변환에 한정하고 런타임 규칙을 중복 구현하지 않는다.
7. 기존 `sources/`는 정식 구조 편입 시 `legacy/shader-spike/`로 이동해 회귀 참고로 보존하되 프로덕션 번들에서 제외한다.

## 소스 모듈 구조

```text
src/
├── main.ts
├── app/
│   ├── bootstrap.ts
│   ├── game-app.ts
│   ├── lifecycle.ts
│   └── error-boundary.ts
├── core/
│   ├── clock.ts
│   ├── random.ts
│   ├── ids.ts
│   ├── result.ts
│   ├── event-stream.ts
│   └── invariant.ts
├── domain/
│   ├── model.ts
│   ├── campaign/
│   ├── service/
│   ├── cooking/
│   ├── economy/
│   ├── progression/
│   └── story/
├── application/
│   ├── commands/
│   ├── game-store.ts
│   ├── simulation.ts
│   ├── selectors/
│   ├── use-cases/
│   └── ports/
├── infrastructure/
│   ├── content/
│   ├── persistence/
│   ├── audio/
│   ├── platform/
│   └── diagnostics/
├── presentation/
│   ├── input/
│   ├── ui/
│   └── three/
│       ├── renderer.ts
│       ├── scene-root.ts
│       ├── camera/
│       ├── bar-counter/
│       ├── stations/
│       ├── objects/
│       ├── materials/
│       ├── vfx/
│       └── assets/
└── styles/
    ├── tokens.css
    ├── layout.css
    └── accessibility.css
```

## 의존 방향과 책임

```text
core <- domain <- application <- infrastructure
                         ^       <- presentation
                         └----------- app/bootstrap
```

8. 모든 import는 위 화살표의 안쪽을 향한다. `domain`은 `application`, Three.js, DOM, 저장소와 오디오를 import하지 않는다.
9. `core`는 게임 규칙을 포함하지 않고 시간·난수·ID·불변식 같은 범용 계약만 제공한다.
10. `domain`은 캠페인·손님·주문·조리·경제·성장·이야기의 상태와 전이만 소유한다.
11. `application`은 명령 직렬화, 시뮬레이션 진행, 유스케이스, selector와 외부 포트를 조정한다.
12. `infrastructure`는 콘텐츠 파일, 로컬 저장, Web Audio, 브라우저 수명주기와 진단 포트의 구현체다.
13. `presentation/ui`는 selector가 만든 화면 모델만 읽고 도메인 객체를 직접 수정하지 않는다.
14. `presentation/three`는 렌더 스냅샷과 단발 연출 이벤트만 읽으며 품질·보상·대기 판정을 계산하지 않는다.
15. `app/bootstrap.ts`만 구체 구현체를 조립하며 전역 singleton은 렌더러·오디오 컨텍스트처럼 브라우저가 하나만 허용해야 하는 자원으로 제한한다.

## 도메인 경계

| 모듈 | 소유 상태·규칙 | 소유하지 않는 것 |
|---|---|---|
| `campaign` | S0, D1~D31, 영업일 단계, 목표, 해금, 다음 날 | 조리 판정과 화면 전환 애니메이션 |
| `service` | 좌석, 그룹, 손님, 주문, 대기, 서빙, 정리, 러시 | 메뉴 가격과 Three.js 좌표 |
| `cooking` | 제작물, 공정, 스테이션, 면별 익힘, 맥주층, 품질 | 손님 만족과 에셋 표현 |
| `economy` | 판매 원장, 팁, 골드, 명성, 배율 상한, 정산 | UI 숫자 포맷과 결제 |
| `progression` | 메뉴 복원·판매 준비, 업그레이드, 직원, 운영 경향 | 날짜별 대사 연출 |
| `story` | 장면, 플래그, 대사 노드, 스킵 요약, 대체 분기 | DOM 레이아웃과 초상 애니메이션 |

교차 모듈 변경은 상대 모듈 상태를 직접 쓰지 않고 명령 결과로 발생한 도메인 이벤트를 다음 모듈의 명시적 핸들러에 전달한다. 예를 들어 `cooking`의 `ProductCompleted`가 `service`의 주문 완성 대기로 연결되고, `OrderServed`가 `economy`의 판매 원장 항목을 생성한다.

## 상태 모델과 소유권

### 루트 상태

| 영역 | 최소 필드 | 저장 여부 |
|---|---|---|
| `meta` | 저장 스키마, 콘텐츠 버전, 캠페인 ID, 시드 | 체크포인트 |
| `campaign` | 날짜, 스테이지, 영업일 단계, 목표, 해금 | 체크포인트 |
| `service` | 좌석·손님·주문·파동·위험 | 영업 중 메모리, 진단 스냅샷만 |
| `cooking` | 제작물·공정·스테이션·타이머·품질 | 영업 중 메모리, 진단 스냅샷만 |
| `economy` | 재화, 명성, 오늘 원장, 완료 식별자 | 체크포인트 |
| `progression` | 레시피, 메뉴 준비, 업그레이드, 직원, 경향 | 체크포인트 |
| `story` | 장면 플래그, 튜토리얼, 관계와 노트 기록 | 체크포인트 |
| `runtime` | 일시정지 원인, 활성 포인터, 화면, 오류 | 저장하지 않음 |

16. 루트 `GameState`의 쓰기 권한은 `GameStore` 하나만 가진다.
17. 화면과 렌더러에 전달하는 배열·객체는 읽기 전용 스냅샷이며 역참조로 상태를 변경할 수 없어야 한다.
18. 설정은 `SettingsState`로 분리해 음량·진동·왼손 모드·도움 수준·품질 모드를 변경 즉시 저장한다.
19. 콘텐츠 정의 객체는 부팅 뒤 동결하고 영업 중 변경하지 않는다.
20. 상태에는 에셋 URL, Three.js 객체, DOM 노드, 함수와 브라우저 이벤트를 넣지 않는다.

## 명령과 이벤트 계약

### 명령

명령은 사용자·직원 자동화·시스템이 의도한 행동이며 `commandId`, `issuedAt`, `source`, 대상 ID와 입력 값을 가진다.

| 분류 | 대표 명령 |
|---|---|
| 캠페인 | `StartCampaign`, `StartDay`, `ConfirmSettlement`, `AdvanceDay` |
| 주문 | `SeatGroup`, `AcceptOrder`, `ServeProduct`, `CleanSeat` |
| 조립 | `SelectIngredient`, `RemoveLastIngredient`, `MoveSkewerToTray` |
| 그릴 | `PlaceOnGrill`, `FlipSkewer`, `FanCharcoal`, `RemoveFromGrill` |
| 드링크 | `PlaceGlass`, `SetBeerFlow`, `SetFoamFlow`, `StopDrinkFlow` |
| 성장 | `RestoreRecipe`, `PrepareMenu`, `BuyUpgrade`, `AssignStaff` |
| UI | `PauseGame`, `ResumeGame`, `ChangeStation`, `SkipStory` |

### 도메인 이벤트

이벤트는 이미 확정된 사실이며 `eventId`, `sequence`, `occurredAt`, `causationId`와 관련 엔티티 ID를 가진다.

| 분류 | 대표 이벤트 |
|---|---|
| 캠페인 | `DayStarted`, `BusinessOpened`, `DayClosed`, `DayCompleted` |
| 주문 | `OrderCreated`, `OrderRiskChanged`, `CustomerLeft`, `SeatBecameDirty` |
| 조리 | `ProcessStarted`, `SkewerFlipped`, `ProductCompleted`, `ProductFailed` |
| 서빙 | `ProductAssigned`, `OrderServed`, `CustomerReactionSelected` |
| 경제 | `SaleRecorded`, `TipRecorded`, `RewardGranted`, `LedgerClosed` |
| 이야기 | `StoryBeatStarted`, `StoryFlagSet`, `TutorialStepCompleted` |

21. 명령 처리는 현재 상태에서 허용 여부를 먼저 검증하고 거부 시 상태 변경 없이 구조화된 사유를 반환한다.
22. 같은 `commandId`를 다시 처리하면 첫 결과를 재사용하거나 무시해 구매·서빙·보상을 중복 실행하지 않는다.
23. 이벤트 순서는 영업일 안에서 단조 증가하며 오디오·VFX는 마지막 소비 sequence를 보관해 한 번만 재생한다.
24. 렌더 위치가 필요한 이벤트는 Three.js 좌표 대신 `visualAnchorId`를 전달하고 렌더러가 장면 좌표로 해석한다.
25. 도메인 이벤트는 과거형, 명령은 동작형 이름을 사용하고 UI 전용 이름을 도메인에 넣지 않는다.

## 시간·난수·시뮬레이션

26. 도메인 시뮬레이션은 `50ms` 고정 간격을 기본으로 하며 렌더링은 별도 `requestAnimationFrame`에서 가장 최근 상태를 보간한다.
27. 한 렌더 프레임에서 최대 5개 시뮬레이션 step만 처리한다. 더 큰 지연은 시간을 건너뛰지 않고 게임 진행을 일시 중지한 뒤 복귀 요약으로 전환한다.
28. 포인터 피드백은 시뮬레이션 step을 기다리지 않고 presentation에서 즉시 시작하되 논리 결과는 명령 처리 뒤에만 표시한다.
29. 시간 원본은 주입 가능한 단조 증가 clock이며 날짜 시각과 게임 경과 시간을 분리한다.
30. 브라우저 `visibilitychange`, `pagehide`, 포커스 손실과 명시적 일시정지는 같은 pause coordinator로 수렴한다.
31. 난수는 캠페인 시드와 하위 stream 이름으로 분리해 손님 생성, 추가 주문과 시나리오 후보가 서로의 호출 횟수에 영향을 덜 받게 한다.
32. 저장·로드와 테스트 뒤에도 동일 시드·명령·시간열은 동일 도메인 상태와 경제 원장을 만든다.

## 애플리케이션 유스케이스

| 유스케이스 | 입력 | 성공 결과 | 실패 처리 |
|---|---|---|---|
| 새 캠페인 | 콘텐츠 버전, 시드 | S0 상태와 첫 체크포인트 | 필수 콘텐츠·에셋 미검증 시 시작 차단 |
| 영업 시작 | 날짜, 메뉴, 직원 배치 | `day-start` 저장 후 영업 상태 | 저장 실패 시 영업 시작 보류 |
| 입력 처리 | 정규화 포인터 또는 UI 명령 | 상태 전이와 이벤트 묶음 | 허용되지 않은 상태면 이유 반환 |
| 시간 진행 | 고정 step | 타이머, 파동, 조리·대기 전이 | 이벤트 상한 초과 시 진단 중단 |
| 정산 확정 | 오늘 원장 | 보상 1회 적용과 `day-complete` 저장 | 중복 완료 ID면 기존 결과 반환 |
| 불러오기 | 저장 envelope | 마이그레이션된 캠페인 상태 | 백업 제안 또는 새 게임 안내 |

## 콘텐츠와 데이터 로딩

### 콘텐츠 팩

| 파일군 | 대표 내용 |
|---|---|
| `manifest.json` | 콘텐츠 버전, 스키마 버전, 팩 목록, 파일 해시 |
| `campaign/days.json` | S0, D1~D31, 목표, 파동, 해금과 대체안 |
| `recipes/*.json` | 메뉴, 재료, 공정, 경제와 에셋 키 |
| `processes/*.json` | 조립·그릴·드링크·후속 공정 규칙 |
| `customers/*.json` | 유형, 인물, 취향, 반응과 단골 규칙 |
| `progression/*.json` | 업그레이드, 메뉴 준비, 좌석과 운영 경향 |
| `staff/*.json` | 역할, 경험, 품질 상한과 자동화 행동 |
| `story/*.json` | 장면 노드, 조건, 대사 키, 스킵 요약 |
| `locale/ko.json` | 화면·대사 표시 문자열 |

33. 콘텐츠 로더는 `parse -> schema validate -> cross-reference validate -> normalize -> freeze` 순서로 실행한다.
34. 개발·테스트에서는 `candidate` 콘텐츠를 명시적 플래그로 허용할 수 있지만 운영 빌드는 필수 `approved` 값이 없으면 실패한다.
35. 표시 문자열, 오디오와 에셋은 안정된 ID로 참조하고 상대 파일 경로를 도메인 데이터에 직접 쓰지 않는다.
36. D1~D30 연결, 필수 메뉴, 대체 주문, 에셋·오디오 키, 경제 상한은 빌드 검증에서 확인한다.
37. 콘텐츠 오류는 파일, JSON 경로, 레코드 ID, 기대 형식과 실제 값을 함께 보고한다.
38. 운영 빌드에는 개발용 후보 수치, 에셋 원본과 검토 이미지를 포함하지 않는다.
39. 빌드는 검증을 통과한 콘텐츠만 해시가 포함된 파일로 `dist/content`에 출력하고, 런타임은 출력 manifest에서 오늘 필요한 파일을 로드한다.

## 저장 설계

1차 브라우저 저장 어댑터는 `localStorage`를 사용한다. 저장 payload는 ID·수치·플래그만 포함하고 에셋, 콘텐츠 전문과 영업 중 프레임 상태를 넣지 않는다. 이후 IndexedDB가 필요해져도 application의 persistence port는 바꾸지 않는다.

### 저장 키

| 키 | 내용 |
|---|---|
| `yaki-season.save.active` | 마지막 검증 완료 캠페인 체크포인트 |
| `yaki-season.save.pending` | 교체 전 임시 저장 |
| `yaki-season.save.backup.1` | 직전 정상 체크포인트 |
| `yaki-season.save.backup.2` | 두 번째 정상 체크포인트 |
| `yaki-season.settings` | 오디오·입력·접근성·품질 설정 |
| `yaki-season.diagnostics.last-error` | 개인정보 없는 마지막 복구 진단 |

### 저장 envelope

40. 저장 최상위에는 `saveSchemaVersion`, `contentVersion`, `writtenAt`, `checksum`, `checkpointType`, `campaignId`, `completedDayId`와 payload를 둔다.
41. 저장은 pending 쓰기, 재파싱·체크섬 검증, 기존 active 백업 회전, active 교체, pending 삭제 순서로 수행한다.
42. 로드 시 active를 검증하고 실패하면 backup 1, backup 2 순으로 복구 후보를 제시한다. 실패한 원본은 덮어쓰지 않는다.
43. `day-start`에는 영업 전 캠페인 상태, `day-complete`에는 정산·성장 확정 뒤 다음 날 진입 상태를 저장한다.
44. 영업 중 새로고침은 `day-start`에서 시작하며 영업 중 도메인 상태는 자동 저장하지 않는다.
45. 마이그레이션은 `vN -> vN+1` 순수 함수 체인으로 구성하고 각 단계의 fixture와 역호환 테스트를 보유한다.
46. 지원할 수 없는 미래 저장 버전은 새 게임 데이터로 덮어쓰지 않고 별도 오류 화면으로 보낸다.

## UI와 화면 상태

47. 화면 흐름은 `boot`, `loading`, `prologue`, `pre-open`, `business`, `closing`, `settlement`, `growth`, `notebook`, `settings`, `recovery`, `fatal-error`의 명시적 상태로 관리한다.
48. DOM view는 `UiViewModel`만 받고 버튼·포인터 행동을 명령으로 변환한다. HTML에 게임 규칙 조건식을 중복 작성하지 않는다.
49. 영업 HUD는 주문·위험·스테이션 selector를 각각 구독하되 한 render cycle의 같은 루트 상태 revision을 사용한다.
50. 화면 전환 애니메이션 종료가 날짜 진행·구매·보상을 확정하지 않는다. 논리 확정 뒤 애니메이션을 재생한다.
51. 모달은 게임 pause 필요 여부를 명시하고 설정·도움말·복구 화면 외 임의 모달 중첩을 허용하지 않는다.
52. 모바일 가로, 안전 영역, 왼손 모드와 축소 동작은 CSS 변수와 레이아웃 토큰으로 관리한다.
53. 캔버스에 의미 텍스트를 굽지 않고 주문 번호, 경고 설명과 품질 원인은 DOM에서 제공한다.

## 입력 파이프라인

```text
PointerEvent
-> 활성 포인터·pointer capture 확인
-> UI 또는 캔버스 대상 분류
-> Three.js raycast
-> InteractionPlane 로컬 UV 정규화
-> 클릭·홀드·선택형 제스처 해석
-> GameCommand
-> GameStore
```

54. `presentation/input`만 브라우저 이벤트와 raycaster를 알고 도메인에는 정규화 좌표·대상 ID·행동 의미만 전달한다.
55. 한 번에 활성 포인터 하나만 소유하고 `pointercancel`, 화면 회전과 스테이션 전환 때 현재 행동을 명시적으로 취소한다.
56. UI 입력과 3D 입력의 우선순위는 `조작 물건 -> 선택 목적지 -> 스테이션 전환 -> 배경`으로 고정한다.
57. 클릭·탭 명령이 기본 경로이고 드래그·스와이프는 같은 도메인 명령 또는 동일 결과의 빠른 명령으로 수렴한다.
58. 홀드 입력은 시작·유지·종료를 별도 명령으로 보내며 포인터 종료 누락 시 안전하게 `Stop` 명령을 발행한다.

## Three.js 렌더링 구조

### 장면 계층

| Scene group | 기본 내용 | 상태 입력 |
|---|---|---|
| `background` | 뒷벽·외부·선반 슬라이스 | 캠페인 시각 단계, 스테이션 |
| `customers` | 좌석 손님·직원 스프라이트 | 손님·직원 render model |
| `barCounter` | 고정 상판·주문 매트·전달 anchor | 좌석 tier, 화면 crop |
| `stations` | 플레이어측 조립대·그릴·드링크·서빙 모듈 | 스테이션 해금·업그레이드 |
| `interactives` | 꼬치·석쇠·잔·도구·접시 | 제작물·공정·선택 상태 |
| `vfx` | 불씨·연기·기름·판정 | 단발 이벤트·화력 |
| `foreground` | 목재·노렌·제등 프레임 | 화면·시각 단계 |

59. `renderer.ts`는 WebGLRenderer 하나, Scene 하나와 rAF 하나만 소유한다.
60. `SceneRoot`는 `background -> customers -> barCounter -> stations -> interactives -> vfx -> foreground` 그룹 순서와 dispose를 관리하고 각 station controller는 자기 오브젝트와 interaction plane만 소유한다.
61. `RenderSnapshot`은 엔티티 ID, 정규화 수치, 시각 상태와 `visualAnchorId`만 포함하며 도메인 객체를 그대로 전달하지 않는다. `barCounter`와 좌석 tier는 화면 render model에서 분리해 스테이션 변경으로 제거되지 않게 한다.
62. 오브젝트 레지스트리는 엔티티 ID를 Three.js 객체에 매핑하고 snapshot에서 사라진 객체를 풀로 반환한다.
63. 카메라는 바 안쪽 주인공 위치에서 카운터 너머 손님을 바라보는 스테이션 프리셋만 사용하고, 마지막 요청 하나로 수렴하는 `CameraController`가 카운터 선과 손님 방향을 보존하며 0.25~0.45초 전환을 수행한다.
64. 판정 VFX, 오디오와 짧은 애니메이션은 event sequence를 소비하고 완료 콜백으로 도메인 상태를 변경하지 않는다.
65. 리사이즈는 관찰자로 수집해 한 프레임에 한 번 적용하고 모바일 DPR 1.5, 데스크톱 DPR 2.0 상한을 둔다.
66. 모든 geometry, material, texture, audio buffer는 소유 모듈이 dispose하며 장면 전환마다 재로딩하지 않는다.

### 영업 장면 모듈 불변식

- `bar-counter/`는 고정 상판, 손님 가림선, 주문 매트와 손님측·플레이어측 전달 anchor를 소유하며 station controller가 이를 생성·제거하지 않는다.
- `customers`는 카운터 뒤 좌석 anchor를 사용하고, `stations`와 `interactives`는 카운터 앞 플레이어측 작업 bounds를 사용한다.
- station controller는 활성 작업 모듈만 교체하며 배경·손님·고정 카운터 render model을 대체하지 않는다.
- `CameraController`는 셰프측 preset만 등록하고 손님측에서 주방을 보거나 완전한 손님용 스툴이 전경에 놓이는 preset을 거부한다.
- 서빙 tween은 플레이어측 작업 anchor, 고정 상판의 주문 매트, 손님측 도착 anchor를 잇는 `handoffPath`를 사용한다.

### 셰이더 이식

67. 기존 `skewer.frag.glsl`의 익힘 색, 그을음과 타레 광택 수식은 시각 회귀 기준으로 사용한다.
68. 프로덕션 `SkewerMaterial`은 `frontDoneness`, `backDoneness`, `tareAmount`, `heatFlicker`와 베이스 텍스처를 받는다.
69. 앞·뒷면 판정은 도메인이 소유하고 셰이더는 면 방향 또는 재료 서브메시별 값을 선택해 표현만 한다.
70. 기존 풀스크린 쿼드와 `main.js`의 `cooked` 토글 상태는 프로덕션 코드로 이전하지 않는다.
71. 셰이더 검증 화면은 production material과 같은 코드를 import하는 개발 route로 만들어 구현과 데모가 갈라지지 않게 한다.

## 에셋 로딩

72. 부팅은 core asset pack과 오늘 날짜에 필요한 campaign pack을 manifest 해시로 검증한 뒤 입력을 연다.
73. 배경·UI core는 선로드하고 후속 날짜·오디오는 유휴 시간에 지연 로드한다.
74. `AssetManager`는 동일 ID 요청을 하나의 Promise로 합치고 로드 취소·재시도·이전 요청 무효화를 처리한다.
75. 필수 에셋 실패는 빈 캔버스를 보이지 않고 실패 ID, 재시도와 지원 가능한 플레이스홀더를 표시한다.
76. 텍스처는 nearest sampling, 색 공간, 알파와 mipmap 설정을 manifest 종류별 기본값으로 통일한다.
77. `ART-003`에서 `blocked` 또는 `candidate`인 에셋은 운영 manifest에 포함할 수 없다.

## 오디오·진단·오류 경계

78. 오디오는 첫 사용자 제스처 이후 초기화하고 도메인 이벤트를 `audioCueId`로 변환한다.
79. 음소거 상태에서도 경고·판정은 시각·텍스트 피드백을 유지한다.
80. 개발 빌드는 현재 날짜·시드·상태 revision, step 시간, 드로우콜, 삼각형, 텍스처 메모리와 이벤트 tail을 표시할 수 있다.
81. 진단 로그는 순환 버퍼를 사용하고 대사 전문, 브라우저 개인 정보와 사용자 입력 문자열을 보관하지 않는다.
82. 부팅, 콘텐츠, 저장, WebGL, 에셋과 예기치 않은 도메인 불변식 오류를 서로 다른 복구 화면으로 분류한다.
83. 운영 빌드에서는 개발 치트, 상태 편집기, 후보 콘텐츠와 상세 stack을 비활성화한다.

## 테스트 설계

| 계층 | 검증 대상 | 대표 fixture |
|---|---|---|
| 단위 | 도메인 전이, 품질, 대기, 경제, 마이그레이션 | 면별 익힘, 2인 그룹, 중복 정산 |
| 계약 | 명령·이벤트·port·selector 형식 | render snapshot, asset/audio key |
| 콘텐츠 | 스키마·참조·캠페인 연결·상한 | D1~D31 전체 팩 |
| 통합 | store+simulation+save+content | D1 전체 루프, background pause |
| E2E | DOM+Three.js+입력+저장 | S0~D1, 재접속, LOW 모드 |
| 시각 | 레이어·크롭·상태·해상도 | 1280x720, 1920x1080, 모바일 가로 |
| 성능 | 러시 장면 예산·누수·로드 | 손님 2팀, 꼬치 4개, 숯불·연기 |

84. 단위 테스트는 실제 시간, `Math.random`, DOM과 WebGL을 사용하지 않는다.
85. 테스트 fixture는 콘텐츠 ID와 저장 스키마 버전을 명시하고 프로덕션 콘텐츠 변경 시 검증한다.
86. E2E는 포인터 한 개로 클릭 경로를 우선 검증하고 드래그·스와이프는 동일 결과 패리티를 확인한다.
87. 시각 테스트는 캔버스 픽셀 비어 있음, 주요 anchor 크롭, DOM 겹침과 에셋 실패 화면뿐 아니라 셰프측 카메라, 고정 카운터 연속성, 손님 가림선과 플레이어측 작업 bounds를 함께 확인한다.
88. 기본 품질 게이트는 타입 검사, 정적 검사, 단위·콘텐츠 테스트, 프로덕션 빌드, E2E smoke와 `git diff --check`다.

## 빌드와 배포 계약

89. 운영 산출물은 정적 파일이며 앱 기본 경로가 하위 디렉터리여도 asset URL이 깨지지 않아야 한다.
90. 초기 route에는 부팅·D1 core만 포함하고 성장·노트·후반 캠페인 화면은 코드 분할할 수 있다.
91. 소스맵 공개 여부, 캐시 정책과 배포 호스트는 배포 요구가 확정될 때 별도 설정한다.
92. lockfile은 저장소에 포함하고 CI와 로컬은 같은 패키지 설치 명령을 사용한다.
93. 빌드 결과에 `art/source`, `art/review`, 테스트 fixture, 개발 진단 route와 승인되지 않은 콘텐츠가 없는지 검사한다.

## 전환 순서

이 순서는 작업 배정이 아니라 기존 스파이크를 잃지 않고 정식 구조로 이전하기 위한 기술적 선후 관계다.

1. Vite·TypeScript의 빈 composition root와 테스트 환경을 만든다.
2. 렌더러 없이 D1 fixture가 동작하는 `core`, `domain`, `application`을 먼저 연결한다.
3. 콘텐츠·저장 port와 JSON 검증을 연결하고 D1 체크포인트를 검증한다.
4. Three.js scene, asset manager와 기존 익힘 셰이더를 정식 material로 이식한다.
5. DOM 화면·HUD·입력 adapter를 연결해 클릭만으로 S0~D1을 완료한다.
6. 드래그·스와이프, LOW 모드, 오디오와 시각 회귀를 추가한다.
7. 같은 구조에 D2~D30 콘텐츠와 후속 스테이션을 데이터 팩 단위로 확장한다.

## 예외 및 경계 조건

- 콘텐츠는 정상인데 선택 에셋 pack이 실패하면 캠페인 필수 여부에 따라 재시도 또는 해당 선택 콘텐츠 비활성화를 적용한다.
- 스테이션 전환 중 새 명령은 마지막 전환 요청만 보존하고 이미 확정된 조리 명령은 취소하지 않는다.
- 동일 entity가 도메인 상태에는 있지만 렌더 registry에 없으면 게임 규칙을 계속 진행하지 않고 해당 에셋·anchor 진단을 표시한다.
- 저장 공간 부족이면 active 저장을 보존하고 영업 시작·정산 확정을 완료한 것처럼 표시하지 않는다.
- WebGL context가 유실되면 게임 시간을 멈추고 자원 재생성을 한 번 시도한 뒤 복구 화면으로 전환한다.
- 브라우저가 고정 step을 장시간 처리하지 못하면 타이머를 실제 시간으로 건너뛰지 않고 공정한 일시정지로 처리한다.
- 미확정 밸런스 값은 코드 상수로 임의 확정하지 않고 `candidate` 콘텐츠와 개발 플래그로만 실행한다.

## 비기능 요구사항

- 도메인 모듈의 순환 import를 허용하지 않는다.
- 프로덕션 소스의 공개 함수·port·이벤트는 명시적 타입과 실패 결과를 가진다.
- 프레임 루프 안에서 반복 배열 복사, JSON 직렬화, DOM 탐색과 새 Three.js 자원 생성을 금지한다.
- D1 fixture는 렌더러 없이 수 초 안에 수백 회 시뮬레이션할 수 있어야 한다.
- 키보드가 필수 조작은 아니어도 모든 DOM 설정·메뉴는 정상 focus와 접근 가능한 이름을 가진다.
- 새 콘텐츠·에셋 추가가 기존 도메인 분기 복제를 요구하면 schema 또는 공정 조합 설계를 먼저 검토한다.
- 소스 파일은 하나의 책임을 가지며 500줄을 넘는 파일은 생성 전 책임 분리 근거를 검토한다.

## 완료 기준

- [ ] 목표 디렉터리와 의존 방향이 자동 import 규칙으로 검증된다.
- [ ] D1 전체 상태가 DOM·Three.js 없이 명령, 시간과 시드만으로 재현된다.
- [ ] 같은 입력열의 저장 전·후 도메인 결과와 경제 원장이 동일하다.
- [ ] 콘텐츠 JSON의 스키마, 참조, D1~D31 연결과 asset/audio key를 빌드 전에 검증한다.
- [ ] 단일 Three.js 렌더러가 snapshot을 그리며 도메인 규칙을 계산하지 않는다.
- [ ] `SceneRoot`, `CameraController`와 station controller가 셰프측 시점·고정 카운터·손님측·플레이어측 불변식을 계약 테스트와 시각 테스트로 보장한다.
- [ ] 기존 익힘 셰이더가 양면 상태를 받는 정식 material로 이식되고 시각 회귀를 통과한다.
- [ ] S0~D1을 클릭·탭만으로 완료하고 영업 시작·정산 저장을 복구한다.
- [ ] 러시 fixture에서 `SYS-003` 성능 예산과 `QA-002` 출시 게이트를 만족한다.
- [ ] 운영 번들에 원본·검토 에셋, 후보 콘텐츠와 개발 route가 포함되지 않는다.

## 의존성

- `GPL-002` 30일 캠페인과 영업일 루프 `v1.0.0`
- `GPL-003` 주문·손님·좌석과 러시 운영 `v1.0.0`
- `GPL-004` 조리 스테이션과 품질·서빙 `v1.0.0`
- `GPL-005` 경제·성장·직원과 보상 `v1.0.0`
- `UI-002` 전체 게임 화면·입력·접근성 `v2.0.0`
- `SCN-002` 프롤로그와 30일 캠페인 시나리오 `v1.0.0`
- `SYS-003` 전체 게임 런타임·저장·성능 `v2.0.0`
- `DAT-001` 콘텐츠와 밸런스 데이터 계약 `v1.0.0`
- `ART-002` 전체 게임 비주얼·캐릭터·에셋 `v2.0.0`
- `AUD-001` 전체 게임 오디오와 피드백 `v1.0.0`
- `QA-002` 전체 게임 기능·성능·출시 검증 `v1.1.0`

## 미해결 질문

1. 지원 브라우저 최소 버전과 기준 Android·iPhone 실기기 모델.
2. 정적 배포 호스트, base path, 소스맵 공개와 캐시 정책.
3. 영업 중 임시 복귀 저장을 후속 접근성 옵션으로 추가할지.
4. JSON Schema 검증기를 운영 번들에도 남길지 검증 완료 pack에서 제거할지.
5. D31 이후 콘텐츠 pack을 코드 배포와 분리해야 하는지.
6. 세로 화면에서 안내만 제공할지 제한적 화면 탐색을 허용할지.

## 변경 이력

| 이전 버전 | 새 버전 | 날짜 | 변경 유형 | 근거 inbox·지문 | 변경 내용 | epic 영향 |
|---|---|---|---|---|---|---|
| 없음 | `v1.0.0` | `2026-07-22` | 최초 생성 | GDD·렌더링·성능·UX·모순 검토 원본과 `app@970547b` | 정식 app 기술 스택, 디렉터리, 모듈 경계, 상태·이벤트·저장·콘텐츠·렌더링·테스트 설계 | epic·task 작성 없이 설계만 추가, 구현 전 별도 영향 분석 필요 |
| `v1.0.0` | `v2.0.0` | `2026-07-22` | 핵심 장면 아키텍처 변경 | 영업 UI `ee19d6c1...17a9`, 대기·정리 `f952c367...f594`, 반응형 `ecfe341e...7547` 재검증 | `barCounter` 전용 모듈·그룹, 셰프측 카메라 제한, 손님측·플레이어측 anchor와 서빙 경로 계약 추가 | Artist 작업 003·008과 전체 게임 소스 구현·시각 테스트 범위 변경 |
