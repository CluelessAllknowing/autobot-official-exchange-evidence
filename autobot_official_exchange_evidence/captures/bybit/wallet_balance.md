# Bybit V5 Wallet Balance
Official URL: https://bybit-exchange.github.io/docs/v5/account/wallet-balance  
Captured UTC: 2026-09-15T21:44:15Z  
Applicability: UTA wallet with accountType=UNIFIED; Funding wallet is a separate endpoint.

- D: GET `/v5/account/wallet-balance` returns UNIFIED account wallet data; Bybit directs Funding-wallet balance to a separate endpoint.
- D: Bybit explicitly states that account-wide wallet fields are not applicable to isolated margin.
- D: Account-wide fields include account IM/MM rates, total equity, wallet balance, margin balance, available balance, perpetual/futures unrealised PnL, total initial margin and total maintenance margin.
- D: Coin-level fields include equity, USD value, walletBalance, locked, spotHedgingQty, borrowAmount, accruedInterest, totalOrderIM, totalPositionIM, totalPositionMM, unrealisedPnl, cumulative realised PnL, bonus and collateral flags.
- D: `accountLTV` is deprecated; `availableToWithdraw` for UNIFIED and several legacy availability fields are documented as deprecated/account-mode-dependent.
- D: Extreme volatility may increase latency or temporarily delay wallet data.
- E: AutoBot must not use UTA account-wide totals as isolated-position margin evidence where the documentation says those fields are inapplicable.
