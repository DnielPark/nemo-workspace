# Cloudflare Tunnel — files.semonara.com

## 개요

**files.semonara.com** → Cloudflare Tunnel → **localhost:5050** (Flask 파일 서버)

외부에서 사무실 서버의 Flask 파일 서버(5050)에 접속할 수 있게 해주는 터널.

## 연결 정보

| 항목 | 내용 |
|------|------|
| Tunnel ID | `a25968d2-4913-4154-b41b-70f3b1b52946` |
| 도메인 | `files.semonara.com` |
| 로컬 포트 | `localhost:5050` |
| 실행 파일 | `C:\WINDOWS\system32\cloudflared.exe` |
| 설정 파일 | `C:\Users\admin\.cloudflared\config.yml` |
| 인증 파일 | `C:\Users\admin\.cloudflared\a25968d2-4913-4154-b41b-70f3b1b52946.json` |

## 설정 내용 (`config.yml`)

```yaml
tunnel: a25968d2-4913-4154-b41b-70f3b1b52946
credentials-file: C:\Users\admin\.cloudflared\a25968d2-4913-4154-b41b-70f3b1b52946.json

ingress:
  - hostname: files.semonara.com
    service: http://localhost:5050
  - service: http_status:404
```

## 실행 방법

**터널만 실행:**
```cmd
cloudflared tunnel run a25968d2-4913-4154-b41b-70f3b1b52946
```

**Flask 서버도 같이 켜야 접속 가능:**
```cmd
cd /d E:\project
python file_server_external.py
```

## 확인

브라우저에서 `https://files.semonara.com` 접속 (Cloudflare Tunnel → 로컬 5050)

## 참고

- Cloudflare Tunnel **만 실행**하면 터널은 연결되지만 Flask가 꺼져 있으면 502 에러
- Cloudflare edge 위치: **ICN (인천)**
- 프로토콜: QUIC (HTTP/3)
