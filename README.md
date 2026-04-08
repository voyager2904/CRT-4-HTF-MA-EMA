# CRT-4-HTF-MA-EMA

# CRT Sweep Signal v14 — Pine Script Indicator

![TradingView](https://img.shields.io/badge/Platform-TradingView-blue)
![Pine Script](https://img.shields.io/badge/Pine%20Script-v6-orange)
![Timeframe](https://img.shields.io/badge/Timeframe-4H%20Optimised-green)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

A professional **Candle Range Theory (CRT)** sweep indicator for TradingView, optimised for the **4H timeframe on XAUUSD (Gold)**. Built on ICT (Inner Circle Trader) concepts — detects liquidity sweeps and potential reversals with confluence from three EMAs.



## What is CRT?

Candle Range Theory is an ICT concept that identifies how institutional traders manipulate price:

```
Step 1 — C1 (Reference candle)
  A large candle forms, setting a range
  Its HIGH = Buy-Side Liquidity (BSL)
  Its LOW  = Sell-Side Liquidity (SSL)

Step 2 — Manipulation
  Subsequent candles stay inside C1 range
  Then price wicks or gaps BEYOND C1 boundary
  Triggering retail stop losses

Step 3 — Displacement (Signal fires here)
  Price closes back inside C1 range
  Confirming the sweep was manipulation
  A Fair Value Gap (FVG) is often left behind
```

---

## Features

- ✅ Detects bullish and bearish CRT sweeps automatically
- ✅ Catches both **wick sweeps** and **gap sweeps** (open beyond C1)
- ✅ Cooldown system prevents back-to-back signal spam
- ✅ Optional strict inside check for textbook setups only
- ✅ Three EMAs (20 / 50 / 200) as trend reference lines
- ✅ Optional C1 zone box visualisation
- ✅ Optional price labels with pointer lines
- ✅ Info table showing EMA context and C1 levels
- ✅ TradingView alerts for bull and bear sweeps

---

## Installation

1. Open **TradingView** and navigate to any chart
2. Click **Pine Editor** at the bottom of the screen
3. Press `Ctrl+A` (Windows) or `Cmd+A` (Mac) to select all
4. Press `Delete` to clear the editor
5. Copy the contents of [`src/CRT_Sweep_Signal_v14.pine`](src/CRT_Sweep_Signal_v14.pine)
6. Paste into Pine Editor
7. Click **Save** then **Add to chart**
8. Set chart timeframe to **4H** for best results

---

## Settings

### CRT Settings

| Parameter | Default | Description |
|---|---|---|
| C1 min size x ATR | 1.2 | How large C1 must be relative to ATR. Raise to 2.0+ for fewer, higher quality signals |
| Lookback bars for C1 | 15 | How many bars back to search for C1. Use 20 on 4H, 10 on 1H, 6 on 15M |
| Min bars between signals | 4 | Cooldown after signal fires. Prevents spam during volatile moves |
| Strict inside check | OFF | When ON: all candles between C1 and sweep must stay inside C1 range |

### Visual Settings

| Parameter | Default | Description |
|---|---|---|
| Show C1 zone box | OFF | Draws a dashed blue rectangle showing the C1 candle range |
| Show price labels | OFF | Draws BUY/SELL labels with pointer lines. OFF = arrows only |
| Label distance | 5 | How many bars to the right the label sits |

### Moving Averages

| EMA | Colour | Purpose |
|---|---|---|
| 20 EMA | Cyan | Equilibrium / fair value. Best CRT entries happen near the 20 EMA |
| 50 EMA | Orange | Intermediate trend. Price above = bullish bias |
| 200 EMA | Purple | Macro trend. Price below = major downtrend, be cautious with longs |

---

## Recommended Settings by Timeframe

| Timeframe | C1 min ATR | Lookback | Cooldown |
|---|---|---|---|
| 5 minute | 1.0 — 1.2 | 6 — 8 | 3 — 4 |
| 15 minute | 1.0 — 1.2 | 8 — 10 | 4 |
| 1 hour | 1.0 — 1.5 | 10 — 12 | 4 — 5 |
| **4 hour** | **1.2 — 1.5** | **15 — 20** | **4 — 6** |
| Daily | 0.8 — 1.0 | 15 | 2 — 3 |

---

## Trade Validation Checklist

Before acting on any CRT signal, verify:

- [ ] **200 EMA** — Is price above (bullish) or below (bearish) the purple line?
- [ ] **50 EMA** — Does the 50 EMA slope agree with the signal direction?
- [ ] **20 EMA** — Is the signal firing near the cyan line? (highest probability setup)
- [ ] **C1 box** — Turn on "Show C1 zone box" and verify the setup looks clean
- [ ] **Sweep candle** — Did it close convincingly back inside C1 range?
- [ ] **FVG** — Is there a Fair Value Gap left by the displacement candle?



## Setting Up Phone Alerts

1. In TradingView click the **Alerts bell** icon (top toolbar)
2. Click **Create Alert**
3. Set Condition to: `CRT v14` → `CRT Bull Sweep` or `CRT Bear Sweep`
4. Enable **Push Notification**
5. Click **Save**
6. Repeat for each pair (XAUUSD, EURUSD, GBPUSD, AUDUSD)
7. Download the **TradingView mobile app** and log in



## Signal Logic

### Bullish Sweep (BUY signal — green triangle below candle)
```
Condition 1: C1 candle found in lookback window
Condition 2: Inside check passed
Condition 3: low < C1_low  OR  open < C1_low  (wick or gap below SSL)
Condition 4: close > C1_low  (close back above SSL = confirmation)
```

### Bearish Sweep (SELL signal — red triangle above candle)

Condition 1: C1 candle found in lookback window
Condition 2: Inside check passed
Condition 3: high > C1_high  OR  open > C1_high  (wick or gap above BSL)
Condition 4: close < C1_high  (close back below BSL = confirmation)


### Why OR open condition?
The `OR open` catches **gap sweeps** — when a candle opens beyond C1 boundary and closes back inside. Gap sweeps are stronger signals than wick sweeps because the open price itself is at an extreme, indicating more institutional participation.



## ICT Moving Average Rules

The three EMAs are reference lines only — they do not filter signals. Use them to evaluate context:

| Condition | Action |
|---|---|
| Price above ALL three EMAs | Strong uptrend — only consider BUY signals |
| Price below ALL three EMAs | Strong downtrend — only consider SELL signals |
| Price between EMAs | Choppy market — reduce size or wait |
| CRT sweep fires AT the 20 EMA | Highest probability — institutional fair value + sweep confluence |
| 200 EMA below price for months | Major bull trend — aggressive buying of CRT lows is valid |



## Alert Message Format


Bull: CRT BUY XAUUSD 240 close:4521.50 time:2026-04-07T08:00:00
Bear: CRT SELL XAUUSD 240 close:4521.50 time:2026-04-07T08:00:00
```

The alert message includes ticker, timeframe, close price and timestamp — ready to use with webhook integrations.



## Files

```
CRT_Indicator/
├── README.md                          ← This file
├── LICENSE                            ← MIT License
├── src/
│   └── CRT_Sweep_Signal_v14.pine     ← Main indicator code
├── docs/
│   ├── SETTINGS.md                   ← Detailed settings guide
│   └── ICT_CONCEPTS.md               ← CRT and ICT theory explained
└── images/
    └── chart_example.png             ← Example chart screenshot
```
