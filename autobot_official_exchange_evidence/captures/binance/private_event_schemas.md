# Binance USD-M private event schema evidence
Official page title: schema catalog / User Data Streams  
URLs: https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/ws-streams/~schemas ; https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/user-data-streams  
Captured UTC: 2026-09-15T21:44:15Z  
Displayed update: User Data Streams 2026-08-08; schema catalog date not displayed.  
Applicability: standard USD-M Futures.

- D: ORDER_TRADE_UPDATE envelope has event type `e`, event time `E`, transaction time `T`, and nested order object `o`.
- D: ALGO_UPDATE envelope has event type `e`, transaction time `T`, event time `E`, and nested object `o`.
- E: The currently rendered nested object definitions are not sufficiently exposed to establish every short field's exact type, required/optional status, and omission rules. The package therefore does not silently import an older or cross-product schema.
- E: Existing application mappings of nested event codes should be regression-checked against a current machine-readable official schema before any mapping is promoted as fully current.
