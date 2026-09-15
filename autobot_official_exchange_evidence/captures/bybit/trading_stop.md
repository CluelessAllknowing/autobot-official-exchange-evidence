# Bybit V5 Trading Stop
Official URL: https://bybit-exchange.github.io/docs/v5/position/trading-stop  
Captured UTC: 2026-09-15T21:44:15Z  
Applicability: category=linear, position TP/SL/trailing stop.

- D: POST `/v5/position/trading-stop` creates conditional orders internally.
- D: Bybit says those internal conditional orders are cancelled if the position closes and their quantity is adjusted with open-position size.
- D: `tpslMode=Full` represents entire-position TP/SL; `Partial` represents partial-position TP/SL.
- D: Full mode can modify existing full TP/SL; Partial mode can only add partial TP/SL.
- D: One-sided modification of a paired TP/SL can break the binding relationship, after which cancelling one order ID cancels only that side.
- D: `positionIdx=0` is one-way; 1/2 are hedge-side indexes.
- D: Parameters include `stopLoss`, `slTriggerBy`, `trailingStop`, `activePrice`, `slSize`, `slLimitPrice`, and `slOrderType` subject to mode constraints.
- D: Full mode supports Market TP/SL order type; limit TP/SL settings are partial-mode constraints.
- E: The mutation response contains no protective-order identity. Positive mutation response alone therefore does not establish durable stop coverage; readback/stream reconciliation is required.
