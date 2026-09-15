# AutoBot official exchange evidence

Created UTC: **2026-09-15T21:44:15Z**  
Research cutoff: **2026-09-15T21:44:15Z**

## Purpose
This is an offline evidence package for an AutoBot cryptocurrency-futures integration review. It records only current public official documentation evidence that was accessible at the research cutoff.

## Exact scope
- Binance: standard USD-M Futures; Portfolio Margin/PAPI excluded; intended application mode is one-way, isolated margin, long-only.
- Bybit: V5 linear USDT perpetual/futures under Unified Trading Account; intended application mode is one-way, isolated margin, long-only.
- Documentation can describe exchange-supported modes beyond the intended application scope when needed to disambiguate fields.

## Activity and privacy statement
No login was performed. No authentication material was requested, used, displayed, stored, or inferred. No authenticated/private exchange endpoint was called. No live order, account, position, wallet, transfer, or configuration request was sent. No order was placed, amended, cancelled, or queried. No transfer, deposit, withdrawal, margin-mode change, leverage change, trading-stop mutation, or account mutation occurred.

## Evidence labels
- **D** — documented exchange behavior.
- **R** — recommendation explicitly made by the exchange documentation.
- **E** — engineering inference, proposed fail-closed policy, or unresolved matter.

Official documentation examples demonstrate the published documentation shape only. They do **not** prove behavior on a particular live account.

## Important current implementation findings
1. Binance standard USD-M conditional-order handling must be reviewed against the current Algo service. The official USD-M change history documents migration of conditional types to Algo endpoints and ALGO_UPDATE.
2. The currently rendered USD-M User Data Streams page exposes ORDER_TRADE_UPDATE and ALGO_UPDATE status/enumeration text, but its nested payload tables render as a schema placeholder. Exact nested short-field types/optionality therefore remain deliberately unresolved here rather than being filled from memory.
3. Bybit UTA 2.0 uses account-dimension margin mode. The legacy per-symbol cross/isolated endpoint is documented as unsupported for UTA 2.0.
4. For Bybit UTA 2.0, position `tradeMode` must not be treated as the authoritative isolated/cross proof; use Account Info `marginMode` for the account-mode proof and Position Info for position state.

## Fixture policy
The requested fixture directories are present. A fixture is included only where a short official documentation response example could be reproduced without copying authentication material or a large copyrighted example. Long official examples were **not** reconstructed, normalized, or invented. Their absence is recorded in `UNRESOLVED.md`.

## Validation performed
Before ZIP creation the package was checked for:
- strict parsing of all JSON;
- manifest/file existence and coverage;
- recomputed SHA-256 and byte sizes;
- likely credential-material terms;
- executable/script extensions;
- non-official factual-source domains in captures/fixtures;
- ZIP member readability and checksum consistency;
- required top-level directory name.

The manifest schema itself contains the required boolean key `contains_credentials`; that field name is metadata and not credential material.

## Handling instruction
Inspect this ZIP again for sensitive material before committing any derivative artifact to source control. This package was generated to contain no credentials or personal data.

Validation completed UTC: **2026-09-15T21:44:15Z**. Pre-ZIP JSON, manifest coverage, SHA/size, sensitive-term, executable-extension and official-source-domain checks passed.
