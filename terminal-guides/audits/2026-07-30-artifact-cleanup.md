# S0+D1 artifact cleanup manifest — 2026-07-30

- 작성 역할: `Integration QA & Release`
- 범위: `app`, `docs`, `art-workspace`의 S0+D1 공개 경로만. D2~D5는 감사·정리 대상에서 제외한다.
- 안전 기준: tracked dirty, active source, 승인 evidence, provenance, finalizer/handoff, receipt/staging,
  현재 Artist 후보와 release definition은 삭제하지 않는다. 삭제는 이 문서에 기록된 정확한 ignored
  SAFE-TRANSIENT 경로만 가능하다.

## 동시 작업 확인

감사 시점에 `npm run verify`의 Playwright worker와 Chromium이 실행 중이었다. 로컬 캡처 서버도
`8010`, `8011`, `8012`에 존재했다. asset promotion과 art finalizer process는 발견하지 못했다.
따라서 실행 중에는 `app/test-results/`와 모든 promotion receipt/staging의 inventory만 작성했다.
verify 종료 뒤 Playwright·Vitest·promotion·finalizer process 부재를 다시 확인하고, 정확한
`app/test-results/`만 삭제했다. 어떤 process도 종료하지 않았다.

## Cleanup inventory

| repository | path | tracked / ignored | files · size | SHA-256 / directory digest | 생성 역할 | 현재 owner | 상태 | live reference | successor | 분류 | 결정 근거 | 복구 방법 |
|---|---|---|---:|---|---|---|---|---:|---|---|---|---|
| app | `test-results/` | ignored (`.gitignore:2`) | pass 1: 1 · 4 KiB; pass 2: 3 · 4,076 KiB | pass 1 `91bf4caf4a6f10d368dcfc0ea4e5f73221429f7dc68ea54eb12d2a4987d9c9a3`; pass 2 `1655938b1cac621ad78caf9f82ba9c06c3725eda97f3ca822f4d609772364ef3` | Playwright | QA | 삭제됨 | 0 (각 삭제 직전 process 재확인) | 다음 Playwright 실행 | DELETED | ignored·자동 재생성·사용자 source 아님·approval/provenance/handoff 무참조다. 초기 실행 중 4,092 KiB inventory는 보존했고, 첫 삭제 뒤 다른 실행이 재생성한 3 files도 process 종료 뒤 다시 안전하게 삭제했다. | `npm run test:e2e` 또는 다음 Playwright 실행이 재생성 |
| app | `playwright-report/`, `coverage/`, `.cache/` | 해당 없음 | 0 · 0 KiB | 해당 없음 | test tools | QA | 없음 | 0 | 해당 없음 | KEEP | 감사 시 경로가 존재하지 않아 삭제할 대상 없음 | 필요 시 도구가 생성 |
| app | `assets` | symbolic link | 1 link · N/A | target=`app/public/assets` | development server | Developer 2 | 의도된 link | 공개 asset 경로 | 해당 없음 | KEEP | 정적 개발 서버 연결이며 자동 삭제 금지 대상 | `ln -s public/assets assets` (삭제하지 않음) |
| app | `public/assets/manifest.json` | tracked | 1 · 포함 asset manifest | `417d2be0584e620450ebd90e0e58b31dca49881b5f86884a06d5380c124c4b57` | Developer 2 promotion | Developer 2 | runtime source of truth | runtime manifest | 후속 승인 promotion | KEEP | 승인 runtime 증거이며 수동 편집·삭제 금지 | Git/승격 transaction으로만 복구 |
| app | `.asset-promotion-receipts/` | protected | 7 · 28 KiB | dir digest `bb4cfe68ef416c283a59299a4ec2810331a8cdb3fcd7febc628fa68ffcf50673` | asset promotion | Developer 2 | 감사 입력 | promotion audit | 해당 없음 | KEEP | 만료 여부와 무관하게 Developer 2 감사 입력 | 해당 promotion dry-run으로만 재발급 |
| app | `.asset-promotion-staging/` | protected | 0 · 0 KiB | empty digest `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855` | asset promotion | Developer 2 | 보호됨 | promotion workflow | 해당 없음 | KEEP | 비어 있어도 protected 경로이며 현재 병행 작업 중 | promotion workflow가 관리 |
| art-workspace | `.git/lfs/tmp/` | protected | 0 · 0 KiB | empty digest `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855` | Git LFS | repository tooling | 보호됨 | LFS tooling | 해당 없음 | KEEP | 명시적 자동 삭제 금지 경로 | LFS가 관리 |
| art-workspace | `review/artist-000/d1-cooking/assembly/station/r1/` | tracked historical evidence | 7 files (5 binary) · 6,112 KiB | dir digest `3dded63b073e11f060f696d47e1712c3636665abf6b1adcca7be46a7bf961c40` | Artist 1 | Artist 1 | `superseded-by-r2` | >=1: R2 provenance consumes R1 output | `station/r2` | UNKNOWN-BLOCKED | superseded 표시가 있으나 successor provenance의 live input이고 source·review evidence를 포함 | 삭제하지 않음; Git history 및 retained source |
| art-workspace | `review/artist-000/d1-cooking/grill/finished-tray/r1/` | active approval evidence | 5 files (3 binary) · 2,560 KiB | dir digest `02a01a4b59092ff415b352e8e8bf3659bf5d7c2fa2fa2c2b0cc3d1bf5fa61430` | Artist 1 | Artist 1 | approved-by-user | 다수: finished composition/provenance/runtime compose | 현재 approved input | KEEP | 현재 handoff 입력이며 finalizer 전에도 삭제 금지 | Git history와 retained source |
| art-workspace | `review/artist-000/d1-cooking/grill/finished-tray/r3/` | untracked historical candidate | 8 files (3 binary) · 2,996 KiB | dir digest `ee82d38c44ebb0289fd49c5683aa0e3e58df6e4162abdd7818a41b92a82d00cd` | Artist 1 | Artist 1 | superseded-by-user-feedback | 미확정; 외부 정본 참조 0건을 아직 owner 확인 없이 확정 불가 | `finished-tray/r4` 이후 R6 검수 | UNKNOWN-BLOCKED | Artist 삭제 확인·전 저장소 live-reference 확정·tombstone이 아직 없음 | 삭제하지 않음; source/review 유지 |
| art-workspace | `review/artist-000/d1-cooking/grill/finished-tray/r4/` | untracked historical candidate | 8 files (3 binary) · 2,828 KiB | dir digest `ca26a9e4c9861ec6895cf376f0497c175e3bb1da95b23d768e3fe9621f6c0c4f` | Artist 1 | Artist 1 | superseded-by-user-direction | 미확정; 외부 정본 참조 0건을 아직 owner 확인 없이 확정 불가 | `finished-tray/r5` 이후 R6 검수 | UNKNOWN-BLOCKED | Artist 삭제 확인·전 저장소 live-reference 확정·tombstone이 아직 없음 | 삭제하지 않음; source/review 유지 |

