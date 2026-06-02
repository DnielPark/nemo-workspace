# TOOLS.md - Skill Index

## 🖥️ 서버 환경

- **OS:** Windows 10.0.26200 (x64)
- **Node.js:** v24.16.0 / OpenClaw v2026.5.27
- **Gateway:** ws://127.0.0.1:18789 (loopback)
- **서비스:** Windows Scheduled Task (로그인 시 자동 실행 + 자동 재시작)
- **원격:** 텔레그램 @tamsa112

---

## 🛠 스킬 목록

#스킬목록
## [NO.01] server-info — `skills/server-info.md`
- 서버 기본 정보 (IP, OS, 드라이브 등)
## [NO.02] flask-server — `skills/flask_server.md`
- Flask 파일 서버 (5050번 포트, 외부 노출, Cloudflare Tunnel)
## [NO.03] webdav-server — `skills/webdav_server.md`
- WsgiDAV WebDAV (8080번 포트, 내부망 전용, ⛔ 건드리지 말 것)
## [NO.04] git-office — `skills/git-office.md`
- GitHub `office_server` Push / Pull
## [NO.05] cloudflare-tunnel — `skills/cloudflare-tunnel.md`
- Cloudflare Tunnel (files.semonara.com → localhost:5050)
## [NO.06] tel-send — `skills/tel-send.md`
- 프로젝트 문서 → 텔레그램 직접 전송 (AI 개입 없음)
## [NO.07] office-agent-sync — `skills/office-agent-sync.md`
- 에이전트 워크스페이스 ↔ GitHub office-server-agent 동기화

---

## ⛔ 파일 삭제 규칙

**각 공구 문서 (E:\openshare\sungsan1, sungsan2, gudong1, gudong2) 는 절대 함부로 삭제하지 않는다.**

- 파일/폴더 삭제 전 **반드시 내용 목록을 대니얼에게 보여주고 삭제 승인을 받을 것**
- 특히 pdf, hwp, doc 등 현장 문서는 민감 데이터이므로 한 번 더 확인
- `rmtree`, `del`, `Remove-Item` 등 복구 불가능한 삭제는 더더욱 신중하게
- 이 규칙은 모든 세션에서 적용됨
- **그 외 다른 프로젝트/파일은 자유롭게 삭제 가능**

---

> 각 스킬은 `skills/<name>.md` 파일에 절차/명령어/주의사항 기록

## Related

- [Agent workspace](/concepts/agent-workspace)
