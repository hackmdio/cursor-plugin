---
name: setup-hackmd
description: Guide HackMD plugin setup, including CLI install, auth configuration, MCP placeholder replacement, and basic verification.
---

# Setup HackMD

Help the user set up HackMD tooling in Cursor using this sequence:

1. Confirm whether they need HackMD Cloud or HackMD EE setup.
2. Install the CLI if needed:

```bash
npm install -g @hackmd/hackmd-cli
```

3. Configure CLI auth with either:

```bash
hackmd-cli login
```

or:

```bash
export HMD_API_ACCESS_TOKEN=YOUR_TOKEN
```

4. If they use HackMD EE, also configure:

```bash
export HMD_API_ENDPOINT_URL=https://your.hackmd-ee.endpoint
```

5. Open `mcp.json` and replace:

```text
Bearer <HACKMD_API_TOKEN>
```

with the correct bearer token value.

6. Verify CLI access:

```bash
hackmd-cli whoami
hackmd-cli notes
```

7. If the user is scripting or automating content:
- use `hackmd-cli notes` for personal notes
- use `hackmd-cli team-notes` for team content
- use JSON output for automation

8. Never paste real secrets into docs, examples, commits, or screenshots.
