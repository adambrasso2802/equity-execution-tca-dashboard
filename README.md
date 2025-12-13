# Equity Execution TCA Dashboard

Excel-based equity execution dashboard on synthetic orders, fills and intraday OHLCV data.

- Computes per-order VWAP, implementation shortfall (bps), venue mix (lit/dark/block) and fees.
- Uses filters by ticker and urgency to show KPIs and charts on the `99_Dashboard` sheet.
- All data is synthetic and included only for demonstration.

# Do This First
- Open `Equity_Execution_TCA_Dashboard.xlsx`
- Go to `99_Dashboard`
- Use dropdown filters: **Ticker** and **Urgency**
- Review KPI tiles and charts (implementation shortfall, fees, fill rate, venue mix)

# Sheet map and data model
- `01_orders`(parent orders; order window; arrival/end; urgency; benchmark)
- `02_Fills`(child fills; venue type; fee; notional)
- `03_OHLCV_5min`(intraday bars; used for market VWAP/benchmark context)
- `10_TCA_calc`(order-level aggregation + metrics)
- `20_Scenarios`(assumptions: fees/slippage by venue; urgency rules)
- `99_Dashboard`(filters + KPIs + charts)

File:
- [Equity_Execution_TCA_Dashboard.xlsx](./Equity_Execution_TCA_Dashboard.xlsx)

# Dashboard Preview
<img width="1870" height="550" alt="dashboard" src="https://github.com/user-attachments/assets/25a99911-9d39-46f6-af8b-75cb974f2483" />

# Assumptions and Limitations
- Synthetic dataset (demo only)
- No bid/ask spread model (uses prices directly)
- Slippage/fees are simplified (venue assumptions)
- Not a trading strategy; execution/TCA style aggregation only
- “Market VWAP” definition depends on your OHLCV windowing (state how)
