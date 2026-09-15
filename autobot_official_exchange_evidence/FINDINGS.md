# Findings

Research cutoff: **2026-09-15T21:44:15Z**

Every substantive bullet is labelled D/R/E. URLs are official documentation URLs and the capture timestamp is repeated with each source block.

## 1. Binance standard USD-M Futures

### 1.1 ORDER_TRADE_UPDATE

- **D** New orders and order-status changes emit `ORDER_TRADE_UPDATE`. Source: https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/user-data-streams — captured 2026-09-15T21:44:15Z
- **D** Published enum domains currently include BUY/SELL; LIMIT/MARKET/STOP/STOP_MARKET/TAKE_PROFIT/TAKE_PROFIT_MARKET/TRAILING_STOP_MARKET/LIQUIDATION; execution NEW/CANCELED/CALCULATED/EXPIRED/TRADE/AMENDMENT; status NEW/PARTIALLY_FILLED/FILLED/CANCELED/EXPIRED/EXPIRED_IN_MATCH; TIF GTC/IOC/FOK/GTX; working type MARK_PRICE/CONTRACT_PRICE. Source: https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/user-data-streams — captured 2026-09-15T21:44:15Z
- **D** Event envelope uses `e`, `E`, `T`, and nested `o`. Source: https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/ws-streams/~schemas — captured 2026-09-15T21:44:15Z
- **E** The current rendered USD-M page leaves the nested order schema in a loading state. The package therefore cannot certify every nested short field's type/optionality from current accessible official evidence. Source checked: https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/user-data-streams — captured 2026-09-15T21:44:15Z ; https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/ws-streams/~schemas — captured 2026-09-15T21:44:15Z
- **E** Safe mapping now: envelope identity/time fields plus the documented enum domains. Existing nested-code mappings should remain gated behind a schema-regression test rather than being rewritten from memory.

#### Field reconciliation

| Requested concept | Current evidence | Type/optionality conclusion | Source |
|---|---|---|---|
| event type | `e=ORDER_TRADE_UPDATE` | string enum; envelope field | https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/ws-streams/~schemas — captured 2026-09-15T21:44:15Z |
| event time | `E` | int64/ms in schema/general timing contract | https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/ws-streams/~schemas — captured 2026-09-15T21:44:15Z |
| transaction time | `T` | int64/ms | https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/ws-streams/~schemas — captured 2026-09-15T21:44:15Z |
| nested order object | `o` | object | https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/ws-streams/~schemas — captured 2026-09-15T21:44:15Z |
| side/order type/TIF/execution/status/working type | enum domains documented | values established; nested short-key typing not exposed in rendered page | https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/user-data-streams — captured 2026-09-15T21:44:15Z |
| expiry reason | values 0-9 documented | semantics established; current nested key typing not exposed in rendered page | https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/user-data-streams — captured 2026-09-15T21:44:15Z |
| symbol, client ID, exchange order ID, quantities/prices, commission, trade ID, reduce-only, position side, close-position, trailing activation/callback, realised PnL, price protection, STP, price match and other nested fields | requested by integration review | **UNRESOLVED exact current nested short-key/type/optionality in the rendered USD-M schema** | https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/user-data-streams — captured 2026-09-15T21:44:15Z ; https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/ws-streams/~schemas — captured 2026-09-15T21:44:15Z |

#### Stable identity / deduplication
- **E** Exchange order identity should be keyed by the documented order identifier once the nested mapping is independently schema-verified; event-time fields are ordering metadata, not durable order identity. The accessible renderer is insufficient to certify the nested identifier key here. Source checked: https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/ws-streams/~schemas — captured 2026-09-15T21:44:15Z
- **D** For same event type and user on one connection, Binance guarantees strict ordering by both `T` and `E`. Source: https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/user-data-streams — captured 2026-09-15T21:44:15Z
- **R** Binance recommends `E` when comparing different event types. Source: https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/user-data-streams — captured 2026-09-15T21:44:15Z

### 1.2 ALGO_UPDATE and current conditional-order contract

