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
# 백그라운드 실행 (pm2 — 권장)
pm2 start "npx paperclipai run" --name paperclip
pm2 stop paperclip
pm2 restart paperclip
pm2 logs paperclip

# 포그라운드 실행 (설정 유지 — --yes 사용 금지)
npx paperclipai onboard

# 특정 설정 섹션 업데이트
npx paperclipai configure -s server

# 서버 상태 확인
curl http://127.0.0.1:3100/api/health

# 로그 확인
tail -f ~/.paperclip/instances/default/logs/*.log
```

## 프로세스 관리 (pm2)

pm2로 백그라운드 실행 중. EC2 재부팅 시 자동 시작 등록 완료(`pm2 startup` + `pm2 save`).

pm2 로그 위치: `~/.pm2/logs/paperclip-out.log`

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
