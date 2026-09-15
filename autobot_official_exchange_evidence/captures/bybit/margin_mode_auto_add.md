# Bybit cross/isolated and auto-add-margin behavior
Official URLs: https://bybit-exchange.github.io/docs/v5/account/set-margin-mode ; https://bybit-exchange.github.io/docs/v5/abandon/cross-isolate ; https://bybit-exchange.github.io/docs/v5/position/auto-add-margin ; https://bybit-exchange.github.io/docs/v5/acct-mode  
Captured UTC: 2026-09-15T21:44:15Z  
Applicability: V5 UTA, linear USDT.

- D: UTA 2.0 does not support the legacy POST `/v5/position/switch-isolated`; Bybit states margin mode is account-dimension for UTA 2.0.
- D: UTA 1.0 support on that legacy endpoint is documented for inverse contracts, not linear USDT.
- D: Current account-level POST `/v5/account/set-margin-mode` accepts ISOLATED_MARGIN, REGULAR_MARGIN and PORTFOLIO_MARGIN.
- D: GET `/v5/account/info` is the documented readback for account `marginMode`.
- D: POST `/v5/position/set-auto-add-margin` applies to isolated margin positions for linear USDT/USDC and uses `autoAddMargin` 0/1 plus `positionIdx`.
- E: For UTA 1.0 linear USDT, the reviewed official pages do not state a separate symbol-level isolated-margin mutation path. AutoBot should detect account generation first and fail closed rather than assume the abandoned endpoint works.
