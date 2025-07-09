# 프로젝트 Gemini 설정 가이드

이 문서는 Gemini Code Assist 에이전트 모드 및 MCP(Model Context Protocol) 서버를 효과적으로 활용하기 위한 지침을 제공합니다.

---

## MCP 서버 설정 (`settings.json`)

이 프로젝트는 특정 기능을 위해 다음 MCP 서버들을 활용합니다. `settings.json` 파일 내 `mcpServers` 객체에 해당 서버들이 올바르게 구성되었는지 확인하세요.

* **Context7 MCP 서버:**
  * 목적: 최신 라이브러리 및 프레임워크 문서 컨텍스트를 에이전트에게 제공합니다.
  * 설정 예시:

        ```json
        "context7": {
          "command": "npx",
          "args": ["-y", "@upstash/context7-mcp"]
        }
        ```

* **Playwright MCP 서버:**
  * 목적: 웹 상호작용 및 UI 테스트 자동화를 위한 기능을 에이전트에게 제공합니다.
  * 설정 예시:

        ```json
        "playwright": {
          "command": "npx",
          "args": [
            "-y",
            "@executeautomation/playwright-mcp-server"
          ]
        }
        ```

* **Desktop Commander MCP 서버:**
  * 목적: 에이전트가 로컬 데스크톱 환경과 상호작용할 수 있도록 명령어를 실행하는 기능을 제공합니다. (예: 파일 시스템 접근, 애플리케이션 실행 등)
  * 설정 예시:

        ```json
        "desktop-commander": {
          "command": "npx",
          "args": [
            "-y",
            "@wonderwhy-er/desktop-commander"
          ]
        }
        ```

  * **주의:** 이 서버는 시스템에 대한 광범위한 접근 권한을 부여할 수 있으므로, 사용 시 **보안에 각별히 주의**해야 합니다.

* **Sequential Thinking MCP 서버:**
  * 목적: 에이전트가 복잡한 작업을 여러 단계로 나누어 순차적으로 계획하고 실행하는 데 도움을 줍니다. 이는 장기적인 목표 달성이나 복잡한 문제 해결에 유용합니다.
  * 설정 예시:

        ```json
        "sequential-thinking": {
          "command": "npx",
          "args": [
            "-y",
            "@modelcontextprotocol/server-sequential-thinking"
          ]
        }
        ```

* **DBHub Demo MCP 서버:**
  * 목적: AI 모델이 다양한 데이터베이스와 상호작용할 수 있도록 돕는 범용 데이터베이스 게이트웨이입니다. `--demo` 플래그를 통해 실제 데이터베이스 연결 없이 내장된 샘플 데이터베이스로 테스트 및 탐색이 가능합니다.
  * 설정 예시:

        ```json
        "dbhub-demo": {
          "command": "npx",
          "args": ["-y", "@bytebase/dbhub", "--transport", "stdio", "--demo"]
        }
        ```

---

## 환경 변수 사용 지침 및 종류

환경 변수는 애플리케이션을 구성하는 일반적인 방법이며, 특히 API 키와 같은 민감한 정보나 환경에 따라 변경될 수 있는 설정에 유용합니다.

`settings.json` 파일 내의 문자열 값은 **`$VAR_NAME`** 또는 **`${VAR_NAME}`** 구문을 사용하여 환경 변수를 참조할 수 있습니다. 이러한 변수는 설정이 로드될 때 자동으로 해석됩니다.

예시: `MY_API_TOKEN` 환경 변수가 있다면 `settings.json`에서 `"apiKey": "$MY_API_TOKEN"`과 같이 사용할 수 있습니다.

권장 사항: 보안이 민감한 정보(API 키, 토큰 등)는 `settings.json`에 직접 하드코딩하지 않고, 항상 **환경 변수를 통해 관리**하는 것을 강력히 권장합니다.

### 환경 변수 로드 순서

CLI는 `.env` 파일에서 환경 변수를 자동으로 로드합니다. 로드 순서는 다음과 같습니다.

1. 현재 작업 디렉터리의 `.env` 파일
2. 없으면 상위 디렉터리를 검색하여 `.env` 파일을 찾거나 프로젝트 루트(`.git` 폴더로 식별) 또는 홈 디렉터리에 도달할 때까지 검색
3. 그래도 없으면 `~/.env`(사용자 홈 디렉터리)에서 찾음

### 주요 환경 변수 목록

