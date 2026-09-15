# Binance USD-M User Data Streams
Official page title: User Data Streams  
URL: https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/user-data-streams  
Captured UTC: 2026-09-15T21:44:15Z  
Displayed page update: 2026-08-08  
Applicability: standard USD-M Futures.

## Focused contract
- D: A user-data-stream key is valid for 60 minutes; keepalive extends validity by 60 minutes; invalidation/expiry stops subsequent user events until a new valid key is used.
- D: Private stream base is `wss://fstream.binance.com/private`; the stream path is `/ws/<stream-key>`.
- D: A single connection is valid for 24 hours.
- D: On one connection, messages of the same event type for the same user are strictly ordered by transaction time `T` and event time `E`.
- R: Binance recommends using `E` for ordering when comparing different event types and recommends prioritising WebSocket user-data events over REST queries in volatile conditions.
- D: ORDER_TRADE_UPDATE is emitted for new orders and order-status changes. Published domains include side BUY/SELL; order types LIMIT, MARKET, STOP, STOP_MARKET, TAKE_PROFIT, TAKE_PROFIT_MARKET, TRAILING_STOP_MARKET, LIQUIDATION; execution types NEW, CANCELED, CALCULATED, EXPIRED, TRADE, AMENDMENT; statuses NEW, PARTIALLY_FILLED, FILLED, CANCELED, EXPIRED, EXPIRED_IN_MATCH; TIF GTC/IOC/FOK/GTX; working type MARK_PRICE/CONTRACT_PRICE; expiry reasons 0-9.
- D: ALGO_UPDATE is emitted when an algo order is created or changes status. Published statuses are NEW, CANCELED, TRIGGERING, TRIGGERED, FINISHED, REJECTED, EXPIRED.
- D: FINISHED means the triggered conditional order was either filled or cancelled in the matching engine; therefore FINISHED alone does not prove a position was closed.
- D: REJECTED can represent matching-engine denial such as a margin-check failure.
- D: EXPIRED is a system cancellation state.
- E: The current HTML renderer exposes the event envelope/status descriptions but displays the nested event schema as a loading placeholder. Nested short-field types and optionality must not be reconstructed from memory.

Transformation: paraphrased/structurally converted; authentication samples and long event examples omitted.
