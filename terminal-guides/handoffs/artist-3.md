# Artist 3 직전 작업 기록

- 마지막 갱신: `2026-07-30`
- 현재 담당: Artist 작업 025
- 현재 알려진 checkpoint: `review/artist-025/bg-workspace-drink/r2/metadata/runtime-handoff.json`; `review/artist-025/preflight/st-drink-beer-tier-1-zero-pixel-preflight-r1.md`
- 사용자 승인 대기: 원본 마스터·이자카야 reference를 반영한 `ST-DRINK-BEER-TIER-1 R2`의 FHD/720 소비 화면. R1은 사용자 반려로 승인 후보가 아니다. `BG-WORKSPACE-DRINK R2` 최종 소비 화면은 `2026-07-30` 승인됨.
- 개발자 입력 대기: 없음. `BG-WORKSPACE-DRINK R2` promotion gate는 해소됐다. 후속 서빙·정리·엑스트라·정산에는 개발자 1 gameplay inventory의 증분 stable ID가 계속 필요
- 현재 PM 지시: R2 픽셀·카메라·z0·DOM safe rect를 변경하지 않고 `ST-DRINK-BEER-TIER-1` 단일 후보 제작을 재개한다.
- 마지막 완료 동작: `2026-07-30` PM indoor-only topology correction 및 사용자 최종 소비 화면 승인 뒤, R2 `a30b2d44635ba52eef5d5461d4ea9b86ea80a2ab12e47f5377b58279dddeafce`에 대해 completion → provenance → lossless optimization B1 → finalizer → runtime handoff를 생성했다. FHD `c6f2ca31ace91e7a3d5ded07fe464ff5b63c89eeeafa8287e2d43c742d93789c`, 720 `fbc5649efb36bd1e397e5f86b6605e7d443ac319de22af3d4056b0d00404b87e`; body=0, raster UI=0, station=0이다. `ST-DRINK-BEER-TIER-1`에는 pixel 0의 contract preflight만 작성했다.
- Developer 2 promotion 결과: `BG-WORKSPACE-DRINK@R2-B1` dry-run·receipt 검증·원자적
  write·exact `drink.scene` binding 완료. runtime URL
  `/assets/core/drink/bg-workspace-drink-r2-b1.png`, SHA
  `a30b2d44635ba52eef5d5461d4ea9b86ea80a2ab12e47f5377b58279dddeafce`.
  전체 placeholder `34→33`, drink `4→3`, 두 D1 진입점 missing ID 제거,
  `unboundApprovedIds=[]`, `contractAudit.valid=true`, assets 9·Vitest 295·FHD/720 D1 4개 통과.
- Developer 2 재개 통보: 위 promotion 성공 조건을 모두 충족했으므로
  `ST-DRINK-BEER-TIER-1` 단일 후보 제작을 시작해도 됨을 확인했다.
- 마지막 완료 동작: R1을 사용자 반려로 고정했다. 원본 마스터·생맥주 스테이션 컨셉·이자카야
  reference를 대조해 `preflight/st-drink-beer-tier-1-r2-correction-brief.md`에 무픽셀 교정안을
  기록했다. R1은 대형 산업 기계/중복 카운터로 판정되어 promotion 대상이 아니다.
- 다음 한 동작: R2의 소비 화면을 사용자에게 제시한다. 승인 전에는
  completion·provenance·optimization·finalizer·runtime-handoff·catalog 변경을 만들지 않으며,
  glass·lever·liquid·VFX도 병행 시작하지 않는다.
- 병렬 무픽셀 준비: `preflight/ch-extra-commuter-service-zero-pixel-preflight-r1.md`를
  작성했다. Developer 1 `extra-actor-layout v1.0.0`으로 six-seat FHD/720 bounds,
  lower-centre anchor, occlusion line, foreground occlusion, transparent hit/raster 분리는
  수신·감사했다. clip/state mapping과 이름 없는 commuter 의미 addendum을 기다린다. 이 입력과
  사용자 preflight 승인이 전에는 commuter pixel을 만들지 않는다.
- 수정 금지: `review/artist-000/`, `review/artist-023/`
- 후속 화면 기획 제약: 서빙·준비 목록·엑스트라 소비 화면에 번호 순번, FIFO 화살표,
  `먼저 주문한 손님` 강조를 넣지 않는다. 서빙 화면은 `공용 완성품 → 플레이어가 선택한
  손님 → 그 손님의 남은 주문` 구조로 제작한다. 이 제약은 현재 drink station R2 픽셀을
  변경하지 않는다.
- 이동 주의: 미추적 pending 파일은 Git/LFS 또는 승인된 보안 전송 없이는 다른 컴퓨터에서 복구 불가
