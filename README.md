# midnight-wallet-cli

[![npm version](https://badge.fury.io/js/midnight-wallet-cli.svg)](https://www.npmjs.com/package/midnight-wallet-cli)
[![npm downloads](https://img.shields.io/npm/dm/midnight-wallet-cli)](https://npm-stat.com/charts.html?package=midnight-wallet-cli)
[![License](https://img.shields.io/npm/l/midnight-wallet-cli)](https://www.apache.org/licenses/LICENSE-2.0)

A standalone CLI wallet for the [Midnight](https://midnight.network) blockchain. Manage wallets, check balances, transfer NIGHT tokens, and run a local development network — all from the terminal.

## Install

```bash
npm install -g midnight-wallet-cli
```

This installs two commands: `midnight` (or `mn` for short) and `midnight-wallet-mcp`.

## Commands

| Command | Description |
|---------|-------------|
| `midnight generate` | Generate a new wallet or restore from seed/mnemonic |
| `midnight info` | Display wallet address, network, creation date |
| `midnight balance [address]` | Check unshielded NIGHT balance |
| `midnight transfer <to> <amount>` | Send NIGHT tokens to another address |
| `midnight airdrop <amount>` | Fund wallet from genesis (local network only) |
| `midnight dust register` | Register NIGHT UTXOs for dust (fee token) generation |
| `midnight dust status` | Check dust registration status and balance |
| `midnight address --seed <hex>` | Derive an address from a seed |
| `midnight genesis-address` | Show the genesis wallet address |
| `midnight inspect-cost` | Display current block cost limits |
| `midnight config get/set` | Manage persistent config (default network, etc.) |
| `midnight localnet up/stop/down/status` | Manage a local Midnight network via Docker |
| `midnight help` | Show usage for all commands |

## Quick Start

```bash
# 1. Start local network
midnight localnet up

# 2. Generate a wallet
midnight generate --network undeployed

# 3. Fund your wallet (local network only)
midnight airdrop 1000

# 4. Check balance
midnight balance

# 5. Transfer NIGHT tokens
midnight transfer mn_addr_undeployed1... 100
```

See [Getting Started](docs/getting-started.md) for a detailed walkthrough.

## JSON Output

Every command supports `--json` for structured, machine-readable output:

```bash
midnight balance --json
# {"address":"mn_addr_...","network":"undeployed","balances":{"NIGHT":"1000.000000"},"utxoCount":1,"txCount":1}

midnight transfer mn_addr_... 100 --json
# {"txHash":"00ab...","amount":100,"recipient":"mn_addr_...","network":"undeployed"}
```

When `--json` is active:
- stdout receives a single line of JSON
- stderr is fully suppressed (no spinners, no formatting)
- Errors produce: `{"error":true,"code":"...","message":"...","exitCode":N}`

See [JSON Output Reference](docs/json-output.md) for all schemas and error codes.

## MCP Server for AI Agents

The package includes an MCP (Model Context Protocol) server that exposes all wallet operations as typed tools. AI agents call them directly via JSON-RPC over stdio.

Add to your MCP config (`.mcp.json`, `.cursor/mcp.json`, etc.):

```json
{
  "mcpServers": {
    "midnight-wallet": {
      "command": "npx",
      "args": ["-y", "midnight-wallet-cli@latest", "--mcp"]
    }
  }
}
```

See [MCP Server Guide](docs/mcp-server.md) for setup with Claude Code, Claude Desktop, Cursor, VS Code, and Windsurf.

## Networks

| Network | Description | Status |
|---------|-------------|--------|
| `undeployed` | Local network via Docker (`midnight localnet up`) | Supported |

See [Networks](docs/networks.md) for configuration details.

## Documentation

- [Getting Started](docs/getting-started.md) — Full walkthrough from install to first transfer
- [Command Reference](docs/commands.md) — Every command with flags, examples, and JSON schemas
- [JSON Output](docs/json-output.md) — Structured output format, error codes, shell scripting
- [MCP Server](docs/mcp-server.md) — AI agent integration guide
- [Networks](docs/networks.md) — Network configuration and detection
- [Changelog](CHANGELOG.md) — Version history

## Requirements

- **Node.js** >= 20
- **Docker** (for `midnight localnet` commands)

## Issues

Found a bug or have a feature request? [Open an issue](https://github.com/nel349/midnight-wallet-cli-hub/issues).

## License

[Apache-2.0](LICENSE)
