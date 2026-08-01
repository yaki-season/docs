# 개발자 3 - 009 D1 공개용 정적 release artifact와 무주입 부팅 gate

- 상태: `완료`
- 담당자: `개발자 3`

## 참조 spec

- `spec/data/DAT-001_콘텐츠와_밸런스_데이터_계약.md` - `DAT-001` - `v5.23.0`
- `spec/gameplay/GPL-004_조리_스테이션과_품질_서빙.md` - `GPL-004` - `v1.38.0`
- 선행 작업: `developer-3/007 v1.0.2`, `developer-3/008 v1.0.2`

## 목표

작업 007·008의 D1 release definition을 정적 공개 URL의 versioned JSON artifact로 결정적으로
생성하고, route 주입 없이 실제 정적 서버에서 D1 영업이 부팅되는지를 검증한다.

## 범위

### 포함

- 정본 builder를 재사용하는 canonical JSON release 생성기와 명시적 write/check 모드
- `/content/releases/d1-business-day-definition.v1.json` 정적 artifact
- artifact와 현재 정본 builder 출력의 byte 단위 drift 검사
- route 주입 없는 Chromium 1280×720·1920×1080 공개 부팅 smoke
- 404·지원하지 않는 schema version에서 fixture·코드 기본값 fallback 없이 D1 시작 실패를 유지하는 검증
- 개발자 1의 production 소비와 테스트 분리를 위한 handoff

### 제외

- D5 및 D2~D4 데이터, 영업 도메인 규칙, 가격·시간·주문·손님 수치 변경
- `game.js`, domain runtime, UI 디자인, 아트, manifest, runtime asset promotion
- `basePrice=3`의 10% 반올림 정책 변경

## 완료 기준

- [x] 실제 release artifact가 정적 URL에서 HTTP 200으로 제공된다.
- [x] generator check가 정본 builder 출력과 artifact의 byte 일치를 확인하고 drift 최상위 필드를 보고한다.
- [x] 무주입 FHD/720 D1 boot에서 6석·4손님·4주문·9항목·첫 wave 츠키오카가 확인된다.
- [x] 404·schema version 오류에서 production이 개발 fixture나 기본값으로 진행하지 않는다.
- [x] 전용 테스트, 전체 Vitest, `git diff --check`가 통과한다.

## 구현 및 검증 결과

- app 구현 위치: `tools/content/build-d1-release-definition.mjs`, `content/releases/d1-business-day-definition.v1.json`, `tests/unit/d1ReleaseArtifact.test.js`, `tests/e2e/d1-release-static-smoke.spec.js`
- artifact SHA-256: `b1535f2f6b5a7f3fde6f32eb4cb016007df5ad790d760f92456ab70098507941`
- 구현 기준 spec 버전: `DAT-001 v5.23.0`, `GPL-004 v1.38.0`
- 구현 기준 태스크 버전: `v1.0.1`
- 검증 방법: explicit write, drift check, Python static HTTP 200 `application/json`, release unit/consumer tests, route 무주입 Chromium FHD/720 smoke, 전체 Vitest, `git diff --check`
- 검증 결과: artifact build/check 일치; static HTTP `200`; E2E `6/6` (FHD/720 정상 2건, 404/version failure 4건); Vitest `42 files / 299 tests` 통과; diff check 통과
- 남은 위험: production은 artifact만 소비해야 하며, artifact 갱신을 생략한 정본 변경은 check가 배포 전에 차단한다.
