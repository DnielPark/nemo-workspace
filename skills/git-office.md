# Git Office — Push / Pull 스킬

## 개요

`E:\project` (office_server) 를 GitHub와 동기화하는 스크립트.

GitHub 웹 에디터로 파일 수정 후 서버에 반영하거나, 서버에서 수정한 걸 GitHub에 올릴 때 사용.

## 스크립트

| 파일 | 기능 |
|------|------|
| `E:\project\git-office-push.cmd` | 서버 → GitHub (커밋 + 푸시) |
| `E:\project\git-office-pull.cmd` | GitHub → 서버 (풀) |

## 사용법

### GitHub 변경사항 서버에 반영 (Pull)
```cmd
E:\project\git-office-pull
```
또는 더블클릭!

### 서버 변경사항 GitHub에 업로드 (Push)
```cmd
E:\project\git-office-push
```
실행하면 커밋 메시지 입력창 뜸. 엔터치면 자동 메시지.

## 주의사항

- `.env` 파일은 GitHub에 올라가지 않음 (비밀번호 안전)
- `git-office-*.cmd` 파일도 GitHub에 올라가지 않음
- Push 전에 Pull 먼저 하는 습관!
- 충돌 나면 네모에게 물어보셈 👍
