# HackMD Cursor Plugin

Official HackMD plugin scaffold for Cursor. Connects to HackMD through OAuth MCP.

This plugin provides:

- A repo-hosted `logo.png` wired into the plugin manifest
- A `.mcp.json` entry for the official remote MCP server (`https://mcp.hackmd.io/`, OAuth)
- A `hackmd-cli` skill for shell workflows
- A `setup-hackmd` command for local setup and verification
- A light HackMD workflow rule for note-related tasks

MCP does **not** use an API token. OAuth runs in the browser on first use. The REST API and CLI still use personal API tokens; those credentials do not belong in MCP config.

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

Cursor loads plugins from `~/.cursor/plugins/local/<plugin-name>/`. The folder name should match the `name` field in `.cursor-plugin/plugin.json` (this repo: `hackmd`).

**Recommended:** copy or sync the repo into that path (avoids symlink issues some builds have):

```bash
mkdir -p ~/.cursor/plugins/local
rm -rf ~/.cursor/plugins/local/hackmd
rsync -a --delete --exclude='.git' ./ ~/.cursor/plugins/local/hackmd/
```

From anywhere, using an absolute path to your clone:

```bash
rsync -a --delete --exclude='.git' /path/to/cursor-plugin/ ~/.cursor/plugins/local/hackmd/
```

**Alternative:** symlink instead of copying:

```bash
mkdir -p ~/.cursor/plugins/local
ln -sfn "$(pwd)" ~/.cursor/plugins/local/hackmd
```

Quit Cursor completely and reopen it (a window reload is not always enough). Then verify:

- the `hackmd` plugin appears in the local plugin list
- `logo.png` renders correctly
- the `hackmd-cli` skill is available
- the `setup-hackmd` command is available
- the `hackmd-workflow` rule is discoverable
- Cursor has loaded the plugin MCP server `hackmd`

## Setup

### 1. Install the plugin in Cursor

Use this repository as the plugin source in Cursor, or sync it into `~/.cursor/plugins/local/hackmd/` as in [Local Testing](#local-testing).

The plugin entrypoint is:

```text
.cursor-plugin/plugin.json
```

### 2. Connect HackMD MCP (OAuth)

If Cursor does not pick up the plugin `.mcp.json` automatically, merge this into `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "hackmd": {
      "url": "https://mcp.hackmd.io/"
    }
  }
}
```

Fully restart Cursor, then invoke any HackMD tool (or ask Cursor to list your notes). Complete the browser OAuth prompt. Closing it leaves the client unauthorized.

Do not add `mcp-remote`, `Authorization` headers, or `HMD_API_ACCESS_TOKEN` to MCP config. Those are leftover from the token-era setup.

### 3. Optional: HackMD CLI

CLI is not required for MCP. Use it for shell scripting and export.

```bash
npm install -g @hackmd/hackmd-cli
hackmd-cli login
hackmd-cli whoami
```

For HackMD EE instances, also set:

```bash
export HMD_API_ENDPOINT_URL=https://your.hackmd-ee.endpoint
```

### 4. Verify

In Cursor, confirm the `hackmd` MCP server appears and a tool call succeeds after OAuth.

If you installed the CLI:

```bash
hackmd-cli whoami
hackmd-cli notes
```

## Included Components

### Skill: `hackmd-cli`

Use this when working with HackMD notes via CLI, including personal notes, team notes, exports, and scripting workflows. MCP OAuth does not replace CLI login.

### Command: `setup-hackmd`

Use this command to guide users through:

- installing the plugin
- connecting MCP with OAuth
- optionally installing and logging in to the HackMD CLI
- verifying MCP (and CLI, if installed)

### Rule: `hackmd-workflow`

Use this rule for HackMD-related work so Cursor prefers MCP OAuth for chat-native notes, uses the CLI only for shell workflows, and never copies a CLI token into MCP config.

## Notes

- The plugin manifest references the local `logo.png` asset.
- The repo `.mcp.json` is URL-only: `https://mcp.hackmd.io/`.
- The plugin keeps scope narrow so it can be extended safely later.

Setup guide: [HackMD MCP Server Setup](https://hackmd.io/@docs/mcp-server-setup)

Upstream CLI skill: [HackMD CLI skill](https://github.com/hackmdio/hackmd-cli/blob/develop/hackmd-cli/SKILL.md)
