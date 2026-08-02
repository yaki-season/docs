# 2026-08-01 D1·S0 아트 제작 패킷 (v9 holistic one-pass)

- 작성: PM support agent
- v9 보강(2026-08-02): 20개 생성 대상(D1 19 + `CH-AKI-STORY`)에 **화면 내 구도·기존 화면과의 화합·의도·톤앤매너·one-shot 프롬프트** 5필드 추가. codex imagegen이 한 번에 화면에 붙게 하는 것이 목표. `ST-S0-BRAZIER`·`PR-CHARCOAL-IGNITION`은 2026-08-02 S0 점화 대사 대체로 DEPRECATED(생성하지 않음).
- 소비 화면별 그룹: **A `SCR-SVC-CUSTOMERS`(10)** · **B `SCR-SVC-DRINK`(4)** · **C `SCR-POST-CLOSING`/그릴 계열(2)** · **D `SCR-POST-SETTLEMENT`/UI 아틀라스(3)** · **E `SCR-STORY-PROLOGUE`/`SCR-STORY-BEAT`(1)**. 한 그룹을 함께 생성하면 화합이 최대화된다.
- **공통 화합 앵커(모든 그룹이 상속하는 하우스 룩)**: 승인 런타임 배경/카운터/츠키오카 스프라이트 + 승인 후보 그릴/조립 배경이 공유하는 팔레트 = 월넛/번트오렌지 목재 + 낡은 황동 + 따뜻한 호박색 제등광 대 짙은 인디고/네이비 밤, 주홍 종이 제등 악센트, 이끼색 화분, 크림 종이. 배경은 대칭 정면 elevation·젖은 돌바닥 1~2점 소실점, 작업면은 셰프 고정 시점(뒷벽 정면 + 하단 1/3 상판 살짝 top-bevel). 인물은 정면·고립·부드러운 정면광 storybook.
- 공통 합성 z-order(UI-003 분리 묶음): `background < architecture < actors < interactables < state-overlays < vfx < foreground < ui-art < DOM(ui.semantic)`. 기준 캔버스 `1920×1080` contain, `1280×720`=2/3 축소.
- 대상: NOT-STARTED 아트 자산 22종 (D1 19 + S0 3; 이 중 생성 대상 20, DEPRECATED 2)
- 계약: `art-workspace/pipeline/pipeline-contract.json` `YS-ASSET-PIPELINE-v8`
- 생성 규약: **자산 1개 = 프롬프트 1개**. 한 프롬프트는 한 자산의 한 상태만 생성한다(`generationRule.onePromptPerAsset=true`, 통짜 화면 마스터 폐기).
- 공통 스타일 토큰: `YS-HANDCRAFTED-NIGHT-v1` (ART-002). 팔레트 amber·burnt orange·deep indigo/navy·dark walnut·cream paper·aged brass·moss·burgundy. 손그림 연필 밑그림·불규칙 잉크·불투명 과슈 색면·종이결·형태 설명형 픽셀 군집. 매끈 플라스틱·글로시 PBR·풀스크린 tint·사진사실 금지.
- 공통 금지: 의미 문자·숫자·게이지·주문서 문구·좌석 번호·커서·손가락 그림·워터마크·서명·콘셉트시트 패널을 굽지 않는다. 실시간 수치/게이지/focus는 DOM UI 계층이 소유한다.
- 공통 수용(모든 패킷 동일, 각 절에 요약): gate-1 단일 자산 생성 + checkerboard 격리 검수 + source/output SHA-256(사용자 승인) → gate-2 이미 개별 승인된 자산만 소비 화면에 `1920×1080 FHD`+`1280×720` 재조립, anchor·occlusion·style 회귀·loss 정책·성능 예산 확인(사용자 소비-화면 최종 승인) → gate-3 최종 승인 시 finalizer가 `runtimeRegistrationAllowed=true` handoff 파생·원자적 등록(자동). provenance/profile report의 `runtimeRegistrationAllowed`는 항상 `false`, 사람이 편집 금지.

근거: ART-003(계약 행), UI-003(소비 screenId/componentId·프레임), ART-002(스타일), pipeline-contract.json/README(v8 3-게이트).

---

## D1 자산 (19)

## 그룹 A · `SCR-SVC-CUSTOMERS` (10) — 손님 정보·서빙·정리 화면

> 함께 생성 권장. 공통 화합 앵커: 승인 런타임 `ARTIST-010-BACKGROUND-COMPLETE`(`/assets/core/customer/background-complete-r3-b1.png`, 1920×1080, 대칭 정면·중앙 대문 1점 소실점·월넛벽·주홍 제등·인디고 밤골목), `BG-SERVICE-TABLE-ARTIST009`(`/assets/core/customer/service-table-complete-r1-b1.png`, 전폭 카운터·상단 4개 호박색 hotspot·번트오렌지→월넛 낙차), `D1-TSUKIOKA-WAITING/RECEIVED-EATING`(`/assets/core/customer/d1-tsukioka-*.png`, 정면 고립·부드러운 정면광 storybook 인물). 합성 순서(ART-003 L205): `0 배경 → 1 좌석+손님(하체 customerOcclusionLine 아래) → 2 바 카운터(하체 가림·음식 접지) → 3 테이블 음식·잔·빈식기 → 4 상태 UI·전경`. 화면 프레임(UI-003): receiptRail 미사용, `workScene x=104~1816 · y=112~848`, `preparedDock x=104~1816 · y=872~1040`, sideNav 좌 `x=24~88`·우 `x=1832~1896` `y=456~624`, `serviceStatus y=32~96`(DOM). 카메라: 바 안쪽 셰프 정면 고정 시점.

### 모든 손님 인물 공통 계약 — 츠키오카 기준 인체 공간·가구 분리

- **기준 인물**: `D1-TSUKIOKA-WAITING`의 정면 착석 구도와 사람 비율을 모든 성인 손님 이미지의 기준으로 삼는다. 가구를 제거한 R3 후보의 측정 계약은 `art-workspace/review/artist-025/d1-tsukioka/waiting/r3/metadata/character-scale-contract.json`에 둔다. R3 사용자 승인 전에는 runtime 등록하지 않되, 아래 생성 제약은 사용자의 최신 확정으로 즉시 적용한다.
- **고정 캔버스·카메라**: scene-coordinate `1920×1080`, `1280×720`은 nearest-neighbor 정확한 `2/3`; 바 안쪽 플레이어/셰프가 손님을 보는 고정 정면 카메라를 공유한다. 임의의 시점·원근·crop·캔버스 변경 금지.
- **기준 측정값**: 츠키오카 R3 FHD alpha bounds `x=957, y=215, w=258, h=690`, 중심 `x=1086`, head-top `y=215`, eye-line `y=305`, shoulder-line `y=366`, anatomical seat-plane `y=660`, foot-plane `y=905`. 실제 좌석별 배치는 이 사람 공간을 해당 `customers.seat[n]` anchor로 평행 이동한다.
- **사람 크기 수용 범위**: 일반 성인은 츠키오카 apparent seated scale의 `0.90~1.10`, eye-line `±24px`(720 `±16px`), shoulder-line `±32px`(720 `±21px`) 안에서만 나이·성별·체형·자세 차이를 허용한다. 같은 화면에 놓았을 때 축소 인형·거인·다른 카메라 인물처럼 보이면 실패다. 범위를 벗어난 의도적 체격은 별도 캐릭터 디자인 사용자 승인 없이는 생성하지 않는다.
- **레이어 소유권**: character layer는 사람 픽셀만 소유한다. 의자·스툴·벤치·등받이·좌판·쿠션·팔걸이·가구 기둥·다리·footrest는 `BG-SEATING-6`만 소유하며 모든 인물 이미지의 가구 픽셀 수는 `0`이어야 한다. 카운터 가림은 승인 서비스 카운터 전경이 소유한다.
- **착석 인물 생성 규칙**: 가구 없이도 츠키오카와 같은 착석 해부학·골반/무릎 각도·정면 자세를 유지한다. 인물 단독 checkerboard에서 허공에 앉아 보이는 것은 정상이며, 이를 고치려고 임의 의자·바닥·접촉 그림자·받침을 추가하지 않는다. 음식·음료·소품은 해당 상태가 명시적으로 소유할 때만 별도 layer/frame으로 허용한다.
- **필수 Gate-1/Gate-2 증거**: 인물 단독 checkerboard FHD/720, alpha bounds·눈높이·어깨선·seat/foot anchor 수치, 가구 픽셀 `0`, 그리고 `승인 배경 → BG-SEATING-6 → 인물 → 승인 카운터` 순서의 FHD/720 실제 소비 합성을 함께 검수한다. 츠키오카와 나란히 놓은 비율 비교가 어색하면 자동 검증이 통과해도 사용자 승인 요청에 올리지 않는다.

### BG-INTERIOR-BASE
- Owner: Artist 3 (service artist-025)
- Consumer: `SCR-SVC-CUSTOMERS` / `customers.scene` (background). 영업 합성 계약 순서 0 (ART-003 L204, UI-003 L342).
- Format & budget: `2048×1152` JPG/WebP 불투명 배경. profile `complete-layer`(FHD 장면 좌표). 현황 신규/재작업 (ART-003 L179).
- State id: spec: unspecified(공식 state id 없음) — 지원 변형은 「차가운 폐점 tint / 따뜻한 영업 tint + 중앙 대문 개폐」 (ART-003 L179, L204).
- Fixed constraints: 손님측 바·좌석 바로 뒤 화면 수평 중앙의 단일 대문·문틀·문짝·문턱, 개구부 너머 밤 골목, 대문 좌우는 화면 끝까지 막힌 측면 벽. 대문을 막힌 후면벽·전폭 병 선반·별실·두 번째 카운터로 치환 금지, 측면 벽에 개구부 금지(ART-003 L263). 손님에게 가려진 면도 대문틀·문짝·문턱·골목으로 연속(complete-layer 가려진 면 복원). zOrder 최하단. hudSafeRect·barCounterBounds는 이미지에 표시하지 않고 metadata로 측정. 스타일 토큰·팔레트, 문자/게이지 금지.
- 화면 내 구도: `customers.scene` 배경 layer, 합성 순서 0, zOrder 최하단(모든 것 뒤). 2048×1152 원본을 1920×1080 workScene(x=104~1816, y=112~848 이상 전체 프레임 채움)에 contain. 중앙 대문은 화면 수평 중앙(대략 x=960 중심), 손님 좌석 열 바로 뒤 눈높이. 좌석(order 1)·바 카운터(order 2)·테이블 음식(order 3)·전경 UI(order 4)가 순서대로 이 위에 겹침 — 하단 1/3은 카운터·좌석에 가려지므로 그 면도 대문틀·문턱·골목으로 연속 복원. 720p=2/3 축소 동일 배치.
- 기존 화면과의 화합: **최우선 매치 대상** 승인 런타임 `ARTIST-010-BACKGROUND-COMPLETE`(`/assets/core/customer/background-complete-r3-b1.png`) — 대칭 정면 눈높이, 중앙 대문을 향한 강한 1점 소실점(젖은 자갈 골목), 월넛/에스프레소 세로 널벽, 좌측 주홍(vermilion) 종이 제등 1개 + 황동 새장형 제등 2개, 상단걸이 제등의 하향 광 pool, 하단 바닥의 호박색 footlight, 대문 너머 슬레이트-블루 밤 거리·이끼색 화분·먼 상점 창불빛. 이 카메라 높이·소실점·팔레트·따뜻한 내부/차가운 외부 대비를 그대로 계승해 6개 D1 화면이 한 세계로 읽히게 한다. 톤 계열은 `BG-EXTERIOR-S0-CLOSED`(인디고-plum 밤하늘, 낡은 목재 storefront)와도 정합.
- 의도: 순수 배경 — 뒤로 물러나(한 단계 조용) 전경의 손님 얼굴·주문·테이블 음식이 최우선으로 읽히게 한다. 중앙 대문은 가게 출입 동선·공간 단서를 주되 시선을 뺏지 않는다. 실시간 수치/게이지 없음(DOM 소유).
- 톤앤매너(YS-HANDCRAFTED-NIGHT-v1): 연필 밑그림 + 불규칙 잉크 윤곽(계단형, 무안티앨리어싱) + 불투명 과슈 2~4단계 색면 + 종이결. 팔레트 월넛·번트오렌지·낡은 황동·호박 제등광 대 짙은 인디고/네이비 밤 + 주홍 제등 악센트 + 이끼색 화분. 비 갠 저녁 골목 무드, 젖은 돌바닥 반사. 매끈 PBR·풀스크린 주황 tint·사진사실 금지. 문자·게이지 없음(DOM). 불투명 배경(투명 없음).
- Generation prompt(one-shot): `Single opaque full-frame image, 2048×1152 (16:9), no transparency. YS-HANDCRAFTED-NIGHT-v1 hand-drawn 2.5D interior back wall of a small night yakitori shop, matching an existing approved interior: symmetric front eye-level view with ONE strong central one-point vanishing point down a wet-cobblestone night alley seen through ONE and only one central wooden double-door main entrance (frame, leaves, threshold). Solid walnut vertical-plank side walls extend to both screen edges left and right of the doorway. Palette to match: walnut/espresso plank walls, one vermilion paper lantern plus aged-brass caged lanterns casting downward warm-amber light pools, warm honey footlight on the wood floor, deep indigo/navy night street beyond with faint moss-green potted plants and distant lit shopfront windows — warm interior against cool indigo exterior, background one detail-step quieter. Pencil underdrawing, irregular ink, opaque gouache blocks, paper grain, stair-stepped edges, no anti-alias, worn wood and aged brass, restrained highlights (no glossy PBR, no full-screen orange tint, no photoreal). Lower third will be occluded by a counter/seats so keep it continuous door-frame/threshold/alley. No customers, no counter, no seats, no food, no tools, no UI, no text, no numbers, no gauge, no watermark, no concept panels. Never replace the entrance with a sealed rear wall, full-width bottle shelf, back room, or a second counter; never open the side walls.`
- Acceptance (v8): gate-1 격리 checkerboard(불투명 full-frame 모드, transparentPixels=0) + source/output SHA-256 → gate-2 FHD+720 손님 화면 재조립에서 카메라·대문 topology·style·성능 회귀 → gate-3 자동 등록.
- Prior art / notes: `CM-CUSTOMER-SERVICE-R1` topology만 승인됨(참조 전용, 런타임 미등록). 톤·구도 참고는 concept-masters PROMPTS.md `03 손님 정보`(deprecated 참고용).

