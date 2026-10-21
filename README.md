# <img src="assets/ico-radar.svg" alt="" width="24" height="24" align="absmiddle"> OTLK 360° Quantitative Research Toolkit

> <img src="assets/ico-globe.svg" alt="" width="24" height="24" align="absmiddle"> **Live site: <https://the-slag-brothers.github.io/OTLK-tracker/>** — English / Español portal, updated every trading day (last data session: **2026-10-20**).

An independent, open quantitative research suite covering **Outlook Therapeutics, Inc. (NASDAQ: OTLK)** across three interconnected market dimensions:
1. **Cash Equities & Short Flow Microstructure** (FINRA TRF Short Volume, Borrow Fees, SEC Failures to Deliver, SSR Rule 201).
2. **Derivatives & Options Mechanics** (CBOE / Nasdaq / OCC Open Interest, Max Pain, Put/Call ratios, option holders' net delta).
3. **Audited SEC Fundamental Architecture** (10-Q / 8-K Cap Table, Shares OS vs Fully Diluted, $1.10 warrants: cash raised and dilution).

---

## <img src="assets/ico-folder.svg" alt="" width="24" height="24" align="absmiddle"> Repository File Index & Navigation

Everything below is published on the live site and in this repository. Files are regenerated automatically after each trading session.

### <img src="assets/ico-layout.svg" alt="" width="24" height="24" align="absmiddle"> Portal & tracking pages

| File Name | Format | Description & Core Focus |
| :--- | :---: | :--- |
| **`index.html`** / **`index_es.html`** | HTML | **Main portal (English / Spanish).** Last-session market metrics (close, Nasdaq $1.00 compliance clock, short ratio, options positioning, cash runway), catalyst calendar and links to every tool below. |
| **`LYTENAVA_Launch_Tracker.html`** | HTML | **LYTENAVA launch tracker.** US channel, payers, CMS/Medicare, Medicaid, VA, sales-team hiring, UK NHS dispensing, and Europe / rest-of-world rollout, built from public sources only. |
| **`SEC_Filings_Index.html`** | HTML | **SEC filings index.** Searchable, filterable list of OTLK EDGAR filings (8-K, 10-Q/10-K, 13D/G, prospectuses, proxy) with direct links. |

### <img src="assets/ico-chart.svg" alt="" width="24" height="24" align="absmiddle"> Research models & reports

| File Name | Format | Description & Core Focus |
| :--- | :---: | :--- |
| **`OTLK_Shorts_and_Fundamentals_Model.xlsx`** | Excel | **Complete Shorts & Equities Model (No Options).** Contains 4 comprehensive sheets (`Executive Summary`, `Daily Data`, `Charts`, `Statistics`) with daily historical trading, FINRA short volume, Interactive Brokers CTB, SEC FTDs, and technical indicators. All values are computed in Python from raw data and stored as values (no hidden formulas). |
| **`OTLK_Options_Chain_and_Derivatives.xlsx`** | Excel | **Derivatives Chain & Greeks Model.** Live multi-strike chain across all active expirations (2026-2027), Max Pain, Black-Scholes Greeks (recomputed at the official close), Put/Call ratios, and a daily history keyed by market session. |
| **`OTLK_Executive_Dashboard_360.html`** | Interactive HTML | **Visual 360° Market Dashboard.** 100% self-contained page (also works offline once downloaded). Features Dark/Light theme, SVG open interest bar charts, strategic threshold roadmap, option-expiry tables and a data-freshness panel. |
| **`OTLK_360_Market_Intelligence_Report.md`** | Markdown | **Structured Executive Intelligence Report.** Formatted with tables, early warning alerts, and a 10,000-share reference scenario matrix. |
| **`OTLK_360_Market_Intelligence_Report.txt`** | Plain Text | **Clean ASCII Report.** Lightweight, fixed-width text report ideal for quick reading on mobile or sharing in chat rooms/forums. |

---

## <img src="assets/ico-sigma.svg" alt="" width="24" height="24" align="absmiddle"> Quantitative Methodology & Technical Formulas

> [!IMPORTANT]
> **No Black-Box or Scraped Chart Data:**  
> Unlike standard retail posts based on TradingView screenshots, every indicator in this toolkit is computed algorithmically in Python directly from raw regulatory and exchange data (SEC EDGAR, FINRA TRF, Interactive Brokers, OCC, Yahoo Finance).

### 1. Volume-Weighted Price & Money Flow
- **Anchored VWAP (AVWAP):**  
  Traditional rolling VWAPs reset arbitrarily. Our model anchors volume-weighted average price directly to structural regime shifts:
  $$\text{AVWAP}_{t_0, t} = \frac{\sum_{i=t_0}^t \text{Price}_i \times \text{Volume}_i}{\sum_{i=t_0}^t \text{Volume}_i}$$
  - *FDA Approval Anchor (2026-07-24):* Tracks the true aggregate cost basis since binary regulatory de-risking.
  - *$55M Public Offering Anchor (2026-08-13):* Measures institutional entry price from the recent financing round.
- **Chaikin Money Flow (CMF-20):**  
  Evaluates whether volume is flowing into accumulation (closes in the upper half of daily range) or distribution:
  $$\text{CLV} = \frac{(\text{Close} - \text{Low}) - (\text{High} - \text{Close})}{\text{High} - \text{Low}}, \quad \text{CMF}_{20} = \frac{\sum_{i=1}^{20} \text{CLV}_i \times \text{Volume}_i}{\sum_{i=1}^{20} \text{Volume}_i}$$
- **On-Balance Volume (OBV) & 20-day EMA:**  
  Cumulative volume momentum filtered with a 20-period EMA to detect divergence prior to price breakouts.

### 2. Market Microstructure & Regulatory Dynamics
- **FINRA Short Volume Ratio vs. Short Interest:**  
  - *Daily Short Volume Ratio* = `(Total Short Volume / Total FINRA Reported Volume) * 100`. Largely reflects market-maker liquidity provision and intraday shorting; values of 50-70% are common in small caps and do **not** measure new short positions.
  - *Official Short Interest* = bi-monthly exchange-reported short positions (as of the latest report, 21.0% of float, ~31.9M shares). This is the better gauge of actual short exposure.
- **Short Exempt Ratio (%):**  
  `(FINRA Exempt Volume / Total Short Volume) * 100`. Measures short sales executed without Rule 201 uptick restrictions (typically broker-dealer risk mitigation).
- **SEC Regulation SHO FTD Settlement Cycles (T+35):**  
  Tracks cumulative SEC Failures to Deliver. When FTD spikes exceed 50,000 shares, our engine automatically maps the mandatory broker-dealer buy-in deadline (T+35 calendar days).
- **Proprietary Short Squeeze Risk Score (0 - 100):**  
  Multi-factor composite (exactly as implemented in the model):
  1. Borrow fee percentile vs. its own history: ≥90th = 25 pts, ≥75th = 20, ≥50th = 12, ≥25th = 5.
  2. Shares available to borrow (IBKR): < 200k = 20 pts, < 800k = 15, < 2M = 8, otherwise 2.
  3. 5-day MA of the FINRA short-volume ratio: > 65% = 20 pts, > 58% = 14, > 50% = 8, otherwise 2.
  4. Official short interest (% of float): > 35% = 20 pts, > 20% = 15, > 12% = 8, otherwise 3.
  5. FTDs: T+35 spike or ≥ 50,000 shares = 15 pts; any FTD = 5.
  Reading: < 30 low, 30-49 moderate, 50-69 high, ≥ 70 very high.

### 3. Momentum & Volatility Indicators
- **Relative Strength Index (RSI-14):**  
  Computed using J. Welles Wilder's original exponential smoothing algorithm (not simple moving averages).
- **MACD (12, 26, 9):**  
  Exponential moving average spread: `EMA(12) - EMA(26)`, with 9-period signal EMA and histogram.
- **Average True Range (ATR-14):**  
  Mathematical volatility measurement tracking gap risk:
  $$\text{TR} = \max(\text{High} - \text{Low}, |\text{High} - \text{Close}_{\text{prev}}|, |\text{Low} - \text{Close}_{\text{prev}}|)$$

### 4. SEC Audited Cap Table & The $1.10 Warrant Threshold
- **Historical Shares Ladder:**  
  Market capitalization is calculated using audited share count progression from official SEC 10-Q and 8-K filings (26.9M post-RS -> 120.8M -> 187.1M -> **243.4M Basic OS** / **374.0M Fully Diluted**), avoiding outdated third-party quote provider data.
- **$1.10 Warrant Dynamics:**  
  Figures as of the 2026-10-20 session. Distance to the 55,555,556 public warrants strike ($1.10). Full exercise would bring up to **$61,111,112 in cash**, but it is **dilutive**: it issues 55,555,556 new shares (+22.8% vs. shares outstanding).

---

## <img src="assets/ico-monitor.svg" alt="" width="24" height="24" align="absmiddle"> How to Use the Interactive Dashboard

1. **Online:** open the [live portal](https://the-slag-brothers.github.io/OTLK-tracker/) and choose *Executive Dashboard 360°*. Nothing to install.
2. **Offline (optional):** download `OTLK_Executive_Dashboard_360.html` and double-click it; it opens in any modern browser (Chrome, Edge, Firefox, Safari) without an internet connection.
3. **Dark / Light Theme:** click the **Theme** button in the top header.
4. **Print / PDF:** click **Print** to generate an A4 summary report (choose "Save as PDF" in the print dialog).
5. **Portal:** the `Portal 360°` button returns to the main page (English / Spanish).

---

## <img src="assets/ico-shield.svg" alt="" width="24" height="24" align="absmiddle"> Community Disclaimer

This repository is maintained as an independent, open-source educational and quantitative research project. It does **not** constitute financial advice, investment recommendation, or endorsement to buy or sell securities. Markets involve risk; always verify with official SEC EDGAR filings and do your own research (DYOR).
