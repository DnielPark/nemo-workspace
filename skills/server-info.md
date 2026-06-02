# 서버 정보

## 🖥️ 시스템 개요

| 항목 | 내용 |
|------|------|
| 역할 | 사무실 서버 + OpenClaw Gateway 호스트 |
| OS | Windows 10.0.26200 (x64) |
| 호스트명 | DESKTOP |
| 내부 IP | 192.168.1.211 |

## 🧰 개발 환경

| 항목 | 내용 |
|------|------|
| Node.js | v24.16.0 |
| OpenClaw | v2026.5.27 |
| Python | pythonw로 Flask/WsgiDAV 실행 중 |
| Gateway | ws://127.0.0.1:18789 (loopback) |

## 🔧 OpenClaw Gateway 서비스

- **등록 방식:** Windows Scheduled Task (`OpenClaw Gateway`)
- **실행 파일:** `~\.openclaw\gateway.cmd`
- **트리거:** 사용자 로그인 시
- **자동 재시작:** 1분 간격, 최대 3회 (죽으면 다시 살아남)
- **원격 제어:** 텔레그램 @tamsa112 → 네모

## 💾 드라이브 구성

| 드라이브 | 용량 | 설명 |
|---------|------|------|
| C: | 237GB | 시스템 |
| D: | — | CD-ROM (사용 안 함) |
| E: | 1TB (44GB 사용) | **웹하드 저장소** |

## 🌐 네트워크 서비스

### 내부 웹하드 (8080) — ⛔ 건드리지 말 것
- **서비스:** WsgiDAV (WebDAV) — 사내 네트워크 공유 시스템
- **포트:** 8080 (내부망 전용)
- **PID:** 5564 (pythonw)
- **상태:** 실행 중 (2026-05-27부터)
- **Z: 드라이브**로 WebDAV 연결 가능 (현재 연결 안 됨)

> ⚠️ **사내 공유 시스템 — 수정/중단/재시작 금지!**

### 외부 파일 서버 (5050)
- **서비스:** Flask 기반 파일 서버 (대외용)
- **포트:** 5050 (외부망, Cloudflare Tunnel → files.semonara.com)
- **소스:** `E:\project\`
- **대상:** `E:\openshare/`
- **프로젝트:** sungsan1, sungsan2, gudong1, gudong2

#### 프로젝트 소스 구조

```
E:\project\
├── file_server_external.py       ← Flask 메인
├── .env                          ← SMTP/비밀번호 설정
├── local_tokens.json             ← 로컬 폴더 인증 토큰
├── logs/                         ← 서버 로그
└── templates\
    ├── index.html          ← 프로젝트 선택 페이지
    ├── auth.html           ← 비밀번호 인증 페이지
    ├── files.html          ← 파일 탐색/업로드/삭제 페이지
    ├── local.html          ← 로컬 폴더 메인
    ├── local_auth.html     ← 로컬 이메일 인증 페이지
    └── local_admin.html    ← 로컬 폴더 토큰 관리자 페이지
```

#### 핵심 기능

| 라우트 | 설명 |
|--------|------|
| `GET /` | 프로젝트 선택 메인 페이지 |
| `GET/POST /auth/<key>` | 프로젝트별 비밀번호 로그인 |
| `GET /files/<key>/...` | 파일 탐색 + 다운로드 (세션 인증 필요) |
| `POST /upload/<key>/...` | 파일 업로드 |
| `POST /api/mkdir` | 폴더 생성 (JS fetch) |
| `POST /api/delete` | 파일/폴더 삭제 (JS fetch) |
| `GET /logout/<key>` | 로그아웃 |
| `POST /local/send-verification` | 로컬 폴더 인증 메일 발송 |
| `GET /local/verify` | 이메일 인증 링크 처리 → 토큰 발급 |
| `GET /local/admin` | 관리자 페이지 (ADMIN_EMAIL 전용) |

#### 이전 버전 대비 변경점

| 항목 | 이전 (back.txt) | 현재 |
|------|----------------|------|
| 인증 | ❌ 없음 | ✅ 프로젝트별 비밀번호 |
| 세션 | ❌ | ✅ flask session |
| 템플릿 | `render_template_string` 단일 | ✅ 여러 템플릿 (local auth 포함) |
| 폴더 생성 | ❌ | ✅ API |
| 파일 삭제 | ❌ | ✅ API |
| UI | 심플 라이트 | ✅ 다크테마 (GitHub 스타일) |
| 멀티업로드 | 단일 파일 | ✅ 다중 파일 |

### 🔐 로컬 폴더 이메일 인증 시스템

로컬 폴더 접근을 위한 이메일 인증 + 토큰 발급 시스템.

#### 동작 흐름
1. 사용자가 이메일 입력 → 인증 메일 발송 (`POST /local/send-verification`)
2. 메일 속 링크 클릭 → 토큰 발급 (`GET /local/verify`)
3. 토큰을 쿠키에 저장 → 로컬 폴더 접근 허용

#### SMTP 설정 (.env)
```
SMTP_HOST=smtp.mail.nate.com
SMTP_PORT=587
SMTP_SSL=False
SMTP_USER=daeho1001
SMTP_PASS=
SMTP_FROM=daeho1001@nate.com
```

#### 관리자
- **ADMIN_EMAIL:** daeho1001@nate.com (유일한 관리자)
- **관리자 페이지:** `https://files.semonara.com/local/admin`
- 관리자만 이메일 등록/삭제, 토큰 취소 가능

#### 데이터 파일
`E:\project\local_tokens.json` — 토큰/인증코드 저장

#### 알려진 이슈 (2026-06-01)
- ~~`time` 모듈 import 누락으로 인증 메일 500 에러~~ ✅ 수정 완료
  - `file_server_external.py` 12번째 줄에 `time` 추가
  - 2026-06-01 서버 재시작 후 정상 동작 확인

---

#### 실행 방법

```cmd
cd /d E:\project
python file_server_external.py
```

> 필요한 패키지: `flask`, `python-dotenv`

## 📁 E: 드라이브 구조

```
E:\
├── openshare/          ← 외부 파일 서버 제공 폴더
│   ├── sungsan1/      (성산1공구)
│   │   ├── 01. 작업일보/
│   │   └── 02. 안전일지등/
│   ├── sungsan2/      (성산2공구)
│   │   ├── 01. 작업일보/
│   │   └── 02. 안전일지등/
│   ├── gudong1/       (구동1공구)
│   │   ├── 01. 작업일보/
│   │   └── 02. 안전일지등/
│   └── gudong2/       (구동2공구)
│       ├── 01. 작업일보/
│       └── 02. 안전일지등/
├── SharedFiles/        ← 내부 공유 문서
│   ├── 00. scan/
│   ├── 01. 행정문서/
│   ├── 02. 발주처 수명사항/
│   ├── ... (공정회의록, 업무일지, 공사현황 등)
│   └── 70. 관급자재/
├── project/            ← Flask 프로젝트 소스
│   ├── file_server_external.py
│   ├── .env
│   ├── local_tokens.json
│   └── templates/
├── 건설사업관리/         ← 건설사업관리 용역 문서
│   ├── 공정/
│   ├── 본사/
│   ├── 업무일지/
│   ├── 월간 건설사업관리 보고서/
│   └── ...
└── midasCAD_2024.../
```