### BG-SEATING-6
- Owner: Artist 3 (service artist-025)
- Consumer: `SCR-SVC-CUSTOMERS` / `customers.seat[n]` (+`customers.scene` architecture) (UI-003 L343).
- Format & budget: 투명 WebP/PNG + anchor JSON. profile `complete-layer`. 현황 신규/재작업 (ART-003 L181).
- State id: spec: unspecified — 좌석별 변형 「빈 좌석·예약·착석 anchor」 6개 (ART-003 L181; UI-003 seat 상태 빈/입장/착석/퇴장/정리).
- Fixed constraints: 카운터 뒤 등받이·빈·예약·착석 anchor 6개. 상판·플레이어측 스툴 제외. 캐릭터 하체가 `customerOcclusionLine` 아래로 내려가 카운터에 가려지는 접지 계약(합성 순서 1, ART-003 L205). anchor 6개는 겹쳐도 얼굴·대표색·주문 손동작 읽힘(ART-003 L419). anchor 값은 이미지에 표시하지 않고 companion JSON. 스타일 토큰, 문자 금지.
- 화면 내 구도: `customers.seat[n]` architecture layer, 합성 순서 1(배경 위, 바 카운터 아래). 6개 좌석 등받이가 workScene(x=104~1816, y=112~848) 안에서 균등 배치, 중앙 대문 앞 카운터 뒤 눈높이. 각 좌석의 하단(스툴 접지)은 y≈customerOcclusionLine(카운터 상단 근처)에서 잘려 order 2 바 카운터에 가려짐 — 손님 하체 접지와 동일 라인 공유. anchor 6개(빈/예약/착석)는 이미지에 표시하지 않고 companion JSON. zOrder: 손님(actor)과 같은 order지만 좌석은 인물 뒤.
- 기존 화면과의 화합: `ARTIST-010-BACKGROUND-COMPLETE` 위에 놓일 것이므로 그 배경의 눈높이·1점 소실점·월넛 목재·황동 톤·따뜻한 정면광을 계승. 등받이 목재는 배경 벽널과 같은 월넛/에스프레소 grain·낡은 모서리. 착석 인물은 `D1-TSUKIOKA-WAITING`(정면 고립·부드러운 정면광·이끼색 카디건 대비)과 같은 눈높이·scale·정면 정보 시점을 공유해야 좌석에 자연스럽게 앉는다. P1 8·12석과 광원·scale·pivot 공유.
- 의도: 빈/예약/착석 좌석 상태를 좌석별로 명확히 구분하는 정보용 architecture. 배경보다 살짝 앞으로 읽히되 손님 얼굴·주문을 방해하지 않게 조용. 좌석 번호·상태 문자는 DOM.
- 톤앤매너(YS-HANDCRAFTED-NIGHT-v1): 연필+잉크+과슈, 종이결, 닳은 월넛 목재 등받이, 호박색 실내광·인디고 그림자 2~4단계. 매끈 PBR 금지. alpha-cutout 투명(네 모서리 투명). 문자 없음.
- Generation prompt(one-shot): `Single asset only, transparent background (alpha-cutout, transparent corners). YS-HANDCRAFTED-NIGHT-v1 hand-drawn row of six empty customer-side bar seats (backrests/slatted wooden stools behind the counter) for a small night yakitori shop, straight-on front information view at counter eye level, evenly spaced for six positions, unoccupied. Match an existing approved interior: worn walnut/espresso wood with the same grain and aged edges as the back wall, warm amber interior key light from above, indigo shadow. The bottom of each stool will be occluded by a counter — keep grounding consistent along one horizontal occlusion line. Pencil underdrawing, irregular ink, opaque gouache (2–4 value steps), paper grain, stair-stepped edges, no anti-alias, restrained highlights (no glossy PBR). No customers, no counter top, no player-side stool row in front, no food, no UI, no seat numbers, no text, no numbers, no watermark.`
- Acceptance (v8): gate-1 격리 checkerboard(alpha-cutout, 네 모서리 투명) + SHA-256 → gate-2 FHD+720에서 6 anchor·occlusion line·머리 겹침 회귀 → gate-3 자동 등록.
- Prior art / notes: anchor JSON은 승인 마스터/분리본에서 측정. P1 8·12석(`BG-INT-SEATS-8/12`)과 광원·scale·pivot 공유 필요(ART-003 L465-466).

### BG-BAR-COUNTER-BASE
- Owner: Artist 3 (service artist-025)
- Consumer: `SCR-SVC-CUSTOMERS` / `customers.scene`(architecture) · `customers.table[n]` 접지면 (합성 순서 2, ART-003 L206; UI-003 L342·L346).
- Format & budget: 투명 WebP/PNG + anchor JSON. profile `complete-layer`. 현황 신규 (ART-003 L182).
- State id: spec: unspecified(정적 고정 상판). anchor 세트: 주문 mat·접시·잔 전달 anchor.
- Fixed constraints: 고정 바 카운터 상판, 손님측·플레이어측 모서리. 캐릭터 하체를 가리고 손님 앞 음식·음료·빈 식기 접지면 제공. 단일 바 테이블만(두 번째 카운터 금지). anchor(customerOcclusionLine 위 접지)·barCounterBounds는 companion JSON. 스타일 토큰, 문자 금지.
- 화면 내 구도: `customers.scene`(architecture) 접지면, 합성 순서 2 — 좌석·손님(order 1) 위에 겹쳐 캐릭터 하체를 가리고(customerOcclusionLine), 그 위 order 3 테이블 음식·잔·빈식기(FD-*-PLATED, MDL-BEER-GLASS, PR-EMPTY-DISH-SET)의 접지면을 제공. 전폭 수평 카운터가 workScene 하단(대략 y=620~848 대)을 가로지르며 살짝 top-bevel로 상판 sliver 노출. barCounterBounds·주문 mat/접시/잔 전달 anchor는 companion JSON.
- 기존 화면과의 화합: **직접 매치** 승인 런타임 `BG-SERVICE-TABLE-ARTIST009`(`/assets/core/customer/service-table-complete-r1-b1.png`) 및 승인 후보 `complete-layers/table/r1`(`service-table-complete-fhd-r1.png`) — 전폭 수평 woven/burled 월넛 상판, 상단 lip 위 4개 균등 호박색 hotspot이 앞 모서리를 skim, 번트오렌지/honey 표면이 월넛 그림자로 낙차, 상단 프레임은 투명/검정(배경 위 합성용 전경 ledge). 이 상판 grain·황동 trim·정면 elevation·top-bevel·광 hotspot을 그대로 계승. 참고 다크 UI hex(패널) `#24242c`/`#34343e`는 굽지 않음(DOM chrome).
- 의도: 손님 하체를 가려 접지감을 만들고 음식·음료가 앉을 안정된 표면을 제공. 정적 고정 상판이라 상태 변화 없음 — 그 위 음식/손님이 정보 주역. 주문 mat 표식은 동적 decal/DOM.
- 톤앤매너(YS-HANDCRAFTED-NIGHT-v1): 연필+잉크+과슈, 종이결, 촘촘한 woven 목재결 + 낡은 황동 trim, 상단 호박 hotspot·앞면 인디고 그림자. semi-gloss지만 매끈 PBR 아님(수작업 specular 막대). alpha-cutout 투명. 문자 없음.
- Generation prompt(one-shot): `Single asset only, transparent background (alpha-cutout, transparent corners), upper frame fully transparent. YS-HANDCRAFTED-NIGHT-v1 hand-drawn single fixed bar counter top of a small night yakitori shop, straight-on front elevation matching an existing approved counter: one and only one full-width horizontal counter with customer-side near edge and player-side far edge, a slight top bevel exposing a sliver of the tabletop plane, sitting low as a foreground ledge. Densely woven/burled walnut wood surface with aged brass trim, four evenly spaced warm-amber hotspots skimming the front lip, burnt-orange/honey top dropping into walnut shadow, indigo front face. Pencil-ink-gouache, paper grain, stair-stepped edges, no anti-alias, hand-painted specular (no glossy PBR). This layer occludes the seated customers' lower bodies — keep the near edge opaque and continuous. Empty top: no food, no glass, no dishes, no customers, no tools, no UI, no order mats printed, no text, no numbers, no watermark. Not a second counter, not a bottle shelf.`
- Acceptance (v8): gate-1 격리 checkerboard(alpha-cutout) + SHA-256 → gate-2 FHD+720에서 하체 가림·접지 anchor·style 회귀 → gate-3 자동 등록.
- Prior art / notes: 합성 순서상 캐릭터/좌석 뒤·음식 앞. anchor는 측정값으로 확정.

### ST-SERVICE-COUNTER
- Owner: Artist 3 (service artist-025)
- Consumer: `SCR-SVC-CUSTOMERS` / `customers.preparedDock`(플레이어측 서빙 모듈·전달 tray·완성품 대기) (ART-003 L191; UI-003 L348).
- Format & budget: 투명 PNG/WebP. profile `complete-layer`(architecture). 현황 `ST-04` 재사용 후보(FHD 회귀 통과 시 이전) (ART-003 L191, L124).
- State id: spec: unspecified(정적 architecture).
- Fixed constraints: 고정 상판과 중복되지 않는 전달 tray·완성품 대기 영역. 스타일 토큰. 문자·수량 표식 금지. anchor metadata 포함.
- 화면 내 구도: `customers.preparedDock` 플레이어측 서빙 모듈. preparedDock 프레임(x=104~1816, y=872~1040) 하단 대역에 놓이는 architecture — 고정 바 카운터(order 2)보다 플레이어(하단)에 가까운 전달/대기 영역. 완성품 카드(DOM)와 전달 tray(MDL-SERVICE-TRAY)가 이 위에 앉는다. 고정 상판과 시각적으로 겹치지 않게 분리. anchor metadata.
- 기존 화면과의 화합: 같은 화면의 `BG-BAR-COUNTER-BASE`/`BG-SERVICE-TABLE-ARTIST009` 상판과 같은 월넛 woven 목재결·낡은 황동 trim·호박 task light를 공유하되 별도 모듈로 읽히게 한 단계 낮은/앞쪽 ledge. `ST-04` 시각 참고만(스타일 회귀 통과 시 이전). MDL-SERVICE-TRAY(전달 tray GLB)와 재질 계열 일치하되 역할(고정 영역 vs 이동 tray) 분리.
- 의도: 플레이어가 완성품을 얹어 손님에게 전달하는 대기 영역을 명확히 표시. 고정 카운터와 혼동되지 않아야 서빙 동선이 읽힘. 수량/주문 ID는 DOM.
- 톤앤매너(YS-HANDCRAFTED-NIGHT-v1): 연필+잉크+과슈, 종이결, 닳은 월넛 목재+황동, 호박 task light·인디고 그림자. 매끈 PBR 금지. alpha-cutout 투명. 문자·수량 표식 없음.
- Generation prompt(one-shot): `Single asset only, transparent background (alpha-cutout). YS-HANDCRAFTED-NIGHT-v1 hand-drawn player-side serving module for a night yakitori shop — a small hand-off ledge / completed-item waiting area that reads as clearly SEPARATE from the main fixed counter top, sitting nearer the player (lower) edge. Match the house counter material: worn woven walnut wood and aged brass trim, warm amber task light, indigo shadow, restrained highlights (no glossy PBR). Pencil-ink-gouache, paper grain, stair-stepped edges, no anti-alias. Empty: no food, no plates, no glasses, no customers, no tray items, no UI, no text, no numbers, no watermark.`
- Acceptance (v8): gate-1 격리 checkerboard(alpha-cutout) + SHA-256 → gate-2 FHD+720 서빙 화면 재조립·anchor·style·성능 → gate-3 자동 등록.
- Prior art / notes: `ST-04`는 시각 참고만 허용, 스타일 회귀 미통과 시 ID 유지·파일 교체(ART-003 L716). 전달 tray는 GLB `MDL-SERVICE-TRAY`와 역할 중복되지 않게 분리.

