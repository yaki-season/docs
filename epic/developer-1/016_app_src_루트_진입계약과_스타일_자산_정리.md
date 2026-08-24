# 개발자 1 - 016 app/src 루트 진입 계약과 스타일·자산 정리

- 상태: `완료`
- 담당자: `개발자 1`
- 우선순위: `P1`
- 현재 milestone에 필요한 이유: 책임별 구조 개편 뒤에도 페이지 CSS와 브랜드 이미지가 `src` 루트에 남아 진입 파일과 구현 자산의 경계가 불명확하다.
- 하지 않으면 막히는 stage gate: 신규 화면·메뉴 확장 시 루트 파일이 계속 증가하지 않도록 하는 탐색성과 소유권 규칙

## 참조 spec

- `spec/system/SYS-004_app_프로젝트와_소스코드_아키텍처.md` - `SYS-004` - git `604ad29`
- `spec/qa/QA-002_전체_게임_기능_성능_출시_검증.md` - `QA-002` - git `832c9b6`

## 목표

공개 HTML URL, JavaScript 호환 경로와 화면 동작을 유지하면서 `app/src` 루트에는 페이지 HTML·호환 JavaScript·공개 facade만 남긴다. 스타일은 `presentation/styles`, 고정 공개 favicon은 `public`으로 이동하고 루트 허용 파일 계약을 자동 검증한다.

## 범위

### 포함

- 화면 CSS를 `src/presentation/styles/pages`로 이동
- 검증 화면 CSS를 `src/presentation/styles/harnesses`로 이동
- 공유 그릴 CSS를 `src/presentation/styles/shared`로 이동
- 고정 URL favicon을 `public`로 이동해 기존 `/yaki-season-icon-r3-192.png` 유지
- HTML stylesheet 참조와 CSS 내부 import 갱신
- `src` 루트 허용 파일 자동 검사 강화
- 전체 Vitest·Vite 빌드와 공개 FHD/720 E2E 검증

### 제외

- 공개 HTML URL과 페이지 이름 변경
- 루트 JavaScript 호환 경로와 `campaign-runtime.js` export 변경
- CSS selector·규칙·화면 배치·이미지 내용 변경
- 게임 규칙·상태·저장·에셋 manifest 변경
- 사용자 작업인 `app/.gitignore` 수정

## 작업 절차

1. 루트 CSS·이미지의 실제 HTML·테스트 참조를 확인한다.
2. 책임별 스타일과 favicon을 이동하고 참조 경로만 갱신한다.
3. `src` 루트에 허용되지 않은 확장자·파일이 다시 추가되지 않도록 구조 테스트를 보강한다.
4. 전체 단위·통합 테스트, 빌드와 공개 브라우저 경로를 검증한다.
5. 결과와 남은 위험을 기록하고 완료 처리한다.

## 사용자 승인 gate

- 승인 대상: 없음. 사용자가 기존 실행 영향 최소화를 조건으로 루트 정리를 직접 지시했다.
- 승인 전 금지: 공개 URL·동작·화면 스타일·콘텐츠 변경

## 사람 화면 테스트

- 필수 경로: 공개 셸→S0→D1, D1 영업, 기존 검증 화면의 CSS 로드
- 환경: Chromium 1280×720·1920×1080, Vite 개발/테스트 서버
- `PASS` 증거: stylesheet·favicon 요청 실패 없이 기존 공개 E2E가 통과한다.

## 완료 기준

- [x] `src` 루트에는 공개 진입 계약 파일만 남는다.
- [x] 페이지·공유·검증 화면 스타일이 `presentation/styles` 하위에서 구분된다.
- [x] favicon의 기존 공개 URL이 유지된다.
- [x] CSS 규칙과 화면 결과에 의도한 변경이 없다.
- [x] 전체 Vitest·빌드·핵심 공개 E2E가 통과한다.

## 구현 및 검증 결과

- app 구현 위치:
  - `src/presentation/styles/pages/`: `game`, `d1-game`, `d1`, `d1-scene`, `public-shell`, `s0-d3` 페이지 스타일
  - `src/presentation/styles/harnesses/`: 아트 재조립·그릴 UI·S0 외관·단일 손님 검증 화면 스타일
  - `src/presentation/styles/shared/grill-ui-layout.css`: 그릴 화면 공통 override
  - `public/yaki-season-icon-r3-192.png`: 기존 공개 favicon URL의 단일 정본
  - `tests/unit/sourceStructure.test.js`: `src` 루트의 HTML·호환 JS·facade allowlist 계약
- 제거한 중복: `src/yaki-season-icon-r3-192.png`는 `public` 파일과 SHA-256
  `c18cfbbbcbeb0fcf4930616f065bdec74a6f8def06a69ce928fe159bae227a1c`가 같아 중복본만 제거했다.
- 구현 기준 spec·태스크: docs 기준 `7cc1111`, `SYS-004@604ad29`,
  `QA-002@832c9b6`, 본 태스크; app 기준 `e0cadeb`
- 검증 방법: 이전 루트 CSS·이미지 참조 정적 검색, `git diff --check`, 전체 `npm test`,
  `npm run build`, 영향 화면 9개 Playwright spec의 Chromium FHD/720 실행
- 검증 결과:
  - `src` 루트는 공개 페이지 HTML 11개·호환/공개 JavaScript 11개만 유지
  - 전체 Vitest 71개 파일·497개 테스트 통과
  - Vite 프로덕션 빌드 통과, `dist/yaki-season-icon-r3-192.png`와 public 정본 해시 일치
  - 영향 화면 Playwright 78건 중 76건 통과·기존 명시적 legacy 2건 skip
- 남은 위험: 루트 HTML과 얇은 JavaScript는 공개 URL·호환 경로이므로 의도적으로 유지한다.
  Vite의 기존 500kB 초과 Three.js chunk 경고는 이번 스타일·자산 이동 범위 밖이며,
  사용자 작업인 `app/.gitignore` 변경은 수정하지 않았다.
- 완료일: `2026-08-24`
