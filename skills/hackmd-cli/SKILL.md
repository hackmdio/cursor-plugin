---
name: hackmd-cli
description: Use HackMD CLI to create, read, update, delete, export, and automate personal notes and team notes from Cursor.
---

# HackMD CLI

Use this skill when a user wants to manage HackMD content programmatically from Cursor.

## What This Skill Covers

- personal note workflows
- team note workflows
- authentication and setup
- export and sync workflows
- scripting-friendly output formats

## Setup

### Install

```bash
npm install -g @hackmd/hackmd-cli
```

### Configure Access Token

Create an API token at [hackmd.io/settings#api](https://hackmd.io/settings#api), then configure either:

```bash
# Interactive login (stores credentials locally)
hackmd-cli login
```

or:

```bash
export HMD_API_ACCESS_TOKEN=YOUR_TOKEN
```

For HackMD EE instances:

```bash
export HMD_API_ENDPOINT_URL=https://your.hackmd-ee.endpoint
```

## Core Commands

### Authentication

```bash
hackmd-cli login
hackmd-cli logout
hackmd-cli whoami
```

### Personal Notes

```bash
# List notes
hackmd-cli notes

# Get one note
hackmd-cli notes --noteId=<id>

# Create a note
hackmd-cli notes create --content='# Title' --title='My Note'

# Create from stdin
cat README.md | hackmd-cli notes create

# Open editor flow
hackmd-cli notes create -e

# Update a note
hackmd-cli notes update --noteId=<id> --content='# Updated'

# Delete a note
hackmd-cli notes delete --noteId=<id>
```

### Team Notes

```bash
# List team notes
hackmd-cli team-notes --teamPath=<team-path>

# Create a team note
hackmd-cli team-notes create --teamPath=<team-path> --content='# Team Doc'

# Update a team note
hackmd-cli team-notes update --teamPath=<team-path> --noteId=<id> --content='# Updated'

# Delete a team note
hackmd-cli team-notes delete --teamPath=<team-path> --noteId=<id>
```

### Teams, History, and Export

```bash
hackmd-cli teams
hackmd-cli history
hackmd-cli export --noteId=<id>
```

## Output Options

Use these flags on list-style commands when scripting:

```bash
--output=json
--output=yaml
--output=csv
--no-header
--no-truncate
--columns=id,title
--filter=title=README
--sort=title
-x
```

## Common Workflows

### Sync a local file to HackMD

```bash
# Create a new note from a file
cat doc.md | hackmd-cli notes create --title="My Doc"

# Update an existing note from a file
cat doc.md | hackmd-cli notes update --noteId=<id>
```

### Export a note to a local file

```bash
hackmd-cli export --noteId=<id> > note.md
```

### Script against note metadata

```bash
hackmd-cli notes --output=json | jq '.[] | .id'
```

## Guidance

- Use `notes` for personal notes.
- Use `team-notes` only when a `teamPath` is known.
- Prefer environment variables or interactive login over hardcoding tokens.
- Keep examples free of real secrets.

## Source

Adapted from the upstream HackMD CLI skill:
[github.com/hackmdio/hackmd-cli](https://github.com/hackmdio/hackmd-cli/blob/develop/hackmd-cli/SKILL.md)
