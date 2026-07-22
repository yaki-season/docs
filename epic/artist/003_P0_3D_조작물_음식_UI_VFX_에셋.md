# Artist - 003 P0 3D 조작물·음식·UI·VFX 에셋

- 문서 버전: `v2.0.0`
- 최종 변경일: `2026-07-22`
- 상태: `대기`
- 담당자: `Artist`

## 참조 spec

- `spec/art/ART-003_런타임_아트_에셋_목록과_제작_계약.md` - `ART-003` - `v2.0.0`
- `spec/gameplay/GPL-004_조리_스테이션과_품질_서빙.md` - `GPL-004` - `v1.0.0`
- `spec/ui/UI-002_전체_게임_화면_입력_접근성.md` - `UI-002` - `v2.0.0`
- `spec/system/SYS-003_전체_게임_런타임_저장_성능.md` - `SYS-003` - `v2.0.0`
- `spec/system/SYS-004_app_프로젝트와_소스코드_아키텍처.md` - `SYS-004` - `v2.0.0`

## 목표

바 안쪽 플레이어 작업 공간에서 네기마·모모·생맥주를 직접 조작하고 품질·경고·반응을 식별하는 데 필요한 모든 P0 GLB, 재질 텍스처, 음식 표시, UI glyph와 VFX atlas를 제작해 개발자가 임시 PNG·도형을 만들지 않고 장면에 연결할 수 있게 한다.

## 범위

### 포함

- GLB 10종: `MDL-SKEWER-BASE`, 닭, 대파, 석쇠, 집게, 부채, 타레 붓, 맥주잔, 레버, tray
- 텍스처 6군: 음식 base, char mask, tare mask, beer, tools atlas, blob shadow
- 음식 표시 5종: 네기마·모모·생맥주 주문 icon과 네기마·모모 plated visual
- UI 최소 33 glyph: 스테이션, 주문, 위험, 품질, 경제, 상태
- VFX 6군: 불씨, 연기, 기름, 판정, 맥주, 입력 피드백
- GLB node·pivot·socket·clip, atlas frame과 companion metadata
- triangle·texture·particle 예산 검수와 manifest·카탈로그 갱신

### 제외

- 후속 튀김·사시미·하이볼 모델과 계절 메뉴
- 게임 로직, 셰이더 프로그램과 VFX 재생 코드
- 좌석 번호·가격·수량·문구를 raster에 굽는 작업

## 작업 절차

1. 작업 001의 manifest와 작업 002가 `ART-003 v2.0.0`으로 재승인한 셰프측 소실점, 고정 카운터, `playerWorkBounds`와 `handoffPath`를 제작 기준으로 고정한다.
2. 기존 음식 PNG 4종을 형태·팔레트 참고로만 사용해 꼬치·닭·대파를 모듈식 GLB로 만든다.
3. 석쇠, 집게, 부채, 붓, 맥주잔, 단일 레버와 tray를 플레이어측 하단 작업 좌표에 맞춰 각 2,000 triangles 이하로 제작한다.
4. node 이름, origin, grip, socket, slot, 액체 volume과 interaction proxy를 `ART-003` 계약대로 지정한다.
5. 익힘·타레·맥주 셰이더에 필요한 sRGB base와 linear mask를 제작하고 면 방향·UV 이음새를 검수한다.
6. 네기마·모모·생맥주 주문 icon과 plated visual을 만들고 색상 없이도 실루엣으로 구분되는지 확인한다.
7. 33개 이상 UI glyph를 기능별 atlas로 만들고 16·24·32 CSS px 축소와 grayscale을 검수한다.
8. 6개 VFX군을 atlas·mask로 만들고 LOW 모드와 동시 particle 예산을 기록한다.
9. Blender·Aseprite 등 편집 원본, runtime GLB·atlas, companion metadata와 provenance를 분리 저장한다.
10. 모든 ID·실제 경로·해시·node·clip·anchor·예산을 manifest와 카탈로그에 동기화한다.

## 의존성과 인계 조건

- 선행 작업: Artist 작업 001 완료, 작업 002의 조명·소실점·anchor 기준 제공
- 후속 작업: 개발자의 P0 Three.js 오브젝트·셰이더·HUD·VFX 연결, Artist 작업 007
- 다른 역할에게 제공할 입력·출력: 개발자에게 GLB node·pivot·socket, texture 색 공간, atlas frame, UI glyph와 VFX cue별 runtime ID를 인계한다.

## 완료 기준

- [ ] GLB 10종이 올바른 node·pivot·proxy와 개당 2,000 triangles 이하 예산을 가진다.
- [ ] 네기마와 모모를 공통 닭·대파·꼬치 모듈로 조합할 수 있다.
- [ ] 익힘 앞·뒷면, 타레와 맥주·거품 표현에 필요한 base·mask·UV가 존재한다.
- [ ] 주문·품질·경고·경제·상태 UI glyph 최소 33종이 16px에서도 식별된다.
- [ ] VFX 6군이 풀스크린 효과 없이 particle·overdraw 예산을 만족한다.
- [ ] 기존 음식 PNG 4종은 placeholder 상태로 남고 운영 콘텐츠가 최종 GLB ID를 사용한다.
- [ ] 모든 산출물의 runtime/source/provenance·metadata 경로와 SHA-256이 manifest·카탈로그에 기록된다.
- [ ] desktop·mobile·LOW 합성 캡처와 triangle·texture·atlas 검증 결과가 남는다.
- [ ] 모든 GLB·VFX 합성이 고정 카운터 앞 플레이어측 작업 bounds에 접지하고 손님·주문 매트와의 전달 방향을 뒤집지 않는다.

## 구현 및 검증 결과

- app 구현 위치: 미구현. 예정 pack `app/public/assets/core/models/`, `textures/`, `ui/`, `vfx/`
- 구현 기준 spec 버전: `ART-003 v2.0.0`, `GPL-004 v1.0.0`, `UI-002 v2.0.0`, `SYS-003 v2.0.0`, `SYS-004 v2.0.0`
- 구현 기준 태스크 버전: `v2.0.0`
- 검증 방법: GLB 구조·triangle·texture·atlas 자동 검사와 실제 Three.js 합성 검수
- 검증 결과: 미실행
- 남은 위험: 단일 레버 생맥주 기본안이 변경되면 레버·잔 interaction metadata의 재작업이 필요하다.

## 변경 이력

| 이전 버전 | 새 버전 | 날짜 | 변경 유형 | 근거 spec 버전 | 변경 내용 | 재작업 영향 |
|---|---|---|---|---|---|---|
| 없음 | `v1.0.0` | `2026-07-22` | 최초 생성 | `ART-003 v1.1.0` 및 조리·UI·시스템 spec | P0 3D 조작물·음식·UI·VFX 제작과 개발자 인계 작업 생성 | 없음 |
| `v1.0.0` | `v2.0.0` | `2026-07-22` | 핵심 제작 좌표 변경 | `ART-003/UI-002/SYS-003/SYS-004 v2.0.0` | 모든 P0 3D 조작물과 VFX를 셰프측 카메라의 플레이어 작업 bounds 및 고정 카운터 전달 경로에 맞추도록 변경 | 작업 002의 교정 anchor 승인 뒤 제작 시작; 기존 좌표 가정 사용 금지 |
