# epic

`spec/`을 기반으로 생성된 PM·개발자·Artist·통합 QA별 작업 문서를 관리한다.

전체 작업 수, 각 개발자의 현재 작업과 완료 작업은 `작업_현황.md`에서 관리한다.

## 기본 분담

- `pm-orchestration/`: S0·D1·D2·D3/D4 stage 임계경로, 역할 서브에이전트 자동 인계,
  사용자 승인과 사람 화면 테스트 gate
- `developer-1/`: 핵심 시스템, 게임플레이, 런타임 구조 중심
- `developer-2/`: UI, 콘텐츠, 시나리오, 아트/오디오 연동 중심
- `developer-3/`: 밸런스 데이터·로더·검증과 개발·기획용 도구(튜닝 UI·로직 시뮬레이터)
- `qa-release/`: 세 저장소 통합 기준선, 회귀, 에셋 handoff와 공개 gate 검증
- `artist/`: Artist 1·2·3의 아트 제작, 검수, runtime handoff와 경로 카탈로그 관리

### Artist 3인 기본 분담

- Artist 1: D1 조립·그릴 음식 모델, 면별 shader 상태와 해당 소비 화면 finalizer
- Artist 2: S0 열쇠·대문·아키 이야기 초상과 후속 이야기·메타 화면
- Artist 3: D1 드링크·서빙·좌석 정리·엑스트라·정산과 후속 D2~D3 서비스 화면

세 역할은 같은 `artist/` 디렉터리를 사용하되 태스크마다 서로 다른
`art-workspace/review/artist-NNN/` namespace와 단일 `semanticOwner`를 가진다. 다른 Artist의
승인 source는 복사·수정하지 않고 읽기 전용 합성 입력으로 사용한다.

각 태스크 문서는 순번을 포함한 파일명으로 작성한다.

예시:

- `developer-1/001_작업명.md`
- `developer-2/001_작업명.md`
- `artist/001_작업명.md`
- `pm-orchestration/001_스테이지_작업명.md`

## 작업 운영

- 상태는 `대기`, `진행 중`, `완료`, `보류`, `변경 확인 필요` 중 하나로 기록한다.
- 새 작업은 `template/epic_작업_템플릿.md`를 사용한다.
- 문서 이력·버전은 git이 단일 원본이므로 본문에 수동 버전·변경 이력을 유지하지 않는다.
- `spec/` 변경 시 `AGENTS.md`에 따라 대기·진행 중 작업을 갱신하고 필요한 후속 작업을 생성한다.
- 태스크 생성·상태 변경·담당 변경·삭제 시 `작업_현황.md`를 같은 변경에서 갱신한다.
- 실제 구현과 운영은 `app` 저장소에서만 수행한다.

## 스테이지 운영

- PM stage 태스크는 역할별 구현 epic을 대체하지 않고 producer→consumer 의존성과 완료 증거를 묶는다.
- S0와 D1은 충돌 없는 lane을 병렬 실행하고 두 작업 모두 사람 `PASS`를 받은 뒤 최초 공개한다.
- D2는 D1 뒤, D3는 D2 뒤 순차 실행한다.
- 각 후보는 사용자 승인 → Artist finalizer → Developer 2 promotion/binding → gameplay 회귀 →
  QA → 사람 화면 테스트 순으로 인계한다.
- 사람 화면 피드백은 `template/사람_화면_플레이테스트_피드백_템플릿.md`를 사용한다.

## Artist 인계 원칙

- 편집 원본·검토본·출처·증빙은 비Git 로컬 작업공간
  `art-workspace/source/`, `art-workspace/review/`, `art-workspace/provenance/`,
  `art-workspace/evidence/`로 분리한다.
- 제작·검수 상태의 사람용 인덱스는 `art-workspace/ASSET-CATALOG.md`다.
- 승인된 runtime 파일은 complete layer 재조립·최적화까지 통과한 뒤 `app`의
  명시적 write 승격 게이트로만 `app/public/assets/` pack에 추가한다.
- 런타임 단일 원본은 `app/public/assets/manifest.json`이며 `app`에는 `art/`와 `captures/`를 만들지 않는다.
- Artist는 에셋을 생성·수정·폐기한 작업에서 로컬 카탈로그를 갱신하고 실제 파일·해시·상태와 자동 대조한다.
- 개발자는 파일명을 추측하거나 재명명하지 않고 에셋 ID와 카탈로그 경로를 사용한다.