## 결과와 후속 gate

- SAFE-TRANSIENT 삭제: 총 `4` files, `4,080 KiB` (`app/test-results/`, 두 안전 pass). 실행 중 Playwright와
  충돌하지 않았고, 승인·runtime·handoff evidence 손실은 `0`건이다.
- OWNER-CONFIRM: `0`건. 위 historical tray R3/R4는 Artist 1의 묶음 확인 전에는 UNKNOWN-BLOCKED다.
- 코드·fixture·중복 binary는 파일명이나 revision만으로 삭제하지 않았다. import/dynamic import/HTML script,
  package script, 테스트, 문서 생성 경로의 완전 무참조 감사와 원래 Developer owner의 확인이 선행되어야 한다.
- 공개 gate 증빙: cleanup 후 `npm run assets:validate`는 runtime asset `9`개, `npm run visual:references:validate`는
  reference `36`장을 통과했다. runtime inventory는 binding `9`·placeholder `33`·drink `3`·unbound approved `0`이고,
  manifest SHA는 위 값과 동일하다. `node pipeline/validate-pipeline.mjs`는 `29` 오류(spec SHA `4`, S0 topology
  `2`, legacy provenance `23`)로 실패했다. 정적 artifact `content/releases/d1-business-day-definition.v1.json`은
  존재하지만 이 shell에서 static D1 URL과 FHD/720 smoke는 실행하지 않았으므로 SKIP이며, 이전 결과를 현재 통과로
  사용하지 않는다.
