# Changelog

All notable changes to [midnight-wallet-cli](https://www.npmjs.com/package/midnight-wallet-cli) are documented here.

## [0.2.1] - 2026-03-19

### Added
- **Network-agnostic wallets** — one seed derives addresses for all three networks (undeployed, preprod, preview). No more regenerating wallets when switching networks.
- **Preview network** support — all commands now work on undeployed, preprod, and preview
- **DApp Connector** (`midnight serve`) — WebSocket JSON-RPC server implementing the Lace `ConnectedAPI` interface
- **[midnight-wallet-connector](https://www.npmjs.com/package/midnight-wallet-connector)** — npm package for connecting browser DApps to `mn serve`
- `midnight wallet` subcommands — `generate`, `list`, `use`, `info`, `remove` for named multi-wallet management
- `midnight cache clear` command for clearing wallet state cache
- `--approve-all` and `--no-auto-approve-reads` flags on `midnight serve`

### Changed
- **Simplified network resolution** — 3-step chain (flag → config → fallback) instead of 5-step. Wallet file no longer influences network selection.
- Wallet file format: `addresses` map replaces single `address` + `network` fields
- Old wallet files auto-migrate on load (derives all addresses from seed, writes back)
- `midnight generate` deprecated in favor of `midnight wallet generate <name>`

### Fixed
- Network confusion when wallet stored one network but config had another
- Old wallets now work on any network without regeneration

## [0.2.0] - 2026-03-11

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
