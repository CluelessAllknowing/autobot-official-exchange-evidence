# Binance USD-M rate limits and ambiguous execution recovery
Official URLs: https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/general-info ; https://developers.binance.com/en/docs/products/derivatives-trading-usds-futures/error-code  
Captured UTC: 2026-09-15T21:44:15Z  
Displayed page update: General Info 2026-08-08.  
Applicability: standard USD-M Futures.

- D: HTTP 403 represents WAF-limit violation, 408 backend-response timeout, 429 request-rate violation, 418 automatic IP ban after continued 429 behavior, and 5xx internal exchange errors.
- D: A 503 response with the documented unknown-execution variant means execution status is UNKNOWN and the operation may have succeeded.
- R: Binance says not to treat that 503 variant as immediate failure; first verify through WebSocket updates or order-ID query to avoid duplicates.
- R: For documented service-unavailable failures Binance recommends exponential backoff; for system overload it recommends backoff and reduced concurrency.
- D: Rate information is exposed through used-weight headers per IP and order-count headers for account order usage; repeated 429 violations can escalate to 418.
- D: IP weight is shared by requests from the same IP.
- E: Any AutoBot mutation receiving an execution-unknown response must enter reconciliation state rather than automatically submit a duplicate mutation.
