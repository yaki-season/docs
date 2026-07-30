# 역할별 작업 인계 기록 템플릿

- 마지막 갱신:
- 담당 역할:
- 작성한 터미널:
- runId:
- stage:
- producer → consumer:
- 담당 epic·버전:

## 저장소 체크포인트

| 저장소 | branch | HEAD | 담당 dirty·untracked 경로 | 다른 역할 변경 주의 |
|---|---|---|---|---|
| `docs` |  |  |  |  |
| `app` |  |  |  |  |
| `art-workspace` |  |  |  |  |

## 작업 체크포인트

- 마지막 완료 동작:
- 현재 단일 작업 또는 단일 후보:
- 허용 쓰기 경로:
- 읽기 전용 입력·artifact SHA:
- prerequisite 충족 여부:
- 제외 범위:
- 사용자 승인 대기:
- 사람 화면 테스트 gate:
- 다른 역할 입력 대기:
- 알려진 실패·재현 명령:
- 마지막 검증 명령:
- 마지막 검증 결과:
- 다음 한 동작:
- 완료 뒤 자동 인계할 역할·epic:
- 같은 경로·stable ID의 동시 writer:
- 수정 금지·읽기 전용 경로:
- 다른 컴퓨터 이동 시 아직 전달되지 않은 파일:

## 재개 시 검증

- [ ] dashboard와 epic 상태가 이 기록보다 최신인지 확인
- [ ] 세 저장소 branch·HEAD 확인
- [ ] dirty·untracked 파일 실재 확인
- [ ] 관련 spec 버전 확인
- [ ] 마지막 실패 재현 또는 최소 기준선 확인
- [ ] artifact·finalizer·promotion receipt의 exact SHA 확인
- [ ] 사용자 승인과 사람 화면 테스트 상태를 추측하지 않았는지 확인
- [ ] 같은 출력의 동시 writer가 없는지 확인
- [ ] 복구 보고 후 다음 한 동작만 시작
