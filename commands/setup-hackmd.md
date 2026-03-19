---
name: setup-hackmd
description: Guide HackMD plugin setup, including CLI install, auth configuration, safe local MCP setup, and basic verification.
---

# Setup HackMD

Help the user set up HackMD tooling in Cursor using this sequence:

1. Confirm whether they need HackMD Cloud or HackMD EE setup.
1. Install the CLI if needed:

```bash
npm install -g @hackmd/hackmd-cli
```

1. Configure CLI auth with either:

```bash
hackmd-cli login
```

or:

```bash
export HMD_API_ACCESS_TOKEN=YOUR_TOKEN
```

1. If they use HackMD EE, also configure:

```bash
export HMD_API_ENDPOINT_URL=https://your.hackmd-ee.endpoint
```

1. Copy the `hackmd-mcp` entry from the plugin `.mcp.json` into the user's local Cursor MCP config at `~/.cursor/mcp.json`.
1. Use a final local config shaped like:

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

1. Replace:

```text
Bearer <HACKMD_API_TOKEN>
```

with the correct bearer token value in `~/.cursor/mcp.json`, not in the tracked plugin repository.

1. Verify CLI access:

```bash
hackmd-cli whoami
hackmd-cli notes
```

1. Verify the MCP setup by confirming Cursor can see the configured `hackmd-mcp` server from the user's local MCP configuration.
1. If the user is scripting or automating content:

- use `hackmd-cli notes` for personal notes
- use `hackmd-cli team-notes` for team content
- use JSON output for automation

1. Never paste real secrets into docs, examples, commits, screenshots, or tracked plugin files.
