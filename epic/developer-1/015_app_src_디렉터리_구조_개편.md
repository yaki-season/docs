# 개발자 1 - 015 app/src 디렉터리 구조 개편

- 상태: `완료`
- 담당자: `개발자 1`
- 우선순위: `P1`
- 현재 milestone에 필요한 이유: 화면 진입·렌더링·브라우저 UI 구현이 `src` 루트와 `src/render`에 혼재해 있어 기능 추가 시 수정 위치와 의존 경계를 파악하기 어렵다.
- 하지 않으면 막히는 stage gate: 신규 메뉴·영업일·화면 확장 과정에서 기존 공개 경로와 게임 동작을 보존한 채 구현 책임을 독립적으로 변경하는 유지보수성

## 참조 spec

- `spec/system/SYS-004_app_프로젝트와_소스코드_아키텍처.md` - `SYS-004` - git `604ad29`
- `spec/qa/QA-002_전체_게임_기능_성능_출시_검증.md` - `QA-002` - git `832c9b6`

## 목표

현행 JavaScript 구현과 모든 사용자 동작을 유지하면서 `app/src`의 디렉터리를 책임별로 재배치한다. 공개 HTML·스크립트 경로와 `campaign-runtime.js` 공개 export는 호환 진입점으로 보존하고, 실제 구현은 `app`, `presentation`, 기존 `domain/application/infrastructure` 경계 아래에서 찾을 수 있게 한다.

## 범위

### 포함

- `src` 루트의 브라우저 실행 구현을 `src/app/entrypoints`로 이동하고 기존 URL에는 얇은 호환 진입점 유지
- `src/render` 구현을 화면 표현과 영업 조정 책임에 따라 `src/presentation`과 `src/application`으로 재배치
- `src/public-shell` 브라우저 UI 구현을 `src/presentation/public-shell`로 재배치
- 이동한 파일의 내부 import와 테스트 import 일괄 정합화
- 사용되지 않는 빈 디렉터리·중복 진입 구현 제거
- 정적 참조 검사, 전체 Vitest, 프로덕션 빌드와 핵심 공개 E2E 검증

### 제외

- 게임 규칙·밸런스·DOM selector·CSS class·화면 배치·에셋·저장 형식 변경
- 공개 HTML 경로, 루트 스크립트 경로와 `campaign-runtime.js` export 이름 변경
- JavaScript의 TypeScript 전환, 프레임워크·외부 의존성 추가
- 대형 진입 로직 자체의 재작성이나 기능 추가
- 사용자 작업인 `app/.gitignore` 수정

## 목표 구조

```text
src/
├── app/
│   └── entrypoints/          # 페이지별 실제 부팅 구현
├── application/
│   ├── businessDay/
│   ├── campaign/
│   ├── gameplay/             # 영업 조정·스테이션 유스케이스
│   ├── persistence/
│   └── ports/
├── core/
├── domain/
├── infrastructure/
├── presentation/
│   ├── public-shell/         # 공개 셸 DOM·브라우저 UI
│   └── three/                # Three.js 렌더러·재질·VFX·시각 어댑터
└── ...                       # 안정된 config/content/assets/scenario 등
```

`src/*.js` 중 HTML과 외부 소비자가 직접 참조하는 파일은 URL 호환용 얇은 진입점으로만 남긴다.

## 작업 절차

1. 현재 `src` 파일·HTML 진입 경로·정적 import 관계를 조사해 이동 대상과 공개 호환 경계를 고정한다.
2. 루트 실행 구현, 공개 셸 UI, Three.js 표현, 영업 조정 코드를 목표 디렉터리로 이동한다.
3. 모든 내부·테스트 import를 새 경로에 맞추고 공개 진입 파일과 facade export가 동일하게 작동하도록 한다.
4. 이전 경로 참조와 순환·누락 import를 검사한다.
5. 전체 Vitest, 프로덕션 빌드와 핵심 공개 E2E를 실행하고 결과·남은 위험을 기록한다.

## 병렬·순차 계획

- 다른 역할과 병렬 가능한 범위: `app/src`를 수정하지 않는 아트 제작과 문서 기획은 병렬 가능하다.
- 반드시 순차로 진행할 범위: import 조사 → 파일 이동·호환 진입점 작성 → 정적 참조 검사 → 전체 회귀 순으로 진행한다.
- 동시 writer를 금지할 경로·stable ID·finalizer: `app/src`, 이동 대상의 직접 테스트와 본 태스크 문서는 단일 writer가 수정한다.

## 사용자 승인 gate

