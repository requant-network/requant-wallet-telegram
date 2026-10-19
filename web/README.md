# Shared Requant wallet interface

> **Historical adapter only.** New swaps are paused across the native RQT wallet family. Do not launch this retired Mini App or create/fund a swap. Keep original swap backups and recover them in the current RQT wallet. The descriptions below record historical behavior.

This public repository includes the shared Web Wallet 0.5.0 interface as the basis of the Telegram Mini App. The Telegram adapter is in `src/telegram`; its entry renders the same `src/App.tsx`, protocol screens, storage/service layer and Rust/WASM core. The portable wallet library is 0.10.0; its pinned source and adaptations are recorded in `../vendor/wallet/UPSTREAM.json`.

Build with `npm run build`; output is `dist-web`. Run `npm run dev` for local development. Retired Telegram entry: `/telegram-wallet/` is maintenance-only. Shared browser fallback: https://requant.network/wallet/.

The shared interface includes HD creation/recovery, encrypted/legacy/watch-only imports, multiple wallets, receive/change addresses, recovery scans, exact atom amounts, balances/history/UTXOs, multi-recipient payments, fees, consolidation, review/sign/broadcast, offline signing, spending conditions, swaps, payment channels and transaction packages. BTC Signet, Lightning testnet and USDT TRON Nile are test networks. No mainnet or fiat pricing is enabled.

Wallet cryptography runs in a worker. Passwords and recovery phrases stay on the device. Telegram storage is separated by account on each device, and export uses additional local encryption. Static code/font/WASM assets use a separate `/telegram-wallet/` service-worker scope; RPC responses, backups and export payloads are never cached by it. Updates wait for protected wallet actions to finish.

See the [repository README](../README.md) for features/build instructions and [Telegram deployment documentation](../telegram/README.md) for storage, export, bot and proxy configuration. Tests use unfunded fixtures and do not broadcast real transactions.
