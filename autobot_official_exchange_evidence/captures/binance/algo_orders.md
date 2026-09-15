# Binance USD-M conditional Algo orders
Official pages: New Algo Order; Query Algo Order; User Data Streams; official change log  
URLs: https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/trade/New-Algo-Order ; https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/trade/Query-Algo-Order ; https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/user-data-streams ; https://developers.binance.com/zh-CN/docs/products/derivatives-trading-coin-futures/change-log  
Captured UTC: 2026-09-15T21:44:15Z  
Displayed update: User Data Streams 2026-08-08; change log rolling.  
Applicability: standard USD-M Futures.

- D: Official change history documents migration of USD-M conditional order types to the Algo service, including STOP_MARKET, TAKE_PROFIT_MARKET, STOP, TAKE_PROFIT and TRAILING_STOP_MARKET.
- D: Query Algo Order accepts an algorithm identity and exposes `algoId`, `clientAlgoId`, `algoType`, `orderType`, `symbol`, `side`, `positionSide`, `timeInForce`, `quantity`, `algoStatus`, and trigger/price-related settings.
- D: Query readback exposes `actualOrderId`; before trigger it is empty, and after successful trigger it links to the resulting exchange order.
- D: The query also documents actual execution fields that become meaningful after triggering/fill, including actual price/type/quantity as applicable.
- D: ALGO_UPDATE status sequence includes NEW, TRIGGERING, TRIGGERED and terminal/other states CANCELED, FINISHED, REJECTED, EXPIRED.
- D: Official change history documents an `ia` activation-state addition for trailing-stop algo updates in 2026, so a trailing stop can emit NEW updates before trigger with differing activation state.
- D: Official migration notes say conditional orders do not perform margin check before trigger; a later matching-engine rejection remains possible.
- E: A pre-trigger NEW algo readback proves an accepted, not-yet-triggered algo record; it is not by itself a guarantee that position closure will later occur.
- E: For protection reconciliation, the strongest documented linkage is algo identity -> `actualOrderId` after trigger -> ordinary order/private-order state -> position state.