- **D** ALGO_UPDATE states are NEW, CANCELED, TRIGGERING, TRIGGERED, FINISHED, REJECTED and EXPIRED. Source: https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/user-data-streams — captured 2026-09-15T21:44:15Z
- **D** FINISHED means the triggered order was either filled **or cancelled** in the matching engine; it does not prove closure. Source: https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/user-data-streams — captured 2026-09-15T21:44:15Z
- **D** REJECTED can be produced by matching-engine denial including margin-check failure. Source: https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/user-data-streams — captured 2026-09-15T21:44:15Z
- **D** The official USD-M change history moved the standard conditional types to the Algo service and later added trailing-stop activation state behavior to ALGO_UPDATE. Source: https://developers.binance.com/zh-CN/docs/products/derivatives-trading-coin-futures/change-log — captured 2026-09-15T21:44:15Z
- **D** Query Algo Order exposes `actualOrderId`; it is empty before trigger and links the algo to the actual exchange order after trigger. Source: https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/trade/Query-Algo-Order — captured 2026-09-15T21:44:15Z
- **E** Algo status alone is insufficient to prove position closure. Closure evidence must ultimately reconcile the resulting order state and the position state.

#### ALGO_UPDATE reconciliation

| Field/concept | Current evidence | Conclusion | Source |
|---|---|---|---|
| `e`, `T`, `E`, `o` | schema envelope | event type, transaction time, event time, nested object | https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/ws-streams/~schemas — captured 2026-09-15T21:44:15Z |
| algo status | seven status values documented | exact status semantics established | https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/user-data-streams — captured 2026-09-15T21:44:15Z |
| trailing activation state `ia` | documented by 2026 change history | activation-state behavior established | https://developers.binance.com/zh-CN/docs/products/derivatives-trading-coin-futures/change-log — captured 2026-09-15T21:44:15Z |
| `algoId`, `clientAlgoId` | canonical REST Query Algo Order fields | stable algo readback identities | https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/trade/Query-Algo-Order — captured 2026-09-15T21:44:15Z |
| `actualOrderId` | canonical REST Query Algo Order linkage | empty before trigger; links resulting order after trigger | https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/trade/Query-Algo-Order — captured 2026-09-15T21:44:15Z |
| exact ALGO_UPDATE nested short-key types/requiredness | nested renderer unavailable | **UNRESOLVED** | https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/ws-streams/~schemas — captured 2026-09-15T21:44:15Z |

### 1.3 Position mode, margin mode, account and position proof

- **D** `dualSidePosition=false` from GET `/fapi/v1/positionSide/dual` is the documented one-way-mode proof. Source: https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/account/Get-Current-Position-Mode — captured 2026-09-15T21:44:15Z
- **D** Isolated/cross selection is documented through margin type ISOLATED/CROSSED. Source: https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/trade/Change-Margin-Type — captured 2026-09-15T21:44:15Z
- **D** Position V3 signed `positionAmt`: positive is long, negative is short. Source: https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/account/Position-Information-V3 — captured 2026-09-15T21:44:15Z
- **D** Position V3 exposes mark and liquidation prices, isolated margin/wallet values and maintenance/initial margin fields. Source: https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/account/Position-Information-V3 — captured 2026-09-15T21:44:15Z
- **R** Binance recommends combining Position V3 with ACCOUNT_UPDATE for timely/accurate status. Source: https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/account/Position-Information-V3 — captured 2026-09-15T21:44:15Z

### 1.4 Protection and liquidation safety

