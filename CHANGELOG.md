# Changelog

All notable changes to this plugin will be documented in this file.

The format is based on Keep a Changelog, and this project follows Semantic Versioning.

## [Unreleased]

### Changed

- MCP connects to `https://mcp.hackmd.io/` with OAuth 2.1 (URL-only `.mcp.json`). No API token and no `mcp-remote` for MCP.
- README and `setup-hackmd` walk through the browser OAuth flow instead of `HMD_API_ACCESS_TOKEN` on the MCP path.
- CLI still uses `hackmd-cli login` or a personal API token; that token is independent of MCP OAuth.

## [0.1.0] - 2026-03-31

### Added

- Cursor plugin manifest (`plugin.json`) and `logo.png`
- `.mcp.json` template for `hackmd-mcp` (`mcp-remote@0.1.38`, `https://mcp.hackmd.io/`) with `Authorization:Bearer ${env:HMD_API_ACCESS_TOKEN}` (Cursor env interpolation; tokens stay in the environment)
- `hackmd-cli` skill, `setup-hackmd` command, and `hackmd-workflow` rule
- README with MCP and CLI setup, and local testing via `~/.cursor/plugins/local`
- `LICENSE`
