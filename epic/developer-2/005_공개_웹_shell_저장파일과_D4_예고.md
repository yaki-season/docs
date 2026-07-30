# 개발자 2 - 005 공개 웹 shell·저장 파일과 D4 예고

- 문서 버전: `v1.1.0`
- 최종 변경일: `2026-07-30`
- 상태: `진행 중`
- 담당자: `개발자 2`

## 참조 spec

- `spec/system/SYS-003_전체_게임_런타임_저장_성능.md` - `SYS-003` - `v2.0.0`
- `spec/system/SYS-004_app_프로젝트와_소스코드_아키텍처.md` - `SYS-004` - `v3.1.0`
- `spec/ui/UI-002_전체_게임_화면_입력_접근성.md` - `UI-002` - `v5.25.0`
- `spec/ui/UI-003_전체_게임_상세_화면_설계.md` - `UI-003` - `v1.2.0`
- `spec/qa/QA-002_전체_게임_기능_성능_출시_검증.md` - `QA-002` - `v1.1.0`

## 목표

정적 웹 URL에서 새 게임·이어하기·설정·저장 파일 백업·오류 진단·D4 개발 예고를 제공하는
공개용 app shell을 완성한다.

## 사용자 확정 실행 조건

- 최신 Chrome·Edge의 FHD/720을 공식 지원한다.
- 정적 웹 URL 하나를 계속 갱신하며 별도 설치·계정·원격 수집은 없다.
- 저장 내보내기·불러오기를 제공하고 기존 정상 저장을 백업한 뒤 교체한다.
- D4는 공개 빌드에서도 경고 확인 뒤 들어갈 수 있는 읽기 전용 예고다.
- 오디오는 후속 범위이며 현재 설정에는 기능이 없는 오디오 제어를 노출하지 않는다.

## 범위

### 포함

- 부팅·타이틀·새 게임·이어하기·설정·도움·일시정지·복구 화면
- 저장 파일 내려받기, 파일 선택, 검증 결과, 교체 확인과 복구 안내
- 개인정보 없는 진단 정보 복사·파일 내려받기
- D3 완료 뒤 `개발 중 콘텐츠` 확인과 읽기 전용 D4 예고 화면
- 정적 배포의 base path·캐시·에셋 오류·새 버전 갱신 UI
- 키보드 focus·44px 입력 영역·비색상 상태 표시

### 제외

- 저장 도메인·마이그레이션 로직 — 개발자 1 작업 008
- 원격 분석·오류 자동 전송·클라우드 저장
- Electron·PWA·WebView·모바일 공식 지원
- D4 실제 gameplay

## 작업 절차

1. UI-003 screen/overlay registry에 공개 app shell을 구현한다.
2. 작업 008의 저장 port에 이어하기·내보내기·불러오기·복구 UI를 연결한다.
3. 진단 정보에서 개인·경로·원본 콘텐츠를 제거하고 명시적 사용자 다운로드만 제공한다.
4. D4-preview 경고·읽기 전용 화면·복귀 동작을 구현한다.
5. 정적 배포에서 base path·캐시 갱신·필수 에셋 실패를 검증한다.
6. Chrome·Edge FHD/720과 키보드·마우스 종단을 검증한다.

## 의존성과 인계 조건

- 선행 작업: 개발자 1 작업 008, 개발자 2 작업 004
- 병행 작업: 개발자 1 작업 009·010
- 후속 작업: 개발자 2 작업 006
- 배포 호스트 선택은 실제 배포 착수 시 origin·cache 계약을 보존하는 범위에서 결정한다.

## 완료 기준

- [x] 새 게임·이어하기·설정·도움·복구 화면이 빈 상태와 오류를 처리한다.
- [x] 저장 파일 내보내기·호환 불러오기·비호환 거부·기존 저장 백업이 검증된다.
- [x] 어떤 플레이·진단 데이터도 사용자 동작 없이 외부로 전송되지 않는다.
- [x] D4 예고는 경고 뒤 읽기 전용으로 열리고 저장·보상·주문을 변경하지 않는다.
- [x] 정적 URL 새 버전 갱신과 필수 에셋 실패가 빈 화면 없이 안내된다.
- [ ] 최신 Chrome·Edge의 FHD/720 마우스·키보드 E2E가 통과한다.

## 구현 및 검증 결과

