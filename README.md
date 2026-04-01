# Study-05: Paperclip AI Agent Orchestration

Paperclip AI 에이전트 오케스트레이션 인스턴스 실습 프로젝트.

## 개요

[Paperclip](https://github.com/paperclipai/paperclip)은 AI 에이전트를 구조화된 조직(org chart, heartbeat, 비용 제어, 거버넌스)으로 운영하는 오픈소스 플랫폼이다.

## 서버 접속

| 항목 | 값 |
|------|-----|
| UI | http://15.164.190.193:3100 |
| 배포 모드 | `authenticated` |
| 노출 | `public` |
| 호스트 | `0.0.0.0` |
| 포트 | `3100` |

## 인스턴스 경로

| 항목 | 경로 |
|------|------|
| 설정 | `~/.paperclip/instances/default/config.json` |
| 로그 | `~/.paperclip/instances/default/logs/` |
| DB | `~/.paperclip/instances/default/db/` (내장 PostgreSQL, 포트 54329) |
| 백업 | `~/.paperclip/instances/default/data/backups/` |
| 스토리지 | `~/.paperclip/instances/default/data/storage/` |
| 시크릿 | `~/.paperclip/instances/default/secrets/master.key` |

## 주요 명령어

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
```

## 프로세스 관리

pm2로 백그라운드 실행 중. EC2 재부팅 시 자동 시작 등록 완료.

## 주의 사항

- `npx paperclipai onboard --yes` 실행 시 `config.json`이 기본값으로 초기화된다 (`host: 127.0.0.1`, `exposure: private`).
- 항상 `npx paperclipai onboard` (옵션 없이) 실행 후 **Advanced setup**을 선택해야 설정이 유지된다.
- 설정이 초기화된 경우 `paperclip/config.json` 스냅샷을 참고해 `npx paperclipai configure -s server`로 복구한다.
- AWS Security Group에서 TCP 포트 `3100`을 `0.0.0.0/0`으로 열어야 외부 접속이 가능하다.
