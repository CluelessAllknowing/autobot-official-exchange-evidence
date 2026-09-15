# Bybit V5 private position stream
Official URL: https://bybit-exchange.github.io/docs/v5/websocket/private/position  
Captured UTC: 2026-09-15T21:44:15Z  
Applicability: private position.linear topic.

- D: Every create/amend/cancel order action can emit a position message even if position economics did not change.
- D: Message envelope contains `id`, topic, creationTime and data array.
- D: Position fields include category, symbol, side, size, `positionIdx`, positionValue, risk tier, entry/mark prices, leverage, autoAddMargin, position IM/MM, liqPrice, TP/SL/trailing stop, unrealised/realised PnL, positionStatus and update sequencing.
- D: `positionIdx=0` is the one-way representation.
- D: `seq` is a cross sequence used to associate position updates; settings such as leverage or risk-limit changes can update it.
- E: Because position messages can be emitted without an economic position change, state consumers must compare the relevant fields and sequencing rather than count messages as position transitions.
