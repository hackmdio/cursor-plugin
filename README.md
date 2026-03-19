# HackMD Cursor Plugin

Official HackMD plugin scaffold for Cursor.

This plugin provides:

- A ready-to-configure `mcp.json` entry for the HackMD MCP server
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
├── README.md
└── mcp.json
```

## Setup

### 1. Configure HackMD MCP

Edit `mcp.json` and replace:

```text
Bearer <HACKMD_API_TOKEN>
```

with a real HackMD API bearer token.

The default MCP server uses:

```bash
npx mcp-remote https://mcp.hackmd.io/ --header "Authorization:${AUTH_HEADER}"
```

### 2. Install HackMD CLI

```bash
npm install -g @hackmd/hackmd-cli
```

### 3. Configure HackMD CLI authentication

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

## Included Components

### Skill: `hackmd-cli`

Use this when working with HackMD notes via CLI, including personal notes, team notes, exports, and scripting workflows.

### Command: `setup-hackmd`

Use this command to guide users through:

- installing the HackMD CLI
- configuring authentication
- replacing the MCP auth placeholder
- verifying CLI and MCP access

### Rule: `hackmd-workflow`

Use this rule for HackMD-related work so Cursor prefers the HackMD CLI workflow, distinguishes personal notes from team notes, and keeps auth placeholders out of committed examples.

## Notes

- The MCP config intentionally ships with a placeholder token.
- The plugin keeps scope narrow so it can be extended safely later.
- The `hackmd-cli` skill adapts the upstream HackMD CLI guide for Cursor plugin packaging.

Upstream reference: [HackMD CLI skill](https://github.com/hackmdio/hackmd-cli/blob/develop/hackmd-cli/SKILL.md)
