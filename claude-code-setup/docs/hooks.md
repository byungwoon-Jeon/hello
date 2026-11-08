# Hooks — 반복 검증 자동화

Hooks는 특정 이벤트 전후에 로컬 명령을 실행한다. 초기 설치에서는 **활성화하지 않는다**. 실제 프로젝트 테스트 명령을 확인한 뒤 제한적으로 적용한다.

## 많이 쓰는 이벤트

| 이벤트 | 용도 |
|---|---|
| `PreToolUse` | 도구 실행 전 검사·차단 |
| `PostToolUse` | 파일 수정 후 정적 검사 |
| `Stop` | 작업 종료 시 검증 조건 확인 |

## 예시: Python 파일 수정 후 컴파일 검사

**전제:** 프로젝트에 `src/`가 있고 `python`이 올바른 Python 가상환경을 가리키는 경우에만 사용한다.

실제 프로젝트의 `.claude/settings.json`에 이미 다른 설정이 있다면 `hooks` 객체만 병합한다.

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "python -m compileall -q src"
          }
        ]
      }
    ]
  }
}
```

### 주의

- 위 예시는 모든 편집 이벤트 후 호출되므로 성능 비용이 있고 `__pycache__` 파일이 생길 수 있다.
- `compileall`은 문법 검사이며 실제 기능 테스트가 아니다.
- Python 실행 경로와 프로젝트 폴더가 다르면 명령을 수정해야 한다.
- 외부 네트워크 전송이나 운영 DB에 연결되는 스크립트는 Hook으로 자동 실행하지 않는다.
- `/hooks`에서 등록 여부를 확인하고, 테스트용 프로젝트에서 먼저 검증한다.
- 회사에서 관리하는 Hooks/정책이 있을 수 있으므로 이를 우회하지 않는다.

## 권장 확장

기존 프로젝트의 안전한 테스트 명령이 확인된 뒤에만 포맷터, 린터, 단위 테스트를 연결한다. 실행 시간이 긴 전체 테스트는 매 파일 편집마다 돌리는 대신 명시적으로 수행하는 편이 좋다.

공식 출처:
- https://code.claude.com/docs/en/hooks-guide
- https://code.claude.com/docs/en/hooks
