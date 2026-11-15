# Changelog

All notable changes to this plugin will be documented in this file.

The format is based on Keep a Changelog, and this project follows Semantic Versioning.

## [Unreleased]

## [0.2.0] - 2026-11-15

### Changed

- MCP now uses the remote HackMD server over HTTP with OAuth sign-in. A root `mcp.json` (`hackmd` → `https://mcp.hackmd.io/`) is installed with the plugin, so users no longer copy MCP config by hand.
- The old `mcp-remote` + API token setup moved to the README as a manual alternative for users' own `~/.cursor/mcp.json`.
- README local testing copies the plugin into `~/.cursor/plugins/local/hackmd` instead of symlinking it. Cursor skips symlinks that point outside that folder.
- `setup-hackmd` command and `hackmd-workflow` rule updated for OAuth MCP.

### Added

- `homepage` and `repository` in `.cursor-plugin/plugin.json`.
- Links to the HackMD Terms of Service and Privacy Policy in the README.

### Removed

- `.mcp.json` (Cursor plugins don't read it) and the `${env:HMD_API_ACCESS_TOKEN}` interpolation in plugin config.

## [0.1.0] - 2026-03-31

### Added

- Cursor plugin manifest (`plugin.json`) and `logo.png`
- `.mcp.json` template for `hackmd-mcp` (`mcp-remote@0.1.38`, `https://mcp.hackmd.io/`) with `Authorization:Bearer ${env:HMD_API_ACCESS_TOKEN}` (Cursor env interpolation; tokens stay in the environment)
- `hackmd-cli` skill, `setup-hackmd` command, and `hackmd-workflow` rule
- README with MCP and CLI setup, and local testing via `~/.cursor/plugins/local`
- `LICENSE`
