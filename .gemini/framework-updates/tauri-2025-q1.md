# Tauri 2025년 1분기 주요 업데이트 분석

## 서론: 2025년 초 Tauri 생태계 탐색

2025년 1분기(1월 1일부터 3월 31일) 동안 Tauri 프레임워크의 패치 릴리즈(v2.3, v2.4 등)는 공식적으로 발표되지 않았습니다. 그러나 이는 개발이 정체되었다는 의미가 아닙니다. 오히려 이 기간은 Tauri의 미래 방향성을 제시하는 중대한 아키텍처 발표와, 모든 개발자가 숙지해야 하는 기존의 거대한 구조적 변화가 맞물린 중요한 시기였습니다.

따라서 이 보고서는 2025년 Tauri 개발자를 위한 전략적 기술 가이드 역할을 하고자 합니다. 본 보고서는 두 가지 핵심적인 부분으로 구성됩니다.

1. Verso 렌더링 엔진의 실험적 통합: Tauri의 근본적인 렌더링 방식을 바꿀 수 있는 잠재력을 지닌 새로운 개발 사항.
2. Tauri 2.0 아키텍처의 완전한 이해와 적응: 2024년 말 안정 버전으로 출시된 Tauri 2.0의 핵심 파괴적 변경(breaking changes) 사항 총정리.

---

## 1부: 핵심 변화 1: Verso 렌더링 엔진의 실험적 통합

2025년 1분기 중 가장 주목할 만한 새로운 소식은 3월 17일에 발표된 Verso 렌더링 엔진의 실험적 통합입니다. 이는 Tauri의 근본적인 작동 방식에 대한 새로운 가능성을 제시합니다.

### 1.1. Verso와 Servo: 새로운 패러다임의 이해

* **Servo**: Rust로 작성된 독립적인 오픈소스 웹 렌더링 엔진입니다.
* **Verso**: Servo 엔진을 기반으로 구축된 브라우저로, Tauri와 같은 프레임워크에 더 쉽게 통합될 수 있도록 설계된 고수준의 인터페이스를 제공합니다.
* **기존 Tauri와의 차이**: Tauri의 기본 런타임인 WRY는 운영체제에 내장된 네이티브 WebView(macOS는 WebKit, Windows는 WebView2, Linux는 WebKitGTK)를 활용합니다. 이는 Tauri 애플리케이션의 바이너리 크기를 작게 유지하는 핵심적인 장점입니다.
* **Verso 통합 시도 이유**:
  * Servo의 강력하지만 저수준의 API 사용 복잡성을 Verso가 추상화하여 실용적인 통합을 가능하게 합니다.
  * Tauri의 핵심 아키텍처 의존성을 완화하려는 전략적 움직임입니다. 현재 Tauri는 Apple, Google, Microsoft가 제어하는 WebView 구성 요소에 전적으로 의존하는데, 이는 플랫폼별 렌더링 차이, 버그, 기능 지원 불일치 등의 위험을 내포합니다. Servo는 특정 기업에 종속되지 않은 오픈소스이므로, Verso를 통한 통합은 이러한 종속성에서 벗어날 수 있는 '플랜 B'를 구축하는 것과 같습니다. 이는 Tauri의 장기적인 생존 가능성과 독립성을 높입니다.

### 1.2. 개발자를 위한 기회와 과제

Verso의 통합은 개발자에게 새로운 기회와 함께 고려해야 할 과제를 제시합니다.

* **기회**:
  * 렌더링 환경의 일관성 확보: 모든 데스크톱 플랫폼에서 동일한 Servo 엔진을 사용함으로써 플랫폼별 WebView 차이 디버깅 시간을 절약합니다.
  * Rust 기반 렌더러를 통한 긴밀한 통합: 장기적으로 Rust 백엔드와 UI 간의 직접적인 DOM 조작을 통해 성능을 극대화할 가능성.
* **과제 및 트레이드오프**:
  * 바이너리 크기 증가: Verso를 사용하면 렌더링 엔진 자체가 애플리케이션 바이너리에 포함되어 Tauri의 '아주 작은' 바이너리 크기라는 핵심 장점과 상충됩니다.

