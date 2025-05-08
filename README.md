# MQL5 Expert Advisor: CapitalFlowEA

**Version:** 1.5 (See `CapitalFlowEA.mq5` for latest)
**Author:** [Your Name/Company Name]
**Contact:** [Your Email or Website]

## Overview

This repository contains the MQL5 source code for the `CapitalFlowEA`, an Expert Advisor designed for MetaTrader 5. The EA aims to identify potential trading opportunities on the **EURUSD** currency pair by analyzing capital flows across a basket of related USD and EUR currency pairs.

The core strategy involves monitoring the **Money Flow Index (MFI)** and other confirmation indicators (OBV, MA) across multiple timeframes for selected currency pairs to gauge the relative strength or weakness of the USD and EUR. When significant capital flow divergence is detected (e.g., money flowing out of USD pairs and into EUR pairs), the EA generates corresponding Buy or Sell signals for EURUSD.

## Features

* **Multi-Symbol Analysis:** Monitors a configurable list of USD and EUR based currency pairs (Default: `EURUSD,GBPUSD,AUDUSD,NZDUSD,USDCAD,USDCHF,USDJPY,EURJPY,EURGBP,EURAUD,EURCHF`).
* **Multi-Timeframe Indicators:** Calculates MFI, OBV, and a Moving Average (proxy for VWMA) on M15, H1, H4, and D1 for each monitored symbol.
* **Capital Flow Logic:** Aggregates indicator data to generate relative 'Currency Strength' indices for EUR and USD.
* **Signal Generation:** Creates EURUSD Buy/Sell signals when EUR strength is high & USD strength is low (for Buy) or vice-versa (for Sell), subject to confirmation and filters.
* **Confirmation Indicators:** Uses On-Balance Volume (OBV) slope and price position relative to a Moving Average (MA) to confirm primary signals. (Note: CMF requires a custom indicator implementation and is currently disabled).
* **Dynamic Lot Sizing:** Calculates position size based on account balance using a configurable factor (`InpRiskBalanceFactor`).
* **Comprehensive Risk Management:**
    * Stop Loss: Fixed points or ATR-based (`InpStopLossPoints`, `InpAtrSlMultiplier`).
    * Take Profit: Fixed points or ratio-based to SL (`InpTakeProfitPoints`, `InpTpRatioToAtrSl`).
    * ATR Trailing Stop: Optional trailing stop based on Average True Range (`InpUseAtrTrailingStop`, `InpAtrPeriodTS`, `InpAtrMultiplierTS`).
    * Money Flow Reversal Exit: Option to close trades if the underlying capital flow signal reverses strongly (`InpUseMoneyFlowExit`, `InpExitSignalThreshold`).
    * Max Trade Duration: Optional time limit for open trades (`InpMaxTradeDurationHours`).
    * Daily Loss Limit: Stops trading for the day if loss exceeds a percentage of the starting balance (`InpMaxDailyLossPercent`).
    * Max Drawdown Limit: Stops trading permanently (requires reset) if equity drawdown from peak exceeds a percentage (`InpMaxDrawdownPercent`).
* **Filters:** Includes basic volatility (ATR) and spread filters to avoid trading in unfavorable conditions (`InpMinVolatilityATR`, `InpMaxSpreadPoints`).
* **Time Filter:** Configurable trading window (disabled by default in sample, but logic can be uncommented).

## Requirements

* **Platform:** MetaTrader 5 (MT5)
* **Broker:** An ECN or Standard account with access to the specified currency pairs. Ensure sufficient history is available for all monitored symbols and timeframes.
* **Symbols:** All symbols listed in the `InpMonitoredSymbols` input must be enabled and visible in your MT5 Market Watch.

## Installation

1.  **Download:** Download the `CapitalFlowEA.mq5` file (and any necessary include files if separated later).
2.  **Open MetaEditor:** In MT5, go to `Tools` > `MetaQuotes Language Editor` (or press F4).
3.  **Navigate:** In MetaEditor's "Navigator" panel (usually on the left), expand `Experts`.
4.  **Copy File:** Right-click on `Experts` and select `Open Folder`. Copy the downloaded `.mq5` file into this folder.
5.  **Refresh/Compile:** Back in MetaEditor, right-click on `Experts` in the Navigator panel and select `Refresh`. Double-click `CapitalFlowEA.mq5` to open it. Click the `Compile` button (or press F7). Check the "Errors" tab at the bottom for any compilation issues.

## Usage

1.  **Open Chart:** Open a chart for the `EURUSD` currency pair (or the symbol specified in `InpTradeSymbol`) in your MT5 terminal. The timeframe of this chart does *not* affect the EA's analysis timeframes (which are set in the inputs).
2.  **Attach EA:** Find `CapitalFlowEA` under "Expert Advisors" in the MT5 "Navigator" panel. Drag and drop it onto the EURUSD chart.
3.  **Configure Inputs:** The EA's input parameter window will appear. Adjust the settings carefully:
    * **Trading:** Set `InpMagicNumber` (unique for each EA instance/chart), `InpRiskBalanceFactor`.
    * **Indicators:** Adjust periods if needed.
    * **Signals:** Tune thresholds based on backtesting/optimization.
    * **Risk Management:** **CRITICAL:** Set appropriate SL, TP, trailing stop, and account protection limits (`InpMaxDailyLossPercent`, `InpMaxDrawdownPercent`).
    * **Analysis:** Verify the list of `InpMonitoredSymbols` matches available symbols and desired analysis scope. Choose analysis timeframes.
4.  **Enable Algo Trading:** Ensure the "Algo Trading" button in the main MT5 toolbar is enabled (green).
5.  **Check EA Status:** Verify the EA icon in the top-right corner of the chart has a "happy face" (🙂), indicating it's running correctly. Check the "Experts" tab in the Terminal window for initialization messages or errors.
