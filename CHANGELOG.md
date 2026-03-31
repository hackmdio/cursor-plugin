# Changelog

All notable changes to this plugin will be documented in this file.

The format is based on Keep a Changelog, and this project follows Semantic Versioning.

## [Unreleased]

## [0.1.0] - 2026-03-31

### Added

- Cursor plugin manifest (`plugin.json`) and `logo.png`
- `.mcp.json` template for `hackmd-mcp` (`mcp-remote@0.1.38`, `https://mcp.hackmd.io/`) with `Authorization:Bearer ${env:HMD_API_ACCESS_TOKEN}` (Cursor env interpolation; tokens stay in the environment)
- `hackmd-cli` skill, `setup-hackmd` command, and `hackmd-workflow` rule
- README with MCP and CLI setup, and local testing via `~/.cursor/plugins/local`
- `LICENSE`
