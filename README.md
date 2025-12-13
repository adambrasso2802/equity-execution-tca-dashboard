# Equity Execution TCA Dashboard

Excel-based equity execution dashboard on synthetic orders, fills and intraday OHLCV data.

- Computes per-order VWAP, implementation shortfall (bps), venue mix (lit/dark/block) and fees.
- Uses filters by ticker and urgency to show KPIs and charts on the `99_Dashboard` sheet.
- All data is synthetic and included only for demonstration.

# Do this first
- Open `Equity_Execution_TCA_Dashboard.xlsx`
- Go to `99_Dashboard`
- Use dropdown filters: **Ticker** and **Urgency**
- Review KPI tiles and charts (implementation shortfall, fees, fill rate, venue mix)

# Sheet map and data model
- `01_Orders`(parent orders; order window; arrival/end; urgency; benchmark)
- `02_Fills`(child fills; venue type; fee; notional)
- `03_OHLCV_5min`(intraday bars; used for market VWAP/benchmark context)
- `10_TCA_calc`(order-level aggregation + metrics)
- `20_Scenarios`(assumptions: fees/slippage by venue; urgency rules)
- `99_Dashboard`(filters + KPIs + charts)

File:
- [Equity_Execution_TCA_Dashboard.xlsx](./Equity_Execution_TCA_Dashboard.xlsx)

# Dashboard preview
<img width="1870" height="550" alt="dashboard" src="https://github.com/user-attachments/assets/25a99911-9d39-46f6-af8b-75cb974f2483" />

## Assumptions & limitations

- **Demo dataset:** Inputs are **synthetic** and intended to demonstrate an end-to-end TCA workflow (not live broker/exchange data).

- **Order ↔ fill linkage:** Each fill is mapped to a parent order via `order_id`. Order-level metrics aggregate all fills with the same `order_id`.

- **Time windowing:** Benchmarks are computed over the **order window** (`start_dt` → `end_dt`) and are assumed to be in one consistent timezone.

- **Market VWAP benchmark:** “Market VWAP” is approximated from `03_OHLCV_5min` bars for the same ticker within the order window using:
  - **Market VWAP = Σ(pv) / Σ(volume)**, where **pv = close × volume** and bars satisfy **start_dt ≤ dt ≤ end_dt**.

- **Fill VWAP:** Executed VWAP is computed from fills only:
  - **Fill VWAP = Σ(fill_price × fill_qty) / Σ(fill_qty)**.

- **Implementation shortfall (IS) sign convention:** IS is reported in **bps** as a **cost to the trader** (positive = worse execution):
  - **Buy:** `IS (bps) = 10,000 × (Fill VWAP − Arrival Price) / Arrival Price`
  - **Sell:** `IS (bps) = 10,000 × (Arrival Price − Fill VWAP) / Arrival Price`

- **Fees:** Fees are venue-assumed and applied at fill level using `20_Scenarios` rates:
  - **LIT 0.15 bps, DARK 0.10 bps, BLOCK 0.05 bps**
  - `fee_cost = notional × fee_bps / 10,000`, and order-level **fees (bps)** are aggregated as
    `10,000 × Σ(fee_cost) / Σ(notional)`.

- **Venue mix:** Lit/Dark/Block mix is computed as the **share of filled quantity** by venue type.

- **Dislocation metric:** Dislocation is defined as the **stock return over the order window minus the benchmark return** over the same window (using arrival → end prices for each).

- **Scenario recommendations:** Recommended block allocation (`rec_block_pct`) is derived from urgency-based base rates in `20_Scenarios` with additive adjustments for urgency/stress conditions; it is a heuristic demonstration rather than an optimised allocator.

- **Limitations:** The model does not include full microstructure effects (bid/ask, spread dynamics, queue position), latency, market impact models, or real execution routing constraints. This is **execution analytics/TCA**, not a trading strategy backtest.
