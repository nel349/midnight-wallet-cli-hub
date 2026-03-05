# MCP Server Guide

midnight-wallet-cli includes an MCP (Model Context Protocol) server that exposes all wallet operations as typed tools. AI agents call them directly via JSON-RPC over stdio — no shell spawning or output parsing needed.

## What is MCP?

[Model Context Protocol](https://modelcontextprotocol.io/) is a standard for connecting AI assistants to external tools and data sources. The midnight-wallet-cli MCP server lets AI agents manage wallets, check balances, make transfers, and control the local network through structured tool calls.

## Setup

### Claude Code

Create `.mcp.json` in your project root:

```json
{
  "mcpServers": {
    "midnight-wallet": {
      "command": "midnight-wallet-mcp"
    }
  }
}
```

### Claude Desktop

Add to `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS) or `%APPDATA%\Claude\claude_desktop_config.json` (Windows):

```json
{
  "mcpServers": {
    "midnight-wallet": {
      "command": "midnight-wallet-mcp"
    }
  }
}
```

### Cursor

Create `.cursor/mcp.json` in your project root:

```json
{
  "mcpServers": {
    "midnight-wallet": {
      "command": "midnight-wallet-mcp"
    }
  }
}
```

### VS Code (GitHub Copilot)

Create `.vscode/mcp.json` in your project root:

```json
{
  "servers": {
    "midnight-wallet": {
      "type": "stdio",
      "command": "midnight-wallet-mcp"
    }
  }
}
```

### Windsurf

Add to `~/.codeium/windsurf/mcp_config.json`:

```json
{
  "mcpServers": {
    "midnight-wallet": {
      "command": "midnight-wallet-mcp"
    }
  }
}
```

> **Tip:** If you haven't installed globally, use `"command": "npx"` with `"args": ["-y", "midnight-wallet-cli@latest", "--mcp"]` instead.

## Available Tools

Once connected, your AI agent has access to 17 tools:

| Tool | Description |
|------|-------------|
| `midnight_generate` | Generate or restore a wallet |
| `midnight_info` | Show wallet info (no secrets) |
| `midnight_balance` | Check NIGHT balance |
| `midnight_address` | Derive address from seed |
| `midnight_genesis_address` | Show genesis wallet address |
| `midnight_inspect_cost` | Show block cost limits |
| `midnight_airdrop` | Fund wallet from genesis |
| `midnight_transfer` | Send NIGHT tokens |
| `midnight_dust_register` | Register UTXOs for dust generation |
| `midnight_dust_status` | Check dust status |
| `midnight_config_get` | Read config value |
| `midnight_config_set` | Write config value |
| `midnight_localnet_up` | Start local network |
| `midnight_localnet_stop` | Stop local network |
| `midnight_localnet_down` | Remove local network |
| `midnight_localnet_status` | Show service status |
| `midnight_localnet_clean` | Remove conflicting containers |

## Example Workflow

An AI agent can orchestrate a complete workflow:

1. Start the local network: `midnight_localnet_up`
2. Generate a wallet: `midnight_generate` with `network: "undeployed"`
3. Fund it: `midnight_airdrop` with `amount: "1000"`
4. Check balance: `midnight_balance`
5. Transfer tokens: `midnight_transfer` with `to` and `amount`
6. Verify: `midnight_balance` again

Each tool returns structured JSON — the same format as `midnight <command> --json`.
