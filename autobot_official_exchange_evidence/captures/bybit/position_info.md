# Bybit V5 Position Info
Official URLs: https://bybit-exchange.github.io/docs/v5/position ; https://bybit-exchange.github.io/docs/v5/acct-mode  
Captured UTC: 2026-09-15T21:44:15Z  
Applicability: category=linear, Unified Trading Account.

- D: GET `/v5/position/list` for linear requires `symbol` or `settleCoin`; page size is 1-200, default 20, and continuation uses `nextPageCursor`.
- D: `positionIdx` is 0 one-way, 1 hedge buy side, 2 hedge sell side.
- D: `side` is Buy for long and Sell for short; empty position side is an empty string. `size` is always positive, so direction comes from `side`.
- D: Fields include symbol, side, size, positionIdx, riskId, riskLimitValue, avgPrice, positionValue, autoAddMargin, positionStatus, leverage, breakEvenPrice, markPrice, liqPrice, positionIM/MM, takeProfit, stopLoss, trailingStop, unrealised/realised PnL and timestamps.
- D: `autoAddMargin` 0/1 reports whether automatic margin addition is enabled for isolated mode.
- D: Isolated liquidation price is provided as a position liquidation-price readback subject to min/max-price empty-string conditions; cross liquidation price is described as estimated; portfolio margin returns no liquidation price.
- D: `seq` is a cross sequence; Bybit documents `seq + symbol` as a unique update association key and notes settings changes can update it.
- D: Under UTA 2.0, Bybit's account-mode migration page says the meaning/use of `tradeMode`, `liqPrice`, and `bustPrice` changed and that cross/isolated mode moved to account dimension.
- E: For UTA 2.0, use Account Info `marginMode` to prove isolated account mode; do not use legacy per-position `tradeMode` as the authoritative proof.