* `GEMINI_API_KEY` (필수):
  * Gemini API용 API 키입니다.
  * 작동에 필수적이며, 이 키 없이는 CLI가 작동하지 않습니다.
  * 셸 프로필(예: `~/.bashrc`, `~/.zshrc`) 또는 `.env` 파일에 설정하십시오.
* `GEMINI_MODEL`:
  * 사용할 기본 Gemini 모델을 지정합니다.
  * 하드코딩된 기본값을 재정의합니다.
  * 예시: `export GEMINI_MODEL="gemini-2.5-flash"`
* `GOOGLE_API_KEY`:
  * Google Cloud API 키입니다.
  * Express 모드에서 Vertex AI를 사용하는 데 필요합니다.
  * 필요한 권한을 가지고 `GOOGLE_GENAI_USE_VERTEXAI=true` 환경 변수를 설정했는지 확인하십시오.
  * 예시: `export GOOGLE_API_KEY="YOUR_GOOGLE_API_KEY"`
* `GOOGLE_CLOUD_PROJECT`:
  * Google Cloud 프로젝트 ID입니다.
  * Code Assist 또는 Vertex AI를 사용하는 데 필요합니다.
  * Vertex AI를 사용하는 경우, 필요한 권한을 가지고 `GOOGLE_GENAI_USE_VERTEXAI=true` 환경 변수를 설정했는지 확인하십시오.
  * 예시: `export GOOGLE_CLOUD_PROJECT="YOUR_PROJECT_ID"`
* `GOOGLE_APPLICATION_CREDENTIALS` (문자열):
  * Google Application Credentials JSON 파일의 경로입니다.
  * 예시: `export GOOGLE_APPLICATION_CREDENTIALS="/path/to/your/credentials.json"`
* `OTLP_GOOGLE_CLOUD_PROJECT`:
  * Google Cloud의 원격 분석용 Google Cloud 프로젝트 ID입니다.
  * 예시: `export OTLP_GOOGLE_CLOUD_PROJECT="YOUR_PROJECT_ID"`
* `GOOGLE_CLOUD_LOCATION`:
  * Google Cloud 프로젝트 위치(예: `us-central1`)입니다.
  * Express 모드가 아닌 Vertex AI를 사용하는 데 필요합니다.
  * Vertex AI를 사용하는 경우, 필요한 권한을 가지고 `GOOGLE_GENAI_USE_VERTEXAI=true` 환경 변수를 설정했는지 확인하십시오.
  * 예시: `export GOOGLE_CLOUD_LOCATION="YOUR_PROJECT_LOCATION"`
* `GEMINI_SANDBOX`:
  * `settings.json`의 `sandbox` 설정에 대한 대체 옵션입니다.
  * `true`, `false`, `docker`, `podman` 또는 사용자 지정 명령 문자열을 허용합니다.
* `SEATBELT_PROFILE` (macOS 전용):
  * macOS에서 Seatbelt(샌드박스 실행) 프로필을 전환합니다.
  * `permissive-open`: (기본값) 프로젝트 폴더(및 몇 가지 다른 폴더, `packages/cli/src/utils/sandbox-macos-permissive-open.sb` 참조)에 대한 쓰기를 제한하지만 다른 작업은 허용합니다.
  * `strict`: 기본적으로 작업을 거부하는 엄격한 프로필을 사용합니다.
  * `<profile_name>`: 사용자 지정 프로필을 사용합니다. 사용자 지정 프로필을 정의하려면 프로젝트의 `.gemini/` 디렉터리(예: `my-project/.gemini/sandbox-macos-custom.sb`)에 `sandbox-macos-<profile_name>.sb`라는 파일을 생성하십시오.
* `DEBUG` 또는 `DEBUG_MODE` (종종 기본 라이브러리 또는 CLI 자체에서 사용됨):
  * `true` 또는 `1`로 설정하여 상세 디버그 로깅을 활성화합니다. 문제 해결에 유용합니다.
* `NO_COLOR`:
  * 아무 값이나 설정하여 CLI의 모든 색상 출력을 비활성화합니다.
* `CLI_TITLE`:
  * 문자열로 설정하여 CLI의 제목을 사용자 지정합니다.
* `CODE_ASSIST_ENDPOINT`:
  * 코드 어시스트 서버의 엔드포인트(endpoint)를 지정합니다.
  * 개발 및 테스트에 유용합니다.

---

