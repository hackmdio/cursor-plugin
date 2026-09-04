---
name: setup-hackmd
description: Guide HackMD plugin setup, including OAuth MCP, optional CLI install, and basic verification.
---

# Setup HackMD

Help the user set up HackMD tooling in Cursor using this sequence.

The MCP server (`mcp.hackmd.io`) authenticates with **OAuth 2.1**. The CLI continues to use interactive login or a personal API token. The two are independent — connecting MCP does not replace CLI credentials.

1. Confirm they have a HackMD account at [hackmd.io](https://hackmd.io).
2. Confirm the plugin is installed (this repo, or synced to `~/.cursor/plugins/local/hackmd/`).
3. Merge the plugin `.mcp.json` into the user's Cursor MCP config at `~/.cursor/mcp.json` (or confirm Cursor already loaded the plugin's MCP entry):

```json
{
  "mcpServers": {
    "hackmd": {
      "url": "https://mcp.hackmd.io/"
    }
  }
}
```

Do **not** add `mcp-remote`, `Authorization` headers, or API tokens to this MCP config. MCP does not use `HMD_API_ACCESS_TOKEN`.

4. Fully quit and reopen Cursor (a window reload is not enough).
5. Ask Cursor to list HackMD notes (or call any HackMD MCP tool). Complete the browser OAuth prompt. Closing it leaves the client unauthorized.
6. Optional: install the CLI for shell workflows (not required for MCP):

```bash
npm install -g @hackmd/hackmd-cli
hackmd-cli login
hackmd-cli whoami
```

`hackmd-cli login` is enough for most people. An API token is only for CLI/REST, never for MCP.

7. Verify MCP: the `hackmd` server appears in Cursor's MCP list, and a tool call succeeds after OAuth.
8. Never paste real secrets into docs, examples, commits, screenshots, or tracked plugin files.