### CH-EXTRA-COMMUTER-SERVICE
- Owner: Artist 3 (service artist-025)
- Consumer: `SCR-SVC-CUSTOMERS` / `customers.actor[n]` (actor atlas) (UI-003 L344, L180).
- Format & budget: service 공통 16 clip atlas. `128×160` 논리 canvas 4배 nearest export 또는 동일 밀도 atlas. profile `bundle-model`(atlas+companion). 현황 신규 (ART-003 L412, L416).
- State id: 16 clip = `enter·considering·order-ready·waiting·urgent·receiving·tasting·eating·drinking·satisfied·disappointed·angry·mismatch·retry·checkout·leave` (ART-003 L390-407). 식별 축: 빠른 주문·안도.
- Fixed constraints: 이름 없는 통근객 엑스트라 — 고정 인물 이름·서사·회상 금지(ART-003 L412). 위 `츠키오카 기준 인체 공간·가구 분리` 계약의 캔버스·정면 카메라·사람 비율·눈높이·좌석 scale을 공유하고 character layer 가구 픽셀은 `0`이어야 한다. 감정은 얼굴·고개·손·자세 중 2개 이상으로 구분, 색 tint만 변경 금지(L418). 색각/grayscale/LOW 모드 유지. 스타일 토큰. 문자·주문 숫자 굽지 않음.
- 화면 내 구도: `customers.actor[n]` actor layer, 합성 순서 1(좌석과 함께 배경 위). 하단 중앙 pivot, 좌석 6 anchor 중 하나에 착석. 하체가 customerOcclusionLine 아래로 내려가 order 2 바 카운터에 가려짐(상반신·얼굴·손·주문 손동작만 카운터 위로 노출). 좌석 scale·눈높이 공유. workScene(y=112~848) 안, 얼굴은 카운터 위 상단부.
- 기존 화면과의 화합: **자세·눈높이·정면광 매치** 승인 런타임 `D1-TSUKIOKA-WAITING/RECEIVED-EATING`(`/assets/core/customer/d1-tsukioka-*.png`) — 정면 고립 스프라이트, 부드러운 정면 key(좌상단 따뜻)·오른쪽 미세 cool shade, 저대비 storybook, 착석 자세, 손을 입으로/잔을 드는 동작. 통근객은 이 눈높이·좌석 scale·정면 정보 시점·정면광을 그대로 계승하되 네이비·호박 대표색(느슨한 넥타이·가방)으로 츠키오카(이끼색)와 실루엣 구분. 배경 `ARTIST-010-BACKGROUND-COMPLETE`의 따뜻한 팔레트 위에 앉도록 warm/cool 균형 동일.
- 의도: 러시 중 한눈에 손님 유형·감정·주문 상태가 읽혀야 함. 16 clip 감정은 얼굴·고개·손·자세 중 2개 이상으로 구분(색 tint만 변경 금지), LOW/색각/grayscale 유지. 겹쳐도 얼굴·대표색·주문 손동작 판독.
- 톤앤매너(YS-HANDCRAFTED-NIGHT-v1): 연필+잉크+과슈, 표시 크기 형태 설명형 픽셀 군집, 종이결. 호박 실내광·인디고 그림자, 네이비/호박 대표색. 반복 얼굴·플라스틱 피부·손 접촉 오류 금지. alpha-cutout 투명, 동일 canvas/pivot/조명. 문자·주문 숫자 없음.
- Generation prompt(대표 1 clip, 나머지 clip은 같은 규약으로 각 1프롬프트): `Single asset only, one clip frame set of ONE character on transparent background (alpha-cutout, bottom-center pivot), person pixels only. YS-HANDCRAFTED-NIGHT-v1 hand-drawn SEATED night-yakitori-shop customer sprite, using D1 Tsukioka as the canonical 1920×1080 front-camera human-scale reference: same seat plane, apparent adult body scale 0.90–1.10, eye line within ±24 FHD px, shoulder line within ±32 FHD px, soft front key light from upper-left, low-contrast storybook. An unnamed tired commuter, quick-order personality, read by hair/build/clothing and a distinct navy/amber accent color (loose necktie, bag) — not a skin-swap, silhouette distinct from a moss-green regular. Clip "<CLIP>" pose only (e.g. waiting); emotion carried by at least two of face/head/hands/posture, not color alone. Preserve anatomically seated hips/knees even though no furniture is present; lower body will be occluded by the separate counter layer. Warm amber light, indigo shadow, pencil-ink-gouache, paper grain, shape-explaining pixel clusters at display size, no anti-alias, no plastic skin. Absolutely no chair, stool, bench, backrest, seat, cushion, armrest, furniture post/leg/footrest, floor or contact shadow. No second character, no counter, no food, no order card, no UI, no text, no numbers, no watermark, no name.`
- Acceptance (v8): gate-1 clip별 격리 checkerboard(alpha-cutout, 동일 canvas/pivot/조명) + SHA-256 → gate-2 FHD+720 손님 화면에서 좌석 scale·occlusion·감정 구분·색각/LOW 회귀 → gate-3 자동 등록.
- Prior art / notes: 초상 `CH-EXTRA-COMMUTER-PORTRAIT`는 P1 별도 자산(대표색·안경·비대칭 특징 공유, ART-003 L482, L494). atlas frame 수는 미해결 질문 3(ART-003 L784) — spec: 최종 frame 수 unspecified, PM/디자인 결정 필요.

### CH-EXTRA-SOLO-SERVICE
- Owner: Artist 3 (service artist-025)
- Consumer: `SCR-SVC-CUSTOMERS` / `customers.actor[n]` (actor atlas) (UI-003 L344).
- Format & budget: service 공통 16 clip atlas. `128×160` 논리 4배 nearest 또는 동일 밀도 atlas. profile `bundle-model`. 현황 신규 (ART-003 L413).
- State id: 16 clip(위와 동일). 식별 축: 관찰·호기심.
- Fixed constraints: 이름 없는 1인 손님 엑스트라 — 고정 이름·서사 금지. pivot·scale·감정 2축·색각/LOW 규칙 동일. 스타일 토큰, 문자 금지.
- 화면 내 구도: `customers.actor[n]` actor layer, 합성 순서 1. 위 `츠키오카 기준 인체 공간·가구 분리` 계약을 그대로 적용하고, 하단 중앙 pivot·좌석 anchor 착석·하체 customerOcclusionLine 아래 카운터 가림을 공유한다. `CH-EXTRA-COMMUTER-SERVICE`와 동일 좌석 scale·눈높이·pivot·조명으로 같은 화면에 나란히 놓아도 사람 비율이 어색하지 않아야 하며 character layer 가구 픽셀은 `0`이다.
- 기존 화면과의 화합: 승인 `D1-TSUKIOKA-WAITING/RECEIVED-EATING`의 정면 고립·부드러운 정면광·저대비 storybook·착석 자세를 계승. 1인 손님은 딥 틸·크림 대표색(메뉴·숄더백)으로 츠키오카(이끼색)·통근객(네이비/호박)과 실루엣·색 구분. 배경 warm 팔레트와 정합.
- 의도: 관찰·호기심 성격이 자세·시선으로 읽히고, 러시 중 감정 16 clip이 얼굴·자세 2축으로 구분. LOW/색각 유지. 좌석에 정확히 대응.
- 톤앤매너(YS-HANDCRAFTED-NIGHT-v1): 연필+잉크+과슈, 형태 설명형 픽셀, 종이결, 호박 실내광·인디고 그림자, 딥 틸/크림 대표색. 반복 얼굴 금지. alpha-cutout 투명, 동일 canvas/pivot/조명. 문자 없음.
- Generation prompt(대표 1 clip, 나머지 clip은 같은 규약으로 각 1프롬프트): `Single asset only, one clip frame set of ONE character on transparent background (alpha-cutout, bottom-center pivot), person pixels only. YS-HANDCRAFTED-NIGHT-v1 hand-drawn SEATED night-yakitori-shop solo customer sprite, using D1 Tsukioka as the canonical 1920×1080 front-camera human-scale reference: same seat plane, apparent adult body scale 0.90–1.10, eye line within ±24 FHD px, shoulder line within ±32 FHD px, same soft front key light. An unnamed observing/curious solo diner, identity read by hair/build/clothing and a distinct deep-teal/cream accent color (menu, shoulder bag) — silhouette distinct from a moss-green regular and a navy commuter. Clip "<CLIP>" pose only; emotion via at least two of face/head/hands/posture, not color alone. Preserve anatomically seated hips/knees with no furniture; the separate counter layer occludes the lower body. Warm amber light, indigo shadow, pencil-ink-gouache, paper grain, shape-explaining pixel clusters, no anti-alias, no plastic skin. Absolutely no chair, stool, bench, backrest, seat, cushion, armrest, furniture post/leg/footrest, floor or contact shadow. No second character, no counter, no food, no UI, no text, no numbers, no watermark, no name.`
- Acceptance (v8): gate-1 clip별 격리 checkerboard + SHA-256 → gate-2 FHD+720 좌석 scale·occlusion·감정·색각/LOW 회귀 → gate-3 자동 등록.
- Prior art / notes: `CH-EXTRA-SOLO-PORTRAIT`(P1)과 특징 공유. atlas frame 수 spec: unspecified(미해결 질문 3).

### PR-SERVING-PLATE
- Owner: Artist 3 (service artist-025)
- Consumer: `SCR-SVC-CUSTOMERS` / `customers.table[n]`(interactables) — 손님 앞 기본 접시 (ART-003 L193; UI-003 L346).
- Format & budget: 투명 PNG 또는 저복잡도 GLB. profile `standalone-raster`(PNG) 또는 `bundle-model`(GLB). 현황 `PR-01` 재사용 후보 (ART-003 L193, L127).
- State id: spec: unspecified — 「빈 접시 + 주문 표식 decal 영역」. 주문 표식은 DOM·동적 decal로 분리(굽지 않음).
- Fixed constraints: 빈 접시만. 주문 표식·좌석 번호·수량 문자 금지(DOM/decal). 스타일 토큰. 음식 조합은 런타임 anchor.
- 화면 내 구도: `customers.table[n]` interactables, 합성 순서 3 — 바 카운터(order 2) 상판 위, 손님별 tableFoodAnchors에 접지. 손님 앞(카운터 near edge 대역)에 놓이는 작은 접시, 상반신 손님과 카운터 사이. 3/4 near-top 각도로 카운터 top-bevel 원근과 일치. 음식(FD-*-PLATED)은 런타임 anchor로 접시 위 조합.
- 기존 화면과의 화합: 카운터 `BG-SERVICE-TABLE-ARTIST009` 상판의 호박 hotspot·정면 near-top 원근과 접지 일치. 승인 order 아이콘 `order-icon-negima`(3/4 tilt, 캐러멜 글레이즈)와 같은 도자기/음식 재질 계열이되 접시는 빈 상태. `PR-01` 시각 참고(회귀 통과 시 이전).
- 의도: 음식이 올라갈 중립 표면 — 접시 자체는 조용하고 그 위 음식·주문 표식(decal/DOM)이 정보 주역. 좌석/주문 대응이 접지로 읽혀야 함.
- 톤앤매너(YS-HANDCRAFTED-NIGHT-v1): 연필+잉크+과슈, 종이결, 닳은 유약 도자기(작은 비대칭·닳은 모서리), 호박 하이라이트·인디고 그림자, 절제된 highlight(매끈 PBR 금지). alpha-cutout 투명. 주문 표식·문자 없음.
- Generation prompt(one-shot): `Single asset only, transparent background (alpha-cutout, transparent corners). YS-HANDCRAFTED-NIGHT-v1 hand-drawn EMPTY ceramic serving plate for a night yakitori shop, three-quarter/near-top view consistent with a front counter's top bevel so it grounds on the counter surface. Worn glazed ceramic with small asymmetry and worn rim, warm amber highlight, indigo shadow, pencil-ink-gouache, paper grain, restrained highlights (no glossy PBR). Empty: no food, no skewer, no order marks, no decals, no seat number, no text, no numbers, no watermark.`
- Acceptance (v8): gate-1 격리 checkerboard(alpha-cutout) + SHA-256 → gate-2 FHD+720 테이블 anchor·접지·style 회귀 → gate-3 자동 등록.
- Prior art / notes: `PR-01`은 스타일 회귀 통과 시 이전, 미통과 시 ID 유지·파일 교체. PNG vs GLB 형식 결정은 소비 화면 접지 검수에서 확정(spec: 형식 unspecified 선택지 2).