- **D** Current standard USD-M conditional protection belongs to the Algo service, and algo readback provides trigger/result linkage. Source: https://developers.binance.com/zh-CN/docs/products/derivatives-trading-coin-futures/change-log — captured 2026-09-15T21:44:15Z ; https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/trade/Query-Algo-Order — captured 2026-09-15T21:44:15Z
- **D** A conditional algo can be accepted before trigger yet later be REJECTED when the matching engine performs checks. Source: https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/user-data-streams — captured 2026-09-15T21:44:15Z ; https://developers.binance.com/zh-CN/docs/products/derivatives-trading-coin-futures/change-log — captured 2026-09-15T21:44:15Z
- **E** To establish active long protection, AutoBot should require: expected one-way long position state; an active algo record with matching symbol/side/position-side/quantity/trigger settings; and, after trigger, reconciliation through `actualOrderId` plus ordinary order and position state. This is an engineering policy built from documented contracts, not an exchange guarantee.
- **D** Bracket readback exposes maintenance-margin tiers and Position V3 exposes `liquidationPrice` and `markPrice`. Source: https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/account/Notional-and-Leverage-Brackets — captured 2026-09-15T21:44:15Z ; https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/account/Position-Information-V3 — captured 2026-09-15T21:44:15Z
- **E** The reviewed pages do not establish a complete formula that lets this package guarantee liquidation remains a fixed percentage below a stop. Fail closed if live readback does not show adequate separation.

### 1.5 Private stream and recovery

- **D** User-data-stream key lifetime is 60 minutes; keepalive extends it; connection lifetime is 24 hours. Source: https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/user-data-streams — captured 2026-09-15T21:44:15Z
- **R** Prefer WebSocket user data over delayed REST queries during volatility. Source: https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/user-data-streams — captured 2026-09-15T21:44:15Z
- **D** A 503 unknown-execution response can represent a request that succeeded; execution status is UNKNOWN. Source: https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/general-info — captured 2026-09-15T21:44:15Z
- **R** Verify by WebSocket/order readback before retrying an unknown-execution mutation. Source: https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/general-info — captured 2026-09-15T21:44:15Z
- **D** 429 indicates request-rate violation; repeated abuse can lead to 418; IP used-weight and account order-count headers convey usage. Source: https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/general-info — captured 2026-09-15T21:44:15Z

---

## 2. Bybit V5 linear USDT / Unified Trading Account

### 2.1 Account generation and Account Info

- **D** `unifiedMarginStatus`: 1 classic; 3 UTA1.0; 4 UTA1.0 Pro; 5 UTA2.0; 6 UTA2.0 Pro. Source: https://bybit-exchange.github.io/docs/v5/acct-mode — captured 2026-09-15T21:44:15Z
- **D** GET `/v5/account/info` returns `unifiedMarginStatus`, `marginMode`, `updatedTime`, and other account configuration fields. Source: https://bybit-exchange.github.io/docs/v5/account/account-info — captured 2026-09-15T21:44:15Z
- **D** `marginMode`: ISOLATED_MARGIN, REGULAR_MARGIN, PORTFOLIO_MARGIN. Source: https://bybit-exchange.github.io/docs/v5/account/account-info — captured 2026-09-15T21:44:15Z

| Field | Type | Meaning/applicability | Source |
|---|---|---|---|
| unifiedMarginStatus | integer | account generation/status values above | https://bybit-exchange.github.io/docs/v5/acct-mode — captured 2026-09-15T21:44:15Z |
| marginMode | string | isolated / regular-cross / portfolio | https://bybit-exchange.github.io/docs/v5/account/account-info — captured 2026-09-15T21:44:15Z |
| isMasterTrader | boolean | copy-trading leader flag | https://bybit-exchange.github.io/docs/v5/account/account-info — captured 2026-09-15T21:44:15Z |
| spotHedgingStatus | string | ON/OFF | https://bybit-exchange.github.io/docs/v5/account/account-info — captured 2026-09-15T21:44:15Z |
| updatedTime | string | account update timestamp ms | https://bybit-exchange.github.io/docs/v5/account/account-info — captured 2026-09-15T21:44:15Z |
| dcpStatus | string | deprecated | https://bybit-exchange.github.io/docs/v5/account/account-info — captured 2026-09-15T21:44:15Z |
| timeWindow | integer | deprecated; field table says always 0, while official example shows 10 | https://bybit-exchange.github.io/docs/v5/account/account-info — captured 2026-09-15T21:44:15Z |
| smpGroup | integer | deprecated | https://bybit-exchange.github.io/docs/v5/account/account-info — captured 2026-09-15T21:44:15Z |

