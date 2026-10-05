# Nifty 50 — PER · 지수가격 월별 트렌드

인도 Nifty 50 지수의 **월별 PER과 지수가격**을 1999-01 ~ 2026-10 (334개월, 27.8년) 구간으로 본다.

- `index.html` · `data.js` · `echarts.min.js` — **self-contained**. `index.html`을 더블클릭하면 그냥 열린다 (CDN·fetch 없음).
- 원천: nifty-pe-ratio.com (NSE Indices 원천) 주간 시계열 → **각 월의 첫 관측치**를 그 달 값으로 사용.
- 같은 데이터의 TSV: `output/excel/nifty50_지수_월별_PER_가격.tsv`

## 왜 30년이 아니라 27.8년인가
공개 소스의 Nifty 50 PER·가격 시계열은 **1999-01부터**다. 지수 자체는 1996-04-22 산출 개시(기준시점 1995-11-03 = 1000pt)지만
그 이전 월별 PER은 공개 데이터가 없다. NSE 공식 API(`api/historical/indicesHistory`)는 403, stooq·MacroTrends는 봇 차단.

## 검증 (CDP 실제 마우스 이벤트)
- dataZoom `startValue` ↔ 화면 좌측 실제 시각 **오차 0일** (전체·3년 프리셋 모두)
- 두 차트 구간 동기화 OK · 「구간 시작=100」 첫 점 정확히 100
- 커스텀 툴팁: 선 위가 아닌 임의 지점에서도 표시 + 범례 양방향 강조
- 다크/라이트 각각 별도 스텝 색상 (라이트 `#2a78d6`/`#eb6834`, 다크 `#3987e5`/`#d95926`) — dataviz 검증기 전 항목 PASS

⚠️ `convertFromPixel({xAxisIndex:0},[x,y])` 는 ECharts 5.5.1에서 `null` 을 반환한다. **`{gridIndex:0}`** 을 쓸 것.

## 원화 환산 (2026-10-05 추가)
- `환율(KRW/INR) = FRED DEXKOUS(원/달러) ÷ DEXINUS(루피/달러)` — 미 연준 H.10. FRED에 없는 6개월만 ECB 참조환율로 보완.
- **교차검증**: FRED vs ECB 공통 6,604영업일 평균 상대오차 0.202%·중앙값 0.139% · 환율 관측일과 지수 관측일 격차 평균 0.01일(최대 1일).
- 결과: INR CAGR **12.16%** → KRW CAGR **9.54%** (환율 드래그 **−2.62%p**). 24.2배 → 12.5배.
- 대시보드: 「가격 통화: INR/KRW」 토글 + 「현지통화 vs 원화 환산(구간 시작=100)」 비교 차트.