### PR-EMPTY-DISH-SET
- Owner: Artist 3 (service artist-025)
- Consumer: `SCR-SVC-CUSTOMERS` / `customers.table[n]`(퇴장 뒤 빈 식기) · 정리 대상 좌석 (ART-003 L194; UI-003 L346-347).
- Format & budget: 투명 sprite atlas 또는 저복잡도 GLB. profile `bundle-model`. 현황 신규 (ART-003 L194).
- State id: spec: unspecified — 조합 상태 「빈 접시·빈 맥주잔·빈 컵」 (ART-003 L194).
- Fixed constraints: 퇴장 뒤 빈 식기 조합. 정리(`ST-CLEANUP-OVERLAY`)·완료 후 빈 좌석 복구 흐름과 접지 일치(ART-003 L744). 스타일 토큰, 문자 금지. atlas frame 동일 canvas/pivot/조명(ART-003 L627).
- 화면 내 구도: `customers.table[n]` interactables, 합성 순서 3 — 손님 퇴장 뒤 카운터 상판(order 2) 위 tableFoodAnchors에 빈 식기 조합 배치. 정리 대상 좌석 앞. `ST-CLEANUP-OVERLAY` 정리 흐름·완료 후 빈 좌석 복구와 접지 일치. near-top 카운터 원근.
- 기존 화면과의 화합: `BG-SERVICE-TABLE-ARTIST009` 상판 접지·호박 hotspot 원근 계승. 빈 맥주잔은 `MDL-BEER-GLASS` 빈 상태(투명 유리·pale highlight)와 광원·scale 시각 일관. `PR-SERVING-PLATE` 접시와 같은 도자기 재질.
- 의도: 퇴장 직후 "정리 필요" 상태를 좌석별로 알림 — 빈 식기가 남아 정리 액션을 유도하되 좌석을 완전히 가리지 않음. 상태 조합(접시/맥주잔/컵)이 색 없이도 실루엣으로 구분.
- 톤앤매너(YS-HANDCRAFTED-NIGHT-v1): 연필+잉크+과슈, 종이결, 닳은 도자기/유리·옅은 잔여물(faint residue), 호박광·인디고 그림자, 절제된 highlight. alpha-cutout 투명, 동일 canvas/pivot/조명. 문자 없음.
- Generation prompt(상태별 각 1프롬프트): `Single asset only, transparent background (alpha-cutout). YS-HANDCRAFTED-NIGHT-v1 hand-drawn set of EMPTY used tableware after a customer leaves — empty plate, empty beer glass, empty cup — state "<STATE>" only, near-top counter view consistent with the house counter's top bevel. Worn ceramic/glass with faint residue, warm amber light, indigo shadow, pencil-ink-gouache, paper grain, restrained highlights (no glossy PBR); empty beer glass matches the beer-glass model's clear glass with pale highlights. States distinguished by silhouette, not color alone. No food, no drink liquid, no customer, no cleanup icon, no UI, no text, no numbers, no watermark.`
- Acceptance (v8): gate-1 상태별 격리 checkerboard + SHA-256 → gate-2 FHD+720 테이블 anchor·정리 흐름·style 회귀 → gate-3 자동 등록.
- Prior art / notes: 빈 맥주잔은 `MDL-BEER-GLASS` 빈 상태와 시각 일관(광원·scale). sprite atlas vs GLB 형식 spec: unspecified(선택지 2).

### ST-CLEANUP-OVERLAY
- Owner: Artist 3 (service artist-025)
- Consumer: `SCR-SVC-CUSTOMERS` / `customers.cleanup[n]`(state-overlays; 3초 홀드·원형 게이지는 DOM) (ART-003 L192; UI-003 L347, L351).
- Format & budget: 투명 sprite atlas + DOM UI anchor. profile `bundle-model`(atlas+companion). 현황 신규 (ART-003 L192).
- State id: spec: unspecified — 상태 요소 「빈 접시·빈 잔·정리 아이콘·치우는 모션」 (원형 게이지 안전 영역 `cleanupBounds`).
- Fixed constraints: 정리 아이콘·치우는 모션 프레임만 래스터. 3초 원형 게이지·`정리 필요`·`3초 동안 눌러 정리` 문구·게이지 수치는 DOM(굽지 않음, ART-003 L435; UI-003 L351). `cleanupBounds`가 손님·주문·음식 미가림(ART-003 L744). 색만으로 정리 상태 전달 금지 — 실루엣·기호 병행. 스타일 토큰.
- 화면 내 구도: `customers.cleanup[n]` state-overlays layer, 합성 순서 4대(테이블 음식 위, 전경 UI 대역). 정리 대상 좌석 위치의 카운터/식기 위에 치우는 모션·정리 글리프를 얹음. 3초 원형 게이지·`정리 필요`·`3초 동안 눌러 정리` 문구는 DOM(cleanupBounds 안전 영역). cleanupBounds가 손님·주문·음식을 가리지 않게(ART-003 L744). workScene 안 좌석 대역.
- 기존 화면과의 화합: `PR-EMPTY-DISH-SET` 빈 식기 조합·`BG-SERVICE-TABLE-ARTIST009` 상판과 접지·광원 일관. 정리 글리프는 `UI-STATE-ICONS`의 `정리 중` glyph와 시각 계열 공유(같은 broom/cloth 실루엣 언어).
- 의도: "액션 필요"를 즉시 신호하되 좌석·손님을 가리지 않음. 색만으로 정리 상태 전달 금지 — 실루엣·기호 병행. 완료 후 빈 좌석 복구는 도메인/DOM.
- 톤앤매너(YS-HANDCRAFTED-NIGHT-v1): 연필+잉크+과슈 축약 icon/모션 프레임, 호박광·인디고 그림자, 색각 비의존 실루엣. 게이지·수치·문구 굽지 않음(DOM). alpha-cutout 투명, atlas 동일 canvas/pivot/조명.
- Generation prompt(상태별 각 1프롬프트): `Single asset only, transparent background (alpha-cutout). YS-HANDCRAFTED-NIGHT-v1 hand-drawn seat cleanup overlay for a night yakitori counter — a wiping/clearing motion cue and a cleanup glyph (broom/cloth silhouette matching the house state-icon language), state "<STATE>" frame only, sized to sit over a seat's tableware without covering the customer or food. Warm amber light, indigo shadow, pencil-ink-gouache, paper grain. Non-color-dependent silhouette (readable in grayscale/LOW). No circular gauge, no percentage, no text, no numbers, no customer, no food baked in, no watermark. The 3-second hold gauge and copy are DOM.`
- Acceptance (v8): gate-1 격리 checkerboard + SHA-256 → gate-2 FHD+720 `cleanupBounds` 비가림·색각/grayscale·완료 후 빈 좌석 복구 회귀 → gate-3 자동 등록.
- Prior art / notes: 완료 후 빈 좌석 상태 복구는 도메인/DOM. 빈 식기 조합은 `PR-EMPTY-DISH-SET` 재사용.

### MDL-SERVICE-TRAY
- Owner: Artist 3 (service artist-025)
- Consumer: `SCR-SVC-CUSTOMERS` / `customers.preparedDock`·`customers.serveAnchor[n]`(전달 tray) (ART-003 L307; UI-003 L348-349).
- Format & budget: GLB 2.0, ≤`1,000` triangles. profile `bundle-model`. 현황 신규 (ART-003 L307).
- State id: 정적 모델. node/anchor: `중앙·접시·잔 snap anchor` (ART-003 L307).
- Fixed constraints: meter 단위·Y-up·+Z 전면·object origin=interaction pivot(ART-003 L309). 보이지 않는 뒷면 제거(L310). 충돌은 단순 proxy(L312). clip 동작 의미 명명(idle/pick/place/serve, L313). 과도 PBR 광택 금지 — 2D 배경과 분리되지 않게(L688). 스타일 토큰. 접시·잔 snap anchor를 manifest와 일치.
- 화면 내 구도: `customers.preparedDock`·`customers.serveAnchor[n]` — preparedDock 프레임(x=104~1816, y=872~1040) 위 `ST-SERVICE-COUNTER` 영역에 놓이는 이동/전달 GLB. object origin=interaction pivot(플레이어가 잡아 손님 좌석 serveAnchor로 전달). 접시·잔 snap anchor에 완성품 얹힘. blob shadow로 2D 배경에 접지. 셰프 고정 시점 소실점 정합.
- 기존 화면과의 화합: `ST-SERVICE-COUNTER`·`BG-SERVICE-TABLE-ARTIST009`의 월넛 목재+낡은 황동 재질을 hand-painted로 계승(과도 PBR 광택 금지 — 2D 배경과 분리되지 않게). 재질 톤은 승인 카운터 상판의 호박 hotspot 광과 정합. 고정 카운터(정적 영역)와 역할 분리 — 이건 이동 tray.
- 의도: 완성품을 손님에게 전달하는 조작 대상 — 잡기·놓기가 실루엣으로 읽히고 접시/잔 snap anchor가 manifest와 일치. 카메라 고정 전제라 저복잡도.
- 톤앤매너(YS-HANDCRAFTED-NIGHT-v1): hand-painted base color·거친 roughness·제한 하이라이트, 월넛/황동, 따뜻한 호박 밤 실내와 정합. matte(매끈 PBR 금지). 그림형 blob shadow 접지. 문자·로고 없음.
- Generation prompt(one-shot): `Single asset only, a low-poly GLB (≤1000 tris, Y-up, +Z front, origin at interaction pivot). One night-yakitori-shop hand-off serving tray with center + plate + glass snap anchors, for a fixed chef-POV serving screen. Material reads as worn walnut wood / aged brass in YS-HANDCRAFTED-NIGHT-v1 hand-painted style, matte with rough roughness and restrained highlights (no glossy PBR), consistent with warm amber night interior and the approved counter's material so it does not read as separate from the 2D background. Grounds via a painted blob shadow. No food, no glass, no plate modeled in, no back-face geometry, no text, no logos.`
- Acceptance (v8): gate-1 격리 검수(GLB triangle·node·clip·bounds·재질) + source/output SHA-256 → gate-2 FHD+720 소비 화면에서 소실점·blob shadow·pixel density·2D 배경 정합·성능(≤2000 tri) 회귀 → gate-3 자동 등록.
- Prior art / notes: `ST-SERVICE-COUNTER`(전달 tray 영역)와 역할 중복 금지 — 서빙 tray는 이동/전달 GLB, 카운터는 고정 영역.

## 그룹 B · `SCR-SVC-DRINK` (4) — 생맥주 스테이션

> 함께 생성 권장. 공통 화합 앵커: 승인 런타임 `BG-WORKSPACE-DRINK`(`/assets/core/drink/bg-workspace-drink-r2-b1.png`, 1920×1080) — 정면 눈높이, 뒷벽 선반 flat, 하단 1/3 전폭 카운터(살짝 top-bevel), 좌 주홍 제등·우 황동 새장 제등, muted teal/sage 병, 낡은 황동 jar, 무거운 어둠 속 따뜻한 호박 pool(low-key). 조리 화면 shell: `receiptRail x=176~1744 · y=104~248`(참조용 주문서), `workScene x=104~1816 · y=264~848`(도구·잔 작업면), `preparedDock x=104~1816 · y=872~1040`. 카메라: 셰프가 **옆으로 고개를 돌린** 고정 시점(주류 작업면). z-order: architecture(tower) < interactable(lever, glass) < vfx(foam/overflow) < DOM(result 도장·게이지).

