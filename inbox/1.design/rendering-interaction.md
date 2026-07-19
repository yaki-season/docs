---
id: TECH-REND-001
title: 렌더링 & 인터랙션 (영민 담당 영역)
status: draft
owner: 영민
reviewer: 파트너          # 게임플레이 수치 변경 시에만 상호 리뷰
depends_on: [TECH-PERF-001]
sources: [inbox/ideas/yakitori-gdd-v0.1.md, inbox/ideas/25d-first-person.md]
last_updated: 2026-07-17
---

# 렌더링 & 인터랙션 스펙 — 영민 담당 정의서

## 목적 (Why)

이 문서는 **영민이 만들어야 하는 것의 전체 목록이자 경계선**이다.
파트너 담당(게임 시스템 로직·데이터·인프라)과의 인터페이스를 명시해,
서로의 AI 세션이 상대 영역의 코드를 지어내지 않게 한다.

> 담당 원칙: **화면에 보이는 것, 손에 느껴지는 것 = 영민.**
> 숫자가 굴러가는 것, 배포되는 것 = 파트너.

---

## 1. 씬 구조 — 3계층 2.5D

```
[Layer 0] 배경 (Background)
  - 텍스처 쿼드 2~3장 슬라이스: 뒷벽 / 선반 / 카운터 앞면
  - AI 생성 이미지, 라이팅 무드(주황 숯불 vs 남색 저녁)는 이미지에 베이크
  - 스테이션 전환 시 슬라이스별 이동 비율 차등 → 패럴랙스
  - 화면 가장자리 전경 프레임(노렌 자락/제등 실루엣) 알파 쿼드 1장

[Layer 1] 인터랙티브 메시 (진짜 3D — "손맛" 담당)
  - 석쇠, 꼬치(재료별 서브메시), 맥주잔+탭, 도구(집게/붓)
  - 개당 ≤ 2,000 tri, 텍스처 ≤ 1024²

[Layer 2] 이펙트
  - 숯불 불씨(InstancedMesh), 연기/김(쿼드+UV스크롤), 판정 연출, 글로우 스프라이트
```

카메라: **고정 앵글 1인칭.** 스테이션당 1개 프리셋(그릴/드링크/카운터 = 3개).
전환은 스냅(0.25s ease). 자유 시점 없음 — 이 전제가 깨지면 배경/성능 설계 전부 무효.

## 2. 셰이더 (GLSL) — 핵심 딜리버러블

### 2.1 익힘 셰이더 (꼬치/재료 공용) ★최우선

- 유니폼: `uDoneness` (0=날것 ~ 1=탄 상태), `uTareAmount` (0~1)
- 구현: 베이스 텍스처 → 익힘 색 lerp + 그을음 마스크(노이즈 텍스처, doneness 임계로 확장)
  + 타레 스페큘러(uTareAmount에 비례한 specular/광택)
- **면별 독립**: 앞/뒤면 doneness 분리 (뒤집기 판정과 연동)
- 값의 소유권: uDoneness의 진행 계산은 파트너의 시스템 로직.
  영민은 그 값을 받아 그리는 쪽. (인터페이스 §5 참조)

### 2.2 맥주 셰이더

- 잔 내부 액체 높이 + 거품 층 경계 표현 (7:3 판정의 시각화)
- 기울기에 따른 수면 각도 (셰이더 or 메시 변형, 프로토타입에서 결정)

### 2.3 배경/연출 보조

- 연기 UV 스크롤 셰이더, 숯불 발광 플리커(sin 노이즈), 글로우 스프라이트(가짜 블룸)

## 3. VFX

| 이펙트 | 방식 | 트리거 |
|---|---|---|
| 숯불 불씨 | InstancedMesh ≤80, 위로 상승+흔들림 | 상시 (화력에 비례) |
| 연기 | 쿼드 ≤6 + UV 스크롤 | 굽는 중 |
| 기름 튐 | 버스트 ≤30, 수명 0.5s | 뒤집기 순간 |
| Perfect 판정 | 링 확산 + 파티클 버스트 | 판정 이벤트 수신 |
| 김 (맥주) | 작은 쿼드 스크롤 | 따르기 완료 |

## 4. 인터랙션 (입력 → 3D)

