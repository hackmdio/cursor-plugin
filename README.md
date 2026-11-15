# HackMD Cursor Plugin

The official HackMD plugin for Cursor.

When you install the plugin, Cursor connects to the HackMD MCP server at `https://mcp.hackmd.io/`. You sign in to HackMD in your browser with OAuth, so you don't need an API token.

The plugin includes:

- **MCP server** (`mcp.json`): the remote HackMD MCP server, which uses OAuth sign-in
- **Skill** `hackmd-cli`: manage personal and team notes from the shell with the HackMD CLI
- **Command** `setup-hackmd`: walks you through setup and checks that it works
- **Rule** `hackmd-workflow`: guidance for HackMD note tasks, including keeping credentials safe

- Homepage: <https://hackmd.io>
- Source: <https://github.com/hackmdio/cursor-plugin>
- [HackMD Terms of Service](https://hackmd.io/s/terms)
- [HackMD Privacy Policy](https://hackmd.io/s/privacy)

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
├── mcp.json
├── CHANGELOG.md
├── LICENSE
├── logo.png
└── README.md
```

## Install

Install **HackMD** from the [Cursor Marketplace](https://cursor.com/marketplace), or from **Customize** in Cursor, once the plugin is listed there. To install it from this repository instead, see [Local Testing](#local-testing).

## Connect HackMD MCP (OAuth)

The plugin's `mcp.json` registers this server:

```json
{
  "mcpServers": {
    "hackmd": {
      "url": "https://mcp.hackmd.io/"
    }
  }
}
```

Cursor names plugin MCP servers with a `plugin-` prefix, so this one appears as `plugin-hackmd-hackmd` in Customize and in approval prompts.

1. Open **Customize** and check that the HackMD MCP server is listed and turned on.
2. Ask the agent to do something with HackMD, for example "show my HackMD profile" (the `get-me` tool) or "list my HackMD notes".
3. When Cursor opens the HackMD sign-in page in your browser, sign in and approve access. If you close that page without approving, the server stays unauthorized.

The OAuth connection doesn't use an API token. Don't add `Authorization` headers, `mcp-remote`, or `HMD_API_ACCESS_TOKEN` to the plugin's MCP config.

More detail: [HackMD MCP Server Setup](https://hackmd.io/@docs/mcp-server-setup).

## Local Testing

Cursor loads local plugins from `~/.cursor/plugins/local/<plugin-name>/`. Name the folder after the `name` field in `.cursor-plugin/plugin.json`, which is `hackmd` for this repo.

From your clone, **copy** the repository into that folder:

```bash
mkdir -p ~/.cursor/plugins/local
rm -rf ~/.cursor/plugins/local/hackmd
rsync -a --exclude='.git' ./ ~/.cursor/plugins/local/hackmd/
```

If you don't have `rsync`, use `cp` instead:

```bash
mkdir -p ~/.cursor/plugins/local
rm -rf ~/.cursor/plugins/local/hackmd
cp -R . ~/.cursor/plugins/local/hackmd
rm -rf ~/.cursor/plugins/local/hackmd/.git
```

Don't symlink your clone into `~/.cursor/plugins/local`. Cursor loads a symlink there only if its target is inside that folder, and it skips symlinks that point to a repository somewhere else on disk. Copy the files again whenever you change them.

Restart Cursor, or run **Developer: Reload Window**. Then open **Customize** and check that:

- the `hackmd` plugin appears and `logo.png` shows correctly
- the HackMD MCP server (`plugin-hackmd-hackmd`) is listed, and you can complete the OAuth sign-in from [Connect HackMD MCP](#connect-hackmd-mcp-oauth)
- the agent can run the `get-me` tool and get your HackMD profile back
- the `hackmd-cli` skill, the `setup-hackmd` command, and the `hackmd-workflow` rule are available

If the plugin doesn't show up:

- If HackMD is already installed from the Marketplace, uninstall it first. A Marketplace install takes precedence over a local copy with the same name.
- On Enterprise plans, an admin must turn on **Allow Local Plugin Imports**.

## Manual MCP Setup (API token, alternative)

Use this only if you can't use OAuth, for example on a headless machine or in an environment without a browser. It runs the server through `mcp-remote` with a personal API token, and you configure it in your **user-level** `~/.cursor/mcp.json`, not in this plugin.

1. Create an API token at [hackmd.io/settings#api](https://hackmd.io/settings#api).
2. Set `HMD_API_ACCESS_TOKEN` in the environment Cursor starts with, for example in `~/.zshrc` or `~/.bashrc` or in your OS user environment. Then quit and reopen Cursor.
3. Add this entry to `~/.cursor/mcp.json`:

   ```json
   {
     "mcpServers": {
       "hackmd-token": {
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

   `${env:...}` is Cursor's syntax for reading an environment variable in a user `mcp.json`. The file holds only the variable name, never the token.

4. In **Customize**, turn off the plugin's OAuth server (`plugin-hackmd-hackmd`) so you don't have two HackMD servers.

To check the command from a terminal:

```bash
export HMD_API_ACCESS_TOKEN=YOUR_TOKEN
npx mcp-remote@0.1.38 https://mcp.hackmd.io/ --header "Authorization:Bearer $HMD_API_ACCESS_TOKEN"
```

Never commit a real token or paste it into tracked files.

## HackMD CLI (optional)

You don't need the CLI for MCP. It's useful for shell scripting, exports, and HackMD EE endpoints.

```bash
npm install -g @hackmd/hackmd-cli
hackmd-cli login        # or: export HMD_API_ACCESS_TOKEN=YOUR_TOKEN
hackmd-cli whoami
hackmd-cli notes
```

For HackMD EE instances, also set:

```bash
export HMD_API_ENDPOINT_URL=https://your.hackmd-ee.endpoint
```

CLI credentials and the MCP OAuth sign-in are separate. Signing in to one doesn't sign you in to the other.

## Included Components

### Skill: `hackmd-cli`

For working with HackMD notes through the CLI: personal notes, team notes, exports, and scripts.

### Command: `setup-hackmd`

Walks you through:

- connecting the HackMD MCP server with OAuth
- optionally installing the HackMD CLI and logging in
- checking that MCP (and the CLI, if installed) works

### Rule: `hackmd-workflow`

Guidance for HackMD-related work. It uses MCP with OAuth for notes in chat, uses the CLI for shell workflows, and keeps tokens out of MCP config and committed files.

## Terms and Privacy

The HackMD service that this plugin connects to is covered by:

- [HackMD Terms of Service](https://hackmd.io/s/terms)
- [HackMD Privacy Policy](https://hackmd.io/s/privacy)

## License

[MIT](LICENSE)

Upstream CLI skill: [HackMD CLI skill](https://github.com/hackmdio/hackmd-cli/blob/develop/hackmd-cli/SKILL.md)