### MDL-BEER-GLASS
- Owner: Artist 3 (service artist-025 — drink station)
- Consumer: `SCR-SVC-DRINK` / `drink.glass`(잔·액체·거품 layer) · `SCR-SVC-CUSTOMERS` `customers.table[n]`(제공된 맥주) (ART-003 L305; UI-003 L390, L346).
- Format & budget: GLB 2.0, ≤`1,500` triangles. profile `bundle-model`. 현황 신규 (ART-003 L305).
- State id: node/anchor `grip`, 액체 volume, `70%·100% marker anchor` (ART-003 L305). 익힘/채움 수치는 도메인·`TEX-BEER-LIQUID` 셰이더.
- Fixed constraints: meter·Y-up·+Z 전면·origin=pivot. `grip` node, 70%/100% marker anchor를 manifest와 일치. 액체·거품은 별도 layer/텍스처(굽지 않음). 뒷면 제거·proxy 충돌·의미 clip. 매끈 PBR 금지. 스타일 토큰.
- 화면 내 구도: `drink.glass` — workScene(x=104~1816, y=264~848) 중앙 작업면, tower/nozzle(architecture) 아래 잔 위치. origin=grip pivot(플레이어가 잡아 기울임). `grip` node·70%/100% marker anchor를 따라 `TEX-BEER-LIQUID`(액체)·`VFX-BEER-CORE`(거품/넘침)가 별도 layer로 채워짐. `SCR-SVC-CUSTOMERS`에서는 order 3 테이블 위 제공된 맥주로도 재사용. blob shadow 접지.
- 기존 화면과의 화합: 승인 order 아이콘 `order-icon-draft-beer`(정면 성에 낀 유리잔·golden-amber body·cream 거품·bright specular)의 유리·맥주 색을 3D로 계승하되 매끈 PBR 금지. `BG-WORKSPACE-DRINK` 카운터의 호박 rim light와 정합. 빈 잔은 `PR-EMPTY-DISH-SET` 빈 맥주잔과 광원·scale 시각 일관.
- 의도: 직접 조작(기울이기)·채움 상태를 읽는 조작 대상. grip·70/100 marker가 도메인 수치와 정확히 매칭. 액체·거품은 굽지 않고 텍스처/VFX가 소유.
- 톤앤매너(YS-HANDCRAFTED-NIGHT-v1): hand-painted 유리 재질, matte·절제된 호박 rim light, 인디고 그림자, 손그림 흔들림. 매끈 PBR 금지. 액체·거품 미모델. blob shadow. 문자·로고 없음.
- Generation prompt(one-shot): `Single asset only, a low-poly GLB (≤1500 tris, Y-up, +Z front, origin at grip pivot). One EMPTY beer glass for a night yakitori drink station with a grip node and 70% and 100% marker anchors along the liquid volume, viewed for a fixed chef side-POV work surface. Hand-painted glass material in YS-HANDCRAFTED-NIGHT-v1 style matching the approved draft-beer look (clear glass, golden-amber when filled, cream foam — but here empty), matte with restrained warm amber rim light, no glossy PBR, no liquid, no foam modeled (liquid = TEX-BEER-LIQUID, foam/overflow = VFX-BEER-CORE), grounds via a blob shadow, no back-face clutter, no text, no logos.`
- Acceptance (v8): gate-1 GLB 격리 검수 + SHA-256 → gate-2 드링크 화면 FHD+720 잔 scale·marker anchor·`TEX-BEER-LIQUID`/`VFX-BEER-CORE` 정합·성능 회귀 → gate-3 자동 등록.
- Prior art / notes: 빈 잔은 `PR-EMPTY-DISH-SET` 빈 맥주잔과 시각 일관. 액체는 `TEX-BEER-LIQUID`, 거품/넘침은 `VFX-BEER-CORE`.

### MDL-BEER-LEVER
- Owner: Artist 3 (service artist-025 — drink station)
- Consumer: `SCR-SVC-DRINK` / `drink.lever`·`drink.tower`(단일 lever·노즐) (ART-003 L306; UI-003 L388-389).
- Format & budget: GLB 2.0, ≤`1,000` triangles. profile `bundle-model`. 현황 신규 (ART-003 L306).
- State id: node `중앙 pivot`, 상·하 hit zone (ART-003 L306). 「아래 맥주→위 거품 순차 홀드」 입력은 도메인(UI-003 L389).
- Fixed constraints: meter·Y-up·+Z 전면·origin=중앙 pivot. 상·하 hit zone anchor를 manifest와 일치. 단일 레버·노즐만. proxy 충돌·의미 clip(idle/pull). 매끈 PBR 금지. 스타일 토큰.
- 화면 내 구도: `drink.lever`·`drink.tower` — workScene 중앙 작업면, tower/nozzle(정적 접촉 기준, architecture) 위/옆의 단일 레버. origin=중앙 pivot, 상·하 hit zone anchor(아래 맥주→위 거품 순차 홀드는 도메인 입력). glass(잔) 뒤/위 architecture 대역. 단일 레버·노즐만(다중 레버·잠긴 칸 없음).
- 기존 화면과의 화합: `BG-WORKSPACE-DRINK`의 낡은 황동 jar·황동 새장 제등과 같은 aged brass/dark metal 재질 계열, 호박 highlight 정합. `MDL-BEER-GLASS`·`MDL-SERVICE-TRAY`의 hand-painted matte 금속 톤과 일관.
- 의도: 순차 홀드 입력의 조작 대상 — 상·하 hit zone이 실루엣·기본 운동 방향으로 읽혀야 함. 정적 tower와 이동 레버 분리.
- 톤앤매너(YS-HANDCRAFTED-NIGHT-v1): hand-painted 낡은 황동/짙은 금속, matte·호박 highlight·인디고 그림자, 손그림 질감. 매끈 PBR 금지. 유리·액체 미포함. 문자·로고 없음.
- Generation prompt(one-shot): `Single asset only, a low-poly GLB (≤1000 tris, Y-up, +Z front, origin at central lever pivot). One SINGLE beer-tap lever with nozzle for a night yakitori drink station, with upper and lower hit-zone anchors around the central pivot, for a fixed chef side-POV work surface. Aged brass / dark metal hand-painted material in YS-HANDCRAFTED-NIGHT-v1 style matching the house brass fittings, matte, warm amber highlight, indigo shadow, no glossy PBR, no glass, no liquid, no second lever, no back-face clutter, no text, no logos.`
- Acceptance (v8): gate-1 GLB 격리 검수 + SHA-256 → gate-2 드링크 화면 FHD+720 tower 접촉 기준·hit zone·성능 회귀 → gate-3 자동 등록.
- Prior art / notes: `drink.tower`(정적 접촉 기준)와 lever 분리. 잠긴 칸/다중 레버 없음(단일 lever 계약).

### TEX-BEER-LIQUID
- Owner: Artist 3 (service artist-025 — drink station)
- Consumer: `SCR-SVC-DRINK` / `drink.glass`(맥주·거품 층 셰이더) — `MDL-BEER-GLASS` 텍스처 (ART-003 L350; UI-003 L390).
- Format & budget: sRGB ramp + foam noise, `256~512`. profile `bundle-model`(texture). 현황 신규 (ART-003 L350).
- State id: 익힘/채움처럼 base+ramp+도메인 수치의 셰이더 표현 — 날것/적정 이미지 개별 제작 금지, 채움·거품은 수치 구동(ART-003 L356). state id: unspecified(데이터 텍스처).
- Fixed constraints: sRGB 색 텍스처(맥주 ramp) + foam noise(구분). nearest sampling 회귀·무손실(ART-003 L354, L628). 손실 압축·자동 필터로 픽셀 군집 흐림 금지. UV 이음새·거품 방향 검수.
- 화면 내 구도: 화면에 직접 배치되지 않는 데이터 텍스처 — `drink.glass`의 `MDL-BEER-GLASS` 볼륨 셰이더가 샘플링(맥주 ramp + foam noise). 70%/100% marker anchor를 따라 채움이 도메인 수치로 구동. workScene 중앙 잔 위에서만 시각화. nearest sampling·무손실.
- 기존 화면과의 화합: 승인 order 아이콘 `order-icon-draft-beer`의 golden-amber body·cream 거품 색을 ramp로 재현(deep amber→pale gold, 상단 cream foam). `MDL-BEER-GLASS` 유리 위에서 그 하이라이트와 정합.
- 의도: 익힘처럼 개별 이미지 대신 base+ramp+수치로 채움/거품 표현. UV 이음새·거품 방향이 셰이더에서 자연스럽게. 손실 압축·자동 필터로 픽셀 군집 흐림 금지.
- 톤앤매너(YS-HANDCRAFTED-NIGHT-v1): sRGB 색 텍스처, 수작업 painterly grain(단순 그라디언트 아님), 맥주 ramp·foam noise 분리 채널/영역. nearest·무손실. 문자·3D·유리 형태 없음.
- Generation prompt(one-shot): `Single asset only, a data/albedo texture (256–512, sRGB) for a glass-volume shader. A beer liquid vertical color ramp — deep amber at bottom to pale gold at top, matching the approved draft-beer icon color — plus a SEPARABLE cream foam noise band at the top, hand-crafted painterly grain consistent with YS-HANDCRAFTED-NIGHT-v1 (no smooth gradient, no auto-filter blur). Tileable/seam-aware. No glass shape, no 3D, no text, no numbers, no watermark; foam and liquid must stay separable channels/regions; preserve crisp pixel clusters for nearest sampling.`
- Acceptance (v8): gate-1 텍스처 격리 검수(치수·색공간·nearest 무손실) + SHA-256 → gate-2 `MDL-BEER-GLASS` 위 70%/100% marker·거품·성능(64MB GPU) 회귀 → gate-3 자동 등록.
- Prior art / notes: `TEX-` 접두사 데이터 텍스처, linear/sRGB 구분(ART-003 L354). 거품 넘침 연출은 `VFX-BEER-CORE`.

### VFX-BEER-CORE
- Owner: Artist 3 (service artist-025 — drink station)
- Consumer: `SCR-SVC-DRINK` / `drink.overflow`·`drink.glass`·`drink.result`(거품·넘침·완성 김) (ART-003 L445; UI-003 L390-392).
- Format & budget: sprite atlas quad·mask, 합계 예산 내(동시 particle ≤200). profile `bundle-model`(vfx atlas). 현황 신규 (ART-003 L445).
- State id: 효과 상태 「거품·넘침·완성 김」 (ART-003 L445).
- Fixed constraints: sprite atlas + 수치 조합. 풀스크린 블룸·동영상·대형 반투명 fog 금지(ART-003 L448). LOW 모드는 같은 atlas·instance/quad만 축소(L449). 넘침 자동 확정 금지 — 제공/폐기 선택 anchor(UI-003 L392). Perfect/실패는 대상 주변 효과(L450). 스타일 토큰.
- 화면 내 구도: `drink.overflow`·`drink.glass`·`drink.result` — workScene 중앙 잔(`MDL-BEER-GLASS`) 상단/테두리에서 발생하는 대상 주변 효과(vfx layer, interactable 위). 거품 상승·넘침·완성 김. 넘침은 자동 확정 금지 — 제공/폐기 선택 anchor(DOM choice frame). 동시 particle ≤200. LOW는 같은 atlas·instance/quad만 축소.
- 기존 화면과의 화합: `MDL-BEER-GLASS`·`TEX-BEER-LIQUID`의 golden-amber 맥주·cream 거품 색과 정합해 잔 위에서 이어지게. `BG-WORKSPACE-DRINK`의 low-key 어둠 위에서 절제된 호박 하이라이트. `VFX-JUDGEMENT`(품질 ring)와 역할 분리.
- 의도: 거품/넘침/완성을 반응성 있게 표현하되 풀스크린 블룸·대형 fog로 화면을 덮지 않음. Perfect/실패는 대상 주변 효과. 넘침 선택 UX 보존.
- 톤앤매너(YS-HANDCRAFTED-NIGHT-v1): sprite atlas quad·mask, painterly 프레임, 호박 대 인디고, 절제. 풀스크린 블룸·fog·동영상 금지. 투명 배경. 문자 없음.
- Generation prompt(상태별 각 1프롬프트): `Single asset only, one VFX sprite-atlas state on transparent background, painterly frames. YS-HANDCRAFTED-NIGHT-v1 hand-drawn beer effect, state "<STATE>" (foam rise / overflow spill / finished steam) only, sized to sit at the rim/top of a beer glass as a target-local effect, warm amber and cream against indigo, matching the house beer color. Restrained: no full-screen bloom, no fog, no glass, no liquid body, no UI, no text, no numbers, no watermark.`
- Acceptance (v8): gate-1 상태별 격리 checkerboard(투명) + SHA-256 → gate-2 드링크 화면 FHD+720 잔 접지·넘침 선택 anchor·LOW·동시 particle 예산 회귀 → gate-3 자동 등록.
- Prior art / notes: 액체 채움은 `TEX-BEER-LIQUID`, 잔은 `MDL-BEER-GLASS`. `VFX-JUDGEMENT`(품질 ring)와 역할 분리.

