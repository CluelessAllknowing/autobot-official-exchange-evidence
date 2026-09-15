# Bybit same-venue wallet and internal-transfer semantics
Official URLs: https://bybit-exchange.github.io/docs/v5/account/wallet-balance ; https://bybit-exchange.github.io/docs/v5/asset/transfer/create-inter-transfer ; https://bybit-exchange.github.io/docs/v5/asset/transfer/inter-transfer-list ; https://bybit-exchange.github.io/docs/changelog/v5  
Captured UTC: 2026-09-15T21:44:15Z  
Applicability: documentation-only review; no transfer was executed.

- D: Wallet Balance with accountType=UNIFIED is distinct from Funding-wallet balance, which Bybit exposes through a separate asset endpoint.
- D: POST `/v5/asset/transfer/inter-transfer` transfers between different account types under the same UID.
- D: `transferId` is caller-generated UUID identity; returned transfer status can be STATUS_UNKNOWN, SUCCESS, PENDING or FAILED.
- D: GET `/v5/asset/transfer/query-inter-transfer-list` can query by transferId/status and uses cursor pagination; its default query horizon is 7 days and the maximum explicit range is 7 days.
- D: Bybit change history instructs checking the transfer-query API for final status when a transfer is PENDING.
- E: The reviewed pages did not establish the exact permission-name requirement for this transfer endpoint or define an idempotency guarantee beyond caller-provided transferId.
- E: A timeout/unknown transfer outcome must be reconciled by transferId before any retry; this is a fail-closed engineering policy based on the documented stable transfer identifier/status readback.
