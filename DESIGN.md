# DESIGN.md

`index.html`에 구현된 디자인 시스템 기록. 새 섹션·컴포넌트를 추가할 때 이 톤과 토큰을 따른다.

## 컨셉

**Soft Coastal / 에메랄드 바다** — 제주 바다를 모티프로 한 부드럽고 세련된 휴양 무드. 둥근 모서리, 유리질(backdrop-blur) 표면, 깊은 바다색에서 민트로 흐르는 그라데이션, 따뜻한 모래색 포인트. 과하지 않게 정제된(refined) 방향이며 맥시멀이 아니다. 기본은 화이트 배경 + 바다색 강조.

## 색 (CSS 변수, `:root`)

색은 항상 변수로 참조한다. 하드코딩 금지.

| 변수 | 값 | 용도 |
|---|---|---|
| `--deep` | `#03506f` | 가장 진한 바다. 제목·강조 텍스트 |
| `--deep2` | `#0a4d68` | 진한 바다 보조 |
| `--ocean` | `#088395` | 메인 바다색. eyebrow·링크·그라데이션 시작 |
| `--teal` | `#14b8a6` | 청록. 그라데이션·아이콘 |
| `--emerald` | `#2dd4bf` | 에메랄드 |
| `--mint` | `#5eead4` | 밝은 민트. 그라데이션 끝·점선·hover 보더 |
| `--aqua` | `#e0f7fa` | 연한 배경·아이콘칩·hover 배경 |
| `--aqua2` | `#f0fcfb` | 가장 연한 틴트 배경 |
| `--sand` / `--sand2` | `#fde68a` / `#fcd34d` | 모래색 포인트(배지·accent) |
| `--ink` | `#0f3a48` | 본문 텍스트 |
| `--muted` | `#4b6b75` | 보조 텍스트 |
| `--white` | `#ffffff` | 배경·카드 |

**그림자/모서리**: `--shadow` `0 14px 40px rgba(3,80,111,.14)`, `--shadow-sm` `0 6px 18px rgba(3,80,111,.10)`, `--radius` `20px`.

**대표 그라데이션**:
- 헤더·버튼·활성요소: `linear-gradient(135deg, var(--ocean), var(--teal))` 또는 `var(--deep) → var(--ocean)`
- 히어로 배경: 여러 `radial-gradient`(민트/에메랄드 빛망울) + `linear-gradient(160deg, #022f43, var(--deep), var(--ocean))`
- 틴트 섹션: `linear-gradient(180deg, var(--aqua2), var(--aqua))`
- 모래 포인트(tip 박스): `linear-gradient(135deg, #fffbeb, #fef3c7)`

## 타이포그래피

- 폰트 스택: `-apple-system, BlinkMacSystemFont, "Apple SD Gothic Neo", "Pretendard", "Noto Sans KR", "Malgun Gothic", sans-serif`. 웹폰트 임포트 없이 시스템 폰트로 한글 최적화. (별도 디스플레이 폰트 미사용)
- 제목은 굵게: 히어로 `h1` `900`, 섹션 `h2`/`h3` `800~900`, `letter-spacing: -.02~-.03em`.
- 유동 크기: `clamp()` 사용. 예) `h1: clamp(2.8rem, 9vw, 5.6rem)`, `h2: clamp(1.8rem, 4.5vw, 2.6rem)`.
- `eyebrow`(섹션 라벨): `--ocean`, `800`, `letter-spacing .12em`, `text-transform: uppercase`, 이모지 동반.
- 본문 `line-height: 1.6`, 보조 텍스트는 `--muted`에 `.82~.92rem`.

## 레이아웃

