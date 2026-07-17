# MEMORY.md — 장기 기억

> ⚠️ 이 파일은 공개 레포용 버전입니다. 서버 IP, 실제 경로, 인증 토큰 등 민감 정보는 제거됨.
> 전체 버전은 USB 별도 보관.

---

## ⛔ 파일 삭제 규칙

**각 공구 문서(sungsan1/2, gudong1/2)는 500억 현장의 중요 문서.**
- 삭제는 절대 직접 하지 말고 항상 대니얼에게 먼저 목록을 보여주고 승인받을 것
- pdf, hwp 등 현장 문서는 특히 민감하게 처리
- 이 규칙은 모든 세션에서 적용

## 주요 서비스

- **Flask 파일 서버** — 5050 포트, `file_server_external.py`
- **Cloudflare Tunnel** — files.semonara.com → localhost:5050
- **WsgiDAV WebDAV** — 8080 포트, 사내망 전용
- **OpenClaw Gateway** — 예약 작업 자동 실행 + 재시작
- **원격 제어** — 텔레그램 @tamsa112 (대니얼)

## GitHub

- **nemo-flask-server** (공개) — Flask 파일 서버 소스코드
- **nemo-workspace** (공개) — 네모 워크스페이스 (👈 지금 여기)
- **로컬 동기화:** git push/pull

## 🗂️ 스킬 목록

| NO | 스킬 | 설명 |
|----|------|------|
| 01 | server-info | 서버 기본 정보 |
| 02 | flask-server | Flask 파일 서버 (5050) |
| 03 | webdav-server | WsgiDAV WebDAV (8080, 내부망 전용, ⛔ 건드리지 말 것) |
| 04 | git-office | GitHub Push/Pull |
| 05 | cloudflare-tunnel | Cloudflare Tunnel (files.semonara.com) |
| 06 | tel-send | 프로젝트 문서 → 텔레그램 직접 전송 |
| 07 | office-agent-sync | 워크스페이스 ↔ GitHub office-server-agent 동기화 |
| 08 | doc-builder | files.semonara.com 문서 생성기 |
| 09 | home-server-agent | 대니얼 집 리눅스 서버 |

## 📄 문서 생성기 (doc_builder)

- **경로:** Flask 프로젝트 내 `/devnote/doc-builder`
- **현재 문서:** 일요일 공사 승인 요청서 (sunday_chat.html)
- **AI:** DeepSeek (V3 → V4 업그레이드 예정)
- **설계 방향 (2026-06-09):**
  - 템플릿+JSON 구조 (DB 대신 폴더+JSON)
  - 3단계 프로세스: auto(자동입력) → ask(정확질문) → ai_generate(AI생성)
  - schema.json 필드 타입: auto/ask/ai_generate/manual
  - 문서 종류별 폴더 확장 (sunday_approval/ → 추후 추가)
  - users/ 폴더에 사용자 정보 저장

## 🟩 미르 (Mir) — 홈서버 동생 OpenClaw (2026-06-20)

- **서버:** 대니얼 집 리눅스 (라이젠 4000, 16GB)
- **역할:** 아들 게이트웨이와 독립된 새 OpenClaw 인스턴스
- **이름:** 미르 (용) 🟩
- **상태:** ⏳ 설치 대기
- **⚠️ 주의:** 아들 게임 서버 / 언론사 사이트 / Playit 설정 건드리지 말 것

## 알려진 문제 (Windows 서버)

- 매일 새벽 인터넷 회선 끊김 현상
- WsgiDAV: 이모지 포함 출력 시 cp949 UnicodeEncodeError → 크래시 가능. `PYTHONIOENCODING=utf-8` 필요
- Flask/Cloudflared 자동 재시작 미설정

## 대니얼

- 한국어 사용자, 텔레그램 주인
- GitHub: DnielPark
- 연락처: @tamsa112
