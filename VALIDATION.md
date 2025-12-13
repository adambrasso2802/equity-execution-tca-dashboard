# Evidence of correctness

- `SUM(fills.fill_qty by order_id) == filled_qty` in `10_TCA_Calc`
- `fill_notional ≈ SUM(fill_qty × fill_price)`
- Fees bps recompute correctly from fee_cost and notional
