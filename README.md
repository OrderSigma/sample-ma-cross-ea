# sample MA Cross EA

**Languages:** English | [日本語](./README.ja.md)

## 1. Overview

- This Expert Advisor (EA) is a straightforward automated trading system for MetaTrader 4 that initiates trades based on the crossover of two Simple Moving Averages (SMAs). It maintains a single open position and promptly reverses direction when an opposite signal is detected.
- Key features include trailing stops, customizable take‑profit and stop‑loss (TP/SL) levels, spread filtering, and comprehensive log output.
- This EA is provided solely as a portfolio sample. The author assumes no responsibility for any loss or damage arising from its use. Please use this EA at your own risk.

## 2. Features

- **SMA Crossover Strategy**  
  Detects the crossover between the Fast and Slow SMAs and opens a BUY on a golden cross or a SELL on a death cross.

- **Single Position & Instant Reversal (Hard Reverse)**  
  Keeps only one position open; when an opposite signal appears, it closes the current trade and immediately opens a new one in the opposite direction.

- **Customizable TP/SL**  
  Set take‑profit (TP) and stop‑loss (SL) levels in pips; specify 0 to disable either level.

- **Trailing‑Stop Functionality**  
  Separate parameters for Start, Distance, and an optional Step let you automate risk management as profits grow.

- **Spread Filter & Slippage Control**  
  Skips trades when the spread exceeds the specified limit and caps slippage in points during order execution.

- **Arrow Drawing Option**  
  Optionally plots BUY (blue) and SELL (red) arrows on the chart whenever a position is opened.

- **Detailed Log Output**  
  When `LogEnabled` is set to `true`, the EA records signals, order actions, and trailing‑stop updates in the terminal log.

- **Safety Checks**  
  On initialization the EA validates key inputs—such as `Lots > 0`, `FastMA < SlowMA`, and `TrailingDist >= TrailingStep`—to help prevent misconfiguration.

## 3. Input Parameters

| Parameter | Var. Name | Default | Units / Data Type | Notes |
| :-------- | :-------- | :------ | :---------------- | :---- |
| Fast SMA period (must be < Slow)        | FastMAPeriod      | 25   | period / int  | - |
| Slow SMA period                         | SlowMAPeriod      | 75   | period / int  | - |
| Lot size (must be > 0)                  | Lots              | 0.10 | lots / double | - |
| Take Profit (pips)                      | TakeProfitPips    | 30   | pips / double | 0 = disables TP |
| Initial Stop Loss (pips)                | StopLossPips      | 20   | pips / double | 0 = disables SL |
| Max allowed spread (pips)               | MaxSpreadPips     | 2.0  | pips / double | - |
| Max slippage (points)                   | Slippage          | 3    | points / int  | - |
| Start trailing after this profit (pips) | TrailingStartPips | 20   | pips / double | See § 4 “How to Use” for details |
| Distance from price (pips)              | TrailingDistPips  | 20   | pips / double | See § 4 “How to Use” for details |
| Update step (pips), 0 = every tick      | TrailingStepPips  | 0    | pips / double | 0 = every tick |
| Magic number                            | MagicNumber       | 20250630 | - / int   | - |
| Draw BUY/SELL arrows on entry           | DrawArrows        | true     | - / bool  | - |
| Log ON/OFF                              | LogEnabled        | true     | - / bool  | - |

## 4. How to Use

**4‑1. Terminology**
- Fast SMA…short‑term simple moving average
- Slow SMA…long‑term simple moving average

**4-2. Disabling TP/SL**
- To run the EA without a take-profit, set “Take Profit (pips)” to 0.
- To run the EA without a stop-loss, set “Initial Stop Loss (pips)” to 0.

**4-3. Trailing-Stop Settings**
- Enable Trailing-Stop — enter positive values for “Start trailing after this profit (pips)” and “Distance from price (pips)”. The value of “Distance from price” must be greater than or equal to “Update step (pips)”. If the distance value is smaller than the update step, the stop-loss cannot keep up with the price and the EA will return an OrderModify error. Setting “Update step (pips)” to 0 updates the stop-loss on every tick.
- Use only Trailing-Stop — after the above settings, also set “Take Profit (pips)” to 0.
- Disable Trailing-Stop completely — set all three of the following fields to 0:  
“Start trailing after this profit (pips)”,  
“Distance from price (pips)”,  
“Update step (pips), 0 = every tick”.

**4-4. Magic Number (Trade Identifier)**
- When running several charts or multiple EAs in the same account, assign a unique integer to the “Magic Number (Trade Identifier)” parameter for each chart or EA instance. The EA will then manage only its own orders and positions.
- Example: EURUSD → 101, GBPUSD → 102, backtest → 1001

**4-5. Improving Chart Visibility**
- The EA calculates SMAs internally but does not plot them on the chart. If you want to see the crossover, manually add fast and slow SMA indicators to the chart.

**4-6. Note**
- The momentary ZigZag log entries you see in the Journal are generated by a colorless ZigZag indicator added solely to reduce the Strategy Tester’s rendering speed; they have no connection to this EA’s logic.

## 5. Backtest Preview

- The GIF below is a visual reference that demonstrates how the EA executes trades. Because this EA is intended solely as a portfolio sample, no detailed backtesting has been performed.

<img src="images/SampleEA_GIF_Journal18f150_64d_nodither_l20.gif" width="700">

<img src="images/SampleEA_GIF_Results18f200_256_nodither_l20.gif" width="700">

## 6. Requirements

|                    |                                                              |
| :----------------- | :----------------------------------------------------------- |
| Platform           | MetaTrader 4, Build 1350 or later (latest build recommended) |
| Broker Digits      | Compatible with both 4- and 5-digit brokers                  |
| Account Type       | Standard account (hedging allowed)                           |
| OS                 | Windows 10/11 (64-bit) recommended                           |
| VPS                | Stable connection or VPS (ping < 100 ms)                     |
| External Libraries | None (EA is self-contained)                                  |
| Permission         | Enable AutoTrading in MT4                                    |

## 7. License

- For the full MIT License text, please refer to the [`LICENSE`](./LICENSE) file (layout may appear distorted on some mobile devices, but the content remains unchanged).
