# Claude Code + DeepSeek 사내 개발 환경 가이드

Anthropic **Claude Code VS Code 확장**에 회사에서 제공하는 **DeepSeek 모델/사내 API 게이트웨이**를 연결해 사용하는 개발자를 위한 복사형 설정 템플릿이다.

> **중요:** 회사에서 이미 작동 중인 API 주소, 인증 토큰, 모델 매핑은 변경하지 않는다. 이 저장소에는 사내 비밀정보가 없다. 프로젝트별로 필요한 설정만 복사하고 사내 정책에 맞춰 검토한다.

## 빠른 시작 (20~30분)

1. VS Code의 Claude Code에서 `/status` 실행 → 버전·연결 모델 확인 (API 키/내부 URL은 공유하지 않기)
2. `/help` 실행 → 실제 설치 버전에서 지원하는 명령어 확인
3. `/permissions` 실행 → **Manual** 모드에서 시작
4. 프로젝트에서 `/init` 실행 → 자동 생성된 `CLAUDE.md` 검토
5. 이 템플릿의 `CLAUDE.md`에서 필요한 규칙을 실제 프로젝트의 `CLAUDE.md`에 병합
6. 이 템플릿의 `.claude/settings.json`에서 적절한 권한 규칙만 적용
7. `.claude/skills/`를 복사하고 `/impact-analysis`, `/regression-check` 시험
8. 익숙해진 후에만 Hooks 또는 회사 승인 MCP 연결 고려

## 파일 구성

```text
claude-code-setup/
├── README.md
├── CLAUDE.md
├── .claude/
│   ├── settings.json
│   └── skills/
│       ├── impact-analysis/SKILL.md
│       └── regression-check/SKILL.md
└── docs/
    ├── commands.md
    ├── vscode.md
    ├── hooks.md
    └── deepseek.md
```

**이 폴더에 있는 파일은 배포용 템플릿이다.** Claude Code가 실제 프로젝트에서 자동으로 읽게 하려면 다음처럼 배치해야 한다.

- `CLAUDE.md` → **실제 프로젝트 루트** (또는 해당 프로젝트의 `.claude/CLAUDE.md`)
- `.claude/settings.json` → 실제 프로젝트의 `.claude/settings.json`에 **규칙을 검토·병합**. 기존 파일을 무조건 덮어쓰지 않는다.
- `.claude/skills/<name>/SKILL.md` → 실제 프로젝트의 동일 경로. 여러 프로젝트에서 개인적으로 쓰려면 `~/.claude/skills/<name>/SKILL.md`.
- `docs/` → 읽기용 가이드로, 별도 설치 불필요.

회사에서 저장소 접근이 안 된다면 GitHub 웹에서 필요한 파일 원문을 복사해도 된다.

## 권장 작업 흐름

**분석 → 계획 → 승인 → 수정 → 검증** 순서로 진행한다.

1. `/impact-analysis 기능 변경 요구사항`: 관련 파일·호출 관계·파급효과 정리 (수정 없음)
2. `/plan 기능 변경 요구사항`: 작업 계획 검토
3. 승인 후 필요한 부분만 수정. 파일별 diff 확인
4. `/regression-check 이번 변경 사항`: 정상·오류·호환성 테스트 설계
5. 가능한 테스트를 실제 실행하고 결과·미검증 항목 보고
6. 같은 업무의 긴 대화에서는 `/context`, `/compact`. 새 업무는 `/clear`

## 자주 쓰는 프롬프트

```text
@관련파일.py
현재 코드의 호출 관계와 입력→처리→출력 흐름을 추적해줘.
수정 대상, 회귀 위험, 검증할 테스트를 정리해.
확인된 사실과 추정을 구분하고 코드는 아직 변경하지 마.
```

```text
앞서 합의한 수정 범위에서만 작업해.
기존 함수 시그니처·반환 스키마를 유지하고
변경 파일 목록, git diff 요약, 실행한 테스트 결과,
실행하지 못한 테스트를 각각 보고해.
```

## 우선순위

| 단계 | 추천 | 이유 |
|---|---|---|
| 1 | `CLAUDE.md`, 수동 권한 모드 | 기존 시스템의 호환성 보존 |
| 2 | 파일 `@` 참조, `/plan`, `/compact` | 작업 범위·컨텍스트 통제 |
| 3 | Impact/Regression Skills | 반복적인 분석과 검증 표준화 |
| 4 | Hooks | 테스트 스크립트가 안정된 뒤 자동화 |
| 5 | MCP/서브에이전트 | 사내 승인·비용·접근 권한 확인 후 |

## 공식 문서

- [Claude Code 명령어](https://code.claude.com/docs/en/commands)
- [VS Code 확장 가이드](https://code.claude.com/docs/en/vs-code)
- [프로젝트 메모리 (CLAUDE.md)](https://code.claude.com/docs/en/memory)
- [설정 및 권한](https://code.claude.com/docs/en/settings)
- [Skills](https://code.claude.com/docs/en/skills)
- [Hooks](https://code.claude.com/docs/en/hooks-guide)
- [DeepSeek의 Claude Code 연동](https://api-docs.deepseek.com/quick_start/agent_integrations/claude_code)
- [DeepSeek Anthropic API 호환성](https://api-docs.deepseek.com/guides/anthropic_api)
