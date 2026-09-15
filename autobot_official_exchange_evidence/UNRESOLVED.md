# Unresolved items

Research cutoff: 2026-09-15T21:44:15Z

Anything below was not silently filled from remembered API behavior.

## U1 — Binance ORDER_TRADE_UPDATE nested field contract
- **Venue:** Binance USD-M Futures
- **Question:** exact current nested short field names, types, required/optional status, omission/null rules for every requested ORDER_TRADE_UPDATE member.
- **Pages checked:** https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/user-data-streams; https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/ws-streams/~schemas
- **Why unresolved:** the current official renderer exposes the event/status text and envelope but leaves the nested order schema in a loading placeholder.
- **Safe fail-closed behavior:** do not promote new nested-field mappings solely from this package; validate the machine-readable official schema or captured live-testnet contract separately before changing production mapping.
- **What would resolve it:** accessible current official USD-M machine-readable schema or official page rendering the complete nested object.

## U2 — Binance ALGO_UPDATE nested field contract
- **Venue:** Binance USD-M Futures
- **Question:** exact current event-object short-code field table, types and optionality for every ALGO_UPDATE member.
- **Pages checked:** https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/user-data-streams; https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/ws-streams/~schemas; https://developers.binance.com/zh-CN/docs/products/derivatives-trading-coin-futures/change-log; https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/trade/Query-Algo-Order
- **Why unresolved:** event nested schema is not rendered; REST Algo query fields and event status semantics do not by themselves establish every WebSocket short code.
- **Safe fail-closed behavior:** use REST Algo readback plus only independently verified event mappings; unknown event members must not drive position-closure decisions.
- **What would resolve it:** official rendered/machine-readable `algoUpdate` nested schema.

## U3 — Binance full verbatim fixtures
- **Venue:** Binance
- **Question:** complete official JSON examples for ORDER_TRADE_UPDATE, ALGO_UPDATE, Account Information V3, Position Information V3 and protection readback.
- **Pages checked:** all corresponding official pages in `sources/binance_sources.md`.
- **Why unresolved:** current event pages do not render the nested examples in the accessible view, and long official examples were not copied wholesale. No example was invented.
- **Safe fail-closed behavior:** validate parser contracts against current official machine-readable schema or controlled testnet fixtures before production promotion.
- **What would resolve it:** accessible official examples or schema export that can be lawfully retained as a small focused fixture.

## U4 — Binance exact liquidation formula
- **Venue:** Binance USD-M Futures
- **Question:** complete current isolated USD-M liquidation formula including all qualifications needed to reproduce `liquidationPrice`.
- **Pages checked:** https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/account/Position-Information-V3; https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/account/Notional-and-Leverage-Brackets; https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/account/Account-Information-V3
- **Why unresolved:** reviewed developer pages expose liquidation readback and maintenance tiers but not a complete formula sufficient for a universal local proof.
- **Safe fail-closed behavior:** consume exchange liquidation readback, require a configured safety buffer to the intended stop and block new risk if readback is absent/unsafe.
- **What would resolve it:** current official standard USD-M liquidation formula documentation covering fees/funding/tier adjustments.

## U5 — Binance private stream heartbeat/message-rate contract
- **Venue:** Binance USD-M Futures
- **Question:** user-data-stream-specific ping/pong cadence and message-rate limits.
- **Pages checked:** https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/user-data-streams; https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/general-info
- **Why unresolved:** user-data page establishes stream-key/connection lifetime and ordering but the reviewed evidence did not establish a private-stream-specific heartbeat/message cap without importing rules from a different WebSocket product.
- **Safe fail-closed behavior:** implement reconnect/liveness detection conservatively and re-reconcile state after reconnect.
- **What would resolve it:** official USD-M private-stream transport page explicitly defining heartbeat/message limits.

