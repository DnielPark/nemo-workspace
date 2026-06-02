# Office Agent Sync — GitHub 동기화

## 개요

에이전트 워크스페이스(`C:\Users\admin\.openclaw\workspace`)를 GitHub 비공개 레포 `office-server-agent`와 동기화.

**목적:** 서버 접속 없이도 네모의 정체성, 스킬, 프로젝트 파일을 웹에서 확인/관리.

## GitHub 레포

- **URL:** https://github.com/DnielPark/office-server-agent
- **접근:** 비공개 (DnielPark만 접근 가능)
- **인증:** GitHub PAT (remote URL에 포함)

## 동기화 범위

### ✅ 포함
- `SOUL.md`, `IDENTITY.md`, `USER.md`
- `TOOLS.md`, `AGENTS.md`, `HEARTBEAT.md`
- `skills/*.md` (6개 스킬 문서)
- `.gitignore`
- 추후 workspace 내 프로젝트 파일들

### ❌ 제외 (`.gitignore`)
- `MEMORY.md` — 민감 정보(IP/경로) 포함
- `memory/` — 일일 기록
- `.openclaw/workspace-state.json` — 내부 상태
- `_*.ps1`, `_*.py`, `query` — 임시 스크립트
- 인증/토큰 관련 파일

## Push

### 수동 Push
```cmd
cd /d C:\Users\admin\.openclaw\workspace
git add -A
git commit -m "업데이트 내용"
git push origin master
```

### 자동 Push (cron)
필요시 주기적 cron job으로 자동 push 설정 가능.

## Pull (외부에서 GitHub 수정 시)
```cmd
cd /d C:\Users\admin\.openclaw\workspace
git pull origin master
```

## 주의사항
- `.gitignore`에 등록된 파일은 절대 push되지 않도록 확인
- 충돌 발생 시 수동 해결 필요
- GitHub 웹 에디터로 수정 시 반드시 서버에서 `git pull` 할 것