- app 구현 위치:
  - shell 진입점: `app/src/public-shell.html`
  - 화면 composition: `app/src/public-shell.js`, `app/src/public-shell.css`
  - 설정·도움·일시정지·진단 overlay: `app/src/public-shell/publicShellDialogs.js`
  - 진단·D4·설정 순수 계약: `app/src/public-shell/publicShellContract.js`
  - 파일 읽기·복사·다운로드 adapter: `app/src/public-shell/browserFiles.js`
  - 새 게임의 첫 checkpoint 전 원본 보존: `app/src/scenario/s0-d3-campaign.js`, `app/src/s0-d3.js`
- 구현 기준 spec 버전: 위 참조 spec
- 구현 기준 태스크 버전: `v1.1.0`
- 저장 port 연결:
  - Developer 1 작업 008의 `createSaveFilePort`, `CampaignSaveRepository`,
    `SettingsRepository`, `DiagnosticsRepository`, `LocalStorageAdapter` 공개 export만 소비
  - 호환 파일은 검증 뒤 사용자 확인 전까지 write 0, 확인 뒤 기존 정상 active를
    `backup-1`로 회전하고 교체
  - 미래 schema·손상 파일은 active·backup 무변경으로 거부
  - 손상 active 복원 시 원본을 `RECOVERY_SOURCE`에 보존
- D4 불변식:
  - `d4-preview/preview` 저장만 버튼 활성화
  - `OVR-CONFIRM` 뒤 `SCR-SYS-D4-PREVIEW` 진입
  - active·backup 1·backup 2·recovery source를 진입 전후 대조하며 gameplay·보상·저장
    port의 write 명령은 호출하지 않음
- 진단 불변식:
  - 허용 목록 9필드만 직렬화하며 로컬 경로·저장 payload·대사·원본 콘텐츠를 제외
  - 원격 수집은 없고 복사·다운로드는 명시적 사용자 입력에만 실행
- 검증 방법: Vitest, Playwright Chromium 1280×720·1920×1080, 정적 HTTP server,
  `assets:validate`, `visual:references:validate`, 전체 `verify`
- 검증 결과:
  - runtime asset 8개, reference image 36장 통과
  - 전체 Vitest 32파일·243개 통과
  - public shell Playwright 16개 통과
  - public shell+S0 관련 Playwright 28개 통과
  - 전체 `verify`: Playwright 152개 통과·2개 skip, Developer 1 소유
    `grill-tuner.spec.js:40`의 기존 `uDoneness` 회귀가 두 viewport에서 실패해 종료 코드 1
- 남은 위험:
  - 이 환경에는 Microsoft Edge가 설치돼 있지 않아 branded Edge E2E가 미실행이다.
  - 실제 정적 호스트의 origin·base path·cache 정책은 배포 착수 뒤 별도 smoke가 필요하다.
  - 위 두 검증이 남아 있으므로 작업 상태를 `진행 중`으로 유지한다.

## 변경 이력

| 이전 버전 | 새 버전 | 날짜 | 변경 유형 | 근거 spec 버전 | 변경 내용 | 재작업 영향 |
|---|---|---|---|---|---|---|
| 없음 | `v1.0.0` | `2026-07-30` | 최초 생성 | `SYS-003 v2.0.0`, `SYS-004 v3.1.0`, `UI-002 v5.25.0`, `UI-003 v1.1.0`, `QA-002 v1.1.0` | 공개 웹 shell·저장 파일·진단·D4 예고 작업 생성 | 없음 |
| `v1.0.0` | `v1.0.1` | `2026-07-30` | 작업 시작·참조 동기화 | `SYS-003 v2.0.0`, `SYS-004 v3.1.0`, `UI-002 v5.25.0`, `UI-003 v1.2.0`, `QA-002 v1.1.0` | PM 지시에 따라 공개 shell 구현을 시작하고 현행 UI-003 참조를 동기화 | 별도 shell 진입점과 작업 008 공개 저장 port만 사용 |
| `v1.0.1` | `v1.1.0` | `2026-07-30` | 기능 구현·검증 | 동일 | 별도 public shell, 새 게임·이어하기·설정·도움·일시정지·복구, 저장 export/import/backup, safe diagnostic, D4 읽기 전용 예고와 FHD/720 회귀를 구현 | branded Edge·실제 정적 호스트 smoke 전까지 진행 중 유지 |
