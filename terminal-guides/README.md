# 역할별 터미널 재시작 가이드

이 디렉터리는 컴퓨터·터미널·대화 세션이 바뀌어도 YAKI SEASON 작업을 최신 정본과 직전
작업 상태에서 이어가기 위한 시작 지침을 관리한다.

## 역할별 시작 지침

새 터미널에서 해당 문서의 `터미널 시작 프롬프트` 전체를 첫 메시지로 사용한다.

| 역할 | 시작 지침 | 직전 작업 기록 |
|---|---|---|
| 기획/PM/오케스트레이터 | [PM.md](PM.md) | [handoffs/pm.md](handoffs/pm.md) |
| 개발자 1 | [DEVELOPER-1.md](DEVELOPER-1.md) | [handoffs/developer-1.md](handoffs/developer-1.md) |
| 개발자 2 | [DEVELOPER-2.md](DEVELOPER-2.md) | [handoffs/developer-2.md](handoffs/developer-2.md) |
| 개발자 3 | [DEVELOPER-3.md](DEVELOPER-3.md) | [handoffs/developer-3.md](handoffs/developer-3.md) |
| 통합 QA·릴리스 | [INTEGRATION-QA.md](INTEGRATION-QA.md) | [handoffs/integration-qa.md](handoffs/integration-qa.md) |
| Artist 1 | [ARTIST-1.md](ARTIST-1.md) | [handoffs/artist-1.md](handoffs/artist-1.md) |
| Artist 2 | [ARTIST-2.md](ARTIST-2.md) | [handoffs/artist-2.md](handoffs/artist-2.md) |
| Artist 3 | [ARTIST-3.md](ARTIST-3.md) | [handoffs/artist-3.md](handoffs/artist-3.md) |

## 현재 PM 실행 진입점

- [PM/오케스트레이터 단일 시작 지침](PM.md)
- [PM stage epic](../epic/pm-orchestration/)
- [현재 작업 현황](../epic/작업_현황.md)
- [사람 화면 플레이테스트 피드백 템플릿](../template/사람_화면_플레이테스트_피드백_템플릿.md)

과거 `dispatches/` 문서는 해당 시점의 기록이며 새 세션의 실행 정본이 아니다. PM은 현재 stage epic과
실제 파일·검증 증거를 복구해 역할 서브에이전트에 다음 작업을 자동 배정한다.

## 어디서나 이어가기 위한 전제

이 가이드는 파일이 실제로 새 컴퓨터에 전달됐을 때만 작업을 복구할 수 있다.

- `docs`, `app`, `art-workspace` 세 디렉터리가 같은 workspace 아래에 있어야 한다.
- 세 디렉터리는 서로 별도 Git 저장소이므로 각각 올바른 branch와 commit을 받아야 한다.
- `art-workspace`의 raster·검수 파일은 Git LFS 객체까지 받아야 한다.
- 로컬 미커밋·미추적 파일은 다른 컴퓨터에 자동으로 전달되지 않는다.
- 다른 컴퓨터로 옮기기 전에는 사용자 승인을 받은 방식으로 commit/push하거나 안전하게 파일을
  전송해야 한다. 원격과 권한이 확인되지 않은 상태에서 AI가 임의로 원격을 만들거나 push하지 않는다.
- 승인 대기 중인 비공개 아트가 원격 추적 금지 대상이면 binary를 임의로 공개하지 않는다. 이때는
  승인된 보안 전송 방식과 handoff 기록이 둘 다 필요하다.

즉, `handoff` 문서는 작업의 의미를 복구하고 Git·LFS 또는 승인된 파일 전송은 실제 파일을 복구한다.
둘 중 하나라도 없으면 누락을 숨기지 말고 PM에게 차단 상태로 보고한다.

## 공통 부팅 절차

모든 역할은 새 세션에서 다음 순서를 지킨다.

1. 현재 디렉터리가 `docs`, `app`, `art-workspace`의 상위 workspace인지 확인한다.
2. workspace 안의 모든 `AGENTS.md`를 찾아 담당 경로에 적용되는 지침을 읽는다.
3. 세 저장소의 branch, HEAD, dirty·untracked 파일을 읽기 전용으로 확인한다.
4. `docs/epic/작업_현황.md`에서 자기 역할의 실제 진행 중·대기 작업을 확인한다.
5. 자기 역할의 `handoffs/*.md`를 읽되, dashboard·epic·실제 파일과 다르면 최신 실제 상태를 우선한다.
6. 담당 epic의 목표·범위·의존성·완료 기준·구현 및 검증 결과·마지막 변경 이력을 읽는다.
7. 관련 `spec`의 현재 문서 버전과 epic이 참조한 버전을 대조한다.
8. 실제 diff와 마지막 테스트 결과를 확인하고 직전 작업을 재구성한다.
9. 파일을 바꾸기 전에 `복구 보고`를 사용자에게 제시한다.
10. 복구 보고가 끝난 뒤에만 다음 한 동작을 수행한다.

### 권장 읽기 전용 확인 명령

