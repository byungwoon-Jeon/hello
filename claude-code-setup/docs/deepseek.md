# DeepSeek + Anthropic Claude Code (사내망) 주의사항

## 작동 구조

- **Claude Code**: VS Code에서 코드 읽기·편집·명령 실행을 담당하는 도구
- **DeepSeek**: 모델 추론을 담당하는 백엔드
- **사내 게이트웨이**: 인증·모델 매핑·접근 제어를 회사 정책에 따라 관리할 수 있는 중간 계층

회사에서 정상 작동 중인 `ANTHROPIC_BASE_URL`, 토큰, 모델 매핑 등을 **변경하거나 외부 DeepSeek 서버로 바꾸지 않는다**.

## 공식 DeepSeek API와 사내 게이트웨이의 차이

DeepSeek의 **공식 Anthropic 호환 API**에서는 다음 매핑을 문서화하고 있다.

| Claude 모델 이름 | DeepSeek 공식 API 동작 |
|---|---|
| `claude-opus*` | `deepseek-v4-pro` |
| `claude-sonnet*` 또는 `claude-haiku*` | `deepseek-flash` |

사내 게이트웨이가 같은 매핑을 쓴다는 보장은 없다. `/status`와 회사 모델 제공 문서로 실제 모델을 확인한다.

## 호환성 점검 항목

1. `/status`에서 버전과 표시 모델 확인. 인증 토큰은 공개하지 않기
2. 간단한 파일 읽기와 프로젝트 탐색 가능 여부
3. 파일 수정 제안과 Diff/승인 절차 확인
4. 로컬 단위 테스트 호출 가능 여부
5. `/compact`, `/plan`, Skills 사용 가능 여부
6. 이미지·PDF 등 멀티모달 사용은 **실제 사내 모델의 지원 여부를 별도 확인**

DeepSeek 공식 문서의 현재 API 호환성 예시에 따르면:
- `deepseek-flash`는 사용자 메시지의 이미지를 지원하지만 모델/버전별 조건이 있다.
- `deepseek-v4-pro`는 이미지를 직접 처리하지 않는다고 문서화되어 있다.
- Anthropic API 형식의 `document` 콘텐츠 블록은 지원되지 않는다.
- 일부 고급 파라미터·서버 도구 기능은 Anthropic 모델과 동일하지 않다.

이 내용은 **공식 DeepSeek 엔드포인트에 대한 설명**이다. 사내 프록시가 다른 모델 버전이나 호환 구현을 사용하면 결과가 달라질 수 있다.

## 오류 진단

| 증상 | 우선 확인 |
|---|---|
| 연결 실패/401 | 사내 인증·게이트웨이 설정 및 권한 |
| 400/422 | 모델명·도구 호출·입력 포맷 호환성 |
| 429 | 사내 게이트웨이 요청 제한·동시 처리량 |
| 모델이 파일을 읽지 못함 | 확장의 도구 권한·사내 정책 |
| 모델 응답이 부정확함 | 파일 @참조·요구사항 구체화·테스트 검증 |

로그 공유 시 토큰, 내부 URL, 고객 정보, 원본 데이터는 반드시 제거한다.

## 권장 작업

- 처음에는 Manual/Plan 모드로 시작
- 많은 파일을 한 번에 수정시키지 않고 영향 범위부터 분석
- 실데이터 대신 허용된 가명·합성 데이터로 테스트
- 모든 '테스트 통과' 주장에 실제 실행 명령과 결과를 요구
- 외부 MCP/검색 플러그인은 회사가 허용한 것만 연결

공식 문서:
- https://api-docs.deepseek.com/quick_start/agent_integrations/claude_code
- https://api-docs.deepseek.com/guides/anthropic_api
- https://code.claude.com/docs/en/settings