## 이 프로젝트에서 에이전트 모드 활용 팁

* **최신 라이브러리 사용:** `"Build a feature using [library_name] version [version_number]."` 와 같이 프롬프트에 버전을 명시하여 Context7이 최신 문서를 참조하도록 유도할 수 있습니다.
* **코드 리팩토링:** 특정 파일이나 디렉토리를 참조(`@file.js` 또는 `@src/`)하여 에이전트에게 리팩토링이나 개선을 요청하세요.
* **Playwright 기반 자동화:** 웹 상호작용 및 UI 테스트 자동화에 Playwright MCP 서버를 활용하도록 에이전트에게 요청할 수 있습니다. (예: "로그인 페이지에서 사용자의 로그인 과정을 시뮬레이션하는 Playwright 스크립트를 작성해줘.")
* **데스크톱 작업 자동화:** Desktop Commander MCP 서버를 사용하여 로컬 환경에서 파일 작업, 애플리케이션 실행 등 데스크톱 관련 작업을 자동화할 수 있습니다. (예: "이름이 `report.xlsx`인 파일을 열고 내용을 요약해줘.")
* **복잡한 작업 계획:** Sequential Thinking MCP 서버를 사용하여 복잡한 목표를 에이전트가 단계별로 계획하고 실행하도록 지시할 수 있습니다. (예: "새로운 사용자 인증 모듈을 개발하는 전체 계획을 세워줘.")
* **데이터베이스 상호작용:** DBHub Demo MCP 서버를 활용하여 AI가 데이터베이스에 쿼리를 실행하거나 스키마를 탐색하는 등의 작업을 수행하도록 할 수 있습니다. (예: "직원 데이터베이스에서 모든 직원의 이름을 조회해줘.")

---

## 파일 시스템 접근 도구 사용 시 주의사항

Gemini CLI 사용 시 `write_file`, `read_file`, `replace` 등 **파일 시스템에 접근하는 도구들은 상대 경로를 지원하지 않습니다.** 상대 경로를 사용하면 "File path must be absolute" 오류가 발생합니다. **항상 절대 경로를 사용**하여 파일에 접근해야 합니다.

---

## `settings.json`에서 사용 가능한 기타 주요 설정

다음은 `settings.json` 파일에서 설정할 수 있는 기타 유용한 옵션들입니다. 이 설정들은 Gemini CLI의 동작 방식을 세부적으로 제어하는 데 사용됩니다.

* `contextFileName` (문자열 또는 문자열 배열)
  * 설명: 컨텍스트 파일의 파일명(예: `GEMINI.md`, `AGENTS.md`)을 지정합니다. 단일 파일명 또는 허용되는 파일명 목록을 사용할 수 있습니다.
  * 기본값: `GEMINI.md`
  * 예시: `"contextFileName": "AGENTS.md"`

* `bugCommand` (객체)
  * 설명: `/bug` 명령어의 기본 URL을 재정의합니다.
  * 기본값: `"urlTemplate": "https://github.com/google-gemini/gemini-cli/issues/new?template=bug_report.yml&title={title}&info={info}"`
  * 속성:
    * `urlTemplate` (문자열): `{title}` 및 `{info}` 플레이스홀더를 포함할 수 있는 URL입니다.
  * 예시:

        ```json
        "bugCommand": {
          "urlTemplate": "[https://bug.example.com/new?title=](https://bug.example.com/new?title=){title}&info={info}"
        }
        ```

* `fileFiltering` (객체)
  * 설명: `@` 명령어 및 파일 검색 도구에 대한 Git 인식 파일 필터링 동작을 제어합니다.
  * 기본값: `"respectGitIgnore": true, "enableRecursiveFileSearch": true`
  * 속성:
    * `respectGitIgnore` (부울): 파일 검색 시 `.gitignore` 패턴을 존중할지 여부를 결정합니다. `true`로 설정하면 Git에 의해 무시된 파일(예: `node_modules/`, `dist/`, `.env`)은 `@` 명령어 및 파일 목록 작업에서 자동으로 제외됩니다.
    * `enableRecursiveFileSearch` (부울): 프롬프트에서 `@` 접두사 완성 시 현재 트리 아래의 파일명을 재귀적으로 검색할지 여부를 활성화합니다.
  * 예시:

        ```json
        "fileFiltering": {
          "respectGitIgnore": true,
          "enableRecursiveFileSearch": false
        }
        ```