workspace 위치는 컴퓨터마다 다르므로 절대 경로를 지침에 고정하지 않는다.

```bash
pwd
rg --files -g 'AGENTS.md'
git -C docs status -sb
git -C app status -sb
git -C art-workspace status -sb
git -C docs log -1 --oneline
git -C app log -1 --oneline
git -C art-workspace log -1 --oneline
git -C art-workspace lfs status
```

위 명령은 상태 확인용이다. dirty 파일을 정리하기 위해 `reset`, `checkout`, `clean`, `stash`, 삭제를
실행하지 않는다.

## 정본 우선순위

서로 다른 기록이 충돌하면 다음 순서로 판단한다.

1. 사용자의 최신 확정 지시
2. 현재 확정 `spec`
3. 현재 담당 `epic`
4. `epic/작업_현황.md`
5. 역할별 `handoff`
6. 실제 구현·아트 파일과 검증 결과
7. 과거 대화 기억

실제 파일이 spec과 다르면 파일을 정본으로 승격하지 않는다. 차이를 보고하고 PM이 spec·epic 영향을
처리할 때까지 의미 변경을 멈춘다.

## 공통 복구 보고 형식

각 역할은 새 세션의 첫 상태 보고를 다음 형식으로 작성한다.

```text
역할:
현재 branch/HEAD:
담당 진행 중 epic과 버전:
발견한 dirty·untracked 담당 파일:
직전 완료 동작:
마지막 검증과 결과:
사용자 승인 대기:
외부/다른 역할 입력 대기:
다음 한 동작:
정본 충돌 또는 복구 불가 파일:
```

사용자 승인 대기 후보가 있으면 새 후보나 다음 기능을 만들지 않고 그 후보부터 보고한다.

## 공통 종료·인계 절차

작업을 멈추거나 컴퓨터를 바꾸기 전 다음을 수행한다.

1. 담당 파일만 대상으로 상태와 diff를 확인한다.
2. 관련 최소 검증을 실행하고 정확한 명령·통과·실패 수를 기록한다.
3. 담당 epic의 구현·검증 결과가 바뀌었다면 문서 버전과 변경 이력을 함께 갱신한다.
4. 자기 역할의 `handoffs/*.md`만 갱신한다.
5. `마지막 완료 동작`과 명령형의 `다음 한 동작`을 각각 하나로 적는다.
6. 사용자 승인·다른 역할 입력·알려진 실패를 분리한다.
7. 세 저장소의 branch·HEAD·dirty·untracked 경로를 기록한다.
8. 다른 컴퓨터로 옮길 예정이면 미커밋 파일의 전달 방식이 실제로 준비됐는지 확인한다.
9. commit/push 또는 외부 전송은 사용자 승인과 저장소 공개 범위를 확인한 뒤 수행한다.

handoff 작성 형식은 [HANDOFF-TEMPLATE.md](HANDOFF-TEMPLATE.md)를 따른다.

## 공통 금지 사항

- 이전 대화 기억만으로 작업 재개
- 다른 역할의 dirty 파일 덮어쓰기
- 사용자 파일을 `git reset`, `checkout`, `clean`으로 제거
- 완료 epic 수정
- spec이나 stable ID를 추측해 구현·아트 생성
- 사용자 검토 후보가 있는데 다음 후보 선행 생성
- review 파일을 app runtime 경로에 직접 복사
- `runtimeRegistrationAllowed` 수동 변경
- manifest 수동 편집
- 검증하지 않은 상태를 완료로 기록
- 로컬 미커밋 파일이 다른 컴퓨터에서도 존재한다고 가정

## 현재 고정 제작·공개 계약

역할별 가이드는 최신 spec을 다시 읽어야 하며, 아래 핵심 계약을 축소할 수 없다.

- 최신 Chrome·Edge, 마우스·키보드, `1920×1080`과 `1280×720`
- S0는 열쇠 선택 → 대문 열기 → 숯 점화의 무실패 3클릭
- 조립·그릴·드링크 조작 화면의 플레이어 신체 부위 0
- 아사노 아키와 츠키오카만 고정 인물, 나머지는 이름 없는 엑스트라
- 그릴은 명성에 따라 다음 영업일부터 `2→4→6→8`칸으로 해금되며 D1은 2칸으로 시작
- D1 첫 네기마 2개는 시작 2칸에 모두 배치된 뒤 동시에 조리 시작
- D1 첫 주문은 전체 단계 목록·현재 단계·다음 행동을 초보 보조 토글과 별개로 상시 표시
- 그릴에 닿은 한 면만 진행하고 그릴 밖·뒤집기 중에는 정지
- S0와 D1 최초 공개의 사용자 노출 placeholder 0
- D1 전체 영업·정산·저장·D2 전환과 S0를 완성한 뒤 최초 공개
- D2·D3는 기능 완성 시 명시적 placeholder를 허용해 순차 공개
- D4는 경고 뒤 진입하는 읽기 전용 개발 예고
- 오디오 후속, 원격 수집 없음, 저장 파일 내보내기·호환 복구 제공