## 그룹 C · `SCR-POST-CLOSING` / `SCR-SVC-GRILL` 숯불 계열 (2)

> 함께 생성 권장. 소비 화면: `SCR-SVC-GRILL` `grill.slot[n]` 하부 숯불, `SCR-DAY-PREP` `prep.charcoal`, `SCR-POST-CLOSING` `closing.charcoal`(낮아지는 숯). 공통 화합 앵커(승인 후보, art-workspace): 그릴 배경 `bg-workspace-grill-fhd-r1`(월넛/번트오렌지 목재 프레임 + 낡은 황동 코너 + 짙은 인디고 작업면; 따뜻한 프레임/차가운 중앙 split), 그릴 몸체 `st-grill-tier-1-fhd-r2`(검정 주철 grid + 하부 ember 오렌지-레드 underglow + 황동 bar + 월넛 base, 3/4 elevated), `st-grill-waiting-rack-fhd-r2`. `SCR-POST-CLOSING`은 빈 카운터+낮아지는 숯을 손님 scene 재사용 위에 얹음. 카메라: 셰프가 **고개를 숙여** 보는 불판(top-down/3-4). z-order: grill body(architecture) < charcoal state < ember vfx < DOM.

### ST-CHARCOAL-CORE
- Owner: Artist 1 (cooking artist-000)
- Consumer: `SCR-SVC-GRILL` / `grill.slot[n]` 하부 숯불층 · `SCR-DAY-PREP` `prep.charcoal` · `SCR-POST-CLOSING` `closing.charcoal` (ART-003 L189; UI-003 L286·L374·L422).
- Format & budget: 투명 PNG/WebP, 색+발광 mask. profile: spec: unspecified — 단일 투명 래스터면 `complete-layer`/`standalone-raster`, 상태 mask를 묶으면 `bundle-model`. 현황 `ST-03` 기반 확장 (ART-003 L189).
- State id: 상태 「꺼짐·약함·적정·과다」 (색+발광 mask) (ART-003 L189).
- Fixed constraints: 숯불층만(그릴 몸체 `ST-GRILL-TIER-1` 제외). 상태는 색상 하나에 의존하지 않고 실루엣·발광 등 병행(ART-003 L732). 숯불 방향·blob shadow·pixel density 정합(L685). 익힘 이미지 개별 제작 대신 base+mask(L356). 스타일 토큰. 문자 금지.
- 화면 내 구도: `grill.slot[n]` 하부 숯불층 — 그릴 몸체(`ST-GRILL-TIER-1`, architecture) 아래/안쪽에 깔리는 state layer. workScene(x=104~1816, y=264~848) 중앙 불판 하부, 셰프가 고개를 숙여 보는 top-down 격자 아래. `SCR-DAY-PREP` `prep.charcoal`·`SCR-POST-CLOSING` `closing.charcoal`에서도 같은 layer 재사용(마감은 숯이 낮아지는 상태). 숯불층만(그릴 몸체 제외).
- 기존 화면과의 화합: 승인 후보 `st-grill-tier-1-fhd-r2`의 하부 ember 오렌지-레드 underglow·검정 주철 grid를 그 아래 숯불층으로 이어지게(같은 방향·광원). `bg-workspace-grill-fhd-r1`의 인디고 작업면·따뜻한 프레임 대비와 정합. 숯 방향·blob shadow·pixel density를 그릴 몸체와 일치.
- 의도: 꺼짐/약함/적정/과다 화력을 색상 하나에 의존하지 않고 실루엣·발광 세기로 구분(색각/LOW 유지). 익힘처럼 개별 이미지 대신 base+발광 mask. 그릴 위 꼬치 익힘의 근거 광원.
- 톤앤매너(YS-HANDCRAFTED-NIGHT-v1): 연필+잉크+과슈, 형태 설명형 픽셀 군집, 색+발광 mask, 호박색 ember 대 검정 주철·인디고 그림자. 매끈 PBR·풀스크린 tint 금지. alpha-cutout 투명, 상태별 동일 canvas. 문자 없음.
- Generation prompt(상태별 각 1프롬프트): `Single asset only, transparent background (alpha-cutout). YS-HANDCRAFTED-NIGHT-v1 hand-drawn bed of charcoal in a grill, glow state "<STATE>" (out / weak / proper / excessive) only, top-down grill view matching the approved grill station's ember orange-red underglow and black cast-iron direction. Color + emissive glow read the state without relying on hue alone (readable in grayscale/LOW); warm amber embers against dark iron and indigo shadow, pencil-ink-gouache, paper grain, shape-explaining pixel clusters, no anti-alias. No grill body, no skewers, no food, no flames beyond ember glow, no UI, no text, no numbers, no watermark.`
- Acceptance (v8): gate-1 상태별 격리 checkerboard + SHA-256 → gate-2 그릴/준비/마감 화면 FHD+720 숯불 방향·slot 접지·색각/LOW·성능 회귀 → gate-3 자동 등록.
- Prior art / notes: `ST-03` 시각 참고·확장. 점화 시퀀스는 `VFX-EMBER-CORE`(불씨)/`PR-CHARCOAL-IGNITION`(S0). profile/상태 packing 결정 필요(spec: unspecified).

### VFX-EMBER-CORE
- Owner: Artist 1 (cooking artist-000)
- Consumer: `SCR-SVC-GRILL` / `grill.faceSignal[n]`·`grill.warning`(불씨) · `SCR-DAY-PREP` charcoal 점화 · `SCR-POST-CLOSING` `closing.charcoal` (ART-003 L442; UI-003 L376·L381·L422).
- Format & budget: sprite atlas, `6~8 frame`, 동시 `80 instance`. profile `bundle-model`(vfx atlas). 현황 신규 (ART-003 L442).
- State id: 상태 「약함·적정·과다」 (ART-003 L442).
- Fixed constraints: sprite atlas + 수치. 풀스크린 블룸·fog 금지. LOW 모드 instance/quad만 축소, 의미 상태 불변(ART-003 L449). 동시 particle ≤200·80 instance 예산. 대상 주변 효과. 스타일 토큰. 문자 금지.
- 화면 내 구도: `grill.faceSignal[n]`·`grill.warning`(불씨) — workScene 중앙 불판, 숯불층(`ST-CHARCOAL-CORE`) 위 vfx layer로 상승하는 불씨. `SCR-DAY-PREP` charcoal 점화·`SCR-POST-CLOSING` `closing.charcoal`(낮아지는 숯)에서 재사용. 대상(슬롯) 주변 효과, 동시 80 instance·particle ≤200. LOW는 instance/quad만 축소, 의미 상태 불변.
- 기존 화면과의 화합: `ST-CHARCOAL-CORE`·승인 후보 `st-grill-tier-1-fhd-r2`의 ember 오렌지-레드 발광과 같은 색·방향으로 이어지게(숯 위에서 피어오름). `bg-workspace-grill`의 어두운 인디고 위 절제된 호박 spark. `VFX-SMOKE-CORE`(연기)·`VFX-OIL-SPLASH`·`VFX-JUDGEMENT`와 역할 분리.
- 의도: 화력(약함/적정/과다)을 상시 비례로 신호하되 풀스크린 블룸·fog 금지. 숯불 방향과 정합해 조리 근거로 읽힘.
- 톤앤매너(YS-HANDCRAFTED-NIGHT-v1): sprite atlas 6~8 frame, painterly, 호박 spark 대 인디고, 절제. 풀스크린 블룸·fog·연기 몸체 금지. 투명 배경. 문자 없음.
- Generation prompt(상태별 각 1프롬프트): `Single asset only, one VFX sprite-atlas ember state on transparent background, 6–8 frames. YS-HANDCRAFTED-NIGHT-v1 hand-drawn rising charcoal embers, intensity "<STATE>" (weak / proper / excessive) only, rising from a charcoal bed and matching the approved grill's ember orange-red glow and direction, warm amber sparks against indigo, painterly, restrained (no full-screen bloom, no fog). No grill body, no food, no smoke body, no UI, no text, no numbers, no watermark.`
- Acceptance (v8): gate-1 상태별 격리 checkerboard(투명) + SHA-256 → gate-2 그릴/준비/마감 FHD+720 숯불 방향·LOW·80 instance 예산 회귀 → gate-3 자동 등록.
- Prior art / notes: `VFX-SMOKE-CORE`(연기)·`VFX-OIL-SPLASH`(튐)·`VFX-JUDGEMENT`(판정)과 분리. S0 점화 시퀀스 `PR-CHARCOAL-IGNITION`과 시각 계열 공유.

## 그룹 D · `SCR-POST-SETTLEMENT` / UI 의미 아틀라스 (3)

> 함께 생성 권장(같은 icon 언어 공유). 이들은 scene z-order가 아니라 **`ui.frame`(ui-art) DOM 계층**에서 CSS 16/24/32px로 그려진다 — 배경 위 합성이 아니라 DOM 패널(정산 ledger·구매 wallet/list·영업 serviceStatus) 안 glyph. 공통 화합 앵커: 승인 런타임 order 아이콘 `order-icon-negima`/`order-icon-draft-beer`(잉크+과슈 icon, 호박 글레이즈·drop shadow, 투명 배경)와 패널 skin `customer-order-wait-panel-skin`(다크 `#24242c~#34343e` + 얇은 황동/호박 stitched outline)의 icon 언어·황동 악센트를 공유. 등급/종류는 색 외 실루엣·내부 기호로 구분(색각/grayscale/LOW). 무손실. `1920×1080`/`1280×720` 모두 판독.

### UI-QUALITY-ICONS
- Owner: Artist 1 (cooking artist-000) — 조리·서빙 품질 판정. (도메인 상 owner 추정; UI atlas 전담 아티스트 부재는 아래 note)
- Consumer: `SCR-SVC-DRINK` `drink.result` · `SCR-SVC-GRILL` 품질 signal · `SCR-POST-SETTLEMENT` `settlement`(품질 분포) (ART-003 L431; UI-003 L391, L432).
- Format & budget: atlas, 최소 `4` glyph `Perfect·Good·OK·Fail`. 각 glyph 16/24/32 CSS px 판독 `32×32` 또는 `48×48` 논리 원본. profile `bundle-model`(atlas). 무손실. 현황 신규 (ART-003 L431, L435).
- State id: glyph set = Perfect/Good/OK/Fail.
- Fixed constraints: 텍스트·숫자·커서·손가락 금지(ART-003 L435). 등급은 색상 외 실루엣·내부 기호 차이(L435, L693). atlas frame 동일 canvas/pivot/조명(L627). 무손실만(L628). 스타일 토큰(의미 icon).
- 화면 내 구도: `ui.frame` DOM glyph — `SCR-SVC-DRINK` `drink.result`(품질 도장), `SCR-SVC-GRILL` 품질 signal, `SCR-POST-SETTLEMENT` `settle-service` 품질 분포 ledger에서 CSS 16/24/32px로 렌더. scene 합성 아님 — DOM 패널 안 아이콘. 32×32 또는 48×48 논리 원본, atlas.
- 기존 화면과의 화합: 승인 `order-icon-negima`/`order-icon-draft-beer`의 잉크+과슈 icon 언어·호박 악센트·투명 배경·drop shadow를 계승. 패널 skin의 황동 outline과 같은 세계. `VFX-JUDGEMENT`(ring VFX)와 icon 분리.
- 의도: 러시·정산에서 한눈에 품질 등급 판독 — 기능 가독성·색각 비의존이 질감보다 우선. Perfect/Good/OK/Fail을 실루엣+내부 기호로 구분(색만 다르게 금지).
- 톤앤매너(YS-HANDCRAFTED-NIGHT-v1): 잉크+과슈 축약 icon, 선·색·픽셀 리듬 계승하되 가독성 우선, 호박/인디고 팔레트. 동일 canvas/pivot/조명, 무손실. 문자·숫자·커서·손가락 없음.
- Generation prompt(glyph별 각 1프롬프트): `Single asset only, one UI meaning glyph on transparent background (32×32 or 48×48 logical, crisp/legible at 16/24/32 CSS px). YS-HANDCRAFTED-NIGHT-v1 hand-crafted quality-judgement icon "<GRADE>" (Perfect / Good / OK / Fail) — distinguished by silhouette and internal symbol, NOT color alone (readable in grayscale/LOW), in the same ink+gouache icon language and warm-amber/brass accent as the approved order icons. Ink + gouache on warm/indigo palette. No letters, no numbers, no cursor, no finger, no watermark.`
- Acceptance (v8): gate-1 glyph별 격리 checkerboard(무손실 alpha) + SHA-256 → gate-2 소비 화면 FHD+720 16/24/32px 판독·색각/grayscale/LOW 회귀 → gate-3 자동 등록.
- Prior art / notes: **spec gap(owner)** — 1/2/3 중 UI atlas 전담 아티스트 없음. 품질=조리 판정 근거로 Artist 1 배정했으나 PM 확정 필요. `VFX-JUDGEMENT`(ring VFX)와 icon 분리.

