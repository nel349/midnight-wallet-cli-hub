# Getting Started

This guide walks you through installing midnight-wallet-cli, setting up a local network, and making your first transfer.

## Prerequisites

- **Node.js** >= 20 ([download](https://nodejs.org/))
- **Docker** with Docker Compose v2 ([download](https://www.docker.com/))

## Install

```bash
npm install -g midnight-wallet-cli
```

Verify the installation:

```bash
midnight --version
```

## Start the Local Network

The CLI manages a local Midnight network via Docker Compose. Start it:

```bash
midnight localnet up
```

This starts three containers:
- **node** — Midnight blockchain node (port 9944)
- **indexer** — Transaction indexer (port 8088)
- **proof-server** — ZK proof generation server (port 6300)

Check status:

```bash
midnight localnet status
```

## Generate a Wallet

```bash
midnight generate --network undeployed
```

This creates a wallet file at `~/.midnight/wallet.json` containing your seed, address, and network. You'll see a BIP-39 mnemonic — **save it securely**.

To view your wallet info without revealing secrets:

```bash
midnight info
```

## Fund Your Wallet

On the local network, you can airdrop NIGHT tokens from the genesis wallet:

```bash
midnight airdrop 1000
```

This transfers 1000 NIGHT from the genesis wallet (seed `0x01`) to your wallet. The first airdrop on a fresh network may take a few minutes while dust generation capacity grows.

## Check Balance

```bash
midnight balance
```

## Register Dust

Dust tokens are required to pay transaction fees on Midnight. After receiving NIGHT, register your UTXOs for dust generation:

```bash
midnight dust register
```

On a fresh wallet, this may take 3-5 minutes while the dust generation capacity accumulates. The CLI retries automatically with a progress spinner.

Check dust status:

```bash
midnight dust status
```

> **Note:** Dust registration happens automatically when you make a transfer. You only need to run `dust register` manually if you want dust ready before your first transfer.

## Transfer NIGHT

Send tokens to another address:

```bash
midnight transfer mn_addr_undeployed1... 100
```

The CLI handles dust registration, ZK proof generation, and transaction submission automatically.

## JSON Mode

Add `--json` to any command for machine-readable output:

```bash
midnight balance --json
# {"address":"mn_addr_...","network":"undeployed","balances":{"NIGHT":"900.000000"},"utxoCount":1,"txCount":2}
```

## Cleanup

When you're done:

```bash
# Stop containers (preserves state for fast restart)
midnight localnet stop

# Or fully remove everything
midnight localnet down
```

## Next Steps

- [Command Reference](commands.md) — All commands with flags and examples
- [JSON Output](json-output.md) — Structured output for scripting
- [MCP Server](mcp-server.md) — AI agent integration
- [Networks](networks.md) — Network configuration
