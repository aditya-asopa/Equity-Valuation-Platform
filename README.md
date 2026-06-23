# 📈 Equity Research Engine — FCFF Edition

> A fully offline, formula-driven stock valuation tool. No API. No backend. No cost.

![Version](https://img.shields.io/badge/version-3.0-c8a84b?style=flat-square)
![License](https://img.shields.io/badge/license-MIT-2db87a?style=flat-square)
![No Dependencies](https://img.shields.io/badge/dependencies-none-2db87a?style=flat-square)
![Works Offline](https://img.shields.io/badge/works-offline-2db87a?style=flat-square)

---

## What It Does

Load up an annual report, punch in the numbers, and get a complete institutional-grade equity valuation — right inside your browser. Every calculation is local. Nothing leaves your machine.

Built around the **FCFF (Free Cash Flow to Firm)** methodology used in CFA-level equity research. Uses WACC as the discount rate and an EV → Equity bridge to compute intrinsic value per share.

---

## Features

| Module | What's included |
|---|---|
| **DCF Model** | FCFF projected over 5 years using ROIC-based sustainable growth, fading to terminal. Full EV → Equity bridge. |
| **WACC Engine** | CAPM cost of equity + after-tax cost of debt, weighted by market value |
| **DuPont Decomposition** | Net Margin × Asset Turnover × Equity Multiplier → ROE cross-check |
| **ROIC Analysis** | ROIC vs WACC value spread, capital employed, NOPAT |
| **Trading Multiples** | EV/EBITDA, P/E, P/B, P/S vs sector benchmarks with RICH / FAIR / CHEAP signals |
| **Sensitivity Matrix** | 5×5 grid — full FCFF chain recomputed per cell across WACC ± 2% and g ± 1% |
| **Quality Scorecard** | 5-dimension scoring: Earnings Quality, Balance Sheet, Capital Efficiency, Valuation, Leverage |
| **Verdict + Thesis** | UNDERVALUED / FAIRLY VALUED / OVERVALUED with auto-generated investment thesis, catalysts, and risks |

---

## Getting Started

No installation. No npm. No build step.

```bash
# Clone the repo
git clone https://github.com/your-username/equity-research-engine.git

# Open in browser
open index.html
```

Or just download the HTML file and double-click it. That's literally it.

---

## How to Use

1. **Open the file** in any modern browser (Chrome, Firefox, Edge, Safari)
2. **Go to the INPUT DATA tab** — fill in figures from the company's Annual Report
3. **Enter raw currency values** (not in thousands/millions) — the tool handles formatting
4. Hit **CALCULATE FULL VALUATION →**
5. Browse across tabs: **DCF MODEL → DUPONT → SENSITIVITY → VERDICT**

### Where to find each input

| Field | Where to look |
|---|---|
| Revenue, EBIT, Net Income | Income Statement (P&L) |
| D&A | Notes to Accounts or Cash Flow Statement |
| Total Assets, Debt, Equity, Cash | Balance Sheet |
| CapEx | Cash Flow from Investing ("Purchase of PPE / Fixed Assets") |
| ΔWorking Capital | Cash Flow from Operations (change in working capital line) |
| Tax Rate | Tax note in P&L, or: Tax Expense ÷ PBT |
| Beta | screener.in, moneycontrol.com, or Bloomberg |
| Risk-Free Rate | 10-year govt bond yield (India ≈ 7%, US ≈ 4.5%) |
| ERP | Equity Risk Premium (India ≈ 7%, US ≈ 5.5%) |
| Terminal Growth Rate | Conservative: 2–4%. **Must be less than WACC.** |

---

## The Math

### FCFF

```
FCFF = EBIT × (1 − Tax Rate) + D&A − CapEx − ΔWorking Capital
```

### WACC

```
WACC = Ke × (E/V) + Kd × (1 − T) × (D/V)

where:
  Ke  = Rf + β × ERP        (CAPM)
  Kd  = Interest Expense / Total Debt
  E   = Market Cap (Price × Shares Outstanding)
  V   = E + D
```

### Sustainable Growth (ROIC-based)

```
g = ROIC × Reinvestment Rate

ROIC              = NOPAT / Capital Employed
Reinvestment Rate = (CapEx − D&A + ΔWC) / NOPAT
Capital Employed  = Equity + Debt − Cash
```

Growth fades linearly from the sustainable rate → terminal rate across the 5-year projection window.

### Terminal Value → Intrinsic Value

```
TV               = FCFF₅ × (1 + g) / (WACC − g)
EV               = Σ PV(FCFF₁..₅) + PV(TV)
Equity Value     = EV − Total Debt + Cash
Intrinsic/share  = Equity Value / Shares Outstanding
```

---

## Sector Benchmarks

Built-in sector averages are used for multiples comparisons:

| Sector | EV/EBITDA | P/E | P/B |
|---|---|---|---|
| Technology | 22× | 28× | 5.5× |
| Banking / Finance | 14× | 18× | 2.2× |
| Consumer / FMCG | 18× | 35× | 7.0× |
| Healthcare / Pharma | 20× | 30× | 4.5× |
| Energy | 10× | 12× | 1.8× |
| Utilities | 12× | 16× | 2.0× |
| Industrial | 14× | 20× | 3.0× |
| Telecom | 8× | 14× | 1.5× |
| Real Estate | 18× | 25× | 2.5× |

---

## Supported Currencies

₹ Indian Rupee · $ US Dollar · £ British Pound · € Euro

---

## Technical Notes

- **Zero dependencies** — pure HTML, CSS, and vanilla JS. Charts rendered on an HTML5 `<canvas>`; no Chart.js, no D3.
- **Fully local computation** — no data is sent to any server at any point.
- **Print / PDF ready** — use the "Print / Save as PDF" button on the Verdict tab for a clean single-page export.
- **Input validation** — alerts fire if WACC ≤ 0 or if terminal growth rate ≥ WACC (both break the Gordon Growth Model).

---

## Limitations & Assumptions

- Single-year inputs only — no multi-year averaging. For cyclical businesses, manually normalise EBIT/FCFF before entering.
- Growth projection uses a linear fade from ROIC-based sustainable growth to terminal. Doesn't model segment-level growth or step-changes.
- Sector benchmarks are static estimates. Update the `SECTOR` object in the JS for market-specific calibration.
- No adjustment for options, warrants, convertibles, or minority interest in the equity bridge.

---

## Disclaimer

This tool is for **educational and research purposes only**. It does not constitute financial advice. Always conduct independent due diligence before making investment decisions.

---

## License

MIT — free to use, modify, and distribute.
