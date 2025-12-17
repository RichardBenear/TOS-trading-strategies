# PMO Coherent Pulse Strategy

**Author:** Richard Benear  
**Platform:** Thinkorswim (ThinkScript)  
**Timeframes:** Optimized for 5-minute and 10-minute charts  
**Market Type:** Liquid index ETFs / futures (SPY, QQQ, ES, NQ, etc.)  
**Trade Direction:** Long & Short (regime-aware)

---

## Overview

**PMO Coherent Pulse** is an intraday momentum strategy built around a **multi-band Price Momentum Oscillator (PMO) filter bank** with *coherent detection*.  
Rather than relying on a single oscillator or crossover, the strategy requires **simultaneous agreement across three PMO bandwidths**, dramatically reducing noise and false signals.

The system is further protected by **regime gating**, **opening gap detection**, and **volatility-based fast exits**, making it particularly effective in **5–10 minute intraday environments**.

---

## Core Concepts

### 1. Multi-Band PMO Filter Bank

The strategy uses **three PMO bands** (low, mid, high) for both long and short signals:

- Each PMO is a **2-pole IIR filter**
- Bands are tuned to different effective periods
- A signal is valid **only when all three bands agree**

This behaves similarly to a **coherent detector** in communications theory, where multiple filtered channels must confirm a signal before it is accepted.

**Result:**  
✔ Noise rejection  
✔ Reduced whipsaws  
✔ Cleaner trend identification

---

### 2. Coherent Trend Detection

A trend is defined only when:

- PMO > Signal line across **all three bands** (long)
- PMO < Signal line across **all three bands** (short)

If any band disagrees, no new trade is initiated.

This prevents:
- Over-trading
- Entries during weak or transitional momentum
- False breakouts

---

### 3. Regime Gating (Channel / Squeeze Filter)

The strategy includes a **TTM-style squeeze detector**:

- Bollinger Bands vs Keltner Channels
- When Bollinger Bands are inside Keltner → **market is in a channel**
- **New entries are blocked** during these periods
- Existing trades are still allowed to exit

**Why this matters:**  
Most momentum strategies fail during compression. This filter avoids trading when momentum edges are statistically weakest.

---

### 4. Gap-Down Protection (Short Trades)

Short entries are optionally blocked after large **gap-down opens**, recognizing modern **buy-the-dip (BTFD)** behavior.

- Detects gap % from prior close
- Blocks shorts for a programmable number of bars
- Prevents fading strong institutional dip buying

---

### 5. Fast Momentum Exits (ATR-Based)

Markets fall faster than they rise and often form sharp **V-bottoms**.

To handle this asymmetry:
- Long exits trigger on large down bars (> ATR multiple)
- Short exits trigger on large up bars (> ATR multiple)

This:
✔ Protects profits  
✔ Reduces drawdowns  
✔ Avoids holding through violent reversals  

---

## Trade Logic Summary

### Entry Conditions
- Within active trading hours
- Not already in a position
- Not in a squeeze / channel
- All three PMO bands agree
- Gap-down protection (shorts only)

### Exit Conditions
- PMO coherence breaks
- ATR-based fast exit triggers
- End-of-day flatting logic

---

## Key Inputs

| Parameter | Description |
|---------|-------------|
| `pmoSpeedMult` | Global speed control for PMO responsiveness |
| `longSignalSensitivity` | Long signal smoothing factor |
| `shortSignalSensitivity` | Short signal smoothing (faster by design) |
| `gapBlockBars` | Number of bars to block shorts after gap down |
| `gapPctThreshold` | Minimum gap % to trigger protection |
| `inChannelLength` | Lookback for squeeze detection |

---

## Recommended Usage

✔ Best on **5-minute and 10-minute charts**  
✔ Liquid instruments with tight spreads  
✔ Intraday session only  
✖ Not designed for daily charts  
✖ Not intended for low-liquidity stocks  

---

## Design Philosophy

This strategy borrows heavily from **digital signal processing** principles:

- Filter banks
- Coherent detection
- Noise rejection
- Regime-aware gating

Rather than predicting price, it focuses on **detecting when momentum is real and tradable**.

---

## Disclaimer

This script is provided for **educational and research purposes only**.  
It is not financial advice. Past performance does not guarantee future results.

---

## Version History

- **v1.0** – Initial public release  
- Multi-band PMO coherence  
- Channel gating  
- Gap-down protection  
- ATR fast exits  

---

## Acknowledgments

- PMO concept: Carl Swenlin
- FPL Extended P&L framework: EddieLee394
- Signal-processing inspiration: classical DSP & demodulation theory
