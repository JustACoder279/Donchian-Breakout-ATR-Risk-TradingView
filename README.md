# Donchian Breakout + ATR Risk (Robust Trend) — TradingView Pine v5

A simple trend-following breakout strategy using a Donchian Channel (range breakout), ATR-based regime filter, and ATR-based risk management (stop + optional trailing stop).

> ⚠️ Note: Backtest results vary heavily by asset, timeframe, fees, and date range. The author observed strong results on BTC at Weekly timeframe, but that does **not** guarantee similar performance elsewhere.

---

## Strategy Summary (Plain English)

This strategy:
- Draws a Donchian Channel:
  - **Upper** = highest high over last `dcLen` bars
  - **Lower** = lowest low over last `dcLen` bars
- **Enters Long** when price closes above the previous upper channel (`upper[1]`)
- **Enters Short** when price closes below the previous lower channel (`lower[1]`)
- Optional **EMA Trend Filter**:
  - Only take longs if price is above EMA (`emaLen`)
  - Only take shorts if price is below EMA
- **Volatility (Regime) Filter** using ATR:
  - Only trade when ATR is at least `atrMult * SMA(ATR, atrBaseLen)`
- Risk is managed with:
  - **ATR Stop** (`riskATR`)
  - Optional **ATR Trailing Stop** (`trailATR`)
  - Optional fixed **RR target** mode

---

## Files

- `donchian_breakout.pine` — Pine v5 strategy script

---

## Recommended Settings

### Best-known configuration (observed)
- Symbol: **BTC (TradingView BTCUSD / BTC)**  
- Timeframe: **1D (Daily)**  
- Donchian Length: `55`
- EMA Filter: `200`, enabled
- ATR Regime Filter: `atrLen=14`, `atrBaseLen=50`, `atrMult=1.0`
- Stop: `riskATR=2.0`
- Trailing: enabled, `trailATR=2.5`
- Commission: set realistically (0.02% is optimistic for many venues)

> This configuration produced ~410% return in one observed backtest window.
> Different windows or fees can change results dramatically.
> My simulation was done using a initial startup capital of $152,364.78

---

## How to Use in TradingView (Day-to-Day)

### 1) Add the strategy to a chart
1. Open TradingView chart
2. Open **Pine Editor**
3. Paste code, save, click **Add to chart**
4. Set your timeframe to your target (e.g., Weekly for BTC)

### 2) Confirm Strategy Tester assumptions (VERY IMPORTANT)
Open **Strategy Tester → Settings**:
- Commission: match your exchange (crypto often higher than 0.02%)
- Slippage: add realistic slippage (especially on lower timeframes)
- Order size: set position sizing rules you will actually use

### 3) Paper Trade (recommended before live)
- Use TradingView’s **Paper Trading** panel
- The strategy will simulate fills in the tester
- Track behavior for at least a few weeks of signals (weekly signals are rare)

### 4) Live / Semi-Auto Execution via Alerts
TradingView strategies do not place trades automatically at most brokers unless you:
- Create alerts (strategy order alerts)
- Send alerts to a webhook bot (or manually execute)

To do this:
1. Click **Alerts**
2. Create an alert for the strategy
3. Use “**Order fills**” / “**Strategy order alerts**” (depending on your UI)
4. Optionally configure **webhook URL** for automation

---

## Automation Notes (Webhook Bot)

If you want actual automation:
- Your bot needs to:
  - Receive TradingView webhook alert
  - Parse signal (LONG/SHORT/EXIT)
  - Place trade with your exchange/broker API
  - Handle errors, retries, and risk controls

Recommended:
- Start with paper trading alerts first
- Then switch to live with small size
- Always use exchange-side stop-loss if possible

---

## Limitations / Reality Checks

- High returns on one asset/timeframe may be due to:
  - strong trending period
  - specific backtest window
  - unrealistic fees/slippage
  - survivorship bias / curve fit

- Donchian breakouts can suffer during sideways regimes (whipsaw).
- This is a **baseline strategy**, not a guarantee of profit.

---

## License
MIT 
