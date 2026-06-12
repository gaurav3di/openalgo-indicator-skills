# OpenAlgo Indicator Skills for Agentic Coding Tools

A comprehensive collection of technical indicator skills for charting, analysis, and custom indicator development using OpenAlgo. Works with **40+ AI coding agents** via [skills.sh](https://github.com/vercel-labs/skills) — including Claude Code, Cursor, Codex, OpenCode, Cline, Windsurf, GitHub Copilot, Gemini CLI, Roo Code, and more.

Supports **Indian markets** via OpenAlgo and **US/Global markets** via yfinance. Includes 100+ indicators powered by the openalgo 2.x compiled Rust core, Plotly dark-themed charts, Dash and Streamlit web dashboards, real-time WebSocket feeds, multi-symbol scanners, and a custom indicator builder using vectorized NumPy on Rust-core primitives.

## Powered by openalgo 2.x (Rust Core)

As of openalgo 2.0, the indicator engine is a compiled Rust core (`openalgo._oaindicators`, built with PyO3) that ships inside the wheel — a plain `pip install openalgo` is all you need. Highlights:

- **Compile-on-first-use dependencies removed** — the old LLVM-based runtime compiler stack is gone entirely. There is no runtime compilation, no first-call delay, and no compile cache; the very first call runs at full speed.
- **Python 3.12+** — openalgo 2.x requires Python >= 3.12 (supports 3.12, 3.13, 3.14).
- **Unchanged API** — `from openalgo import ta` works exactly as before; fully backward compatible.
- **18 new TA-Lib-compatible indicators** — mom, rocp, rocr, rocr100, apo, midpoint, midprice, avgprice, medprice, typprice, wclprice, plus_dm, minus_dm, dx, adxr, stochf, linregangle, linregintercept.
- **O(n) everywhere, benchmarked against TA-Lib** on 924k bars: the regression/statistics family (linreg, tsf, stdev, cci, macd, ...) runs faster than TA-Lib; the rest are on par. See the [benchmark results](https://github.com/marketcalls/openalgo-python-library/blob/master/benchmark/TALIB_PERF_COMPARE.md) and [TA-Lib compatibility notes](https://github.com/marketcalls/openalgo-python-library/blob/master/docs/TALIB_COMPATIBILITY.md).

## Quick Install

Install the skills into your project using [npx skills](https://github.com/vercel-labs/skills). The CLI auto-detects your AI coding agent and installs skills to the correct directory.

```bash
# GitHub shorthand
npx skills add marketcalls/openalgo-indicator-skills

# Full GitHub URL
npx skills add https://github.com/marketcalls/openalgo-indicator-skills
```

Install a specific skill only:

```bash
npx skills add marketcalls/openalgo-indicator-skills -s indicator-chart
npx skills add marketcalls/openalgo-indicator-skills -s custom-indicator
npx skills add marketcalls/openalgo-indicator-skills -s indicator-dashboard
npx skills add marketcalls/openalgo-indicator-skills -s indicator-scanner
npx skills add marketcalls/openalgo-indicator-skills -s live-feed
npx skills add marketcalls/openalgo-indicator-skills -s indicator-setup
```

List available skills before installing:

```bash
npx skills add marketcalls/openalgo-indicator-skills -l
```

Install globally (available across all projects):

```bash
npx skills add marketcalls/openalgo-indicator-skills -g
```

### Supported AI Coding Agents

Skills are installed via [skills.sh](https://github.com/vercel-labs/skills) which supports 40+ agents. Each agent reads skills from its own directory:

| Agent | Skills Directory |
|-------|-----------------|
| Claude Code | `.claude/skills/` |
| Cursor | `.agents/skills/` |
| Codex | `.agents/skills/` |
| OpenCode | `.agents/skills/` |
| Cline | `.agents/skills/` |
| Windsurf | `.agents/skills/` |
| GitHub Copilot | `.agents/skills/` |
| Gemini CLI | `.agents/skills/` |
| Roo Code | `.agents/skills/` |
| + 30 more | Auto-detected by `npx skills` |

The `npx skills add` command detects which agents you have installed and places the skill files in the correct paths automatically.

## Supported Markets

| Market | Data Source | Method | Example Symbols |
|--------|------------|--------|-----------------|
| **India (Equity)** | OpenAlgo | `client.history()` | SBIN, RELIANCE, INFY |
| **India (Index)** | OpenAlgo | `client.history(exchange="NSE_INDEX")` | NIFTY, BANKNIFTY |
| **India (F&O)** | OpenAlgo | `client.history(exchange="NFO")` | NIFTY30DEC25FUT |
| **US/Global** | yfinance | `yf.download()` | AAPL, MSFT, SPY |

> **Market detection**: If a symbol looks Indian (SBIN, RELIANCE, NIFTY), skills use OpenAlgo. If US (AAPL, MSFT), skills use yfinance. Automatic — no configuration needed.

## Capabilities

### Skills (User-Invocable Commands)

| Command | What It Does |
|---------|-------------|
| `/indicator-setup` | Detects OS, creates venv, installs all packages (openalgo, plotly, dash, streamlit, yfinance, matplotlib, seaborn), configures `.env` with API keys |
| `/indicator-chart` | Charts any indicator on a symbol with Plotly dark theme — overlay or subplot, with signal markers and plain-language explanation |
| `/custom-indicator` | Creates a custom indicator using vectorized NumPy on Rust-core `ta` primitives — generates `indicator.py` + `chart.py` + `benchmark.py` |
| `/indicator-dashboard` | Builds a Plotly Dash or Streamlit web application — single-symbol, multi-symbol, multi-timeframe, or scanner dashboard |
| `/indicator-scanner` | Scans multiple symbols (NIFTY 50, BANKNIFTY stocks) with indicator conditions — RSI, EMA crossover, Supertrend, volume spike |
| `/live-feed` | Real-time indicator computation on WebSocket streaming data — LTP, quote, or depth mode with rolling buffer |

### Pre-Built Chart Templates (13)

| Template | Type | Description |
|----------|------|-------------|
| EMA Chart | Overlay | EMA(10/20/50) overlay with crossover signal markers |
| RSI Chart | Subplot | RSI(14) with overbought/oversold zones and color fills |
| MACD Chart | Subplot | MACD line + signal + histogram with color coding |
| Supertrend Chart | Overlay | Direction-colored Supertrend with buy/sell markers |
| Bollinger Chart | Overlay + Subplot | Bollinger Bands + %B + Bandwidth (squeeze detection) |
| Multi-Indicator | Multi-Panel | Candlestick + EMA + RSI + MACD + Volume with bias assessment |
| Basic Dashboard | Web App | Single-symbol Plotly Dash app with indicator checkboxes and stats cards |
| Multi Dashboard | Web App | Multi-timeframe Dash app (5m/15m/1h/D grid) with confluence detection |
| Streamlit Basic | Web App | Single-symbol Streamlit app with sidebar, metrics, plotly charts |
| Streamlit Multi | Web App | Multi-timeframe Streamlit app with confluence summary |
| Custom Indicator | NumPy | Z-Score example composing `ta` primitives + pandas wrapper + benchmark |
| Live Feed | WebSocket | Real-time LTP feed with EMA/RSI computation on rolling buffer |
| Scanner | Multi-Symbol | NIFTY 50 scanner with 5 scan types (RSI, EMA, Supertrend, Volume) |

### Knowledge Base (12 Rule Files)

| Category | What's Covered |
|----------|---------------|
| **Indicators** | Complete 100+ indicator reference with signatures, parameters, return types. Trend (20), Momentum (9), Volatility (16), Volume (15), Oscillators (20+), Statistical (9), Hybrid (6+), TA-Lib Compatible (18), Utilities |
| **Data Fetching** | OpenAlgo history/quotes/depth/intervals, yfinance for US/Global, data normalization (datetime index, sort, strip timezone), option chain + option greeks (IV, delta, gamma, theta, vega, rho) APIs |
| **Plotting** | Plotly dark theme, candlestick overlays, multi-panel subplots, fill-between bands, color-coded direction, signal markers, save to HTML |
| **Custom Indicators** | Compose from `openalgo.ta` Rust-core primitives + vectorized NumPy, single/multi-output, NaN handling, DO/DON'T rules, performance tips |
| **WebSocket Feeds** | LTP/Quote/Depth subscription, polling stored data, unsubscribe/disconnect, real-time indicator computation with rolling buffer |
| **Performance (Rust Core)** | The compiled Rust core, first-call full speed (no compile step), O(n) guarantees, TA-Lib benchmark results, NumPy vectorization tips for custom code |
| **Dash Dashboards** | Dash app structure, multi-indicator layout, dynamic subplot callbacks, stats cards, auto-refresh with `dcc.Interval` |
| **Streamlit Dashboards** | Streamlit app structure, sidebar inputs, `st.plotly_chart()`, `st.metric()`, auto-refresh, scanner tables, dark theme |
| **Multi-Timeframe** | Fetch multiple timeframes, same indicator across TFs, confluence detection (all bullish/bearish/mixed), MTF grid chart |
| **Signal Generation** | Core 4-step pipeline, crossover/crossunder, `ta.exrem()` cleaning, common patterns (EMA, RSI, Supertrend, MACD, Bollinger, ADX) |
| **Indicator Combinations** | Category mixing rules, 6 combination patterns (Trend+Momentum, Triple Screen, BB+Keltner Squeeze, ADX+DI, Multi-Indicator Scorecard) |
| **Symbol Format** | OpenAlgo exchange codes (NSE, BSE, NFO, NSE_INDEX, MCX), equity/futures/options format, common index symbols |

## Prerequisites

### 1. AI Coding Agent

Install any supported AI coding agent. For example:

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) — `npm install -g @anthropic-ai/claude-code`
- [Cursor](https://cursor.com) — Desktop IDE with built-in AI
- [Codex](https://github.com/openai/codex) — `npm install -g @openai/codex`
- [OpenCode](https://github.com/opencode-ai/opencode) — `go install github.com/opencode-ai/opencode@latest`
- [Cline](https://github.com/cline/cline) — VS Code extension
- [Windsurf](https://windsurf.com) — Desktop IDE with AI
- Or any of the [40+ supported agents](https://github.com/vercel-labs/skills)

Then install the skills:

```bash
npx skills add marketcalls/openalgo-indicator-skills
```

### 2. Data Source Setup

**Indian Markets** — requires [OpenAlgo](https://github.com/marketcalls/openalgo):

```bash
git clone https://github.com/marketcalls/openalgo.git
cd openalgo
pip install -r requirements.txt
python app.py
```

OpenAlgo runs locally at `http://127.0.0.1:5000`. You need a broker account connected via OpenAlgo and an API key from the dashboard. See [OpenAlgo documentation](https://docs.openalgo.in/).

**US/Global Markets** — no setup needed. Uses yfinance (public Yahoo Finance data).

### 3. Python Environment Setup

Use the `/indicator-setup` skill for automated setup, or manually:

```bash
python -m venv venv
source venv/bin/activate   # Linux/Mac
# venv\Scripts\activate    # Windows

pip install openalgo yfinance plotly dash dash-bootstrap-components streamlit numpy pandas python-dotenv websocket-client httpx scipy nbformat matplotlib seaborn ipywidgets
```

> **Note**: openalgo 2.x requires Python 3.12 or newer. The compiled Rust indicator core ships inside the wheel — no extras or build tools needed.

### 4. Configure API Keys

```bash
cp .env.sample .env
# Edit .env with your API keys
```

## Usage Examples

### `/indicator-setup` — Environment Setup

Detects OS, creates venv, installs all dependencies, and collects API keys into `.env`.

```
/indicator-setup
/indicator-setup python3.12
```

### `/indicator-chart` — Chart Any Indicator

Create a Plotly chart with indicator overlays or subplots. Auto-detects overlay vs subplot positioning.

```
# Indian Markets
/indicator-chart ema SBIN NSE D
/indicator-chart rsi RELIANCE NSE D
/indicator-chart supertrend NIFTY NSE_INDEX 15m
/indicator-chart macd INFY NSE D
/indicator-chart bbands HDFCBANK NSE D

# US Markets
/indicator-chart ema AAPL
/indicator-chart rsi MSFT
```

### `/custom-indicator` — Build Custom Indicators

Create a custom indicator from Rust-core `ta` primitives and vectorized NumPy, with chart and benchmark.

```
/custom-indicator zscore
/custom-indicator vwap-deviation
/custom-indicator momentum-squeeze
```

### `/indicator-dashboard` — Web Dashboards

Build a Plotly Dash or Streamlit web application with live charts.

```
# Plotly Dash
/indicator-dashboard single SBIN
/indicator-dashboard multi-timeframe RELIANCE
/indicator-dashboard scanner-dashboard

# Streamlit
/indicator-dashboard streamlit-single SBIN
/indicator-dashboard streamlit-multi RELIANCE
/indicator-dashboard streamlit-scanner
```

### `/indicator-scanner` — Scan Stocks

Screen multiple symbols with indicator conditions.

```
/indicator-scanner rsi-oversold
/indicator-scanner rsi-overbought
/indicator-scanner ema-crossover
/indicator-scanner supertrend-buy
/indicator-scanner volume-spike
```

### `/live-feed` — Real-Time WebSocket

Stream live prices with indicator computation.

```
/live-feed SBIN NSE
/live-feed RELIANCE NSE quote
/live-feed NIFTY NSE_INDEX
```

## Key Features

### 100+ Rust-Powered Indicators

All indicators from the OpenAlgo `ta` library run in a compiled Rust core for production-grade speed. Every indicator is O(n), and the first call runs at full speed — there is no runtime compilation step.

```python
from openalgo import ta

ema_20 = ta.ema(close, 20)                    # trend overlay
rsi_14 = ta.rsi(close, 14)                    # momentum oscillator
st, dir = ta.supertrend(high, low, close)      # direction-colored trend
macd, sig, hist = ta.macd(close, 12, 26, 9)   # three-output momentum
```

### Plotly Dark Theme Charts

All charts use `template="plotly_dark"` with `xaxis type="category"` for candlesticks (no weekend gaps).

```python
import plotly.graph_objects as go
fig = go.Figure()
fig.update_layout(template="plotly_dark", xaxis_type="category")
```

### Custom Indicators with NumPy + ta Primitives

Build your own indicators by composing Rust-core `ta` primitives (`ta.sma`, `ta.ema`, `ta.stdev`, `ta.highest`, `ta.lowest`, `ta.true_range`, ...) with vectorized NumPy. No runtime compiler is needed — there is nothing to compile and no first-call delay.

```python
from openalgo import ta
import numpy as np

def zscore(close, period=20):
    mean = ta.sma(close, period)
    std = ta.stdev(close, period)
    return (np.asarray(close, dtype=np.float64) - mean) / std
```

### Signal Cleaning with EXREM

Always use `ta.exrem()` after generating raw buy/sell signals — removes excess signals until the opposite occurs.

```python
from openalgo import ta

buy_raw = ta.crossover(ema_fast, ema_slow).fillna(False)
sell_raw = ta.crossunder(ema_fast, ema_slow).fillna(False)
buy_clean = ta.exrem(buy_raw, sell_raw)
sell_clean = ta.exrem(sell_raw, buy_raw)
```

### Real-Time WebSocket Feeds

Live indicator computation on streaming market data with rolling buffer.

```python
from openalgo import api, ta

client = api(api_key=os.getenv("OPENALGO_API_KEY"))
client.connect()
client.subscribe_ltp(
    [{"exchange": "NSE", "symbol": "SBIN"}],
    on_data_received=on_data
)
```

### Multi-Timeframe Confluence

Analyze the same symbol across 4 timeframes (5m, 15m, 1h, D) with trend alignment detection.

```
STRONG BULLISH — All timeframes aligned
STRONG BEARISH — All timeframes aligned
MIXED — 2/4 bullish
```

### OpenAlgo Data Methods

| Method | Purpose | Returns |
|--------|---------|---------|
| `client.history()` | OHLCV candles | DataFrame |
| `client.quotes()` | Real-time snapshot | Dict |
| `client.multiquotes()` | Multi-symbol quotes | List of dicts |
| `client.depth()` | Market depth (L5) | Dict |
| `client.intervals()` | Available intervals | Dict |
| `client.optionchain()` | Option chain around ATM | Dict |
| `client.optiongreeks()` | Option greeks (delta, gamma, theta, vega, rho) + IV | Dict |
| `client.expiry()` | Expiry dates for F&O | Dict |
| `client.connect()` | WebSocket connect | None |
| `client.subscribe_ltp()` | Live LTP stream | Callback |
| `client.subscribe_quote()` | Live quote stream | Callback |
| `client.subscribe_depth()` | Live depth stream | Callback |

### Output Folder Structure

Scripts go in appropriate directories, created on-demand. Each category folder is self-contained.

```
charts/
├── sbin_ema_chart.py
├── reliance_rsi_chart.py
└── nifty_supertrend_chart.py
dashboards/
├── sbin_dashboard/app.py
└── multi_timeframe/app.py
custom_indicators/
├── zscore/
│   ├── indicator.py
│   ├── chart.py
│   └── benchmark.py
└── momentum_squeeze/
    └── ...
scanners/
├── rsi_oversold_scanner.py
└── volume_spike_scanner.py
```

## Project Structure

```
.
├── .claude/
│   └── skills/
│       ├── indicator-setup/             # /indicator-setup - Environment setup
│       │   └── SKILL.md
│       ├── indicator-chart/             # /indicator-chart - Chart any indicator
│       │   └── SKILL.md
│       ├── custom-indicator/            # /custom-indicator - Custom indicator builder
│       │   └── SKILL.md
│       ├── indicator-dashboard/         # /indicator-dashboard - Dash/Streamlit web apps
│       │   └── SKILL.md
│       ├── indicator-scanner/           # /indicator-scanner - Multi-symbol scanner
│       │   └── SKILL.md
│       ├── live-feed/                   # /live-feed - WebSocket real-time feed
│       │   └── SKILL.md
│       └── indicator-expert/            # Knowledge base (auto-loaded)
│           ├── SKILL.md                 # Main skill (modular reference hub)
│           └── rules/                   # 12 modular rule files
│               ├── indicator-catalog.md
│               ├── data-fetching.md
│               ├── plotting.md
│               ├── custom-indicators.md
│               ├── websocket-feeds.md
│               ├── performance.md
│               ├── dashboard-patterns.md
│               ├── streamlit-patterns.md
│               ├── multi-timeframe.md
│               ├── signal-generation.md
│               ├── indicator-combinations.md
│               ├── symbol-format.md
│               └── assets/              # Production-ready templates
│                   ├── ema_chart/chart.py
│                   ├── rsi_chart/chart.py
│                   ├── macd_chart/chart.py
│                   ├── supertrend_chart/chart.py
│                   ├── bollinger_chart/chart.py
│                   ├── multi_indicator/chart.py
│                   ├── dashboard_basic/app.py
│                   ├── dashboard_multi/app.py
│                   ├── streamlit_basic/app.py
│                   ├── streamlit_multi/app.py
│                   ├── custom_indicator/template.py
│                   ├── live_feed/template.py
│                   └── scanner/template.py
├── .env.sample                          # Environment template (copy to .env)
├── .gitignore
├── requirements.txt
└── README.md
```

## Rule Files Reference

| Rule File | Description |
|-----------|-------------|
| `indicator-catalog.md` | Complete 100+ indicator reference with signatures, parameters, return types |
| `data-fetching.md` | OpenAlgo history/quotes/depth, yfinance for US, data normalization, option chain |
| `plotting.md` | Plotly candlestick overlays, multi-panel subplots, signal markers, save to HTML |
| `custom-indicators.md` | NumPy + `ta`-primitive composition patterns, single/multi-output, NaN handling, DO/DON'T rules |
| `websocket-feeds.md` | LTP/Quote/Depth subscription, rolling buffer, real-time indicator computation |
| `performance.md` | The Rust core, first-call full speed, O(n) guarantees, TA-Lib benchmarks, NumPy vectorization tips |
| `dashboard-patterns.md` | Dash app structure, dynamic subplots, stats cards, auto-refresh |
| `streamlit-patterns.md` | Streamlit app structure, sidebar inputs, `st.plotly_chart()`, metrics, scanner tables |
| `multi-timeframe.md` | Multiple timeframes, confluence detection, MTF grid chart |
| `signal-generation.md` | 4-step pipeline, crossover/crossunder, exrem cleaning, common patterns |
| `indicator-combinations.md` | Category mixing, 6 combination patterns, confluence analysis |
| `symbol-format.md` | Exchange codes, equity/futures/options format, NSE/BSE index symbols |

## Indicator Categories

| Category | Count | Indicators |
|----------|-------|------------|
| **Trend** | 20 | SMA, EMA, WMA, DEMA, TEMA, HMA, VWMA, ALMA, KAMA, ZLEMA, T3, FRAMA, Supertrend, Ichimoku, Chande Kroll Stop, TRIMA, McGinley, VIDYA, Alligator, MA Envelopes |
| **Momentum** | 9 | RSI, MACD, Stochastic, CCI, Williams %R, BOP, Elder Ray, Fisher Transform, Connors RSI |
| **Volatility** | 16 | ATR, Bollinger Bands, Keltner, Donchian, Chaikin Volatility, NATR, RVI, Ultimate Oscillator, True Range, Mass Index, BB %B, BB Width, Chandelier Exit, Historical Volatility, Ulcer Index, STARC |
| **Volume** | 15 | OBV, OBV Smoothed, VWAP, MFI, ADL, CMF, EMV, Force Index, NVI, PVI, Volume Oscillator, VROC, KVO, PVT, RVOL |
| **Oscillators** | 20+ | CMO, TRIX, UO, Awesome Oscillator, Accelerator, PPO, PO, DPO, Aroon Oscillator, Stochastic RSI, RVI Oscillator, Chaikin Oscillator, Choppiness, KST, TSI, Vortex, Gator, STC, Coppock, ROC |
| **Statistical** | 9 | Linear Regression, LR Slope, Correlation, Beta, Variance, TSF, Median, Mode, Median Bands |
| **Hybrid** | 6+ | ADX, DMI, Aroon, Pivot Points, Parabolic SAR, Williams Fractals, RWI |
| **TA-Lib Compatible** | 18 | MOM, ROCP, ROCR, ROCR100, APO, MIDPOINT, MIDPRICE, AVGPRICE, MEDPRICE, TYPPRICE, WCLPRICE, PLUS_DM, MINUS_DM, DX, ADXR, STOCHF, LINEARREG_ANGLE, LINEARREG_INTERCEPT |
| **Utilities** | 11 | Crossover, Crossunder, Cross, Highest, Lowest, Change, ROC, StdDev, EXREM, FLIP, VALUEWHEN, Rising, Falling |

## Data Sources

| Source | Use Case | Example Symbols | API Key Required |
|--------|----------|-----------------|------------------|
| OpenAlgo | Indian markets (primary) | NSE: SBIN, RELIANCE. NFO: NIFTY30DEC25FUT. NSE_INDEX: NIFTY, BANKNIFTY | Yes (`OPENALGO_API_KEY`) |
| yfinance | US markets, global | AAPL, MSFT, SPY, `^GSPC`, `^NSEI` | No |

## Configuration

Copy the `.env.sample` and fill in your API keys:

```bash
cp .env.sample .env
```

The `.env` file supports:

```
# Indian Markets (OpenAlgo)
OPENALGO_API_KEY=your_openalgo_api_key_here
OPENALGO_HOST=http://127.0.0.1:5000
```

US market data via yfinance does not require an API key.

## License

MIT
