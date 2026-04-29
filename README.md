# .claude

[Claude Code](https://docs.claude.com/en/docs/claude-code) 사용자 설정 중 공유 가능한 항목 (skills) 모음.

## 구조

```
.claude/
└── skills/        # 커스텀 슬래시 명령(스킬)
    └── <name>/
        ├── Skill.md   # 스킬 정의 (frontmatter + 설명)
        └── ...        # 실행 스크립트, 리소스 등
```

세션·캐시·플랜·메모리 등 런타임/민감 데이터는 `.gitignore` 로 제외.

## 스킬 목록

| 스킬 | 호출 | 용도 |
|------|------|------|
| [receipt-to-pdf](skills/receipt-to-pdf/) | `/receipt-to-pdf` | 영수증 사진 → 자동 크롭/원근보정/흑백 → PPT 슬라이드 크기 PDF·PPTX |
| bug-report | `/bug-report` | 버그 신고 — 화면/동작/결과 3단계 정리 후 즉시 수정 시작 |
| start-session | `/start-session` | 세션 시작 — CLAUDE.md 읽고 현황 파악 |
| wrap-session | `/wrap-session` | 세션 종료 — 작업 요약 → CLAUDE.md 업데이트 → commit 준비 |

## 설치

`~/.claude/skills/` 아래에 클론하거나 개별 스킬 폴더만 복사:

```bash
git clone https://github.com/pomeloEater/.claude.git ~/.claude-shared
cp -r ~/.claude-shared/skills/<name> ~/.claude/skills/
```