* `coreTools` (문자열 배열)
  * 설명: 모델에서 사용할 수 있는 핵심 도구 이름 목록을 지정할 수 있습니다. 이는 내장 도구 세트를 제한하는 데 사용될 수 있습니다. 핵심 도구 목록은 내장 도구 문서를 참조하십시오. `ShellTool`처럼 명령별 제한을 지원하는 도구에 대해서도 지정할 수 있습니다. 예를 들어, `"coreTools": ["ShellTool(ls -l)"]`은 `ls -l` 명령어만 실행할 수 있도록 허용합니다.
  * 기본값: Gemini 모델에서 사용 가능한 모든 도구
  * 예시: `"coreTools": ["ReadFileTool", "GlobTool", "ShellTool(ls)"]`

* `excludeTools` (문자열 배열)
  * 설명: 모델에서 제외해야 하는 핵심 도구 이름 목록을 지정할 수 있습니다. `excludeTools`와 `coreTools` 모두에 나열된 도구는 제외됩니다. `ShellTool`처럼 명령별 제한을 지원하는 도구에 대해서도 지정할 수 있습니다. 예를 들어, `"excludeTools": ["ShellTool(rm -rf)"]`는 `rm -rf` 명령을 차단합니다.
  * 기본값: 제외되는 도구 없음
  * 예시: `"excludeTools": ["run_shell_command", "findFiles"]`
  * 보안 참고: `run_shell_command`에 대한 `excludeTools`의 명령별 제한은 단순 문자열 일치를 기반으로 하므로 쉽게 우회될 수 있습니다. 이 기능은 보안 메커니즘이 아니므로 신뢰할 수 없는 코드를 안전하게 실행하는 데 의존해서는 안 됩니다. `coreTools`를 사용하여 실행할 수 있는 명령을 명시적으로 선택하는 것이 좋습니다.

* `autoAccept` (부울)
  * 설명: CLI가 안전하다고 간주되는 도구 호출(예: 읽기 전용 작업)을 사용자 확인 없이 자동으로 수락하고 실행할지 여부를 제어합니다. `true`로 설정하면 CLI는 안전하다고 판단되는 도구에 대한 확인 프롬프트를 건너뜁니다.
  * 기본값: `false`
  * 예시: `"autoAccept": true`

* `theme` (문자열)
  * 설명: Gemini CLI의 시각적 테마를 설정합니다.
  * 기본값: `"Default"`
  * 예시: `"theme": "GitHub"`

* `sandbox` (부울 또는 문자열)
  * 설명: 도구 실행을 위한 샌드박스 사용 여부와 방법을 제어합니다. `true`로 설정하면 Gemini CLI는 미리 빌드된 `gemini-cli-sandbox` Docker 이미지를 사용합니다. 자세한 내용은 샌드박스 문서를 참조하십시오.
  * 기본값: `false`
  * 예시: `"sandbox": "docker"`

* `toolDiscoveryCommand` (문자열)
  * 설명: 프로젝트에서 도구를 검색하기 위한 사용자 지정 셸 명령을 정의합니다. 셸 명령은 `stdout`에 함수 선언의 JSON 배열을 반환해야 합니다. 도구 래퍼는 선택 사항입니다.
  * 기본값: 비어 있음
  * 예시: `"toolDiscoveryCommand": "bin/get_tools"`

* `toolCallCommand` (문자열)
  * 설명: `toolDiscoveryCommand`를 사용하여 검색된 특정 도구를 호출하기 위한 사용자 지정 셸 명령을 정의합니다. 셸 명령은 다음 기준을 충족해야 합니다.
    * 첫 번째 명령줄 인수로 함수 이름(함수 선언과 정확히 일치해야 함)을 받아야 합니다.
    * `functionCall.args`와 유사하게 `stdin`에서 JSON으로 함수 인수를 읽어야 합니다.
    * `functionResponse.response.content`와 유사하게 `stdout`에 JSON으로 함수 출력을 반환해야 합니다.
  * 기본값: 비어 있음
  * 예시: `"toolCallCommand": "bin/call_tool"`