### 2.2 Isolated margin by UTA generation

- **D** UTA2.0: margin mode is account-dimension and legacy `/v5/position/switch-isolated` is unsupported. Source: https://bybit-exchange.github.io/docs/v5/acct-mode — captured 2026-09-15T21:44:15Z ; https://bybit-exchange.github.io/docs/v5/abandon/cross-isolate — captured 2026-09-15T21:44:15Z
- **D** Current account-level mutation is `/v5/account/set-margin-mode`; readback is Account Info `marginMode`. Source: https://bybit-exchange.github.io/docs/v5/account/set-margin-mode — captured 2026-09-15T21:44:15Z ; https://bybit-exchange.github.io/docs/v5/account/account-info — captured 2026-09-15T21:44:15Z
- **D** UTA1.0 legacy switch endpoint is documented only for inverse contracts; it does not establish a linear-USDT symbol-level switch path. Source: https://bybit-exchange.github.io/docs/v5/abandon/cross-isolate — captured 2026-09-15T21:44:15Z
- **E** For UTA1.0 linear USDT, isolated-margin selection is **UNRESOLVED** from the reviewed official pages. Detect `unifiedMarginStatus` and fail closed rather than assuming legacy behavior.

### 2.3 Wallet Balance

- **D** Wallet Balance with `accountType=UNIFIED` is the unified wallet; Funding is queried separately. Source: https://bybit-exchange.github.io/docs/v5/account/wallet-balance — captured 2026-09-15T21:44:15Z
- **D** All account-wide fields are documented as not applicable to isolated margin. Source: https://bybit-exchange.github.io/docs/v5/account/wallet-balance — captured 2026-09-15T21:44:15Z
- **D** Coin-level equity/wallet/borrow/order-IM/position-IM/position-MM/unrealised-PnL fields remain separately exposed. Source: https://bybit-exchange.github.io/docs/v5/account/wallet-balance — captured 2026-09-15T21:44:15Z

### 2.4 Position Info reconciliation

| Field | Type | Current documented meaning | Source |
|---|---|---|---|
| positionIdx | integer | 0 one-way; 1 hedge buy; 2 hedge sell | https://bybit-exchange.github.io/docs/v5/position — captured 2026-09-15T21:44:15Z |
| symbol | string | symbol | https://bybit-exchange.github.io/docs/v5/position — captured 2026-09-15T21:44:15Z |
| side | string | Buy long; Sell short; empty string for empty position | https://bybit-exchange.github.io/docs/v5/position — captured 2026-09-15T21:44:15Z |
| size | string | always positive; direction is in side | https://bybit-exchange.github.io/docs/v5/position — captured 2026-09-15T21:44:15Z |
| riskId | integer | risk-tier ID; 0 under portfolio margin | https://bybit-exchange.github.io/docs/v5/position — captured 2026-09-15T21:44:15Z |
| riskLimitValue | string | risk limit; portfolio-margin caveat | https://bybit-exchange.github.io/docs/v5/position — captured 2026-09-15T21:44:15Z |
| avgPrice | string | average entry price | https://bybit-exchange.github.io/docs/v5/position — captured 2026-09-15T21:44:15Z |
| positionValue | string | position value | https://bybit-exchange.github.io/docs/v5/position — captured 2026-09-15T21:44:15Z |
| autoAddMargin | integer | isolated auto-add flag 0/1 | https://bybit-exchange.github.io/docs/v5/position — captured 2026-09-15T21:44:15Z |
| leverage | string | leverage; empty in portfolio margin | https://bybit-exchange.github.io/docs/v5/position — captured 2026-09-15T21:44:15Z |
| markPrice | string | mark price | https://bybit-exchange.github.io/docs/v5/position — captured 2026-09-15T21:44:15Z |
| liqPrice | string | liquidation-price readback with mode/min-max caveats | https://bybit-exchange.github.io/docs/v5/position — captured 2026-09-15T21:44:15Z |
| positionIM / positionMM | string | position initial / maintenance margin | https://bybit-exchange.github.io/docs/v5/position — captured 2026-09-15T21:44:15Z |
| takeProfit / stopLoss / trailingStop | string | position protection settings | https://bybit-exchange.github.io/docs/v5/position — captured 2026-09-15T21:44:15Z |
| createdTime / updatedTime / openTime | string/integer | lifecycle timestamps | https://bybit-exchange.github.io/docs/v5/position — captured 2026-09-15T21:44:15Z |
| seq | long | cross sequence; combine with symbol for unique association | https://bybit-exchange.github.io/docs/v5/position — captured 2026-09-15T21:44:15Z |
| tradeMode | legacy/deprecated semantics under UTA2 | do not use as UTA2 isolated proof | https://bybit-exchange.github.io/docs/v5/acct-mode — captured 2026-09-15T21:44:15Z |

