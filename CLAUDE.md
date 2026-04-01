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
| Storage | `~/.paperclip/instances/default/data/storage/` |
| Secrets | `~/.paperclip/instances/default/secrets/master.key` |

A snapshot of the working config is kept at `paperclip/config.json` in this repo for reference and recovery.

## Commands

```bash
# Start (preserves existing config — do NOT use --yes, it resets to defaults)
npx paperclipai onboard

# Update a specific config section interactively
npx paperclipai configure -s server

# Check server health
curl http://127.0.0.1:3100/api/health

# View logs
tail -f ~/.paperclip/instances/default/logs/*.log
```

## Current Configuration

| Setting | Value |
|---------|-------|
| `server.deploymentMode` | `authenticated` |
| `server.exposure` | `public` |
| `server.host` | `0.0.0.0` |
| `server.port` | `3100` |
| `auth.publicBaseUrl` | `http://52.79.215.84` |
| `auth.baseUrlMode` | `explicit` |
| `auth.disableSignUp` | `false` |
| `database.mode` | `embedded-postgres` (port 54329) |
| `storage.provider` | `local_disk` |
| `secrets.provider` | `local_encrypted` |

Access UI at: **http://52.79.215.84:3100**

## Important: Config Reset Issue

`npx paperclipai onboard --yes` resets `config.json` to defaults (`host: 127.0.0.1`, `exposure: private`) every time. Always start with `npx paperclipai onboard` (no `--yes`) and choose **Advanced setup** to preserve settings. If config is accidentally reset, restore from `paperclip/config.json` in this repo, then run `npx paperclipai configure -s server`.

## AWS Requirement

Security Group inbound rule required: TCP port `3100` open to `0.0.0.0/0` (or specific IP).
