# Binance USD-M account and position contracts
Official URLs: https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/account/Get-Current-Position-Mode ; https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/trade/Change-Position-Mode ; https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/trade/Change-Margin-Type ; https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/account/Account-Information-V3 ; https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/account/Position-Information-V3  
Captured UTC: 2026-09-15T21:44:15Z  
Applicability: standard USD-M Futures.

- D: GET `/fapi/v1/positionSide/dual` returns `dualSidePosition`; `false` is one-way and `true` is hedge mode.
- D: Changing position mode uses POST `/fapi/v1/positionSide/dual`; current documentation notes UM/CM position-mode synchronization constraints.
- D: Margin type mutation uses POST `/fapi/v1/marginType` with ISOLATED or CROSSED.
- D: Account Information V3 includes per-position `symbol`, `positionSide`, `positionAmt`, `unrealizedProfit`, `isolatedMargin`, `notional`, `isolatedWallet`, `initialMargin`, `maintMargin`, and `updateTime`.
- D: Position Information V3 returns `positionAmt` with signed semantics: positive is long and negative is short.
- D: Position V3 also exposes `entryPrice`, `breakEvenPrice`, `markPrice`, `unRealizedProfit`, `liquidationPrice`, `isolatedMargin`, `notional`, `marginAsset`, `isolatedWallet`, initial/maintenance-margin fields, open-order initial margin, and update time.
- R: Binance documents using Position Information V3 together with ACCOUNT_UPDATE for timely/accurate position status.
- E: For the intended long-only application, a position-mode check (`dualSidePosition=false`) and signed position quantity are independent safeguards; both should be reconciled rather than inferred from order side alone.
