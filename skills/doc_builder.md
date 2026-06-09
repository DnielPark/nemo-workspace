# 📄 문서 생성기 — files.semonara.com

> Flask 프로젝트 (`E:\project\`) 의 문서 생성기 기능.

## 접속 경로

- **메인:** `https://files.semonara.com/devnote/doc-builder`
- **현장별:** `https://files.semonara.com/files/<project_key>/doc-builder`

## 현재 문서 종류

| 문서 | 경로 | 상태 |
|------|------|------|
| 일요일 공사 승인 요청서 | `templates/doc_builder/sunday_approval/` | ✅ Phase 2-v3 (3단계 프로세스) |
| Kimi K2.5 OCR 연동 계획 | `templates/doc_builder/phase3_plan.html` | 📝 초안 |

## 백엔드

- **Flask:** `E:\project\file_server_external.py`
- **AI 연동:** DeepSeek (`deepseek-chat` → V4 업그레이드 예정)
- **API 엔드포인트:**
  - `GET /devnote/doc-builder` — 문서 목록
  - `GET /devnote/doc-builder/sunday` — 일요일 공사 승인 요청서 페이지
  - `POST /devnote/doc-builder/sunday/chat` — AI 채팅 API (phase 파라미터 지원)

---

## 📐 설계 방향 (2026-06-09 결정)

### 폴더 구조 (DB 대신 폴더+JSON)

```
doc_builder/
├── index.html                    ← 문서 목록
├── phase3_plan.html              ← Phase 3 계획
├── static/
│   └── sunday_work_approval_schema.json
└── sunday_approval/              ← 📦 문서 패키지 (하나의 hwp와 같은 단위)
    ├── template.html             ← A4 미리보기 HTML
    ├── schema.json               ← 필드 정의 (types, process phases)
    ├── chat.html                 ← 채팅 인터페이스 (sunday_chat.html에서 이관)
    └── fields/
        └── default.json          ← 실제 데이터 저장소
```

**문서 패키지 개념:** `sunday_approval/` 폴더 하나가 완전한 한 세트. 추후 새 문서 추가 시 같은 패턴으로 `work_report/`, `safety_report/` 등 확장.

**DB 안 쓰는 이유:** Flask 파일 서버 환경 최적화. JSON으로 충분. DB는 검색/통계/동시편집 필요할 때 전환.

### schema.json 필드 타입

```json
{
  "fields": [
    {"key": "project_name", "type": "auto", "source": "env"},
    {"key": "work_date", "type": "ask"},
    {"key": "safety_plan", "type": "ai_generate", "prompt": "작업 내용 기반 안전 계획 자동 생성"},
    {"key": "supervisor_opinion", "type": "manual"}
  ]
}
```

- **auto** — .env 또는 users/ 에서 자동 로드
- **ask** — 사용자에게 정확히 질문해서 입력
- **ai_generate** — AI가 자동 생성 (사용자 검수)
- **manual** — 빈칸 유지 (감리단 입력 등)

### 3단계 프로세스

1. **가. 정보입력** — 공사명, 시공사, 건설사업관리단 → `.env`/users/ 에서 자동 로드, 없으면 질문
2. **나. 공사내용 파악** — 공종/위치, 근로자수, 장비, 책임자, 작업내용 → 정확히 질문해서 저장
3. **다. AI 문서 작성**
   - 공사 개요 완성 후 수정사항 확인
   - 일요일 공사 시행 사유: 체크박스 제거 → **서술형**으로 AI가 자동 작성
   - 현장관리계획: AI 자동완성
   - 비상시 대응체계: AI 자동완성
   - 건설사업단 의견: 빈칸 유지

### 개선 전 vs 개선 후

| 항목 | 현재 (Phase 2) | 개선 후 |
|------|---------------|---------|
| AI 질문 방식 | 하나하나 순차 질문 (10단계) | 자동입력+정확질문+AI생성 (3단계) |
| 사유 선택 | 체크박스 5개 중 2-3개 선택 | 서술형 자유 입력 |
| 안전계획/비상대응 | 사용자 직접 입력 | AI 자동 생성 (검수만) |
| 확장성 | sunday_chat.html 단일 파일 | 문서 종류별 폴더 구조 |

## 추후 개선 (Phase 3+)

- Kimi K2.5 OCR 연동 (이미지 업로드 → 텍스트 추출)
- PDF 자동 생성
- 작업일보, 안전일지 등 추가 문서 유형