- 컨테이너 `.wrap`: `width: min(1080px, 100% - 48px); margin-inline: auto`.
- 섹션 `section.block`: 상하 `84px` 패딩. `.tint` 클래스로 연한 배경을 **번갈아** 깔아 리듬을 준다.
- 섹션 헤더 `.sec-head`: 중앙 정렬, `max-width 680px`, `eyebrow → h2 → p` 순.
- 그리드: `.grid.g2`는 2열, `720px` 이하에서 1열. AI Pick `.pick-grid`는 3열 → `880px` 2열 → `580px` 1열.
- 모바일 브레이크포인트: `820px`(네비 햄버거 전환), `720px`/`880px`/`580px`(그리드), `600px`(표).

## 컴포넌트

- **NAV** `.nav`: 상단 고정, 반투명 화이트 + `backdrop-filter: blur(14px)`. 스크롤 시 `.scrolled`로 그림자. 활성 링크는 그라데이션 알약. `820px`↓에서 햄버거 드롭다운.
- **HERO** `.hero`: 풀스크린(`100svh`), 다중 그라데이션 배경 + `::before` 반짝이는 별(`twinkle` 애니), 하단 SVG `wave` 디바이더로 다음 섹션과 연결. 알약형 `hero-badge`·`hero-meta`.
- **카드** `.card`: 화이트, `--radius`, `--shadow-sm`, 미세 보더. `.lift` 추가 시 hover에 `translateY(-6px)` + 진한 그림자.
- **칩** `.chip`: 아이콘+숫자+라벨의 요약 통계 블록(OVERVIEW).
- **flight 카드** `.flight.go`/`.back`: 그라데이션 헤더 + 출발/도착 endpoint + 점선 비행경로.
- **info 카드** `.info`: 아이콘칩 + 제목 + `dl.dl`(점선 구분 dt/dd 행). 렌트카·숙소용.
- **일정** `.day-card`: 그라데이션 `dhead`(DAY n) + `.sched` 안 `.item`(시간 라인 + 아이콘칩 + 텍스트 + 네이버 지도 링크).
- **AI Pick 카드** `.pick`: 이모지+이름+타입배지, 배지칩(`.pb-*`), 설명, 하단 메타(지역/이동/가격). 카테고리 필터·카운트와 연동. JS `PICKS` 배열로 렌더(→ `CLAUDE.md` 참고).
- **배지 알약**: 타입은 `.t-*`(한식 `t-han` 빨강, 카페 `t-cafe` 보라, 관광 `t-spot` 초록 등), 특성은 `.pb-*`(포토/오션뷰/수국 등). 연한 배경 + 진한 글자색 쌍으로 통일.
- **미사용(정의만 됨)**: 지출 표 `.exp`/`table.exp-t`, 팁 박스 `.tip`. 필요 시 바로 가져다 쓸 수 있는 기성 컴포넌트.

## 모션

- 등장: `.reveal` → `IntersectionObserver`로 뷰포트 진입 시 `.in`(opacity + `translateY` 해제). `fadeUp` keyframe도 히어로 요소에 `animation-delay` 스태거(.1s/.2s/.3s)로 적용.
- 앰비언트: 히어로 별 `twinkle`(6s alternate), 스크롤 큐 `bob`(상하 바운스).
- hover: 카드 `.lift` 부상, 버튼·링크 색/배경 전환(`.2~.28s ease`).
- **접근성**: `@media (prefers-reduced-motion: reduce)`에서 모든 애니메이션·스크롤 비활성, `.reveal` 즉시 표시. 새 모션 추가 시 이 분기도 함께 챙긴다.

## 규칙 요약

- 색·그림자·모서리는 변수로. 새 강조색이 필요하면 `:root`에 토큰부터 추가.
- 그라데이션은 `135deg` 대각선이 기본 패턴. 알약(`border-radius: 999px`)은 배지·버튼·메타 칩에 일관 적용.
- 이모지를 라벨·아이콘으로 적극 사용(이 페이지의 시각 언어).
- 새 인터랙티브 요소도 `prefers-reduced-motion`과 모바일 반응형을 기본 충족시킨다.
