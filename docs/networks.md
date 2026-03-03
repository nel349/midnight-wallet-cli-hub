# Networks

midnight-wallet-cli supports multiple Midnight networks. Each wallet is bound to a specific network at creation time.

## Available Networks

| Network | Description | Status |
|---------|-------------|--------|
| `undeployed` | Local development network via Docker | Supported |

## Undeployed (Local Network)

The `undeployed` network runs entirely on your machine via Docker Compose. It includes:

- **Midnight Node** — blockchain node on port `9944`
- **Indexer** — transaction indexer on port `8088`
- **Proof Server** — ZK proof generation on port `6300`

### Setup

```bash
# Start all services
midnight localnet up

# Check status
midnight localnet status

# Stop (preserves state)
midnight localnet stop

# Full teardown
midnight localnet down
```

### Genesis Wallet

The local network includes a pre-funded genesis wallet (seed `0x01`). Use `midnight airdrop` to fund your wallet from it:

```bash
midnight airdrop 1000
```

## Network Detection

The CLI detects the network from multiple sources (in priority order):

1. `--network` flag on the command line
2. Wallet file's `network` field
3. Address prefix (e.g., `mn_addr_undeployed1...`)
4. Default config (`midnight config get network`)

## Address Prefixes

Each network uses a distinct bech32m address prefix:

| Network | Prefix |
|---------|--------|
| `undeployed` | `mn_addr_undeployed1` |
| `preprod` | `mn_addr_preprod1` |
| `preview` | `mn_addr_preview1` |

## Custom Endpoints

For advanced setups, you can override the indexer WebSocket URL:

```bash
midnight balance --indexer-ws ws://custom-host:8088
```
