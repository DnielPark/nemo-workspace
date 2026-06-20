# [NO.09] 홈서버 에이전트 설치 — `skills/home-server-agent.md`

> **서버:** 대니얼 집 리눅스 서버 (라이젠 4000, 16GB)
> **목표:** 아들(18789)과 독립된 미르 게이트웨이(18790) 설치
> **진행:** 대니얼이 리눅스 서버에서 직접 실행

---

## ⚠️ 최우선 원칙

**기존 아들 서비스를 절대 건드리지 않는다.**
- 아들 OpenClaw 게이트웨이 (포트 18789)
- 게임 서버, 언론사 사이트 프로젝트
- Playit 터널 설정

모든 작업은 **완전히 독립된 디렉토리**에서 진행.

---

## 📋 설치 절차 (리눅스 서버 직접 실행)

### 1. Node.js 확인

```bash
node --version
# 20+ 없으면 설치
```

### 2. OpenClaw 설치

```bash
npm install -g openclaw
```

### 3. 전용 디렉토리 생성

```bash
mkdir -p ~/openclaw-mir
cd ~/openclaw-mir
```

### 4. config.yml 생성

```bash
cat > config.yml << 'EOF'
gateway:
  port: 18790
  host: 127.0.0.1
EOF
```

> `port: 18790` = 아들(18789)과 충돌 방지

### 5. GitHub에서 워크스페이스 복제

```bash
cd ~/openclaw-mir
git clone https://github.com/DnielPark/office-server-agent.git workspace
```

> 또는 이미 pull 할 수 있는 환경이면:
> ```bash
> git clone git@github.com:DnielPark/office-server-agent.git workspace
> ```

### 6. 초기화

```bash
cd ~/openclaw-mir
openclaw init --home .
```

### 7. 실행

```bash
openclaw start --home ~/openclaw-mir --config ~/openclaw-mir/config.yml
```

### 8. 확인

```bash
openclaw status
# 또는
curl http://127.0.0.1:18790/health
```

---

## 🔧 systemd 등록 (자동 실행)

```bash
sudo tee /etc/systemd/system/openclaw-mir.service << 'EOF'
[Unit]
Description=OpenClaw Mir Gateway
After=network.target

[Service]
Type=simple
User=여기에_리눅스_사용자명
ExecStart=/usr/bin/openclaw start --home /home/여기에_리눅스_사용자명/openclaw-mir --config /home/여기에_리눅스_사용자명/openclaw-mir/config.yml
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable openclaw-mir
sudo systemctl start openclaw-mir
```

---

## 📱 텔레그램 연동

1. @BotFather 에서 새 봇 생성 → 토큰 발급
2. config.yml 에 추가:

```yaml
telegram:
  token: "발급받은_토큰"
```

> ⚠️ 아들 봇과 **다른 토큰** 사용 필수

---

## 🔄 GitHub 동기화

```bash
# workspace에서 pull
cd ~/openclaw-mir/workspace
git pull
```

---

## 🆔 미르 정체성 설정 (선택)

workspace에 있는 파일들 중 미르에 맞게 수정하면 좋은 것:

| 파일 | 내용 |
|------|------|
| `SOUL.md` | 이름을 "미르"로, vibe는 알아서 |
| `IDENTITY.md` | 네모 → 미르로 변경 |
| `MEMORY.md` | 사무실 정보 지우고 홈서버 정보로 채움 |
| `USER.md` | 동일 (대니얼) |

---

## 🗺 포트 구성

| 서비스 | 포트 | 설명 |
|--------|------|------|
| 아들 OpenClaw | 18789 | 기존 |
| **미르 OpenClaw** | **18790** | **신규** |
| 게임 서버 | ? | 건드리지 말 것 |
| 언론사 사이트 | ? | 건드리지 말 것 |

---

## 🔍 상태 확인

```bash
sudo systemctl status openclaw-mir
sudo journalctl -u openclaw-mir -n 50 -f
ps aux | grep openclaw
```

---

## ⛔ 금지

- 아들 프로세스/파일/설정 절대 건드리지 말 것
- Playit 터널 설정 건드리지 말 것
- 80/443 포트 서비스 건드리지 말 것