* **Tauri의 진화**:
  * "클래식 Tauri" 개발자 그룹: 최소한의 바이너리 크기와 낮은 리소스 사용량을 우선하며 기본 WRY 런타임을 계속 사용할 개발자.
  * "Tauri-Electron" 개발자 그룹: 렌더링 일관성을 중요하게 생각하며, 큰 바이너리 크기에 익숙한 Electron 개발자 경험이 있는 경우, Verso 옵션을 "Rust로 만드는 Electron"과 유사하게 받아들일 개발자.

결론적으로, Verso의 도입은 Tauri가 '가벼운 Electron 대체재'를 넘어, 프로젝트 요구사항에 따라 Electron처럼 동작하도록 설정할 수도 있는 다재다능한 툴킷으로 확장되고 있음을 시사합니다.

### 1.3. Verso 런타임 적용 가이드

현재 공개된 문서를 바탕으로 Verso 런타임을 프로젝트에 적용하는 단계별 가이드입니다.

1단계: `Cargo.toml` 수정

```ini
[build-dependencies]
tauri-build = "2"
tauri-runtime-verso-build = { git = "[https://github.com/versotile-org/tauri-runtime-verso.git](https://github.com/versotile-org/tauri-runtime-verso.git)" }

[dependencies]
tauri = { version = "2", default-features = false, features = ["common-controls-v6"] }
tauri-runtime-verso = { git = "[https://github.com/versotile-org/tauri-runtime-verso.git](https://github.com/versotile-org/tauri-runtime-verso.git)" }
````

2단계: `build.rs` 수정
프로젝트 루트에 `build.rs` 파일을 생성하거나 수정하여, 빌드 과정에서 사전 컴파일된 `versoview` 실행 파일을 다운로드하도록 스크립트를 추가합니다.

```rust
fn main() {
    tauri_runtime_verso_build::get_verso_as_external_bin().unwrap();
    tauri_build::build();
}
```

3단계: `tauri.conf.json` 수정
`tauri.conf.json` 파일에 `bundle.externalBin` 설정을 추가하여, 2단계에서 다운로드한 `versoview` 실행 파일을 번들에 포함하도록 지정합니다.

```json
{
  "bundle": {
    "externalBin": [
      "versoview/versoview"
    ]
  }
}
```

4단계: `main.rs` 수정
애플리케이션의 진입점인 `main.rs` 파일에서 `tauri::Builder`를 `tauri_runtime_verso`의 빌더로 교체합니다.

```rust
// main.rs
fn main() {
    // 기존 코드: tauri::Builder::new()
    tauri_runtime_verso::builder() // 변경된 코드
       .run(tauri::generate_context!())
       .unwrap();
}
```

만약 Verso를 직접 컴파일하여 사용하는 경우, `set_verso_path`와 `set_verso_resource_directory` 함수를 사용하여 실행 파일과 리소스 디렉토리의 경로를 수동으로 지정할 수도 있습니다.

### 1.4. 현재 제약사항 및 전략적 고려사항

Verso 통합은 매우 유망하지만, 2025년 1분기 현재는 초기 실험 단계이며 프로덕션 환경에 사용하기에는 여러 제약이 따릅니다.

* 알려진 제약사항:
  * 불완전한 기능 지원: 현재 Tauri의 창(windowing) 및 웹뷰 관련 기능 중 일부만 지원됩니다.
  * 메뉴 지원: macOS의 앱 전역 메뉴만 지원됩니다.
  * 개발자 도구(DevTools): Verso에는 내장 개발자 도구가 없어 Firefox의 원격 디버거를 사용해야 합니다.
  * IPC 보안: 현재 IPC 통신을 위해 Origin 헤더가 하드코딩되어 있어 보안 검사가 취약합니다.
* 전략적 조언: 현시점에서 Verso는 실험적인 프로젝트, 사내용 도구, 또는 렌더링 일관성이 최우선 순위이며 알려진 제약사항을 감수할 수 있는 특정 애플리케이션에 가장 적합합니다. 2025년 대부분의 프로덕션 애플리케이션 개발에는 여전히 안정적이고 검증된 기본 WRY 런타임을 사용하는 것이 권장됩니다.

## 2부: 핵심 변화 2: Tauri 2.0 아키텍처 완전 정복

2025년 1분기에 Tauri로 실질적인 개발을 진행하기 위해서는, 2024년 말에 있었던 Tauri 2.0 안정 버전 출시의 영향을 완벽하게 이해하는 것이 필수적입니다. 이 변화들은 선택이 아닌 필수입니다.

### 2.1. 2025년, Tauri 2.0 마이그레이션의 중요성

Tauri 2.0은 프레임워크를 근본적으로 재구성한 기념비적인 릴리즈였습니다. 핵심 목표는 iOS 및 Android 모바일 지원 추가와 더 강력하고 모듈화된 플러그인 시스템 구축입니다.

이 과정에서 수많은 '파괴적 변경(breaking changes)'이 도입되었습니다. 2025년에 `create-tauri-app` 도구를 사용하여 새 프로젝트를 시작하는 개발자는 처음부터 v2 아키텍처를 사용하게 되며, 기존 v1 프로젝트를 유지보수하는 개발자는 지속적인 업데이트와 지원을 받기 위해 반드시 v2로 마이그레이션해야 합니다.

### 2.2. 코드에 직접 영향을 미치는 Breaking Changes

Tauri 2.0은 개발자의 코드베이스에 직접적인 수정을 요구하는 여러 가지 중요한 변경 사항을 포함합니다.

* API 재편: Window에서 WebviewWindow로

  * `tauri::Window`와 `tauri::WindowBuilder` 구조체는 `tauri::WebviewWindow`와 `tauri::WebviewWindowBuilder`로 대체되었습니다.
  * 이 변화의 근본적인 이유는 Tauri 2.0이 새롭게 지원하는 다중 웹뷰(multi-webview) 아키텍처 때문입니다. 이제 하나의 네이티브 '창(Window)'이 여러 개의 '웹뷰(Webview)'를 호스팅할 수 있게 되어, 창 제어와 웹뷰 콘텐츠 제어를 명확하게 구분한 것입니다.

* 플러그인 중심 생태계로의 전환

  * Tauri 2.0은 핵심 기능들(예: http, dialog, fs, shell 등)을 프레임워크 본체에서 분리하여 독립적인 플러그인으로 전환하는 대대적인 모듈화를 단행했습니다.
  * 이를 통해 Tauri를 더욱 유연하고 유지보수하기 쉽게 만들고, 핵심 팀과 커뮤니티가 특정 기능들을 Tauri 코어 릴리즈 주기와는 별개로 빠르게 개선하고 업데이트할 수 있게 되었습니다.

* 권한 시스템의 혁신: `allowlist`에서 `capabilities`로

  * v1의 `tauri.conf.json` 파일에 있던 `allowlist` 개념은 "capabilities"라는 이름의 더 강력하고 세분화된 권한 시스템으로 대체되었습니다.
  * 플러그인으로 분리된 핵심 기능들을 사용하려면 명시적으로 권한을 부여해야 합니다. (예: 창 생성 기능을 사용하려면 `core:window:create` 권한 설정)
  * 개발자 편의를 위해 모든 핵심 플러그인의 기본 권한을 한 번에 활성화할 수 있는 `core:default`라는 특별한 권한 집합이 도입되었습니다.

* 설정 파일(`tauri.conf.json`) 구조 변경 상세

  * 업데이터 설정: `tauri > updater` 객체가 `tauri > bundle > updater` 경로로 이동.
  * 프로토콜 스코프: `tauri > allowlist > protocol > assetScope` 경로가 `tauri > security > assetProtocol > scope`로 변경.
  * 아이콘 설정: `app > windows > icon` 설정 제거. 이제 아이콘은 `bundle > icon` 경로에 배열 형태로 정의 (`["icons/icon.ico"]`).
  * 번들 식별자: `app > bundleIdentifier` 키가 `identifier`로 이름 변경.

* IPC 통신 채널 개선 (Channel)

  * Tauri 2.0은 Rust 백엔드와 JavaScript 프론트엔드 간의 통신(IPC)을 위해 `tauri::ipc::Channel` (Rust) 및 이에 상응하는 `Channel` 클래스(JavaScript)를 새롭게 도입했습니다.
  * 이 기능은 Tauri v1의 대용량 데이터 전송 시 느린 IPC 속도 문제를 직접적으로 해결하기 위해 설계되었습니다. 대량의 바이너리 데이터나 연속적인 데이터 스트림을 효율적으로 주고받을 수 있도록 최적화되었습니다.

### 2.3. Tauri 1.x -\> 2.x 마이그레이션 최종 점검

Tauri 1.x에서 2.x로의 마이그레이션은 자동화 도구와 수동 검증의 조합으로 이루어집니다.

1. 가장 먼저 `v2 CLI`가 제공하는 `tauri migrate` 명령을 실행합니다. 이 명령어는 설정 파일과 의존성을 분석하여 마이그레이션 과정의 상당 부분을 자동으로 처리합니다.
2. 자동화 도구를 실행한 후에는, 개발자가 직접 코드를 검토하고 수정하는 수동 검증 과정이 반드시 필요합니다. 특히 v1의 핵심 API들이 v2의 플러그인으로 이전됨에 따라, 기존의 API 호출 코드를 새로운 플러그인 기반 코드로 변경해야 합니다.

Tauri v1 API에서 v2 플러그인으로의 마이그레이션 맵

| 기능 | v1 API 모듈 | v2 Rust 플러그인 의존성 | v2 JS 플러그인 의존성 | 주요 변경점 및 예시 |
|---|---|---|---|---|
| 파일 시스템 | `tauri::api::fs` | `tauri-plugin-fs = "2"` | `@tauri-apps/plugin-fs` | JS: `import { readTextFile } from '@tauri-apps/plugin-fs';`. Rust: `std::fs` 사용 권장. |
| 셸 | `tauri::api::shell` | `tauri-plugin-shell = "2"` | `@tauri-apps/plugin-shell` | JS: `import { Command } from '@tauri-apps/plugin-shell';`. `shell.open()` 사용. |
| 다이얼로그 | `tauri::api::dialog` | `tauri-plugin-dialog = "2"` | `@tauri-apps/plugin-dialog` | JS: `import { ask } from '@tauri-apps/plugin-dialog';`. |
| HTTP | `tauri::api::http` | `tauri-plugin-http = "2"` | `@tauri-apps/plugin-http` | JS: `import { fetch } from '@tauri-apps/plugin-http';`. |
| 업데이터 | `tauri::updater` | `tauri-plugin-updater = "2"` | `@tauri-apps/plugin-updater` | JS: `import { check } from '@tauri-apps/plugin-updater';`. `RunEvent::Updater` 제거됨. |
| 운영체제 정보 | `tauri::api::os` | `tauri-plugin-os = "2"` | `@tauri-apps/plugin-os` | JS: `import { platform, type } from '@tauri-apps/plugin-os';`. `os.kind()` -\> `os.type()`. |
| 클립보드 | `AppHandle::clipboard_manager` | `tauri-plugin-clipboard-manager = "2"` | `@tauri-apps/plugin-clipboard-manager` | JS: `import { writeText } from '@tauri-apps/plugin-clipboard-manager';`. |
| 전역 단축키 | `App::global_shortcut_manager` | `tauri-plugin-global-shortcut = "2"` | `@tauri-apps/plugin-global-shortcut` | JS: `import { register } from '@tauri-apps/plugin-global-shortcut';`. |
| CLI 인수 | `App::get_cli_matches` | `tauri-plugin-cli = "2"` | `@tauri-apps/plugin-cli` | JS: `import { getMatches } from '@tauri-apps/plugin-cli';`. |
| 앱 정보 | `@tauri-apps/api/app` | `tauri-plugin-app = "2"` | `@tauri-apps/plugin-app` | JS: `import { getName, getVersion } from '@tauri-apps/plugin-app';`. |
| 알림 | `@tauri-apps/api/notification` | `tauri-plugin-notification = "2"` | `@tauri-apps/plugin-notification` | JS: `import { sendNotification } from '@tauri-apps/plugin-notification';`. |
| 창 관리 | `@tauri-apps/api/window` | `tauri-plugin-window = "2"` | `@tauri-apps/plugin-window` | JS: `import { getAll } from '@tauri-apps/plugin-window';`. |
| 프로세스 | `@tauri-apps/api/process` | `tauri-plugin-process = "2"` | `@tauri-apps/plugin-process` | JS: `import { relaunch } from '@tauri-apps/plugin-process';`. |

## 3부: Tauri의 미래와 개발 로드맵

Tauri의 장기적인 발전 방향을 가늠하기 위해 2025년 2월 22일에 업데이트된 "향후 작업(Future Work)" 문서를 분석해 보면, 프레임워크가 성숙 단계로 진입하고 있음을 명확히 알 수 있습니다.

로드맵에서 가장 두드러지는 부분은 보안에 대한 깊이 있는 투자입니다.

* 바이너리 분석(Binary Analysis): `cargo-auditable`과 같은 도구를 사용하여 SBOM(Software Bill of Materials) 생성을 지원하고, 프론트엔드 자산을 쉽게 추출하여 `npm audit` 등으로 감사할 수 있는 도구를 제공할 계획입니다.
* WebView 강화(WebView Hardening): WebView 프로세스를 더욱 강력하게 샌드박싱하고 격리하여 공격 표면을 줄이는 방안을 연구하고 있습니다.
* 퍼징(Fuzzing): 개발자들이 자신의 Tauri 애플리케이션에 대한 퍼즈 테스트를 더 쉽고 효율적으로 수행할 수 있도록 관련 프레임워크와 도구를 구축하는 것을 목표로 하고 있습니다.

이러한 로드맵은 Tauri가 초기 단계의 프레임워크에서 성숙하고 엔터프라이즈급 사용에 대비하는 플랫폼으로 전환하고 있음을 강력하게 시사합니다. 이는 개발자와 기술 리더들이 Tauri를 더 큰 신뢰를 가지고 기술 스택으로 채택할 수 있는 근거가 됩니다.

## 결론: 2025년 Tauri 개발자를 위한 최종 요약 및 권장 사항

2025년 1분기는 Tauri에 있어 점진적인 패치가 아닌, 중대한 아키텍처의 진화가 이루어진 시기였습니다. 이 기간의 핵심적인 변화는 두 가지로 요약할 수 있습니다: 미래를 향한 Verso 런타임의 실험적 도입과, 현재의 모든 개발에 적용되는 v2 플러그인 아키텍처의 의무적 숙달입니다.

이러한 분석을 바탕으로, 2025년 Tauri 개발자들을 위한 최종 권장 사항은 다음과 같습니다.

* v2 아키텍처 숙달을 최우선 과제로 삼으십시오. 2025년 Tauri 개발자에게 가장 중요하고 시급한 임무는 v2의 플러그인 중심 아키텍처를 완벽하게 이해하고 적용하는 것입니다. 이는 선택이 아닌 필수 사항입니다. 본 보고서의 2부에서 제공된 마이그레이션 가이드와 API 매핑 테이블을 주요 참고 자료로 활용하여, 모든 신규 및 기존 프로젝트가 v2 표준을 따르도록 해야 합니다.
* Verso의 발전을 전략적으로 주시하십시오. Verso 통합은 매우 유망한 기술이지만 아직 실험 단계에 있습니다. 현재로서는 기본 WRY 런타임을 대체할 수 있는 단계가 아닙니다. 크로스플랫폼 렌더링 일관성이 프로젝트의 최우선 순위인 경우 실험적으로 도입을 고려해 볼 수 있지만, 대부분의 상용 프로덕션 애플리케이션에는 안정성이 검증된 기본 런타임을 계속 사용하는 것이 현명합니다.
* 로드맵에 대한 신뢰를 가지십시오. Tauri 프로젝트가 보안 강화와 안정성 향상에 중점을 두고 있다는 것은 프레임워크의 장기적인 건전성과 신뢰성에 대한 강력한 긍정적 신호입니다. 이는 Tauri가 전문적이고 까다로운 요구사항을 충족할 수 있는 성숙한 플랫폼으로 발전하고 있음을 의미하며, 개발자들이 안심하고 장기적인 프로젝트의 기반으로 선택할 수 있는 근거가 됩니다.
