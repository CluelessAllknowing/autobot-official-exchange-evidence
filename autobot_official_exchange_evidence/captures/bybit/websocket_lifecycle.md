# Bybit V5 WebSocket lifecycle
Official URL: https://bybit-exchange.github.io/docs/v5/ws/connect  
Captured UTC: 2026-09-15T21:44:15Z  
Applicability: V5 private WebSocket transport.

- D: Private V5 WebSocket uses the official private-stream endpoint.
- D: Bybit supports an active-time setting in the documented 30-600 second range; inactivity handling is checked periodically rather than at an exact boundary.
- R: Bybit recommends sending heartbeat ping messages regularly and reconnecting as soon as possible after disconnection.
- D: WebSocket connection creation is subject to IP connection limits, and Bybit advises against frequent connect/disconnect churn.
- E: A reconnect is a transport event, not proof that application state is gap-free; AutoBot should reconcile orders and positions before resuming mutation authority.
