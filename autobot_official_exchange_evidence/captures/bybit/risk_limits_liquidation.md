# Bybit V5 risk limits and liquidation evidence
Official URLs: https://bybit-exchange.github.io/docs/v5/market/risk-limit ; https://bybit-exchange.github.io/docs/v5/position ; https://bybit-exchange.github.io/docs/v5/account/wallet-balance  
Captured UTC: 2026-09-15T21:44:15Z  
Applicability: linear USDT contracts, isolated-margin intended mode.

- D: GET `/v5/market/risk-limit` covers USDT/USDC/inverse contracts and returns risk ID, symbol, riskLimitValue, maintenanceMargin, initialMargin, maxLeverage, tier flags/deductions and pagination cursor.
- D: For category=linear, risk-limit results are paginated at 15 symbols per response.
- D: Position Info returns `markPrice`, `liqPrice`, `positionIM` and `positionMM`; isolated liquidation price is exchange readback and can be empty outside symbol min/max bounds.
- D: Wallet documentation says account-wide margin fields are not applicable to isolated margin.
- E: The reviewed official V5 developer pages do not give a single complete formula/qualification set sufficient to prove a fixed percentage relationship between an isolated-position stop and liquidation price under all fee/funding/tier states.
- E: AutoBot must therefore compare current exchange liquidation-price readback to intended protection and enforce a buffer instead of assuming a constant liquidation distance.
