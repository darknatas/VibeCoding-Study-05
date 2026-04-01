# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This project runs a **Paperclip** AI agent orchestration instance — an open-source platform for operating AI agents as a structured company (org chart, heartbeats, cost control, governance).

- GitHub: https://github.com/paperclipai/paperclip
- Docs: https://paperclip.ing/

## Paperclip Instance

Paperclip is installed globally via npx. Instance data lives outside this directory:

| Item | Path |
|------|------|
| Config | `~/.paperclip/instances/default/config.json` |
| Logs | `~/.paperclip/instances/default/logs/` |
| DB | `~/.paperclip/instances/default/db/` (embedded PostgreSQL, port 54329) |
| Backups | `~/.paperclip/instances/default/data/backups/` |
| Secrets | `~/.paperclip/instances/default/secrets/master.key` |

## Commands

```bash
# Start (preserves existing config — do NOT use --yes, it resets to defaults)
npx paperclipai onboard

# Update a specific config section interactively
npx paperclipai configure -s server

# Check server health
curl http://127.0.0.1:3100/api/health
```

## Current Server Configuration

| Setting | Value |
|---------|-------|
| `deploymentMode` | `authenticated` |
| `exposure` | `public` |
| `host` | `0.0.0.0` |
| `port` | `3100` |
| `publicBaseUrl` | `http://52.79.215.84` |

Access UI at: **http://52.79.215.84:3100**

## Important: Config Reset Issue

`npx paperclipai onboard --yes` resets `config.json` to defaults (`host: 127.0.0.1`, `exposure: private`) every time. Always start with `npx paperclipai onboard` (no `--yes`) and choose **Advanced setup** to preserve settings. If config is accidentally reset, run `npx paperclipai configure -s server` to restore.

## AWS Requirement

Security Group inbound rule required: TCP port `3100` open to `0.0.0.0/0` (or specific IP).