## U6 — Bybit UTA1.0 linear-USDT isolated-mode mutation path
- **Venue:** Bybit V5
- **Question:** exact supported method to select isolated margin for linear USDT under UTA1.0.
- **Pages checked:** https://bybit-exchange.github.io/docs/v5/acct-mode; https://bybit-exchange.github.io/docs/v5/abandon/cross-isolate; https://bybit-exchange.github.io/docs/v5/account/set-margin-mode; https://bybit-exchange.github.io/docs/v5/account/account-info
- **Why unresolved:** legacy switch-isolated is documented for UTA1.0 inverse only; current account-level endpoint is documented but the reviewed pages do not explicitly spell out UTA1.0 linear applicability.
- **Safe fail-closed behavior:** query `unifiedMarginStatus`; if UTA1.0 linear and intended isolation cannot be officially verified, refuse trading.
- **What would resolve it:** explicit current Bybit documentation for UTA1.0 linear margin-mode selection.

## U7 — Bybit complete private-stream / REST example fixtures
- **Venue:** Bybit V5
- **Question:** complete response/event examples for Wallet Balance, Position List, private Order and private Position.
- **Pages checked:** https://bybit-exchange.github.io/docs/v5/account/wallet-balance; https://bybit-exchange.github.io/docs/v5/position; https://bybit-exchange.github.io/docs/v5/websocket/private/order; https://bybit-exchange.github.io/docs/v5/websocket/private/position
- **Why unresolved:** examples are substantially larger than needed for focused evidence and were not copied wholesale. Parser-relevant field tables are retained in structured form instead.
- **Safe fail-closed behavior:** use field tables and controlled testnet fixtures before parser promotion; reject unknown/missing critical fields.
- **What would resolve it:** permission to retain larger official examples or a compact official schema artifact.

## U8 — Bybit exact isolated liquidation formula / fixed stop gap
- **Venue:** Bybit V5
- **Question:** can liquidation be guaranteed a fixed percentage below a stop?
- **Pages checked:** https://bybit-exchange.github.io/docs/v5/position; https://bybit-exchange.github.io/docs/v5/market/risk-limit; https://bybit-exchange.github.io/docs/v5/account/wallet-balance
- **Why unresolved:** docs expose current liquidation price, mark price, margin fields and tier rates but not a complete universal formula/qualification set proving a constant gap.
- **Safe fail-closed behavior:** use current `liqPrice`, risk tier and intended stop readback; block risk if the safety buffer is not satisfied.
- **What would resolve it:** official formula documentation for the exact UTA generation + linear USDT + isolated mode including fee/funding qualifications.

## U9 — Bybit Server Timeout finality
- **Venue:** Bybit V5
- **Question:** does error 10000 prove a mutation failed?
- **Pages checked:** https://bybit-exchange.github.io/docs/v5/error
- **Why unresolved:** official table says only “Server Timeout”; it does not state execution finality.
- **Safe fail-closed behavior:** mark mutation state unknown and reconcile order/position state before any retry.
- **What would resolve it:** official error-recovery documentation defining execution finality for 10000.

## U10 — Bybit transfer permission and idempotency guarantee
- **Venue:** Bybit V5
- **Question:** exact permission label required for same-UID internal transfer and whether reusing `transferId` has a documented idempotent result.
- **Pages checked:** https://bybit-exchange.github.io/docs/v5/asset/transfer/create-inter-transfer; https://bybit-exchange.github.io/docs/v5/asset/transfer/inter-transfer-list; https://bybit-exchange.github.io/docs/changelog/v5
- **Why unresolved:** reviewed endpoint pages define UUID identity/status/readback but do not provide the requested permission-name and full ambiguous-timeout idempotency contract.
- **Safe fail-closed behavior:** no transfer logic should be enabled from this package; if later enabled, reconcile by transfer ID before retrying any ambiguous request.
- **What would resolve it:** official permission matrix plus explicit duplicate/retry semantics for transferId.

## U11 — Bybit ordinary-account endpoint limits vs Pro table
- **Venue:** Bybit V5
- **Question:** are every listed endpoint limit and upgrade rule identical for non-Pro UTA2 accounts?
- **Pages checked:** https://bybit-exchange.github.io/docs/v5/rate-limit
- **Why unresolved:** current table context explicitly identifies UTA2.0 Pro and upgradability.
- **Safe fail-closed behavior:** use runtime response-limit headers and conservative client-side ceilings; do not assume Pro limits on another tier.
- **What would resolve it:** account-tier-specific current rate table or endpoint-specific runtime contract for the intended account.