### UI-ECONOMY-ICONS
- Owner: Artist 3 (service artist-025) — 정산/경제 소비 근접. (도메인 추정; UI atlas 전담 부재 note)
- Consumer: `SCR-POST-SETTLEMENT` `settlement.reward` · `SCR-META-PURCHASE` `purchase.wallet` · 영업 `serviceStatus` 재화 (ART-003 L432; UI-003 L297, L442).
- Format & budget: atlas, 최소 `4` glyph `골드·명성·팁·콤보`. `32×32`/`48×48` 논리, 16/24/32px 판독. profile `bundle-model`. 무손실. 현황 신규 (ART-003 L432).
- State id: glyph set = 골드/명성/팁/콤보.
- Fixed constraints: 수치·통화 문자 금지(DOM). 색 외 실루엣·기호 구분. 동일 canvas/조명·무손실. 스타일 토큰.
- 화면 내 구도: `ui.frame` DOM glyph — `SCR-POST-SETTLEMENT` `settle-economy`/`settlement.reward`(매출·팁·보상 token), `SCR-META-PURCHASE` `purchase.wallet`(재화·명성), 영업 `serviceStatus` 재화. DOM 패널 안 16/24/32px 렌더, 32×32/48×48 논리 atlas. scene 합성 아님.
- 기존 화면과의 화합: 승인 order 아이콘·패널 skin의 잉크+과슈 icon 언어·**낡은 황동/호박 악센트**(정산·경제의 화폐 톤과 자연스러움)를 계승. `UI-QUALITY-ICONS`·`UI-STATE-ICONS`와 같은 canvas/조명·선 리듬으로 한 아틀라스 세트처럼.
- 의도: 골드/명성/팁/콤보를 수치 없이 종류로 즉시 판독 — 수치·통화 문자는 DOM. 색 외 실루엣·기호 구분.
- 톤앤매너(YS-HANDCRAFTED-NIGHT-v1): 잉크+과슈 축약 icon, 낡은 황동/호박 악센트, 가독성 우선, 동일 canvas/조명·무손실. 문자·숫자·통화 표기·커서 없음.
- Generation prompt(glyph별 각 1프롬프트): `Single asset only, one UI meaning glyph on transparent background (32×32 or 48×48 logical, crisp/legible at 16/24/32 CSS px). YS-HANDCRAFTED-NIGHT-v1 hand-crafted economy icon "<KIND>" (gold coin / reputation / tip / combo) — silhouette + internal symbol distinct, NOT color-only (readable in grayscale/LOW), same ink+gouache icon language as the approved order icons with aged-brass/amber accents fitting a settlement ledger. No letters, no numbers, no currency text, no cursor, no watermark.`
- Acceptance (v8): gate-1 glyph별 격리 checkerboard + SHA-256 → gate-2 정산/구매/영업 FHD+720 판독·색각/LOW 회귀 → gate-3 자동 등록.
- Prior art / notes: **spec gap(owner)** — UI atlas 전담 아티스트 부재, PM 확정 필요.

### UI-STATE-ICONS
- Owner: Artist 3 (service artist-025) — 정리·직원·예약 등 서빙/영업 상태 근접. (도메인 추정; UI atlas 전담 부재 note)
- Consumer: `SCR-META-PURCHASE` `purchase.list` badge · `SCR-DAY-BRIEFING` `brief.staff`/`brief.reservation`/`brief.readiness` · `SCR-SVC-CUSTOMERS` `customers.cleanup` · `SCR-META-NOTE` 복원 (ART-003 L433; UI-003 L273·L299·L347).
- Format & budget: atlas, 최소 `7` glyph `잠금·복원·판매 준비·직원·예약·도움·정리 중`. `32×32`/`48×48` 논리, 16/24/32px 판독. profile `bundle-model`. 무손실. 현황 신규 (ART-003 L433).
- State id: glyph set 위 7종.
- Fixed constraints: 문자·수치 금지. 색 외 실루엣·기호 구분(경고·상태 색각 대응). 동일 canvas/조명·무손실. 스타일 토큰. `직원` icon은 캐릭터 얼굴과 혼동 금지(ART-003 L535).
- 화면 내 구도: `ui.frame` DOM glyph — `SCR-META-PURCHASE` `purchase.list` badge, `SCR-DAY-BRIEFING` `brief.staff`/`brief.reservation`/`brief.readiness`, `SCR-SVC-CUSTOMERS` `customers.cleanup`(정리 중), `SCR-META-NOTE` 복원. DOM 패널/badge 안 16/24/32px, 32×32/48×48 논리 atlas 7 glyph. scene 합성 아님.
- 기존 화면과의 화합: 승인 order 아이콘·패널 skin의 잉크+과슈 icon 언어·황동 악센트 계승. `정리 중` glyph는 `ST-CLEANUP-OVERLAY` 정리 아이콘(broom/cloth 실루엣)과 시각 일관. `UI-QUALITY-ICONS`·`UI-ECONOMY-ICONS`와 한 세트처럼 동일 canvas/조명.
- 의도: 잠금/복원/판매 준비/직원/예약/도움/정리 중 7상태를 색 없이 판독(경고·상태 색각 대응). `직원` icon은 캐릭터 얼굴과 혼동 금지(ART-003 L535).
- 톤앤매너(YS-HANDCRAFTED-NIGHT-v1): 잉크+과슈 축약 icon, 호박/인디고 팔레트, 실루엣+내부 기호 구분, 동일 canvas/조명·무손실. 문자·숫자·커서 없음.
- Generation prompt(glyph별 각 1프롬프트): `Single asset only, one UI meaning glyph on transparent background (32×32 or 48×48 logical, crisp/legible at 16/24/32 CSS px). YS-HANDCRAFTED-NIGHT-v1 hand-crafted state icon "<STATE>" (lock / restore / sale-ready / staff / reservation / help / cleaning) — silhouette + internal symbol distinct, NOT color-only (readable in grayscale/LOW); the staff icon must not read as a character face; the cleaning icon shares the broom/cloth silhouette of the seat cleanup overlay. Same ink+gouache icon language as the approved order icons, warm/indigo palette with brass accent. No letters, no numbers, no cursor, no watermark.`
- Acceptance (v8): gate-1 glyph별 격리 checkerboard + SHA-256 → gate-2 브리핑/구매/서빙/노트 FHD+720 판독·색각/grayscale/LOW 회귀 → gate-3 자동 등록.
- Prior art / notes: **spec gap(owner)** — UI atlas 전담 아티스트 부재, PM 확정 필요. `정리 중` glyph는 `ST-CLEANUP-OVERLAY` 정리 아이콘과 시각 일관.

---

## S0 자산 (3 — 생성 대상은 `CH-AKI-STORY` 1개, 나머지 2개 DEPRECATED)

## 그룹 E · `SCR-STORY-PROLOGUE` / `SCR-STORY-BEAT` 스토리 (1)

> 소비 화면: `SCR-STORY-BEAT` `story.actors`(portrait 방식) · `SCR-STORY-PROLOGUE` 후속 대사 `OVR-STORY-DIALOGUE` 발화자 초상 · `SCR-POST-SETTLEMENT` story/settlement portrait. 프롤로그 2 phase(`exterior-key`→`gate-open`)는 승인 런타임 `BG-EXTERIOR-S0-CLOSED`/`BG-EXTERIOR-S0-GATE-OPEN`·`PR-SHOP-KEY`(인디고-plum 밤하늘·월넛 storefront·젖은 돌바닥·호박 제등)가 담당. **2026-08-02 S0 점화가 대사-only로 바뀌면서 아키 초상이 점화-완료 서사의 중심 자산이 됨.** 아키 초상 톤 앵커: 승인 `D1-TSUKIOKA-WAITING`(정면 고립·부드러운 정면광·저대비 storybook)의 초상 톤 + S0 프롤로그 밤 세계 톤. **단, 아키 초상은 preflight `unassigned`로 runtime binding 미확정(아래 spec gap) — Developer 2 binding + 사용자 preflight 승인 전 생성 금지.**

### ST-S0-BRAZIER  [DEPRECATED 2026-08-02 · S0 점화 대사 대체 · 생성하지 않음]
- **DEPRECATED**: 2026-08-02 S0 점화가 직접 조작 없이 `SCR-STORY-BEAT` 대사(`OVR-STORY-DIALOGUE`)로 처리됨(UI-003 L239-241). 아래 본문의 「S0 점화 KEEP·재제작」 결정은 폐기 — **이 자산은 생성하지 않는다.** 이하 내용은 이력 보존용.
- Owner: Artist 2 (S0 artist-023) — `semanticOwner: artist-2.s0-prologue-story`
- Consumer: `SCR-STORY-PROLOGUE` / `prologue.brazier-and-charcoal` (phase `ignite`, 완료 입력 `S0-CHARCOAL-IGNITE`) (ART-003 L167; UI-003 L238, L244).
- Format & budget: 투명 PNG/WebP 또는 저복잡도 GLB. profile `standalone-raster`(PNG) 또는 `bundle-model`(GLB). 현황 신규(재제작). 참고 후보 규격 `624×432` RGBA straight alpha (ART-003 L167; prior-art report).
- State id: `S0-STATE-CHARCOAL` (ART-003 L167). variant: 차가운 화로·숯 접촉 anchor.
- Fixed constraints: 차가운 팔각 철제 화로통·손잡이·다리·빈 basin·금속 받침, 숯 contact anchor. **손·팔·전신·도구 금지, bodyPartCount=0**(ART-003 L172). 보이는 숯 조각·불씨·발광·불꽃·연기·재·spark·ignition mask 제외(그건 `PR-CHARCOAL-IGNITION`). camera `S0-BRAZIER-FIXED-V1` 고정 16:9 contain. layer `architecture`. 스타일 토큰(비 갠 밤 픽셀 외관 조명 방향). 문자·게이지·버튼 금지.
- Generation prompt: `Single asset only, on transparent background (alpha-cutout, transparent corners), 624×432 contract canvas. YS-HANDCRAFTED-NIGHT-v1 hand-drawn cold octagonal iron charcoal brazier for a yakitori shop prologue — body, rim, two handles, four legs, empty inner basin, metal support grate, and a structural charcoal-contact anchor. Deep indigo-navy outline, restrained blue-grey/brown reflected light matching a rainy night alley. No visible charcoal pieces, no embers, no glow, no flame, no smoke, no ash, no spark, no ignition mask, no hands/arms/body, no tools, no note/key/gate, no UI, no text, no numbers, no watermark.`
- Acceptance (v8): gate-1 격리 checkerboard(alpha-cutout, 네 모서리 투명) + source/output SHA-256 → gate-2 프롤로그 ignite phase FHD+720 재조립, brazier+`PR-CHARCOAL-IGNITION` anchor·occlusion·style·bodyPartCount=0 회귀 → gate-3 자동 등록.
- Prior art / notes: **re-open.** 선행 후보 R1 `art-workspace/review/artist-023/s0-prologue/ignite/st-s0-brazier/cold/r1/assets/st-s0-brazier-cold-r1.png`, output SHA-256 `df043cca7d3672e36792e3db6705a88a83a89a4fd916021f34c023605998bcbb`, chroma source SHA `7ca345f13aacaaa747d6ef6bc873d0a8925832490fea00ed5b8b4b6442dc7ca8`, 규격 `624×432`/`181557` bytes, 상태 `withdrawn-by-design`. R1은 2026-07-31 「S0 숯 점화 직접 조작 제거」 결정으로 철회됨. **본 패킷 시점 결정은 S0 숯 점화를 KEEP** — R1을 prior art로 참조해 재제작·재제출한다. preflight 계약 `.../preflight/CONTRACT.md`.