- 승인 대상: 없음. 사용자가 기능 동등 리팩토링과 디렉터리 구조 개편을 직접 지시했다.
- 승인 전 금지: 게임 규칙·화면·콘텐츠·에셋·공개 계약 변경
- 승인 뒤 자동 인계: 자동 검증으로 동작 동등성을 확인하면 결과를 기록한다.

## 사람 화면 테스트

- 필수 경로: 공개 셸에서 S0와 실제 영업 화면 진입, D1 핵심 영업 입력
- 환경: Chromium FHD/720, Vite 프로덕션 빌드 또는 동일한 테스트 서버
- `PASS` 증거: 기존 핵심 공개 E2E가 이동 뒤에도 동일하게 통과하고 공개 HTML·스크립트 요청이 404 없이 로드된다.

## 의존성과 인계 조건

- 선행 작업: 완료된 개발자 1 작업 014의 기능 동등 리팩토링과 회귀 기준선
- 후속 작업: 신규 메뉴, 후속 영업일과 화면 구현
- 다른 개발자에게 제공할 입력·출력: 책임별 구현 위치, 유지되는 공개 호환 경로와 통과한 회귀 기준선
- 아트 작업인 경우 `semanticOwner`·review namespace·읽기 전용 공유 source: 해당 없음

## 완료 기준

- [x] 화면 진입 구현이 `src/app/entrypoints`, 공개 셸 UI가 `src/presentation/public-shell`에 위치한다.
- [x] Three.js 표현과 영업 조정 코드가 각각 `presentation`과 `application` 책임 아래에 위치한다.
- [x] 기존 공개 HTML·스크립트 URL과 `campaign-runtime.js` export가 유지된다.
- [x] 이전 내부 경로의 정적 import가 남지 않고 모든 이동 대상 테스트 import가 정합하다.
- [x] 전체 Vitest와 Vite 프로덕션 빌드가 통과한다.
- [x] 핵심 공개 E2E가 FHD/720에서 통과한다.

## 구현 및 검증 결과

- app 구현 위치:
  - `src/app/entrypoints/`와 `src/app/entrypoints/harnesses/`: 실제 페이지·검증 화면 부팅 구현
  - `src/application/gameplay/`: 조리·손님 운영·정산·드링크·성장·서빙·화면 전환 유스케이스
  - `src/presentation/three/`: Three.js renderer·material·shader parameter·sprite·VFX
  - `src/presentation/ui/`: DOM UI·화면 모델·입력 보조·손님 아트 선택
  - `src/presentation/public-shell/`: 공개 셸 브라우저 파일·계약·dialog
  - `src/*.js`: 기존 HTML URL을 유지하는 얇은 호환 진입점, `campaign-runtime.js`: 기존 facade 유지
- 제거된 이전 구현 위치: `src/render/`, `src/public-shell/`
- 테스트 위치: 이동된 모듈을 소비하는 기존 unit·integration·E2E import 정합화,
  `tests/unit/sourceStructure.test.js`에 호환 진입점과 이전 디렉터리 재유입 방지 계약 추가
- 구현 기준 spec·태스크: docs 기준 `15afc0e`, `SYS-004@604ad29`,
  `QA-002@832c9b6`, 본 태스크; app 기준 `387c995`와 완료된 작업 014의 미커밋 리팩토링 결과
- 검증 방법: 이전 경로 정적 검색과 `git diff --check`; 전체 `npm test`; `npm run build`;
  `npm run verify`; 공개 셸·S0→D1 전용 Playwright
- 검증 결과:
  - 이전 `render/`, `public-shell/` 내부 경로 참조 0건, 구조 회귀 테스트 2건 통과
  - 전체 Vitest 71개 파일·496개 테스트 통과
  - Vite 프로덕션 빌드 통과
  - D1 release drift 일치, D1 core unit 82개와 Chromium FHD/720 E2E 14개 통과
  - 공개 셸·S0→D1 Chromium FHD/720 E2E 32개 통과
- 남은 위험: `src/render/*`와 `src/public-shell/*`는 공개 API가 아닌 내부 경로로 판단해 호환 shim을
  남기지 않았다. 저장소 밖 개발 도구가 이 내부 경로를 직접 import한다면 새 책임 경로로 갱신해야 한다.
  Vite의 기존 500kB 초과 Three.js chunk 경고는 유지되며 이번 기능 동등 구조 개편에서 분할하지 않았다.
  사용자 작업인 `app/.gitignore` 변경은 수정하지 않았다.
- 완료일: `2026-08-24`
