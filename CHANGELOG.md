# Changelog

All notable changes to [midnight-wallet-cli](https://www.npmjs.com/package/midnight-wallet-cli) are documented here.

## [0.2.0] - Unreleased

### Changed
- Upgraded Midnight SDK to v2.0.0-rc
- Build output is now minified for smaller package size
- Improved dust registration retry with timeout-based approach (10 min max, 15s intervals)

### Fixed
- Error 138 (BalanceCheckOverspend) retry during both dust registration and transfer submission
- Suppressed polkadot-js RPC-CORE console noise during transaction retries
- Balance integration tests now detect actual indexer availability instead of relying on CI env var

## [0.1.8] - 2026-03-03

### Added
- Build minification (`--minify`) for smaller published package

### Fixed
- Error 138 retry during transfer submission (not just dust registration)
- RPC-CORE noise suppression covers the full transfer flow

## [0.1.7] - 2026-03-03

### Added
- Dust registration retry with friendly spinner messages on fresh localnets
- Post-airdrop hints about dust generation timing
- "Not enough dust" retry during transfers
- Constants: `DUST_REGISTRATION_TIMEOUT_MS`, `DUST_REGISTRATION_RETRY_DELAY_MS`

### Fixed
- Error 138 (BalanceCheckOverspend) on fresh localnet — dust capacity takes ~5 minutes to grow
- Polkadot-js RPC-CORE console noise suppressed during dust registration retries

## [0.1.6] - 2026-03-02

### Added
- `midnight balance` command with GraphQL WebSocket subscription
- `--json` flag on all commands for structured JSON output
- MCP server (`midnight-wallet-mcp`) with 17 tools for AI agent integration
- Error classification with typed error codes and exit codes
- `midnight help --agent` comprehensive AI reference manual
- `midnight help --json` machine-readable capability manifest

## [0.1.5] - 2026-02-28

### Added
- Initial release
- Commands: generate, info, balance, address, genesis-address, inspect-cost, airdrop, transfer, dust (register/status), config (get/set), localnet (up/stop/down/status/logs/clean), help
- Wallet file management (`~/.midnight/wallet.json`)
- Local network management via Docker Compose
- BIP-39 mnemonic generation and restore
- HD key derivation (BIP-44)
- Animated terminal UI with logo and wordmark
