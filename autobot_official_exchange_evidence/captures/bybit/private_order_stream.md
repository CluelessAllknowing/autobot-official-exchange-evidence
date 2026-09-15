# Bybit V5 private order stream
Official URL: https://bybit-exchange.github.io/docs/v5/websocket/private/order  
Captured UTC: 2026-09-15T21:44:15Z  
Applicability: private order topic, linear USDT subset.

- D: Private order messages contain a message `id`, topic, creationTime and a data array.
- D: Per-order fields include `orderId`, caller-supplied `orderLinkId`, symbol, qty, side, `positionIdx`, orderStatus, createType, cancelType, rejectReason, average price, remaining/cumulative execution fields, orderType, stopOrderType, triggerPrice, TP/SL values and trigger bases, triggerDirection, reduceOnly, closeOnTrigger and timestamps.
- D: `orderId` is the exchange order identity; `orderLinkId` is caller custom identity.
- D: For TP/SL orders, `orderType` is the type after trigger; `stopOrderType` and `triggerPrice` describe conditional-order state.
- D: Bybit explicitly warns that an execution/cancel race can generate two `orderStatus=Filled` messages: one for the fill and another indicating cancellation rejection because the order had already filled.
- E: Consumers must deduplicate/reconcile by order identity and state transition, not by assuming a single Filled message per order.
- E: The docs label top-level `id` a message ID but do not establish it as a durable cross-reconnect order identity; use `orderId` for order identity.