* `mcpServers` (객체)
  * 설명: 사용자 지정 도구를 검색하고 사용하기 위해 하나 이상의 MCP(Model-Context Protocol) 서버에 대한 연결을 구성합니다. Gemini CLI는 구성된 각 MCP 서버에 연결하여 사용 가능한 도구를 검색합니다. 여러 MCP 서버가 동일한 이름의 도구를 노출하는 경우, 충돌을 피하기 위해 도구 이름에 구성에 정의한 서버 별칭(`serverAlias__actualToolName` 등)이 접두어로 붙습니다. 시스템은 호환성을 위해 MCP 도구 정의에서 특정 스키마 속성을 제거할 수 있습니다.
  * 기본값: 비어 있음
  * 속성:
    * `<SERVER_NAME>` (객체): 명명된 서버에 대한 서버 매개변수입니다.
      * `command` (문자열, 필수): MCP 서버를 시작하기 위해 실행할 명령입니다.
      * `args` (문자열 배열, 선택 사항): 명령에 전달할 인수입니다.
      * `env` (객체, 선택 사항): 서버 프로세스에 설정할 환경 변수입니다.
      * `cwd` (문자열, 선택 사항): 서버를 시작할 작업 디렉터리입니다.
      * `timeout` (숫자, 선택 사항): 이 MCP 서버에 대한 요청의 타임아웃(밀리초)입니다.
      * `trust` (부울, 선택 사항): 이 서버를 신뢰하고 모든 도구 호출 확인을 우회할지 여부입니다.
  * 예시:

        ```json
        "mcpServers": {
          "myPythonServer": {
            "command": "python",
            "args": ["mcp_server.py", "--port", "8080"],
            "cwd": "./mcp_tools/python",
            "timeout": 5000
          },
          "myNodeServer": {
            "command": "node",
            "args": ["mcp_server.js", "--verbose"],
            "cwd": "./mcp_tools/node"
          },
          "myDockerServer": {
            "command": "docker",
            "args": ["run", "i", "--rm", "-e", "API_KEY", "ghcr.io/foo/bar"],
            "env": {
              "API_KEY": "$MY_API_TOKEN"
            }
          }
        }
        ```

* `checkpointing` (객체)
  * 설명: 대화 및 파일 상태를 저장하고 복원할 수 있는 체크포인트 기능을 구성합니다. 자세한 내용은 체크포인트 문서를 참조하십시오.
  * 기본값: `{"enabled": false}`
  * 속성:
    * `enabled` (부울): `true`일 경우 `/restore` 명령을 사용할 수 있습니다.

* `preferredEditor` (문자열)
  * 설명: `diff`를 볼 때 사용할 기본 에디터를 지정합니다.
  * 기본값: `vscode`
  * 예시: `"preferredEditor": "vscode"`

* `telemetry` (객체)
  * 설명: Gemini CLI의 로깅 및 메트릭 수집을 구성합니다. 자세한 내용은 원격 분석을 참조하십시오.
  * 기본값: `{"enabled": false, "target": "local", "otlpEndpoint": "http://localhost:4317", "logPrompts": true}`
  * 속성:
    * `enabled` (부울): 원격 분석 활성화 여부입니다.
    * `target` (문자열): 수집된 원격 분석의 대상입니다. 지원되는 값은 `local` 및 `gcp`입니다.
    * `otlpEndpoint` (문자열): OTLP Exporter의 엔드포인트입니다.
    * `logPrompts` (부울): 로그에 사용자 프롬프트 내용이 포함될지 여부입니다.
  * 예시:

        ```json
        "telemetry": {
          "enabled": true,
          "target": "local",
          "otlpEndpoint": "http://localhost:16686",
          "logPrompts": false
        }
        ```

* `usageStatisticsEnabled` (부울)
  * 설명: 사용 통계 수집을 활성화 또는 비활성화합니다. 자세한 내용은 사용 통계를 참조하십시오.
  * 기본값: `true`
  * 예시: `"usageStatisticsEnabled": false`

* `hideTips` (부울)
  * 설명: CLI 인터페이스의 유용한 팁을 활성화 또는 비활성화합니다.
  * 기본값: `false`
  * 예시: `"hideTips": true`

---

## 셸 기록

CLI는 실행한 셸 명령의 기록을 유지합니다. 다른 프로젝트 간의 충돌을 피하기 위해 이 기록은 사용자 홈 폴더 내의 프로젝트별 디렉터리에 저장됩니다.

* 위치: `~/.gemini/tmp/<project_hash>/shell_history`
  * `<project_hash>`는 프로젝트의 루트 경로에서 생성된 고유 식별자입니다.
* 기록은 `shell_history`라는 파일에 저장됩니다.

---

## 명령줄 인수 (CLI 세션 중 재정의)

