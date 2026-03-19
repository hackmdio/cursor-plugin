# Changelog

All notable changes to this plugin will be documented in this file.

The format is based on Keep a Changelog, and this project follows Semantic Versioning.

## [Unreleased]

### Added

- Initial Cursor plugin scaffold for HackMD
- Local `logo.png` asset referenced by the plugin manifest
- `mcp.json` with HackMD MCP server template and auth placeholder
- `hackmd-cli` skill adapted for Cursor plugin format
- `setup-hackmd` command for installation and configuration guidance
- `hackmd-workflow` rule for note workflow and safe auth handling
- `README.md` and `LICENSE`

### Changed

- Pinned `mcp-remote` to `0.1.38` in the MCP template
- Updated setup guidance to keep real tokens in user-local `~/.cursor/mcp.json` instead of the tracked repository
- Expanded `README.md` with plugin installation and safer MCP configuration instructions
