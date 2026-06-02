# Flask 파일 서버 (5050)

## 개요

외부 파일 서버. Cloudflare Tunnel을 통해 `files.semonara.com`으로 외부 접속 가능.

- **포트:** 5050
- **소스:** `E:\project\file_server_external.py`
- **제공 폴더:** `E:\openshare/`
- **프로젝트:** sungsan1, sungsan2, gudong1, gudong2
- **인증:** `.env` 파일의 프로젝트별 비밀번호

## 실행

```cmd
cd /d E:\project
python file_server_external.py
```

## .env 설정

`E:\project\.env`:
```
SUNGSAN1_PW=000000
SUNGSAN2_PW=000000
GUDONG1_PW=000000
GUDONG2_PW=000000

# SMTP (로컬 폴더 이메일 인증)
SMTP_HOST=smtp.mail.nate.com
SMTP_PORT=587
SMTP_SSL=False
SMTP_USER=daeho1001
SMTP_PASS=
SMTP_FROM=daeho1001@nate.com
ADMIN_EMAIL=daeho1001@nate.com
```

> ⚠️ 현재 비밀번호 모두 000000 — 보안 강화 필요 시 변경

## 로컬 폴더 인증
- 이메일 인증 → 토큰 발급 방식
- 관리자(ADMIN_EMAIL)만 `/local/admin` 접근 가능
- 자세한 내용은 `server-info.md` 참고

## 템플릿

`E:\project\templates/`:
- `index.html` — 메인 페이지
- `auth.html` — 인증 페이지
- `files.html` — 파일 목록 페이지
- `local.html` — 로컬 폴더 메인
- `local_auth.html` — 이메일 인증 페이지
- `local_admin.html` — 관리자 페이지

## 상태 확인

```cmd
curl -s -o NUL -w "%%{http_code}" http://localhost:5050/
netstat -ano | findstr :5050
```

## 관련 문서
- `skills/cloudflare-tunnel.md` — 외부 접속 (files.semonara.com)
- `skills/server-info.md` — 서버 기본 정보 및 추가 설정
