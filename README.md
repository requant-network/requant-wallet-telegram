# Retired Requant RQT Mini App

**Retired — 2026-10-19.** This repository preserves the previous native RQT Telegram application for source provenance and recovery. It is not an active wallet deployment. Do not deploy this application or restart its former bot. Native RQT uses the web, Windows, macOS, Linux, Android and browser-extension wallet family, which has no Mini App.

The separate Telegram-only wallet supports TON and jettons, including jRQT once its official master is issued. It is maintained independently, without native RQT accounts, a bridge or RQT conversion.

The former `@requant_wallet_bot` is maintenance-only and `/telegram-wallet/` displays an in-development notice. Keep existing encrypted RQT backups and recovery phrases; restore them in the [RQT Web Wallet](https://requant.network/wallet/) or the native RQT applications. Do not clear old Telegram wallet storage before recovering your backups.

Testnet coins have no value. The network may reset.

## Historical features (inactive)

- Create and restore 24-word wallets; import encrypted, watch-only and legacy wallets; manage multiple wallets.
- Receive addresses and QR codes, recovery scanning, balances, history and UTXOs.
- Multi-recipient payments, send max, fee selection, consolidation and transaction review.
- Watch-only preparation, offline signing and broadcasting; spending conditions and cosigning.
- Payment channels and signed transaction packages.
- Local password protection, encrypted exports, inactivity locking and automatic application updates.

The Telegram entry renders `web/src/App.tsx` and its shared protocol screens. Transactions are prepared, reviewed and signed by the same Rust/WASM core used by the Web Wallet. This repository contains the required source and pinned dependencies; it has no dependency on access to a private GitHub repository.

## Data and backups

Wallet files and protocol state live in IndexedDB on the device, separately for each Telegram account. Telegram login does not recover a wallet and does not synchronize it across devices. Keep an encrypted backup and recovery phrase before clearing Telegram storage.

Passwords, recovery phrases and signing keys stay on the device. The bot handles only requested private commands. Exports are additionally encrypted on the device and temporarily held as ciphertext in server memory. Decryption happens in the external download browser; the key is carried in the URL fragment. Confirm saving the file there before returning to Telegram. Cancelled exports are not marked as backups.

Details: [Telegram adapter, export protocol and deployment](telegram/README.md).

## Historical local build

Requires Node.js 24, Rust **1.99.0** (pinned in `rust-toolchain.toml`) with the `wasm32-unknown-unknown` target, `wasm-bindgen-cli` **0.2.129**, and Clang for the portable secp256k1 dependency.

```sh
npm ci
rustup target add wasm32-unknown-unknown
cargo install wasm-bindgen-cli --version 0.2.129 --locked
npm run build
npm test
npm run test:core
```

Output: `dist-web/`, including the Telegram entry, external export page, shared Web Wallet fallback, self-hosted SDK/fonts/WASM and separate service workers. For development, run `npm run dev`; wallet RPC is proxied to the existing Requant gateway. The local export service must run separately on port 3313 and requires a bot credential in a local secret file.

## Historical deployment — do not redeploy

The retired installation shared the Requant server and authenticated wallet gateway. Keep `/wallet/` assets and `/telegram-wallet/` application routing on the same HTTPS origin. See [Caddy routes](telegram/Caddyfile.fragment) and [Compose service](telegram/compose.telegram-wallet.yml). Reverse proxy `/wallet-api/*` to the constrained [wallet gateway](gateway/README.md), and `/telegram-wallet-api/*` to the Telegram service.

The bot credential is read from `/run/secrets/telegram-bot-token`; create and mount that file on the server. It is never bundled into browser code. `node telegram/configure.mjs` configures commands and the Open Wallet menu, and refuses to replace an existing webhook. The service uses long polling with a persisted update offset. A profile-level Launch app button is an optional BotFather setting; the menu and `/start` work without it.

Publish new immutable assets before replacing HTML/service workers, and retain previous assets for open sessions. Updates wait while a protected wallet flow is active. Existing downloadable wallet installers are not part of this repository.

## Source and validation

[SOURCE.json](SOURCE.json) records the shared application/core versions. Vendored Requant source retains its [provenance](vendor/PROVENANCE.md), [portable-core adaptations](vendor/wallet/UPSTREAM.json) and Apache-2.0 license. IBM Plex fonts retain their bundled license. The official Telegram SDK is self-hosted; its source and checksum are documented in `telegram/README.md`.

Node tests cover gateway constraints, precise amounts, Telegram session authentication, encrypted export lifecycle, backup handling and update safety. Rust tests cover wallet and protocol operations with unfunded fixtures. Browser smoke checks covered import, shared screens, local persistence and encrypted export on the deployed HTTPS build, plus offline launch with the official Telegram SDK and a simulated native bridge. Physical Telegram clients have not all been individually verified.