### 2.5 Trading Stop reconciliation

| Parameter | Type | Current contract | Source |
|---|---|---|---|
| category | string | product category; linear in scope | https://bybit-exchange.github.io/docs/v5/position/trading-stop — captured 2026-09-15T21:44:15Z |
| symbol | string | position symbol | https://bybit-exchange.github.io/docs/v5/position/trading-stop — captured 2026-09-15T21:44:15Z |
| tpslMode | string | Full or Partial | https://bybit-exchange.github.io/docs/v5/position/trading-stop — captured 2026-09-15T21:44:15Z |
| positionIdx | integer | 0 one-way; 1/2 hedge sides | https://bybit-exchange.github.io/docs/v5/position/trading-stop — captured 2026-09-15T21:44:15Z |
| stopLoss | string | SL price; zero cancels | https://bybit-exchange.github.io/docs/v5/position/trading-stop — captured 2026-09-15T21:44:15Z |
| slTriggerBy | string | SL trigger price type | https://bybit-exchange.github.io/docs/v5/position/trading-stop — captured 2026-09-15T21:44:15Z |
| trailingStop | string | trailing distance; zero cancels | https://bybit-exchange.github.io/docs/v5/position/trading-stop — captured 2026-09-15T21:44:15Z |
| activePrice | string | trailing activation price | https://bybit-exchange.github.io/docs/v5/position/trading-stop — captured 2026-09-15T21:44:15Z |
| slSize | string | partial-mode SL size; must equal tpSize | https://bybit-exchange.github.io/docs/v5/position/trading-stop — captured 2026-09-15T21:44:15Z |
| slLimitPrice | string | partial + Limit only | https://bybit-exchange.github.io/docs/v5/position/trading-stop — captured 2026-09-15T21:44:15Z |
| slOrderType | string | Market/Limit with Full/Partial restrictions | https://bybit-exchange.github.io/docs/v5/position/trading-stop — captured 2026-09-15T21:44:15Z |

- **D** Trading Stop creates internal conditional orders, cancels them when the position closes, and adjusts their quantity with the open position. Source: https://bybit-exchange.github.io/docs/v5/position/trading-stop — captured 2026-09-15T21:44:15Z
- **D** One-sided modification may unpair the TP/SL relationship. Source: https://bybit-exchange.github.io/docs/v5/position/trading-stop — captured 2026-09-15T21:44:15Z
- **E** The mutation response carries no protective-order ID. A successful response alone cannot prove persistent stop coverage.

### 2.6 Private order topic reconciliation

