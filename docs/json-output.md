# JSON Output Reference

Every command supports the `--json` flag for structured, machine-readable output.

## Behavior

When `--json` is active:

- **stdout** receives a single line of JSON
- **stderr** is fully suppressed (no spinners, no formatting, no animations)
- Errors produce a structured JSON error object

This makes it safe to pipe commands and parse output programmatically.

## Success Output

Each command produces a JSON object with command-specific fields. See [Command Reference](commands.md) for the fields each command returns.

```bash
midnight balance --json
# {"address":"mn_addr_...","network":"undeployed","balances":{"NIGHT":"1000.000000"},"utxoCount":1,"txCount":1}

midnight airdrop 100 --json
# {"txHash":"00ab...","amount":100,"recipient":"mn_addr_...","network":"undeployed"}
```

## Error Output

When a command fails in `--json` mode:

```json
{
  "error": true,
  "code": "INSUFFICIENT_BALANCE",
  "message": "Insufficient balance: 10.000000 NIGHT available, 100 NIGHT requested",
  "exitCode": 5
}
```

## Error Codes

| Code | Exit Code | Meaning |
|------|-----------|---------|
| `INVALID_ARGS` | 2 | Missing or invalid arguments |
| `WALLET_NOT_FOUND` | 3 | Wallet file does not exist |
| `NETWORK_ERROR` | 4 | Connection refused, timeout, DNS failure |
| `INSUFFICIENT_BALANCE` | 5 | Not enough NIGHT for the operation |
| `DUST_REQUIRED` | 5 | No dust tokens available for fees |
| `TX_REJECTED` | 6 | Transaction rejected by the node |
| `STALE_UTXO` | 6 | UTXOs consumed by another transaction |
| `PROOF_TIMEOUT` | 6 | ZK proof generation timed out |
| `CANCELLED` | 7 | Operation cancelled (SIGINT) |
| `UNKNOWN` | 1 | Unclassified error |

## Exit Codes

| Code | Meaning |
|------|---------|
| 0 | Success |
| 1 | General error |
| 2 | Invalid arguments / usage |
| 3 | Wallet not found |
| 4 | Network / connection error |
| 5 | Insufficient balance |
| 6 | Transaction rejected |
| 7 | Operation cancelled (SIGINT) |

## Shell Scripting Examples

### Parse balance with jq

```bash
NIGHT=$(midnight balance --json | jq -r '.balances.NIGHT // "0"')
echo "Balance: $NIGHT NIGHT"
```

### Check if airdrop succeeded

```bash
RESULT=$(midnight airdrop 100 --json)
if echo "$RESULT" | jq -e '.error' > /dev/null 2>&1; then
  echo "Failed: $(echo $RESULT | jq -r '.message')"
  exit 1
fi
TX=$(echo $RESULT | jq -r '.txHash')
echo "Airdrop tx: $TX"
```

### Pipe wallet address

```bash
# Generate a wallet and capture the address
ADDR=$(midnight generate --network undeployed --json | jq -r '.address')

# Check balance for a specific address
midnight balance "$ADDR" --json
```

### Localnet health check

```bash
STATUS=$(midnight localnet status --json)
ALL_HEALTHY=$(echo "$STATUS" | jq '[.services[] | .health == "healthy"] | all')
if [ "$ALL_HEALTHY" = "true" ]; then
  echo "All services healthy"
else
  echo "Some services unhealthy"
  echo "$STATUS" | jq '.services[] | select(.health != "healthy")'
fi
```

## Capability Manifest

Run `midnight help --json` to get a full machine-readable manifest of all commands, flags, examples, and JSON output schemas. This is useful for building tooling or agents that discover CLI capabilities at runtime.
