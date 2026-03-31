# HackMD Cursor Plugin

Official HackMD plugin scaffold for Cursor.

This plugin provides:

- A repo-hosted `logo.png` wired into the plugin manifest
- A ready-to-configure `.mcp.json` entry for the HackMD MCP server
- A `hackmd-cli` skill adapted for Cursor plugin format
- A `setup-hackmd` command for local setup and verification
- A light HackMD workflow rule for note-related tasks

## Repository Layout

```text
.
├── .cursor-plugin/
│   └── plugin.json
├── commands/
│   └── setup-hackmd.md
├── rules/
│   └── hackmd-workflow.mdc
├── skills/
│   └── hackmd-cli/
│       └── SKILL.md
├── LICENSE
├── logo.png
├── README.md
└── .mcp.json
```

## Local Testing

For day-to-day testing, symlink this repo into Cursor's local plugin directory:

```bash
mkdir -p ~/.cursor/plugins/local
ln -s "$(pwd)" ~/.cursor/plugins/local/hackmd
```

Then fully reload Cursor and verify:

- the `hackmd` plugin appears in the local plugin list
- `logo.png` renders correctly
- the `hackmd-cli` skill is available
- the `setup-hackmd` command is available
- the `hackmd-workflow` rule is discoverable
- the `.mcp.json` template is available for plugin MCP configuration in Cursor builds that support plugin MCP auto-discovery

If you prefer an install-artifact style test, copy the repo instead of symlinking it.

## Setup

### 1. Install the plugin in Cursor

Use this repository as the plugin source in Cursor, or copy the plugin files into a local plugin directory for testing and publishing workflows.

The plugin entrypoint is:

```text
.cursor-plugin/plugin.json
```

### 2. Configure HackMD MCP safely

Do not store a real token in this tracked repository file.

Instead:

1. Copy the `hackmd-mcp` entry from this repo's `.mcp.json`
2. Paste it into your user-level Cursor MCP config at `~/.cursor/mcp.json`
3. Replace the placeholder there, in your local user config only

This repository also includes a plugin-local `.mcp.json` template for local testing and packaging.

Example user-local config:

```json
{
  "mcpServers": {
    "hackmd-mcp": {
      "command": "npx",
      "args": [
        "mcp-remote@0.1.38",
        "https://mcp.hackmd.io/",
        "--header",
        "Authorization:${AUTH_HEADER}"
      ],
      "env": {
        "AUTH_HEADER": "Bearer YOUR_REAL_HACKMD_TOKEN"
      }
    }
  }
}
```

Replace:

```text
Bearer <HACKMD_API_TOKEN>
```

with your real HackMD API bearer token in `~/.cursor/mcp.json`.

The default MCP server uses:

```bash
npx mcp-remote@0.1.38 https://mcp.hackmd.io/ --header "Authorization:${AUTH_HEADER}"
```

### 3. Install HackMD CLI

```bash
npm install -g @hackmd/hackmd-cli
```

### 4. Configure HackMD CLI authentication

Use either interactive login:

```bash
hackmd-cli login
```

or an environment variable:

```bash
export HMD_API_ACCESS_TOKEN=YOUR_TOKEN
```

For HackMD EE instances, also set:

```bash
export HMD_API_ENDPOINT_URL=https://your.hackmd-ee.endpoint
```

### 5. Verify CLI and MCP access

```bash
hackmd-cli whoami
hackmd-cli notes
```

In Cursor, confirm the `hackmd-mcp` server appears from your user MCP configuration.

## Included Components

### Skill: `hackmd-cli`

Use this when working with HackMD notes via CLI, including personal notes, team notes, exports, and scripting workflows.

### Command: `setup-hackmd`

Use this command to guide users through:

- installing the HackMD CLI
- configuring authentication
- copying the MCP template into user-local Cursor config
- verifying CLI and MCP access

### Rule: `hackmd-workflow`

Use this rule for HackMD-related work so Cursor prefers the HackMD CLI workflow, distinguishes personal notes from team notes, and keeps auth placeholders out of committed examples.

## Notes

- The plugin manifest references the local `logo.png` asset.
- The repo `.mcp.json` is a template with a placeholder token and a pinned `mcp-remote` version.
- Users should place real secrets only in their local `~/.cursor/mcp.json`, not in this repository.
- The plugin keeps scope narrow so it can be extended safely later.
- The `hackmd-cli` skill adapts the upstream HackMD CLI guide for Cursor plugin packaging.

Upstream reference: [HackMD CLI skill](https://github.com/hackmdio/hackmd-cli/blob/develop/hackmd-cli/SKILL.md)