| Field | Type | Deduplication/use | Source |
|---|---|---|---|
| id | string | message ID; not documented as durable order identity | https://bybit-exchange.github.io/docs/v5/websocket/private/order — captured 2026-09-15T21:44:15Z |
| creationTime | number | message creation ms | https://bybit-exchange.github.io/docs/v5/websocket/private/order — captured 2026-09-15T21:44:15Z |
| orderId | string | exchange order identity | https://bybit-exchange.github.io/docs/v5/websocket/private/order — captured 2026-09-15T21:44:15Z |
| orderLinkId | string | caller custom order identity | https://bybit-exchange.github.io/docs/v5/websocket/private/order — captured 2026-09-15T21:44:15Z |
| parentOrderLinkId | string | parent relation; may be meaningless for position-level TP/SL created later | https://bybit-exchange.github.io/docs/v5/websocket/private/order — captured 2026-09-15T21:44:15Z |
| symbol / qty / side / positionIdx | mixed | order targeting and mode | https://bybit-exchange.github.io/docs/v5/websocket/private/order — captured 2026-09-15T21:44:15Z |
| orderStatus / createType / cancelType / rejectReason | strings | state transition metadata | https://bybit-exchange.github.io/docs/v5/websocket/private/order — captured 2026-09-15T21:44:15Z |
| avgPrice / leavesQty / cumExecQty / cumExecValue | strings | execution progress | https://bybit-exchange.github.io/docs/v5/websocket/private/order — captured 2026-09-15T21:44:15Z |
| orderType / stopOrderType / triggerPrice | strings | triggered/conditional representation | https://bybit-exchange.github.io/docs/v5/websocket/private/order — captured 2026-09-15T21:44:15Z |
| stopLoss / tpslMode / slTriggerBy | strings | stop settings | https://bybit-exchange.github.io/docs/v5/websocket/private/order — captured 2026-09-15T21:44:15Z |
| reduceOnly / closeOnTrigger | booleans | exposure-reduction semantics | https://bybit-exchange.github.io/docs/v5/websocket/private/order — captured 2026-09-15T21:44:15Z |

- **D** Bybit explicitly documents a race where two Filled messages can occur for one order. Source: https://bybit-exchange.github.io/docs/v5/websocket/private/order — captured 2026-09-15T21:44:15Z
- **E** Deduplicate/reconcile by `orderId` and state rather than assuming a single Filled event.

### 2.7 Private position topic reconciliation

| Field | Type | Current contract | Source |
|---|---|---|---|
| id / topic / creationTime | string/string/number | message envelope | https://bybit-exchange.github.io/docs/v5/websocket/private/position — captured 2026-09-15T21:44:15Z |
| symbol / side / size / positionIdx | mixed | position identity/direction/mode | https://bybit-exchange.github.io/docs/v5/websocket/private/position — captured 2026-09-15T21:44:15Z |
| riskId / riskLimitValue | integer/string | risk tier | https://bybit-exchange.github.io/docs/v5/websocket/private/position — captured 2026-09-15T21:44:15Z |
| entryPrice / markPrice / leverage | strings | valuation/leverage | https://bybit-exchange.github.io/docs/v5/websocket/private/position — captured 2026-09-15T21:44:15Z |
| autoAddMargin | integer | isolated auto-add 0/1 | https://bybit-exchange.github.io/docs/v5/websocket/private/position — captured 2026-09-15T21:44:15Z |
| positionIM / positionMM / liqPrice | strings | margin/liquidation state | https://bybit-exchange.github.io/docs/v5/websocket/private/position — captured 2026-09-15T21:44:15Z |
| takeProfit / stopLoss / trailingStop | strings | position protection state | https://bybit-exchange.github.io/docs/v5/websocket/private/position — captured 2026-09-15T21:44:15Z |
| positionStatus | string | Normal/Liq/Adl | https://bybit-exchange.github.io/docs/v5/websocket/private/position — captured 2026-09-15T21:44:15Z |
| seq | long | cross-sequence association | https://bybit-exchange.github.io/docs/v5/websocket/private/position — captured 2026-09-15T21:44:15Z |

- **D** Order create/amend/cancel can cause a position message even without an economic position change. Source: https://bybit-exchange.github.io/docs/v5/websocket/private/position — captured 2026-09-15T21:44:15Z
- **E** Consumers must compare state/sequencing and not equate message count with position transitions.

### 2.8 What proves one-way mode?

