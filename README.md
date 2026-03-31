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
- the `.mcp.json` template is at the repo root for merging into your user MCP config (see [Setup](#2-configure-hackmd-mcp-safely))

## Setup

### 1. Install the plugin in Cursor

Use this repository as the plugin source in Cursor, or sync it into `~/.cursor/plugins/local/hackmd/` as in [Local Testing](#local-testing).

The plugin entrypoint is:

```text
.cursor-plugin/plugin.json
```

### 2. Configure HackMD MCP safely

Do not put API tokens inside `mcp.json`. In Cursor, `mcp.json` values support interpolation: strings like `${env:VAR}` are resolved from your environment when MCP loads.

1. Copy the `hackmd-mcp` entry from this repo's `.mcp.json`
2. Paste it into your user-level Cursor MCP config at `~/.cursor/mcp.json` (or merge into an existing file)

The header uses the same variable as the HackMD CLI: `HMD_API_ACCESS_TOKEN`.

Example (no secrets in the file):

```json
{
  "mcpServers": {
    "hackmd-mcp": {
      "command": "npx",
      "args": [
        "mcp-remote@0.1.38",
        "https://mcp.hackmd.io/",
        "--header",
        "Authorization:Bearer ${env:HMD_API_ACCESS_TOKEN}"
      ]
    }
  }
}
```

Cursor resolves `${env:HMD_API_ACCESS_TOKEN}` from the environment it inherits when it starts (not from secrets embedded in `mcp.json`). Set the variable in your **shell profile** (for example `~/.zshrc` or `~/.bashrc`) and fully restart Cursor, or define it in your **OS user / system environment** so GUI-launched Cursor sees it—same idea as Cursor’s MCP docs: use config interpolation in `mcp.json` and keep real values in the environment. Use the same token as in [Configure HackMD CLI authentication](#4-configure-hackmd-cli-authentication) below.

To sanity-check the remote command from a terminal (shell variable, not Cursor interpolation):

```bash
export HMD_API_ACCESS_TOKEN=YOUR_TOKEN
npx mcp-remote@0.1.38 https://mcp.hackmd.io/ --header "Authorization:Bearer $HMD_API_ACCESS_TOKEN"
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
- The repo `.mcp.json` is a template with `${env:HMD_API_ACCESS_TOKEN}` interpolation and a pinned `mcp-remote` version.
- Keep tokens in the environment, not in committed or pasted config files.
- The plugin keeps scope narrow so it can be extended safely later.
- The `hackmd-cli` skill adapts the upstream HackMD CLI guide for Cursor plugin packaging.

Upstream reference: [HackMD CLI skill](https://github.com/hackmdio/hackmd-cli/blob/develop/hackmd-cli/SKILL.md)