### 4.1 인터랙션 평면 방식

- 스테이션마다 `InteractionPlane` 정의 (그릴=석쇠 면, 드링크=잔 수직면)
- 입력(터치/마우스) → raycaster → 평면 교차점 → **평면 로컬 2D 좌표로 변환**
  → 이 2D 좌표만 게임 로직에 전달 (로직은 3D를 모른다)

### 4.2 제스처 판정 (영민 구현, 수치는 스펙 승인 대상)

| 제스처 | 인식 규칙 (초안) | 사용처 |
|---|---|---|
| 드래그&드롭 | down→move→up, 드롭 존 판정 | 꼬치 올리기/서빙 |
| 스와이프 | 이동 ≥ 평면 폭 30%, 시간 ≤ 300ms, 수평 성분 우세 | 뒤집기 |
| 경로 드래그 | 샘플링 좌표열 → 커버리지 % 계산 | 타레 바르기 |
| 홀드+각도 | down 유지 중 y축 이동량 → 기울기 각 매핑 | 맥주 따르기 |

### 4.3 게임필 (수치로 관리)

- 입력 반응 시각 피드백 ≤ 50ms (하이라이트/스케일 1.05)
- 드래그 추적 지연: 도구가 커서를 lerp 계수 0.3~0.5로 따라옴 (즉시 부착 금지 — 무게감)
- 판정 이벤트 → 화면 반응(이펙트+사운드 훅) ≤ 100ms

## 5. 파트너와의 인터페이스 (경계 계약)

시스템 로직(파트너)과 렌더러(영민)는 **이벤트/상태 객체로만** 통신한다.

```typescript
// 로직 → 렌더러 (매 프레임 상태)
interface RenderState {
  skewers: { id: string; doneness: [number, number]; tare: number;
             onGrill: boolean; flipped: boolean }[];
  beer: { fillLevel: number; foamLevel: number; tiltAngle: number } | null;
  heatLevel: number;            // 숯불 화력 0~1
  activeStation: 'grill' | 'drink' | 'counter';
}

// 로직 → 렌더러 (단발 이벤트)
type GameEvent =
  | { type: 'JUDGEMENT'; grade: 'perfect'|'good'|'ok'|'fail'; worldPos: Vec3 }
  | { type: 'FLIP'; skewerID: string }
  | { type: 'SERVE_DONE'; skewerID: string };

// 렌더러 → 로직 (입력)
type InputEvent =
  | { type: 'DRAG'; station: string; planeUV: [number, number]; phase: 'start'|'move'|'end' }
  | { type: 'SWIPE'; station: string; direction: 'left'|'right' }
  | { type: 'HOLD_TILT'; station: string; angle: number };
```

이 인터페이스의 변경은 양측 승인 필요 (status: approved 절차).

## 6. 담당 밖 (Non-Goals — 영민이 만들지 않는 것)

- 손님 FSM / 대기 게이지 감소 로직 / 만족도·판정 등급 계산 / 정산·경제
- 메뉴·손님 데이터 스키마 및 로더
- GitHub Actions / Pages 배포
- (사전 과제 스코프 밖) 튀김·사시미 스테이션, 직원, 단골 스토리

## 완료 조건 (Acceptance Criteria)

- [ ] 3계층 씬 + 스테이션 3앵글 스냅 전환 동작
- [ ] 익힘 셰이더: doneness 0→1 연속 변화 + 면별 분리 + 타레 광택, 실기기 확인
- [ ] 제스처 4종이 §5 InputEvent 형식으로 발행되고, 파트너의 HTML 검증 데모와
      동일 입력 → 동일 이벤트 (패리티 테스트)
- [ ] Perfect 판정 연출이 이벤트 수신 후 100ms 내 재생
- [ ] TECH-PERF-001 예산 전 항목 통과 상태 유지

## 열린 질문 (Open Questions)

- [ ] 맥주 수면 표현: 셰이더 vs 메시 변형 — 1주차 프로토타입에서 비교
- [ ] 뒤집기를 스와이프 방향과 메시 회전 방향 동기화할지, 고정 연출로 갈지
- [ ] 배경 슬라이스 3장 vs 2장 — 아트 생성 결과 보고 결정
