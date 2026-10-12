# Security policy

Requant Wallet for Telegram is pre-alpha software for the Requant test network. An internal security review
is planned and has not yet been completed. Test RQT and the test-network assets used in swaps have no value. Do not use this
application to store real funds or import a wallet used on a production network.

## How to report

Email [dev@requant.network](mailto:dev@requant.network?subject=%5BSecurity%5D%20Requant%20Telegram%20Wallet%20vulnerability)
with **[Security]** in the subject, or use this repository's private security advisory form if it is
available. Do not report vulnerabilities through public issues or the Telegram wallet bot.

Include the version or commit, Telegram client and operating system, a minimal reproduction, and the
expected versus observed result. Reproduce with a new unfunded test wallet. Never send a recovery
phrase, private key, password, swap secret, Telegram bot token or export decryption key.

## Most valuable findings

- A transaction differs from its review: recipients, amounts, fee, spending conditions, time locks,
  anyone-can-pay or secret disclosure.
- A recovery phrase, signing key, password, swap secret or decrypted wallet is exposed outside the
  device's local signing flow.
- An export decryption key reaches the HTTP server, bot or logs, or export access, replacement or
  confirmation bypasses the required session authorization or capability checks.
- Forged, expired or replayed Telegram `initData` is accepted, or one Telegram account can access
  another account's local wallet storage or export session.
- A forged source transaction, swap offer, HTLC, TRON contract or token, channel state or transaction
  package is accepted when it should be rejected.
- Unverified code enters the wallet, or an application update interrupts an active recovery, signing,
  payment or export flow.

## Scope and trust boundaries

Transactions are prepared and signed on the device by the shared Rust/WASM wallet core. Telegram
identity separates local IndexedDB storage; it does not recover wallets or synchronize them across
devices. Keep an encrypted backup and recovery phrase before clearing client storage.

Balances, history, confirmations and unspent status depend on the selected node or wallet gateway.
Genesis and source-transaction checks do not independently verify chain proof of work or the current
UTXO set; this is not a light client. Swap and channel protocols use test-network services. Their
design is documented in [SWAPS.md](https://github.com/requant-network/requant/blob/main/SWAPS.md).

The export service temporarily holds device-encrypted ciphertext. The decryption key is carried in
the external download URL fragment and is used in that browser, rather than sent to the HTTP server.
Telegram client behavior and the external browser are part of this flow's trust boundary. See
[Telegram storage, export and deployment](telegram/README.md) and
[wallet gateway](gateway/README.md) for the implementation's limits and deployment requirements.