### PR-CHARCOAL-IGNITION  [DEPRECATED 2026-08-02 · S0 점화 대사 대체 · 생성하지 않음]
- **DEPRECATED**: 2026-08-02 S0 점화 직접 조작 제거로 소비 phase(`prologue.ignition-vfx`) 자체가 사라짐 — **생성하지 않는다.** 이하 내용은 이력 보존용.
- Owner: Artist 2 (S0 artist-023)
- Consumer: `SCR-STORY-PROLOGUE` / `prologue.ignition-vfx` (phase `ignite`) · (재사용) `SCR-DAY-PREP` `prep.charcoal` (ART-003 L168; UI-003 L238·L244·L286).
- Format & budget: VFX atlas 또는 기존 숯+mask. profile `bundle-model`(vfx atlas/mask). 현황 신규/기존 결합 (ART-003 L168).
- State id: VFX 상태 「꺼짐·불씨·점화·안정」 (ART-003 L168). child bounds `vfx / z50`(brazier 위 합성, 후속 독립 gate).
- Fixed constraints: 점화 시퀀스만. `ST-S0-BRAZIER` 위 합성(별도 자산, 굽지 않음). 손·팔·도구·성냥 동작 금지(S0 신체 부위 0, ART-003 L172). 실패·점수 없음(UI-003 L242). 풀스크린 블룸·fog 금지. 스타일 토큰. 문자 금지.
- Generation prompt(상태별 각 1프롬프트): `Single asset only, one ignition VFX/mask state on transparent background. YS-HANDCRAFTED-NIGHT-v1 hand-drawn charcoal ignition, state "<STATE>" (out / spark / igniting / stable) only, composited over a brazier's charcoal contact area, warm amber embers/glow growing against indigo night, painterly, restrained. No brazier body, no hands/arms, no match, no tools, no full-screen bloom, no smoke fog, no UI, no text, no numbers, no watermark.`
- Acceptance (v8): gate-1 상태별 격리 checkerboard(투명) + SHA-256 → gate-2 프롤로그 ignite FHD+720에서 brazier contact anchor·z50 합성·style·bodyPartCount=0 회귀 → gate-3 자동 등록.
- Prior art / notes: 픽셀 제작 이력 없음(never pixeled). `ST-S0-BRAZIER` R1 후보에 「미픽셀·미합성·미등록, 후속 독립 gate에서만 소비」로 명시(candidate report L30-31). 시각 계열은 `ST-CHARCOAL-CORE`/`VFX-EMBER-CORE`와 공유.

### CH-AKI-STORY
- Owner: Artist 2 (S0 artist-023) — **단일 owner, Artist 2만 수정** (ART-003 L479; preflight)
- Consumer: `SCR-STORY-BEAT` / `story.actors` (`SCN-S0-DECISION` 및 후속 story beat) · `SCR-POST-SETTLEMENT`(story/settlement portrait) (preflight 소비 범위; UI-003 L256).
- Format & budget: 이야기 초상(portrait 방식). profile `standalone-raster`(portrait). spec: unspecified — runtime bounds/layer/DOM safe/stateVariant set 모두 preflight `unassigned`(Developer 2 확정 전 제작 금지).
- State id: 표정 4종 「피로·집중·실수·안도」 (ART-003 L479; preflight 표정 계약). story/settlement stateVariant set은 unspecified.
- Fixed constraints: 아사노 아키 — 짧은 흑갈색 머리·피곤한 눈매·마른 체형·걷어 올린 셔츠·짙은 남색 앞치마, 29세 중성적 인상. **영업·조리·S0 상호작용 화면 사용 금지**(`S0-STATE-KEY/GATE/CHARCOAL`에 아키 raster·손·팔·전신 없음)(ART-003 L479; preflight 소비 범위). 표정은 얼굴/자세 미세 변화(과장·눈물·코믹·승리 포즈·보상 UI 금지). actor와 portrait 방식 화면 내 혼합 금지(UI-003 L256). 스타일 토큰. 문자 금지. `CH-OWNER-STORY`(구형) 재사용·등록 금지.
- 화면 내 구도: `SCR-STORY-BEAT` `story.actors`(portrait 방식 — actor atlas와 화면 내 혼합 금지, UI-003 L258)와 `SCR-STORY-PROLOGUE` 후속 이야기 대사 `OVR-STORY-DIALOGUE`(발화자 초상), 및 `SCR-POST-SETTLEMENT` story/settlement portrait. `OVR-STORY-DIALOGUE`는 발화자 이름·본문·다음/닫기만 표시하는 짧은 말풍선 계층(장시간 cutscene 아님)이라 아키 초상은 그 전경 발화자 slot에 앉는다. **정확한 bounds/layer/zOrder/DOM safe rect는 spec: unspecified — preflight `unassigned`(Developer 2 versioned portrait binding + 사용자 preflight 승인 후에만 단일 원본 1장 제작).** 스토리 배경(날짜별 exterior/interior)은 별도 layer. **영업·조리·S0 상호작용(KEY/GATE) 화면 사용 금지.**
- 기존 화면과의 화합: (1) 초상 톤 앵커 승인 `D1-TSUKIOKA-WAITING`(`/assets/core/customer/d1-tsukioka-waiting-r2-b1.png`)의 정면 고립·부드러운 정면광(좌상단 warm key)·저대비 storybook·크림 skin·인디고 shade를 계승하되 인물은 아사노 아키(각진 마른 체형; 둥근 츠키오카와 실루엣 구분). (2) **S0 세계 앵커** 승인 프롤로그 아트 `BG-EXTERIOR-S0-CLOSED`(`/assets/core/s0/prologue/bg-exterior-s0-closed-r2-b1.png`)·`BG-EXTERIOR-S0-GATE-OPEN`·`PR-SHOP-KEY`(`/assets/core/s0/prologue/pr-shop-key-placed-r1-b1.png`)의 비 갠 밤 골목·인디고-plum 밤하늘·호박 제등 온기 톤과 정합해 같은 S0 밤 세계의 인물로 읽히게 한다. `CH-OWNER-STORY`(구형) 재사용·등록 금지.
- 의도: 아키 초상은 **S0 점화가 직접 조작에서 대사로 바뀐(2026-08-02) 뒤 프롤로그 점화-완료 서사를 실제로 전달하는 중심 자산** — 화로에 불이 붙는 순간을 조작이 아니라 아키의 표정·대사로 매듭짓는다. 따라서 표정(피로/집중/실수/안도)이 얼굴·자세 미세 변화로 정확히 읽혀야 하고, 작은 해상도에서도 눈매·입꼬리·자세로 감정 판독. 과장·눈물·코믹·승리 포즈·보상 UI 금지.
- 톤앤매너(YS-HANDCRAFTED-NIGHT-v1): 연필+잉크+과슈, 종이결, 호박 key light·인디고 그림자, 절제된 highlight, storybook 친근함. 반복 얼굴·플라스틱 피부·손 접촉 오류 금지. alpha-cutout 투명, 표정별 동일 canvas/pivot/조명. 문자 없음.
- Generation prompt(표정별 각 1프롬프트 — **preflight 승인 후에만**): `Single asset only, one story portrait on transparent background (alpha-cutout). YS-HANDCRAFTED-NIGHT-v1 hand-drawn portrait of Asano Aki — 29, androgynous, angular lean build, short dark-brown hair, tired eyes, rolled-up shirt sleeves, dark navy apron — in the same soft front-key-light, low-contrast storybook portrait tone as the approved seated regular (warm amber key from upper-left, indigo shade, cream skin), fit to sit in a night story scene. Expression "<EMOTION>" (fatigue / focus / mistake / relief) conveyed by subtle face and posture only. Pencil-ink-gouache, paper grain, restrained highlights, no plastic skin, no repeated face. No cooking action, no hands doing tasks, no tools, no service/kitchen background, no exaggerated grimace/tears/victory pose, no reward UI, no text, no numbers, no watermark.`
- Acceptance (v8): gate-1 표정별 격리 checkerboard(alpha-cutout) + source/output SHA-256 → gate-2 `SCR-STORY-BEAT`/`SCR-POST-SETTLEMENT` FHD+720 portrait bounds·layer·story-only guard·style 회귀 → gate-3 자동 등록.
- Prior art / notes: **preflight-only**, 생성 source·raster·runtime bundle·manifest entry 없음. preflight `.../story/ch-aki-story/preflight/CONTRACT.md`. **spec gap** — runtime `componentId`, story/settlement별 sourceMasterId 규칙, FHD/720 visualBounds·interactionBounds(null 여부)·layer/zOrder·DOM safe rect·story-only guard 모두 `unassigned`(Developer 2 versioned portrait binding + 사용자 preflight 승인 후에만 단일 원본 후보 1장 제작). 그 전 어떤 아키 pixel·표정 variant·handoff도 생성 금지.

---

## APPENDIX — ART-003 계약 공백 플래그

### ST-GRILL-WAITING-RACK (계약 행 없음 · 패킷 미작성)
- **ART-003에 계약 행이 존재하지 않는다(확인된 spec gap).** 그럼에도 앱은 이 자산을 요구하고(그릴 대기 rack), 승인 후보가 이미 존재한다.
- Consumer: `SCR-SVC-GRILL` / `grill.waiting`(대기 tray, 조립 완료 꼬치) + `grill.waitingAction` DOM (UI-003 L372-373).
- 승인 후보: `art-workspace/review/artist-000/d1-cooking/grill/waiting-rack/r2/` (`ST-GRILL-WAITING-RACK R2`, FHD PNG `st-grill-waiting-rack-fhd-r2.png`, SHA-256 `d875b1b50b814a2154e6e8f5f9d753a3b7192dead1e2fa59a2089ea01dc26651`, source bounds FHD `x=85,y=265,w=340,h=530`). Owner Artist 1(cooking artist-000).
- 파생 참조: `ST-GRILL-FINISHED-TRAY`(finished-tray R3/R4)가 이 rack의 얕은 cavity·near rim·top-down black-iron/brass 재질을 직접 파생(ART-003 `ST-GRILL-FINISHED-TRAY` 행 L188 및 finished-tray provenance). PM 지시 초안 `docs/inbox/99.change-request/2026-07-30_D1_완료트레이_음식_원근_톤_보정_PM_지시_초안.md` L32도 R2를 승인 참조로 사용.
- **조치 요청(PM):** ART-003 P0 환경·스테이션 표에 `ST-GRILL-WAITING-RACK` 안정 ID·형식(투명 PNG/WebP)·상태(빈 대기 rack)·`grill.waiting` 소비를 명시해 이미 승인·파생 소비 중인 실물과 계약을 정합화한다. (이미 approved이므로 신규 제작 패킷은 작성하지 않음 — 계약 공백만 플래그.)

---

## 요약

- **작성 패킷 22개**: D1 19종 + S0 3종 (모두 v8 `onePromptPerAsset`, 3-게이트 수용 포함). ART-003 계약 공백인 `ST-GRILL-WAITING-RACK`은 패킷 미작성·APPENDIX 플래그 처리.
- **spec gap(`unspecified` — PM/사용자 결정 필요)**:
  - `CH-AKI-STORY`: runtime componentId·sourceMasterId 규칙·FHD/720 bounds·interactionBounds·layer/zOrder·DOM safe·story-only guard 전부 `unassigned`(Developer 2 binding + preflight 승인 선행).
  - `UI-QUALITY-ICONS`·`UI-ECONOMY-ICONS`·`UI-STATE-ICONS`: Artist 1/2/3 중 UI atlas 전담 owner 부재 → owner 확정 필요(도메인 추정 배정만 함).
  - `ST-CHARCOAL-CORE`: 상태(꺼짐/약함/적정/과다) mask packing → complete-layer vs bundle-model 프로필 결정 필요.
  - `PR-SERVING-PLATE`·`PR-EMPTY-DISH-SET`: PNG vs GLB 형식 미확정(ART-003 선택지 2).
  - `BG-INTERIOR-BASE`·`BG-SEATING-6`·`BG-BAR-COUNTER-BASE`·`ST-SERVICE-COUNTER`: 공식 state id 없음(변형만 서술).
  - `CH-EXTRA-COMMUTER-SERVICE`·`CH-EXTRA-SOLO-SERVICE`: service atlas frame 수 미확정(ART-003 미해결 질문 3).
  - `ST-GRILL-WAITING-RACK`: ART-003 계약 행 부재(APPENDIX).
- **소유자 그룹**:
  - Artist 1 (cooking artist-000, 4종): ST-CHARCOAL-CORE, VFX-EMBER-CORE, UI-QUALITY-ICONS(추정), (+APPENDIX ST-GRILL-WAITING-RACK 후보 owner).
  - Artist 2 (S0 artist-023, 3종): ST-S0-BRAZIER, PR-CHARCOAL-IGNITION, CH-AKI-STORY.
  - Artist 3 (service artist-025, 15종): BG-INTERIOR-BASE, BG-SEATING-6, BG-BAR-COUNTER-BASE, ST-SERVICE-COUNTER, CH-EXTRA-COMMUTER-SERVICE, CH-EXTRA-SOLO-SERVICE, PR-SERVING-PLATE, PR-EMPTY-DISH-SET, ST-CLEANUP-OVERLAY, MDL-SERVICE-TRAY, MDL-BEER-GLASS, MDL-BEER-LEVER, TEX-BEER-LIQUID, VFX-BEER-CORE, UI-ECONOMY-ICONS(추정), UI-STATE-ICONS(추정).
- UI 아틀라스 3종 owner는 추정 배정이며, 확정 시 Artist 1/3 분담이 바뀔 수 있음.
