# Bybit UTA modes and Account Info
Official URLs: https://bybit-exchange.github.io/docs/v5/acct-mode ; https://bybit-exchange.github.io/docs/v5/account/account-info ; https://bybit-exchange.github.io/docs/v5/account/set-margin-mode  
Captured UTC: 2026-09-15T21:44:15Z  
Applicability: V5 Unified Trading Account; linear USDT focus.

- D: `unifiedMarginStatus` values are 1 classic, 3 UTA 1.0, 4 UTA 1.0 Pro, 5 UTA 2.0, 6 UTA 2.0 Pro.
- D: Bybit describes Pro variants as the same account semantics with a trading-API performance advantage.
- D: UTA 2.0 integrates inverse, USDT perpetual/futures, USDC perpetual/futures, spot and options in the unified system.
- D: GET `/v5/account/info` returns `unifiedMarginStatus` and account-level `marginMode`.
- D: `marginMode` values are ISOLATED_MARGIN, REGULAR_MARGIN and PORTFOLIO_MARGIN.
- D: Account Info also returns `updatedTime`, `isMasterTrader`, `spotHedgingStatus`; `dcpStatus`, `timeWindow` and `smpGroup` are documented deprecated.
- D: The current Account Info response example contains a nonzero `timeWindow` despite the field table stating the deprecated field is always zero; the package treats the field table and example as an official documentation inconsistency and does not use `timeWindow` for trading logic.
- D: POST `/v5/account/set-margin-mode` selects account-level margin mode.
