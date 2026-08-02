# Integration QA & Release handoff

- 상태: `진행 중`
- 담당 epic: `qa-release/001 v1.3.1`
- runtime 기준: assets `12/12`, references `36/36`, D1 required `44`, binding `9`, placeholder
  `35`, drink placeholder `5`, unbound approved `0`, manifest SHA
  `3ef18fffe69b4d447a00c1ecb99bdaf22c7e6a0848f242e3a580acf60bab13c7`.
- `QA-D1-011-20260801-R3`: app `main@63df61849f3bbd5e67bbbc63640febd912f7e223` + dirty 6파일
  SHA 전부 일치, task unit `6/6`, FHD/720 E2E `24/24`, 최초 실패·retry·40404 `0`.
  task 011 자동 회귀는 GREEN이지만 정적 URL/build ID·사람 PASS는 없다.
- `QA-ART-PIPELINE-20260801-R4`: art
  `agent/pipeline-v8-contract@c1905689130d18e7bcb2774e1aac98c57d8b5bd3`, diff check PASS,
  contract `YS-ASSET-PIPELINE-v8`, report `25`, provenance `38`, error `3`. 남은 세 건은
  `assembly/r1`, `finished-tray/r1`, `grill/master/r3`의 증거·의미 blocker며 pipeline gate는 RED다.
- 추가 책임: S0+D1 ignored transient·obsolete candidate의 repository hygiene. active process의 test-results/staging/receipt는 보존하고, cleanup manifest에 정확한 경로·SHA·크기·사유·복구 방법을 남긴다.
- 현재 정리 상태: 종료 뒤 ignored `app/test-results/`를 두 차례 SAFE-TRANSIENT로 삭제했다(총 4 files·4,080 KiB). 첫 삭제 뒤 다른 Playwright 실행이 결과를 재생성했으나, 두 번째 삭제 직전에도 관련 process 부재를 확인했다. 다음 Playwright 실행이 재생성한다. `app/assets` 링크, public assets, receipt/staging, provenance·handoff·승인 evidence·현재 Artist 후보는 KEEP이다.
- 다음 한 동작: 사용자 승인 art lane의 finalizer→promotion→binding 뒤 exact gameplay
  회귀를 수신하고, placeholder 0·pipeline 0·static host를 동일 HEAD에서 재검증한다.
- 주의: gameplay·UI·밸런스·아트·manifest를 수정하지 않고, tracked dirty·active source·D2~D5 산출물은 정리하지 않는다.
