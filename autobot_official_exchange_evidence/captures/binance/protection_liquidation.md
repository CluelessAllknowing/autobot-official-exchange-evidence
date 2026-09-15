# Binance USD-M protection, risk tiers and liquidation evidence
Official URLs: https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/trade/Query-Algo-Order ; https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/trade/New-Algo-Order ; https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/account/Position-Information-V3 ; https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/account/Notional-and-Leverage-Brackets ; https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/user-data-streams  
Captured UTC: 2026-09-15T21:44:15Z  
Applicability: standard USD-M Futures.

- D: Current conditional-order readback is through the Algo order contract; after trigger `actualOrderId` links to the resulting ordinary order.
- D: Trigger-basis configuration includes working-price semantics (MARK_PRICE or CONTRACT_PRICE) in the USD-M conditional-order/user-stream documentation.
- D: Position V3 exposes current mark price and liquidation price.
- D: Notional/leverage brackets expose risk brackets including initial leverage, notional floor/cap, maintenance-margin ratio and cumulative bracket adjustment.
- D: ALGO_UPDATE can become REJECTED at trigger time, including margin-check failure; this prevents treating accepted pre-trigger status as an execution guarantee.
- E: The reviewed current developer pages did not provide a complete closed-form liquidation formula sufficient to reproduce every standard USD-M isolated liquidation price from local inputs.
- E: Therefore AutoBot must not assume liquidation is a fixed percentage below entry or stop. The safe contract is to consume exchange readback and risk-tier data and require an explicit safety separation.