CLI를 실행할 때 직접 전달하는 인수는 해당 특정 세션의 다른 구성을 재정의할 수 있습니다. 이는 특정 세션에만 임시로 설정을 변경하고 싶을 때 유용합니다.

* `--model <model_name>` 또는 `-m <model_name>`:
  * 이 세션에서 사용할 Gemini 모델을 지정합니다.
  * 예시: `npm start -- --model gemini-1.5-pro-latest`
* `--prompt <your_prompt>` 또는 `-p <your_prompt>`:
  * 프롬프트를 명령에 직접 전달하는 데 사용합니다. 이는 Gemini CLI를 비대화형 모드로 호출합니다.
* `--sandbox` 또는 `-s`:
  * 이 세션에 샌드박스 모드를 활성화합니다.
* `--sandbox-image`:
  * 샌드박스 이미지 URI를 설정합니다.
* `--debug_mode` 또는 `-d`:
  * 이 세션에 디버그 모드를 활성화하여 더 자세한 출력을 제공합니다.
* `--all_files` 또는 `-a`:
  * 설정된 경우, 현재 디렉터리 내의 모든 파일을 프롬프트의 컨텍스트로 재귀적으로 포함합니다.
* `--help` 또는 `-h`:
  * 명령줄 인수에 대한 도움말 정보를 표시합니다.
* `--show_memory_usage`:
  * 현재 메모리 사용량을 표시합니다.
* `--yolo`:
  * 모든 도구 호출을 자동으로 승인하는 YOLO 모드를 활성화합니다.
* `--telemetry`:
  * 원격 분석을 활성화합니다.
* `--telemetry-target`:
  * 원격 분석 대상을 설정합니다. 자세한 내용은 원격 분석을 참조하십시오.
* `--telemetry-otlp-endpoint`:
  * 원격 분석을 위한 OTLP 엔드포인트를 설정합니다. 자세한 내용은 원격 분석을 참조하십시오.
* `--telemetry-log-prompts`:
  * 원격 분석을 위해 프롬프트 로깅을 활성화합니다. 자세한 내용은 원격 분석을 참조하십시오.
* `--checkpointing`:
  * 체크포인트 기능을 활성화합니다.
* `--version`:
  * CLI 버전을 표시합니다.

---

## 일반적인 문제 및 해결 방법

사용하는 동안 발생할 수 있는 일반적인 문제와 해결 방법은 다음과 같습니다.

* `ERR_MODULE_NOT_FOUND` 오류:
  * 이 오류가 발생하면 `npx` 대신 `bunx`를 사용해 보세요.
  * 예시 구성:

        ```json
        {
          "mcpServers": {
            "context7": {
              "command": "bunx",
              "args": ["-y", "@upstash/context7-mcp@latest"]
            }
          }
        }
        ```

  * 이 방법은 특히 `npx`가 패키지를 제대로 설치하거나 해결하지 못하는 환경에서 모듈 해결 문제를 해결하는 데 도움이 됩니다.

* MCP 클라이언트 오류:
  * 패키지 이름에서 `@latest`를 제거해 보세요.
  * 대안으로 `bunx`를 사용해 보세요.
  * 대안으로 `deno`를 사용해 보세요.

---

## 컨텍스트 파일 새로 고침 조언 ✨

이 문서와 같은 컨텍스트 파일(`GEMINI.md` 등)은 Gemini 모델이 프로젝트에 대한 최신 정보를 기반으로 응답하도록 돕는 중요한 역할을 합니다. 파일의 내용이 변경되었을 때, AI가 이를 즉시 인식하고 새로운 지침을 적용하도록 하려면 **메모리를 새로 고쳐야 합니다.**

* **변경 사항 적용**: `GEMINI.md` 또는 다른 컨텍스트 파일을 수정한 후에는 CLI에서 **`/memory refresh`** 명령을 실행하여 변경된 내용을 AI의 컨텍스트에 즉시 반영할 수 있습니다.
* **현재 컨텍스트 확인**: 현재 AI가 인식하고 있는 전체 컨텍스트를 확인하려면 **`/memory show`** 명령을 사용하십시오. 이를 통해 모든 컨텍스트 파일의 내용이 올바르게 로드되었는지 검증할 수 있습니다.

이러한 명령을 적극적으로 활용하여 AI가 항상 프로젝트의 최신 정보와 지침을 기반으로 작업하도록 유지하세요.
