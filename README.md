# Recession Risk Dashboard v3 — Forecast Edition

An interactive, self-contained economic crisis detection and forecasting dashboard built in a **single HTML file** — no server, no install, no dependencies beyond a modern browser.

---

## Live demo

Open `recession_risk_dashboard_v3.html` directly in Chrome, Firefox, Safari or Edge.  
Or host it for free via **GitHub Pages** (see below).

---

## What it does

- **16 global indicators** modelled from publicly available data sources
- **Historical analysis** — scroll and zoom across 35 years (1990–2025) of FRED-calibrated data
- **Forecast engine** — project indicators and recession probability into 2026–2028 using scenario sliders
- **Real-world anchors** embedded in the forecast:
  - Mortgage delinquency 1.89% (Q1 2026, FRED)
  - Fed balance sheet $6.74T (Jul 2026, FRED)
  - Baltic Dry Index ~3,400 (Sep 2026, Baltic Exchange)
  - FAO Food Price Index 128.3 (Apr 2025, FAO)
  - Kalshi prediction market: 26% recession odds for 2026
  - NY Fed DSGE model: 35.8% over next 4 quarters
  - IMF base case: 3.1% global GDP growth 2026

---

## Data sources (all free & public)

| Indicator | Source | URL |
|-----------|--------|-----|
| Yield curve (T10Y2Y) | FRED / Federal Reserve | https://fred.stlouisfed.org |
| Mortgage delinquency (DRSFRMACBS) | FRED | https://fred.stlouisfed.org |
| Credit card delinquency (DRCCLACBS) | FRED | https://fred.stlouisfed.org |
| Auto loan delinquency (DRAUTOACBSST) | FRED | https://fred.stlouisfed.org |
| Fed balance sheet (WALCL) | FRED | https://fred.stlouisfed.org |
| Home sales (HSN1F) | FRED | https://fred.stlouisfed.org |
| Auto sales (TOTALSA) | FRED | https://fred.stlouisfed.org |
| Gold price (GOLDAMGBD228NLBM) | FRED | https://fred.stlouisfed.org |
| Baltic Dry Index | Baltic Exchange | https://www.balticdryindex.com |
| Global Supply Chain Pressure Index | NY Fed | https://www.newyorkfed.org/research/policy/gscpi |
| Container port TEU volume | World Bank | https://data.worldbank.org/indicator/IS.SHP.GOOD.TU |
| Port & shipping activity | IMF PortWatch | https://portwatch.imf.org |
| FAO Food Price Index | FAO | https://www.fao.org/worldfoodsituation/foodpricesindex/en/ |
| Corn & wheat crop yields | USDA NASS | https://www.nass.usda.gov/Quick_Stats/ |
| NBER recession dates | NBER | https://www.nber.org/research/business-cycle-dating |

> **Note:** The dashboard uses FRED-calibrated simulated data that mirrors the actual historical series. To use live data, connect to the FRED API (free key at https://fred.stlouisfed.org/docs/api/api_key.html) and replace the `buildHistorical()` function with API calls.

---

## Controls

### Timeline
| Slider | Effect |
|--------|--------|
| Window size | Zoom from 2 to 38 years |
| Scroll position | Pan across 1990–2028 |

### View modes
| Mode | Description |
|------|-------------|
| Historical analysis | 1990–2025 data only |
| Forecast 2026–2028 | Projects indicators forward using scenario sliders |
| Combined view | Full 1990–2028 sweep |

### Forecast scenario sliders
| Slider | What it models |
|--------|---------------|
| Yield curve path | Normalization (+) vs further inversion (−) |
| Delinquency trend | Credit stress rising vs stable |
| Fed policy | QE expansion (+) vs QT contraction (−) |
| BDI momentum | Shipping demand recovering (+) vs collapsing (−) |
| Food price shock | FAO index rising stress |
| Geopolitical stress | Amplifies gold, GSCPI, food prices |
| Housing market | Home sales cooling/stress |
| Forecast horizon | Months ahead to project (6–48) |
| Confidence band | Width of uncertainty envelope (σ) |

### Indicator weights
Drag any weight to zero to remove that signal entirely. The model re-normalises automatically.

### Preset scenarios
| Scenario | Model risk | Calibrated to |
|----------|-----------|---------------|
| Soft landing | ~22% | IMF base case, Goldman Sachs |
| Mild recession | ~38% | NY Fed DSGE model |
| Hard landing | ~58% | Stress scenario |
| Stagflation | ~48% | 1970s-style energy/food shock |

---

## Tabs

- **Probability & forecast** — composite score + confidence band + market-implied benchmarks
- **Indicator signals** — all 16 indicators normalized, historical + forecast
- **Scenarios** — four preset paths compared on one chart
- **Shipping & trade** — BDI, TEU volume, GSCPI
- **Agriculture** — FAO food price, wheat, corn yield
- **Correlations** — Pearson r vs NBER recession mask + lead/lag analysis

---

## Deploy to GitHub Pages (free hosting, shareable URL)

```bash
# 1. Create a new repo on github.com, then:
git clone https://github.com/YOUR_USERNAME/recession-dashboard.git
cd recession-dashboard

# 2. Copy the file in
cp path/to/recession_risk_dashboard_v3.html index.html

# 3. Commit and push
git add index.html README.md
git commit -m "Add recession risk dashboard v3"
git push origin main

# 4. Enable GitHub Pages:
#    GitHub repo → Settings → Pages → Source: main branch / root
#    Your dashboard will be live at:
#    https://YOUR_USERNAME.github.io/recession-dashboard/
```

---

## Connect to live FRED data (optional upgrade)

Get a free FRED API key at https://fred.stlouisfed.org/docs/api/api_key.html

Then replace the static arrays in `buildHistorical()` with fetch calls like:

```javascript
const FRED_KEY = 'YOUR_KEY_HERE';

async function fetchFRED(series) {
  const url = `https://api.stlouisfed.org/fred/series/observations?series_id=${series}&api_key=${FRED_KEY}&file_type=json`;
  const r = await fetch(url);
  const d = await r.json();
  return d.observations.map(o => ({ date: o.date, value: parseFloat(o.value) }));
}

// Example: live yield curve
const yieldData = await fetchFRED('T10Y2Y');
```

---

## Tech stack

- Vanilla HTML + CSS + JavaScript (no frameworks)
- [Chart.js 4.4.1](https://www.chartjs.org/) via CDN (cdnjs.cloudflare.com)
- Zero build step — open the file and it runs

---

## Disclaimer

This dashboard is for **educational and research purposes only**. All data is simulated and calibrated to public sources. Nothing here constitutes financial or investment advice. Past recession patterns do not guarantee future results.

---

## License

MIT — free to use, modify and share with attribution.
