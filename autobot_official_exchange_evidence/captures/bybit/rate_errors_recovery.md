# Bybit V5 rate limits and recovery
Official URLs: https://bybit-exchange.github.io/docs/v5/rate-limit ; https://bybit-exchange.github.io/docs/v5/error  
Captured UTC: 2026-09-15T21:44:15Z  
Applicability: V5 UTA endpoints used by the integration.

- D: Bybit API rate limits use a rolling per-second, per-UID model and return limit-status, configured-limit and reset-time response headers.
- D: Current UTA2.0 Pro table shows 50/s for position list, 10/s for trading-stop, 10/s for linear auto-add-margin, 50/s for UNIFIED wallet balance, 50/s for account info and 50/s for order realtime; table context/tier must be respected.
- R: Bybit recommends not operating exactly at the edge of published limits.
- D: WebSocket connection creation is limited to 500 connections in a 5-minute window per IP across the documented stream domains; market-data connection caps are counted separately by product class.
- D: Error 10006 means API rate limit exceeded; HTTP 429 is system-level frequency protection and the documentation says retry.
- D: Error 10000 is documented only as Server Timeout; 10014 is invalid duplicate request; WS 20006 is duplicated reqId; WS 10016 can mean internal error/service restart; WS 10019 says trade service is restarting and in-process requests are not affected.
- E: The error-code page does not establish whether a mutation returning 10000 executed. Treat mutation state as ambiguous until order/position readback resolves it.
