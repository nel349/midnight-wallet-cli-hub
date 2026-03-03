# Security Policy

## Reporting a Vulnerability

If you discover a security vulnerability in midnight-wallet-cli, please report it by [opening an issue](https://github.com/nel349/midnight-wallet-cli-hub/issues) with the **security** label.

### What to include

- CLI version (`midnight --version`)
- Description of the vulnerability
- Steps to reproduce
- Potential impact

### Scope

The following are in scope:

- **CLI wallet** — key handling, seed storage, transaction signing
- **MCP server** — tool execution, data exposure
- **Network communication** — WebSocket connections, RPC calls

### Out of scope

- Issues specific to the Midnight blockchain protocol itself
- Local devnet (undeployed network) configuration issues
- Cosmetic or UX bugs (use a regular bug report instead)

## Supported Versions

| Version | Supported |
|---------|-----------|
| latest  | Yes       |
| < latest | No — please upgrade |
