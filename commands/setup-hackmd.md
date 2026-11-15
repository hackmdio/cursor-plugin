---
name: setup-hackmd
description: Guide HackMD plugin setup, including OAuth MCP sign-in, optional CLI install, and basic verification.
---

# Setup HackMD

Help the user set up HackMD in Cursor by following these steps.

The plugin ships an MCP server (`mcp.json` → `https://mcp.hackmd.io/`) that signs in with **OAuth** in the browser. The HackMD CLI has its own login (interactive login or a personal API token). The two are separate, and connecting MCP doesn't replace CLI credentials.

1. Confirm the user has a HackMD account at [hackmd.io](https://hackmd.io), and ask whether they use HackMD Cloud or HackMD EE.
1. Confirm the plugin is installed: from the Cursor Marketplace, or copied into `~/.cursor/plugins/local/hackmd/` for local testing. A symlink to a clone elsewhere is skipped.
1. In **Customize**, confirm the HackMD MCP server from the plugin (`plugin-hackmd-hackmd`) is listed and turned on. Users don't need to copy anything into `~/.cursor/mcp.json`.
1. Call the `get-me` tool, or ask to list HackMD notes, to start the browser OAuth prompt. The user must sign in and approve. Closing the prompt leaves the server unauthorized.
1. Don't ask for an API token to connect MCP. Don't add `mcp-remote`, `Authorization` headers, or `HMD_API_ACCESS_TOKEN` to the plugin's MCP config.
1. If OAuth isn't possible (for example on a headless machine), point the user to the README section "Manual MCP Setup (API token, alternative)". That setup lives in the user's `~/.cursor/mcp.json` and reads the token through `${env:HMD_API_ACCESS_TOKEN}`, never as a literal value.
1. Optional: install the CLI for shell workflows. MCP doesn't need it.

```bash
npm install -g @hackmd/hackmd-cli
hackmd-cli login
hackmd-cli whoami
```

For HackMD EE, also set:

```bash
export HMD_API_ENDPOINT_URL=https://your.hackmd-ee.endpoint
```

1. Verify:

- MCP: `get-me` returns the user's HackMD profile.
- CLI (if installed): `hackmd-cli whoami` and `hackmd-cli notes` succeed.

1. For scripting or automation with the CLI:

- use `hackmd-cli notes` for personal notes
- use `hackmd-cli team-notes --teamPath=...` for team content
- use `--output=json` for machine-readable output

1. Never paste real secrets into docs, examples, commits, screenshots, or tracked plugin files.
