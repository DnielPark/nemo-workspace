# 🟦 네모 워크스페이스 (Nemo Workspace)

OpenClaw AI 어시스턴트 **네모**의 워크스페이스입니다.  
정체성(SOUL), 스킬 문서, 프로젝트 파일, 설정을 관리합니다.

---

## 📦 리눅스 서버 재설치 가이드

> 기존 Windows 서버에서 리눅스 서버로 이사할 때 참고하세요.

### 1. OpenClaw 설치

```bash
# Node.js 설치 (OpenClaw v2026.5.27 기준)
curl -fsSL https://deb.nodesource.com/setup_24.x | sudo -E bash -
sudo apt install -y nodejs git

# OpenClaw 설치
npm install -g openclaw
```

### 2. 워크스페이스 클론

```bash
cd ~
git clone https://github.com/DnielPark/nemo-workspace.git .openclaw/workspace
```

### 3. 민감 파일 복원

USB에서 다음 파일들을 워크스페이스로 복사하세요:

| 파일 | 설명 |
|------|------|
| `MEMORY.md` | 장기 기억 (서버 IP, 토큰 등 포함) |
| `memory/` | 일별 기록 폴더 |
| `.env` | 환경 변수 (필요시) |

```bash
# USB 마운트 후 복사 예시
cp /mnt/usb/MEMORY.md ~/.openclaw/workspace/
cp -r /mnt/usb/memory ~/.openclaw/workspace/
```

### 4. Flask 파일 서버 설치

별도 레포 `nemo-flask-server` 참고:

```bash
git clone https://github.com/DnielPark/nemo-flask-server.git
cd nemo-flask-server
pip install -r requirements.txt
# .env 파일 USB에서 복사
cp /mnt/usb/flask.env .env
python file_server_external.py
```

### 5. OpenClaw 실행

```bash
# Gateway 실행 (기본 포트 18789)
openclaw gateway run
```

---

## 🔧 워크스페이스 구조

```
workspace/
├── SOUL.md          # 네모의 성격과 행동 원칙
├── IDENTITY.md      # 기본 신원 정보
├── USER.md          # 사용자(대니얼) 정보
├── MEMORY.md        # 장기 기억 (⚠️ git 제외, USB 보관)
├── TOOLS.md         # 스킬 인덱스
├── AGENTS.md        # 에이전트 설정
├── HEARTBEAT.md     # 주기적 체크 목록
├── skills/          # 상세 스킬 문서들
└── memory/          # 일별 기록 (⚠️ git 제외, USB 보관)
```

---

## 📚 관련 레포

| 레포 | 설명 |
|------|------|
| [nemo-workspace](https://github.com/DnielPark/nemo-workspace) | 네모 워크스페이스 (👈 지금 여기) |
| [nemo-flask-server](https://github.com/DnielPark/nemo-flask-server) | Flask 기반 파일 서버 |
