# tel-send — 텔레그램 문서 전송

## 개요

프로젝트 코드/문서 파일을 **AI 개입 없이** 바로 텔레그램으로 전송.  
불필요한 컨텍스트 소비를 막기 위한 스킬.

## 사용법

텔레그램에서 대니얼이 직접 실행:

```
tel-send <파일경로>
```

### 예시

```
tel-send E:\project\file_server_external.py
tel-send E:\project\.env
tel-send E:\project\templates\auth.html
```

## 스크립트 위치

| 파일 | 설명 |
|------|------|
| `E:\project\tel-send.py` | Python 본체 |
| `E:\project\tel-send.cmd` | 실행 배치파일 |

## 동작 방식

1. `tel-send.cmd` → `tel-send.py` 실행
2. 파일 읽기 (UTF-8 → 실패 시 cp949)
3. 4096자 단위로 분할하여 텔레그램 전송
4. 긴 파일은 `(1/n)` 표시로 분할 전송

## 주의사항

- 바이너리 파일(.exe, .zip, .pdf 등)은 지원 안 함 (텍스트 전용)
- 파일이 너무 크면 여러 메시지로 나눠 전송됨
- 봇 토큰은 `openclaw.json`의 텔레그램 채널 설정과 동일
