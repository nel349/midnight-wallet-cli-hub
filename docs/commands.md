# Command Reference

All commands support the `--json` flag for structured output. See [JSON Output](json-output.md) for details.

## generate

Generate a new wallet or restore from an existing seed/mnemonic.

```
midnight generate [--network <name>] [--seed <hex>] [--mnemonic "..."] [--output <file>] [--force]
```

| Flag | Description |
|------|-------------|
| `--network <name>` | Network: undeployed, preprod, preview |
| `--seed <hex>` | Restore from existing seed (64-char hex) |
| `--mnemonic "..."` | Restore from BIP-39 mnemonic (24 words) |
| `--output <file>` | Custom output path (default: `~/.midnight/wallet.json`) |
| `--force` | Overwrite existing wallet file |

```bash
midnight generate --network undeployed
midnight generate --seed 0123456789abcdef...
midnight generate --mnemonic "word1 word2 ... word24" --network preprod
```

## info

Display wallet address, network, and creation date. No secrets are shown.

```
midnight info [--wallet <file>]
```

```bash
midnight info
midnight info --wallet my-wallet.json
```

## balance

Check unshielded NIGHT balance via the indexer.

```
midnight balance [address] [--network <name>] [--indexer-ws <url>]
```

| Flag | Description |
|------|-------------|
| `<address>` | Address to check (or reads from wallet file) |
| `--network <name>` | Override network detection |
| `--indexer-ws <url>` | Custom indexer WebSocket URL |

```bash
midnight balance
midnight balance mn_addr_undeployed1...
midnight balance --network undeployed
```

## transfer

Send NIGHT tokens to another address.

```
midnight transfer <to> <amount> [--wallet <file>]
```

| Flag | Description |
|------|-------------|
| `<to>` | Recipient bech32m address |
| `<amount>` | Amount in NIGHT (supports decimals, e.g., `0.5`) |
| `--wallet <file>` | Custom wallet file path |

```bash
midnight transfer mn_addr_undeployed1... 100
midnight transfer mn_addr_undeployed1... 0.5 --wallet my-wallet.json
```

## airdrop

Fund your wallet from the genesis wallet. Only works on the `undeployed` (local) network.

```
midnight airdrop <amount> [--wallet <file>]
```

```bash
midnight airdrop 1000
midnight airdrop 0.5
```

## dust

Manage dust tokens (required for transaction fees).

### dust register

Register NIGHT UTXOs for dust generation.

```
midnight dust register [--wallet <file>]
```

### dust status

Check dust registration status and balance.

```
midnight dust status [--wallet <file>]
```

```bash
midnight dust register
midnight dust status
```

## address

Derive an unshielded address from a seed.

```
midnight address --seed <hex> [--network <name>] [--index <n>]
```

| Flag | Description |
|------|-------------|
| `--seed <hex>` | Seed to derive from (required, 64-char hex) |
| `--network <name>` | Network for address prefix |
| `--index <n>` | Key derivation index (default: 0) |

```bash
midnight address --seed 0123456789abcdef... --network undeployed
midnight address --seed 0123456789abcdef... --index 1
```

## genesis-address

Display the genesis wallet address (seed `0x01`) for a network.

```
midnight genesis-address [--network <name>]
```

```bash
midnight genesis-address --network undeployed
```

## inspect-cost

Display current block cost limits from the ledger.

```
midnight inspect-cost
```

## config

Manage persistent configuration.

### config get

```
midnight config get <key>
```

### config set

```
midnight config set <key> <value>
```

```bash
midnight config get network
midnight config set network undeployed
```

## localnet

Manage a local Midnight network via Docker Compose.

| Subcommand | Description |
|------------|-------------|
| `up` | Start the local network (node, indexer, proof server) |
| `stop` | Stop containers (preserves state for fast restart) |
| `down` | Remove containers, networks, and volumes (full teardown) |
| `status` | Show service status and ports |
| `logs` | Stream service logs (Ctrl+C to stop) |
| `clean` | Remove conflicting containers from other setups |

```bash
midnight localnet up
midnight localnet status
midnight localnet stop
midnight localnet down
midnight localnet logs
midnight localnet clean
```

## help

Show usage for all commands or a specific command.

```
midnight help [command]
midnight help --agent    # Comprehensive AI reference
midnight help --json     # Machine-readable capability manifest
```
