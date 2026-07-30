# Integration QA & Release handoff

- 상태: `진행 중`
- 담당 epic: `qa-release/001 v1.1.0`
- 최신 runtime 기준: binding `9`, 전체 placeholder `33`, drink placeholder `3`. `BG-WORKSPACE-DRINK@R2-B1` promotion과 `drink.scene` binding은 완료됐다.
- hygiene target audit: assets `9`·reference `36` 통과, runtime의 unbound approved `0`, manifest SHA `417d2be0…c4b57` 확인. art pipeline은 spec SHA 4·S0 topology 2·legacy provenance 23, 총 `29` 오류로 공개를 차단한다.
- 테스트 기준: 이전 Vitest·Playwright 통과/실패 수는 stale이다. hygiene 감사 중 관찰한 다른 역할의 `npm run verify`는 종료됐지만 그 결과를 이 handoff가 수집하지 않았으므로 전체 결과를 확정하지 않는다.
- 추가 책임: S0+D1 ignored transient·obsolete candidate의 repository hygiene. active process의 test-results/staging/receipt는 보존하고, cleanup manifest에 정확한 경로·SHA·크기·사유·복구 방법을 남긴다.
- 현재 정리 상태: 종료 뒤 ignored `app/test-results/`를 두 차례 SAFE-TRANSIENT로 삭제했다(총 4 files·4,080 KiB). 첫 삭제 뒤 다른 Playwright 실행이 결과를 재생성했으나, 두 번째 삭제 직전에도 관련 process 부재를 확인했다. 다음 Playwright 실행이 재생성한다. `app/assets` 링크, public assets, receipt/staging, provenance·handoff·승인 evidence·현재 Artist 후보는 KEEP이다.
- 다음 한 동작: 병행 verify 종료 뒤 SAFE-TRANSIENT를 재감사하고, Developer 1·2·Artist handoff가 모두 안정되면 Chromium·Chrome·Edge, static host, assets/reference/pipeline, placeholder 0 gate를 재검증한다.
- 주의: gameplay·UI·밸런스·아트·manifest를 수정하지 않고, tracked dirty·active source·D2~D5 산출물은 정리하지 않는다.
