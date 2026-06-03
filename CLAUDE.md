# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 개요

황인욱 ♥ 양혜림 결혼 1주년 제주 여행(2026-06-03~05, 2박3일) 안내 페이지. **단일 정적 파일** `index.html` 하나가 전부다. 빌드/의존성/테스트/프레임워크 없음.

## 실행

브라우저로 `index.html`을 직접 열면 된다. 서버 불필요. 외부 자산(이미지·폰트·JS 라이브러리) 의존 없이 `<style>`·`<script>`가 인라인으로 모두 포함됨.

## 구조

`index.html` 한 파일 안에서 위에서 아래로:
- `<head>`의 `<style>` — 전체 CSS. `:root` CSS 변수(`--deep`/`--ocean`/`--teal`/`--emerald` 등 에메랄드 바다 팔레트)로 색을 통일하므로 색 변경은 여기서.
- `<body>` 섹션 순서: NAV → HERO → OVERVIEW(`#intro`) → FLIGHTS(`#flights`) → RENTAL&STAY(`#rental`/`#stay`) → ITINERARY(`#plan`) → AI PICK(`#aipick`) → FOOTER. NAV 링크와 `id`가 1:1로 연결됨.
- 맨 아래 `<script>` — 모바일 메뉴 토글, 스크롤 네비 그림자, `IntersectionObserver` 기반 reveal 애니메이션 + 현재 섹션 하이라이트, AI Pick 렌더링·필터.

## AI Pick 섹션 (데이터 주도)

추천 장소는 `<script>` 안 `PICKS` 배열에서 관리되고 JS가 카드로 렌더한다. 장소 추가/수정은 HTML이 아니라 이 배열을 고친다. 카테고리별로 **별도 섹션 + 가로 스크롤 레일**(`.pick-rail#rail-food`/`rail-cafe`/`rail-spot`/`rail-act`)로 분리해 보여준다(전체보기 없음). JS가 `cat` 값으로 각 레일에 카드를 채운다.

각 항목 필드:
- `cat` — 카테고리. `food`/`cafe`/`spot`/`act` 중 하나. 어느 레일(`#rail-<cat>`)에 렌더될지와 헤딩 카운트(`#cnt-<cat>`)가 이 값으로 동작.
- `cu`(food 전용) — 요리 분류 `한식`/`중식`/`양식`/`일식`. 맛집 레일 위 요리별 필터(`#pickSubFilters .subfilter-btn[data-cu]`)가 이 값으로 카드를 보였다 숨긴다.
- `e` 이모지, `n` 이름, `t` 타입 라벨, `tc` 타입 배지 클래스(`t-han`/`t-cafe`/`t-spot` 등 CSS에 정의됨).
- `b` — 배지 키 배열. `BADGE` 맵에 정의된 키(`photo`/`only`/`near`/`air`/`ocean`/`flower`)만 사용.
- `d` 설명, `area` 지역, `drive` 소요시간, `price` 가격.
- `q`(선택) — 네이버 지도 검색어 오버라이드. 없으면 `이름 제주 지역`으로 자동 생성. 이름이 실제 상호·지명이 아닌 액티비티/체험 항목(예: 투명카약·SUP)은 검색이 안 되므로 실제 검색되는 장소·키워드를 `q`로 지정한다.

새 카테고리를 추가하려면 배열뿐 아니라 (1) 해당 `.pick-cat` 섹션 HTML(`#rail-<cat>`·`#cnt-<cat>`), (2) JS 렌더 루프의 카테고리 목록(`['food','cafe','spot','act']`), (3) `BADGE` 맵 또는 `.t-*`/`.pb-*` CSS 클래스까지 함께 손봐야 한다.

## 컨벤션

- 전부 한국어. 맛집/장소 링크는 네이버 지도 단축 URL(`naver.me/...`)을 `target="_blank" rel="noopener"`로.
- 모바일 우선 반응형. 주요 브레이크포인트 `820px`(네비), `720px`(2단 그리드), `580px`(AI Pick 레일 카드 폭).
- `prefers-reduced-motion` 존중(애니메이션 비활성 분기 있음).
