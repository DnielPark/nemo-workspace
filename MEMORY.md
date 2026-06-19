# MEMORY.md — 장기 기억

## 사무실 서버 (DESKTOP)

- **OS:** Windows 10.0.26200 x64
- **내부 IP:** 192.168.1.211
- **OpenClaw Gateway:** ws://127.0.0.1:18789 (예약 작업 자동 실행 + 재시작)
- **원격 제어:** 텔레그램 @tamsa112 (대니얼)

## ⛔ 파일 삭제 규칙 (2026-06-01)

**각 공구 문서(E:\openshare\sungsan1/2, gudong1/2)는 500억 현장의 중요 문서.**
- 삭제는 절대 직접 하지 말고 항상 대니얼에게 먼저 목록을 보여주고 승인받을 것
- pdf, hwp 등 현장 문서는 특히 민감하게 처리
- 이 규칙은 모든 세션에서 적용

## 주요 경로

- **E:\project\** — Flask 파일 서버 (5050번 포트, files.semonara.com)
- **E:\openshare\** — 파일 서버 공유 폴더
  - 각 공구별 `01. 작업일보/`, `02. 안전일지등/` 폴더 구조 (2026-06-01 재편성)
- **E:\SharedFiles\** — 사내 공유 문서 (건드리지 말 것!)
- **E:\건설사업관리\** — 건설사업관리 용역 문서
- **E:\project\local_tokens.json** — 로컬 폴더 이메일 인증 토큰 저장

## GitHub

- **레포:** DnielPark/office_server (public)
- **로컬:** E:\project\ (main 브랜치)
- **Git 스크립트:** `E:\project\git-office-push.cmd` / `git-office-pull.cmd`

## Cloudflare Tunnel

- **도메인:** files.semonara.com → localhost:5050
- **Tunnel ID:** a25968d2-4913-4154-b41b-70f3b1b52946
- **실행 파일:** C:\WINDOWS\system32\cloudflared.exe

## 🗂️ 두 프로젝트 구분

### 1. 내부 프로젝트 (네모 워크스페이스)
- **경로:** `C:\Users\admin\.openclaw\workspace\`
- **GitHub:** `DnielPark/office-server-agent` (비공개)
- **목적:** 네모 정체성(SOUL.md), 스킬 문서, 프로젝트 파일
- **동기화:** 수동 `git push` (또는 추후 자동화)

### 2. 오피스 서버 프로젝트
- **경로:** `E:\project\`
- **GitHub:** `DnielPark/office_server` (공개)
- **목적:** Flask 파일 서버 소스코드
- **동기화:** `git-office-push.cmd` / `git-office-pull.cmd`

## 문서 구조

- **TOOLS.md** — 스킬 인덱스 (간략 목록)
- **skills/** — 상세 문서들
  - server-info.md, flask_server.md, webdav_server.md, git-office.md, cloudflare-tunnel.md, tel-send.md, office-agent-sync.md, doc_builder.md

## 📄 문서 생성기 (doc_builder)

- **경로:** `E:\project\` — files.semonara.com Flask 프로젝트
- **접속:** `/devnote/doc-builder`
- **현재 문서:** 일요일 공사 승인 요청서 (sunday_chat.html)
- **AI:** DeepSeek (V3 → V4 업그레이드 예정)
- **설계 방향 (2026-06-09):**
  - 템플릿+JSON 구조 (DB 대신 폴더+JSON)
  - 3단계 프로세스: auto(자동입력) → ask(정확질문) → ai_generate(AI생성)
  - schema.json 필드 타입: auto/ask/ai_generate/manual
  - 문서 종류별 폴더 확장 (sunday_approval/ → 추후 추가)
  - users/ 폴더에 사용자 정보 저장
- **상세:** `skills/doc_builder.md`

## WsgiDAV WebDAV (8080포트)

- **경로:** `C:\Users\admin\project\file_server.py`
- **시작:** `C:\Users\admin\project\start_webdav.bat`
- **공유 폴더:** `E:\SharedFiles` (사내 문서)
- **사내 네트워크 공유 시스템**
- **알려진 문제:** 이모지 print문이 cp949 인코딩에서 UnicodeEncodeError → 크래시. 실행 시 `PYTHONIOENCODING=utf-8` 필요
- 서비스 등록 안 되어 있어 자동 재시작 불가

## 🛠️ 전체 서버 안정화 (향후 과제, 2026-06-11)

### 대상 서비스 (3개)
1. **Flask 파일서버** — `E:\project\file_server_external.py` (5050포트)
2. **Cloudflare Tunnel** — `cloudflared tunnel run` (files.semonara.com)
3. **WsgiDAV WebDAV** — `C:\Users\admin\project\file_server.py` (8080포트)

### 문제
- 매일 새벽 인터넷 회선 끊김 → 모든 서비스 영향
- 서비스 등록/자동 재시작 미비
- WebDAV는 인코딩 버그로 크래시 가능

### 제안된 해결책 (보류)
1. Windows 예약 작업 (06:30) — 3개 서비스 재시작
2. OpenClaw Cron (30분 health check) — 백업 감시

### 결정
- 회선 문제 가능성 있어 일단 지켜보기
- 필요시 추진

## 대니얼

- 한국어 사용자, 텔레그램 주인
- GitHub: DnielPark
- 연락처: @tamsa112