- **D** Binance: `dualSidePosition=false`. Source: https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/account/Get-Current-Position-Mode — captured 2026-09-15T21:44:15Z
- **D** Bybit: `positionIdx=0`. Source: https://bybit-exchange.github.io/docs/v5/position — captured 2026-09-15T21:44:15Z
- **E** AutoBot should reject startup if these proofs disagree with its one-way-only configuration.

### 2.9 What establishes active stop coverage?

- **D** Bybit Position Info exposes `stopLoss`, while private order state exposes `orderId`, `orderStatus`, `stopOrderType`, `triggerPrice`, `slTriggerBy`, `reduceOnly`, `closeOnTrigger`, `positionIdx`, and quantity. Source: https://bybit-exchange.github.io/docs/v5/position — captured 2026-09-15T21:44:15Z ; https://bybit-exchange.github.io/docs/v5/websocket/private/order — captured 2026-09-15T21:44:15Z
- **D** Trading Stop says its internal conditional orders track open-position quantity and are cancelled when the position closes. Source: https://bybit-exchange.github.io/docs/v5/position/trading-stop — captured 2026-09-15T21:44:15Z
- **E** Strongest application proof: expected long position + one-way mode + Account Info isolated mode + matching position SL state + matching active conditional-order readback/stream state. No single Trading Stop response establishes all of this.

### 2.10 Liquidation-to-stop safety

- **D** Bybit exposes current `liqPrice`, `markPrice`, position IM/MM and public risk tiers. Source: https://bybit-exchange.github.io/docs/v5/position — captured 2026-09-15T21:44:15Z ; https://bybit-exchange.github.io/docs/v5/market/risk-limit — captured 2026-09-15T21:44:15Z
- **D** Binance exposes liquidation price/mark price and bracket maintenance-margin information. Source: https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/account/Position-Information-V3 — captured 2026-09-15T21:44:15Z ; https://developers.binance.com/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/account/Notional-and-Leverage-Brackets — captured 2026-09-15T21:44:15Z
- **E** The reviewed official developer pages are insufficient to guarantee liquidation is a fixed percentage below a stop under all tier, fee, funding, margin and execution states. The bot should use current readback and fail closed on inadequate separation.

### 2.11 Timeouts and unknown mutation status

- **D** Binance: the documented 503 unknown variant explicitly leaves execution status UNKNOWN; verify before retry. Source: https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/general-info — captured 2026-09-15T21:44:15Z
- **D** Bybit: error 10000 is documented as Server Timeout, but the error-code page does not state final mutation status. Source: https://bybit-exchange.github.io/docs/v5/error — captured 2026-09-15T21:44:15Z
- **E** Therefore Bybit mutation timeout is treated as ambiguous until order/position readback establishes the result.
- **D** Bybit internal transfer returns STATUS_UNKNOWN/PENDING states and provides a transfer-ID read endpoint; official change history directs querying final status for PENDING. Source: https://bybit-exchange.github.io/docs/v5/asset/transfer/create-inter-transfer — captured 2026-09-15T21:44:15Z ; https://bybit-exchange.github.io/docs/v5/asset/transfer/inter-transfer-list — captured 2026-09-15T21:44:15Z ; https://bybit-exchange.github.io/docs/changelog/v5 — captured 2026-09-15T21:44:15Z

### 2.12 Rate-limit summary

- **D** Bybit rolling endpoint limits are per second per UID and expose current/remaining/reset information through response headers. Source: https://bybit-exchange.github.io/docs/v5/rate-limit — captured 2026-09-15T21:44:15Z
- **D** Current UTA2.0 Pro table includes 50/s position-list, 10/s trading-stop, 10/s linear auto-add, 50/s unified wallet, 50/s account-info and 50/s order-realtime limits. Source: https://bybit-exchange.github.io/docs/v5/rate-limit — captured 2026-09-15T21:44:15Z
- **D** Binance exposes IP used-weight and order-count usage; 429 and 418 behavior is documented. Source: https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/general-info — captured 2026-09-15T21:44:15Z
- **E** Runtime rate-limit policy must read current headers and account tier rather than hard-code the documentation table as a universal ceiling.
