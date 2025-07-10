# Tauri 25H1 핵심 변경사항

---

## 서론

본 보고서는 크로스 플랫폼 애플리케이션 개발 프레임워크인 **Tauri**의 2025년 상반기(1월~6월) 동안의 주요 변경사항을 분석하고, 특히 코딩 구현 시 개발자에게 미치는 영향을 중점적으로 다룹니다. Tauri는 Rust 기반의 백엔드와 HTML, CSS, JavaScript로 구성된 프론트엔드를 결합하여 작고 빠른 바이너리를 생성하는 데 강점을 가지고 있습니다. 2024년 10월에 Tauri 2.0의 안정 버전이 출시된 이후, 2025년 상반기는 모바일 지원 강화와 핵심 API의 안정화 및 확장에 초점을 맞춘 시기였습니다. 이 기간 동안의 업데이트는 프레임워크의 성숙도를 높이고 개발자 경험을 개선하는 데 기여했습니다.

---

## 2025년 상반기 Tauri 버전별 주요 변경사항

### Tauri 2.2.5

Tauri 2.2.5 버전은 2025년 1월 25일에 안정 버전으로 릴리스되었습니다. 이 업데이트의 핵심은 iOS 애플리케이션에서 **`tauri::mobile_entry_point`** 비동기 함수 사용 시 발생하던 패닉 현상을 수정한 단일 버그 수정에 있었습니다.

이 시기의 업데이트가 단일 버그 수정에 집중되었다는 점은, Tauri 2.0의 안정화 릴리스(2024년 10월) 이후 초기 단계에서 프레임워크의 안정성 확보와 주요 버그 해결이 최우선 과제였음을 보여줍니다. 새로운 메이저 버전 출시 후 소프트웨어 개발 주기에서 흔히 나타나는 현상으로, 광범위한 기능 추가보다는 기존 기능의 견고성을 다지는 데 집중했음을 의미합니다. 개발자들은 이 업데이트를 통해 Tauri 2.0으로의 마이그레이션 초기에 발생할 수 있는 주요 안정성 문제를 해결할 수 있었으며, 이는 프레임워크의 신뢰도를 높이고 더 복잡한 기능 개발을 위한 견고한 기반을 제공하는 데 중요한 역할을 했습니다. 특히 iOS 모바일 애플리케이션 개발 시 `mobile_entry_point`를 사용하는 개발자에게 안정성 측면에서 중요한 개선으로, 비동기 로직이 포함된 진입점을 사용하는 경우 예기치 않은 크래시를 방지할 수 있게 되었습니다.

### Tauri 2.5.0

Tauri 2.5.0은 2025년 4월 16일에 릴리스되었으며, 다양한 신규 기능, 개선사항, 버그 수정 및 호환성 파괴 변경을 포함했습니다.

#### `tauri` 크레이트 기준 주요 변경사항

* **새로운 기능**:
  * 모든 프레임에서 초기화 스크립트를 실행할 수 있는 `initialization_script_on_all_frames` API가 `WebviewBuilder`, `WebviewWindowBuilder`, `WebviewAttributes`에 추가되었습니다.
  * iOS에서는 `WebviewWindowBuilder::with_input_accessory_view_builder` 및 `WebviewBuilder::with_input_accessory_view_builder`가 추가되어 입력 액세서리 뷰를 제어할 수 있게 되었습니다.
  * macOS/iOS 웹뷰 빌드 시 링크 미리보기(link previews)를 비활성화하거나 활성화하는 옵션(`WindowOptions::allowLinkPreview`, `WebviewOptions::allowLinkPreview`)이 도입되었습니다.
  * 창이 모니터 크기를 넘어가지 않도록 방지하는 `preventOverflow` 설정 옵션과 관련 API(`WindowBuilder::prevent_overflow` 등)도 추가되었습니다.
* **개선사항**:
  * 적은 양의 데이터를 보낼 때 `Channel`의 성능이 향상되었습니다.
  * `Webview::eval` 및 `WebviewWindow::eval`은 `&str` 대신 `impl Into<String>`을 받도록 변경되어 스크립트 전달이 더 유연하고 효율적으로 이루어집니다.
  * `Builder::invoke_system`도 `AsRef<str>`을 받도록 변경되었습니다.
* **버그 수정**:
  * `Channel`의 윈도우에 연결된 콜백이 정리되지 않던 문제, ACL(Access Control List) 오류 메시지에서 `core:` 참조 누락 문제, Windows 디버그 빌드에서 많은 수의 명령어가 큰 구조체를 매개변수로 사용할 때 스택 오버플로우를 유발하던 문제, `invoke`가 `options.headers`에 비ASCII 문자가 포함될 경우 예외를 올바르게 던지지 않거나 Headers를 무시하던 문제, `run_return`이 `restart` 및 `request_restart`에 응답하지 않던 문제가 수정되었습니다.
* **종속성 업데이트**:
  * `tauri-utils@2.4.0`, `tauri-runtime@2.6.0`, `tauri-runtime-wry@2.6.0`, `tauri-macros@2.2.0`, `tauri-build@2.2.0`로 업그레이드되었으며, `webview2-com` (0.37) 및 `windows` (0.61)도 업데이트되었습니다.
* **호환성 파괴 변경**:
  * `tauri-runtime`에서 실수로 노출되었던 `WebviewAttributes` 재내보내기가 제거되었습니다.

#### `tauri-cli` 기준 주요 변경사항

* **새로운 기능**:
  * iOS 및 macOS 설정에 `bundleVersion`을 추가하여 `CFBundleVersion`을 지정할 수 있도록 지원합니다.
* **개선사항**:
  * 환경 변수 `npm_config_user_agent`에서 패키지 관리자를 먼저 감지하도록 개선되었고, iOS 시뮬레이터 사용이 향상되었습니다.
* **버그 수정**:
  * macOS에서 `fileAssociations`에 `LSHandlerRank`가 누락되던 문제와 Xcode 16.3 시뮬레이터에서 iOS 개발이 작동하지 않던 문제가 수정되었습니다.
* **종속성 업데이트**:
  * `tauri-utils@2.4.0`, `tauri-bundler@2.4.0`로 업그레이드되었습니다.

#### `@tauri-apps/api` 2.5.0 기준 주요 변경사항

* **새로운 기능**:
  * 배지(badging) API가 추가되어 `Window`/`WebviewWindow::set_badge_count` (Linux, macOS, iOS), `Window`/`WebviewWindow::set_overlay_icon` (Windows Only), `Window`/`WebviewWindow::set_badge_label` (macOS Only)를 지원합니다.
  * `WebviewWindow.clearAllBrowseData` 및 `Webview.clearAllBrowseData`가 추가되어 웹뷰 브라우징 데이터를 지울 수 있게 되었고, `Window.setTheme` 또는 `setTheme` 함수를 통해 테마를 동적으로 설정하는 기능이 생겼습니다.
  * `Webview.hide` 및 `Webview.show` 메서드도 추가되었으며, `WindowSizeConstraints` 인터페이스를 통해 `Window.setSizeConstraints` 및 `WebviewWindow.setSizeConstraints`를 사용하여 창 크기 제약 조건을 개별적으로 설정하는 API도 추가되었습니다.
* **버그 수정**:
  * `dpi` 모듈에서 `toLogical` 및 `toPhysical`의 회귀 오류와 `BaseDirectory.Home` 및 `BaseDirectory.Font`의 정수 값 회귀 오류가 수정되었습니다.
* **호환성 파괴 변경**:
  * 트레이(tray) 이벤트 구조에 대한 대규모 리팩토링이 있었습니다. `TrayIconEvent`가 구조체 대신 열거형으로 변경되었고, `ClickType` 열거형이 `MouseButton` 열거형으로 대체되었으며, `MouseButtonState` 열거형이 추가되었습니다.

Tauri 2.5.0에서 모바일 관련 기능(iOS 입력 액세서리 뷰, macOS/iOS 링크 미리보기 제어, `bundleVersion` 지원)이 대거 추가되고 CLI의 iOS 시뮬레이터 사용성이 개선되었다는 점은 Tauri 2.0의 핵심 목표였던 모바일 지원을 실질적으로 강화하려는 노력이 반영된 결과로 해석됩니다. 이는 모바일 앱의 완성도를 높이는 데 필수적인 요소들입니다. 또한, `preventOverflow`와 같은 창 관리 기능, `eval` 인자 유연성, `Channel` 성능 개선, 그리고 `tauri-cli`의 패키지 관리자 감지 개선 등은 개발자 생산성과 애플리케이션 품질을 직접적으로 향상시키는 변화입니다. 이러한 변화는 Tauri가 단순히 기능 확장을 넘어, 실제 개발 워크플로우와 사용자 경험을 세심하게 고려하고 있음을 보여줍니다. 이 업데이트는 Tauri를 사용하여 모바일 애플리케이션을 개발하는 개발자들에게 더 풍부한 기능과 개선된 개발 환경을 제공하며, 데스크톱 애플리케이션에서도 더 정교한 UI/UX 제어를 가능하게 합니다. 특히 `preventOverflow`는 다양한 모니터 환경에서 애플리케이션의 시각적 일관성을 유지하는 데 기여하며, `invoke` 헤더 처리 버그 수정은 IPC 통신의 신뢰성을 높여 복잡한 애플리케이션 개발에 긍정적인 영향을 미칩니다.

### Tauri 2.6.0

Tauri 2.6.0은 2025년 6월 10일경에 릴리스되었습니다.

#### `tauri` 크레이트 기준 주요 변경 사항

* **종속성 업데이트**:
  * `tauri-utils@2.5.0`, `tauri-runtime-wry@2.7.0`, `tauri-macros@2.3.0`, `tauri-build@2.3.0`, `tauri-runtime@2.7.0`으로 업그레이드되었습니다. 또한 `tao`가 0.34, `wry`가 0.52, `webview2-com`이 0.38로 업데이트되었습니다.

#### `@tauri-apps/api` 기준 주요 변경사항

* **새로운 기능**:
  * 웹뷰의 자동 크기 조절 기능을 제어하는 `setAutoResize` API가 `@tauri-apps/api`를 통해 노출되었습니다.
  * `Monitor.workArea` 필드가 추가되어 모니터의 실제 작업 영역 정보를 얻을 수 있게 되었습니다.
* **버그 수정**:
  * `path.join('', 'a')`가 잘못된 경로를 반환하던 문제와 `unlisten` 함수 호출 시 이벤트 리스너가 즉시 등록 해제되지 않던 문제가 수정되었습니다.

2.6.0 버전의 주요 변경사항이 대부분 내부 종속성 업데이트와 `@tauri-apps/api`의 소규모 기능 추가 및 버그 수정이라는 점은, Tauri 팀이 2.5.0에서 도입된 주요 기능들을 안정화하고, 하위 레이어(`tao`, `wry` 등)의 최신 버전을 통합하여 프레임워크의 기반을 지속적으로 강화하고 있음을 보여줍니다. `Monitor.workArea`와 같은 새로운 API는 개발자가 사용자 환경에 더욱 동적으로 반응하는 애플리케이션을 만들 수 있도록 지원합니다. 이러한 내부 종속성 업데이트는 잠재적으로 성능 향상, 새로운 웹뷰 기능 지원, 그리고 다양한 OS 버전과의 호환성 개선으로 이어집니다. 개발자 입장에서는 프레임워크의 안정성과 미래 확장성에 대한 신뢰를 가질 수 있으며, 새로운 API를 통해 더욱 정교한 UI/UX를 구현할 수 있는 가능성이 열립니다.

### Tauri 2.6.2

Tauri 2.6.2는 2025년 6월 27일에 소스 코드 수정 및 릴리스가 확인되었습니다.

`tauri info` 출력에 따르면 `tauri: 2.6.2`, `tauri-build: 2.3.0`, `wry: 0.52.1`, `tao: 0.34.0`로 업데이트된 것을 확인할 수 있습니다.

`@tauri-apps/api`는 2.5.0으로 표시되어 있지만, 최신 버전은 2.6.0으로 언급되어 있어, 이 정보는 릴리스 시점의 `tauri info` 결과일 수 있습니다.

2.6.2가 2025년 상반기의 거의 마지막 시점에 릴리스되었고, 주로 종속성 버전 업데이트를 포함한다는 것은, Tauri 팀이 상반기 동안 진행된 주요 기능 개발(2.5.0)과 실험적 통합(Verso) 이후, 모든 컴포넌트의 최신 안정 버전을 통합하고 전반적인 생태계를 정렬하는 데 집중했음을 보여줍니다. `tauri info`에서 `@tauri-apps/api`가 2.5.0으로 표시되면서도 최신은 2.6.0이라고 언급된 것은, 릴리스 간의 미묘한 시간차 또는 특정 빌드 환경에서의 종속성 불일치를 시사할 수 있습니다. 이러한 지속적인 종속성 관리는 Tauri 애플리케이션의 장기적인 유지보수성과 보안성을 보장하는 데 필수적입니다. 개발자들은 최신 버전으로 업데이트함으로써 최신 웹뷰 기술의 이점과 개선된 성능을 누릴 수 있습니다. 이 버전은 주로 내부 종속성 업데이트와 안정화에 중점을 둔 패치 릴리스로 보이며, 개발자에게 직접적인 호환성 파괴 변경보다는 안정성 및 빌드 환경 개선에 기여할 것으로 예상됩니다.

---

## 코딩 구현 관련 핵심 변경사항 및 영향

### 새로운 런타임: Experimental Tauri Verso Integration (실험적 Tauri Verso 통합)

2025년 3월 17일, Tauri는 Servo 기반의 새로운 브라우저인 **Verso**와의 실험적인 통합을 발표했습니다. 이는 `tauri-runtime-verso`라는 새로운 커스텀 런타임을 통해 이루어지며, 기존의 `tauri-runtime-wry`와 유사하게 런타임을 쉽게 교체할 수 있도록 설계되었습니다.

Verso는 Rust로 작성된 웹 브라우저 렌더링 엔진인 Servo를 기반으로 하며, Servo의 저수준 API를 보다 인체공학적으로 사용하기 위해 추상화된 계층을 제공합니다. Tauri는 `tauri-runtime-verso`를 통해 이 Verso를 웹뷰 라이브러리로 활용합니다.

`tauri-runtime-verso`를 사용하려면 `main.rs` 파일에서 특정 설정을 추가해야 합니다. **`set_verso_path("../verso/target/debug/versoview");`**를 통해 `versoview` 실행 파일의 경로를 설정해야 하며, **`set_verso_resource_directory("../verso/resources");`**를 통해 Verso/Servo의 리소스 디렉토리(사용자 에이전트 스타일시트 등) 경로를 설정하는 것이 권장됩니다. 또한, Tauri 애플리케이션 빌드 시 **`tauri::Builder::<VersoRuntime>::new().invoke_system(INVOKE_SYSTEM_SCRIPTS.to_owned())`**를 통해 시스템 스크립트를 호출해야 특정 명령어가 올바르게 작동합니다.

현재 `tauri-runtime-verso`는 `tauri-cli`의 모든 기능, React와 같은 최신 프레임워크와의 호환성, 공식 로그 및 오프너 플러그인, 창 관리 기능(크기, 위치, 최대화, 최소화, 닫기), Vite의 CSS 핫 리로드, `data-tauri-drag-region` 속성 등을 지원합니다.

Tauri가 기존 `wry` (시스템 웹뷰 기반) 외에 Verso라는 새로운 런타임을 실험적으로 통합했다는 것은, 특정 웹뷰 기술에 대한 의존성을 줄이고, 렌더링 엔진 선택의 유연성을 확보하려는 장기적인 전략을 나타냅니다. `wry`는 시스템 웹뷰(WebView2, WebKitGTK, WebKit)를 사용하기 때문에 시스템 환경에 따른 제약이 있지만, Verso는 Rust 기반의 독립적인 렌더링 엔진이므로, Tauri 팀이 렌더링 스택에 대한 더 많은 제어권을 확보하고, 특정 플랫폼의 웹뷰 업데이트 주기에 덜 구속받으려는 의도로 해석될 수 있습니다. 이는 특히 "[evergreen shared Verso](https://tauri.app/blog/2025/03/17/state-of-tauri-verso/#evergreen-shared-verso)"라는 장기 목표에서 명확하게 드러납니다. 이러한 움직임은 개발자에게 더 많은 런타임 선택지를 제공하여 특정 프로젝트 요구사항(예: 번들 크기, 커스텀 렌더링 동작, 특정 웹 표준 지원)에 최적화된 환경을 구축할 수 있는 가능성을 열어줍니다. 이는 Tauri의 아키텍처적 유연성을 강조하며, 향후 더 다양한 웹뷰 기술과의 통합을 위한 발판이 될 수 있습니다. 하지만 현재는 실험 단계이므로, 프로덕션 환경에서의 즉각적인 도입보다는 탐색 및 테스트가 권장됩니다.

`tauri-runtime-verso`의 통합은 단순히 새로운 기능을 제공하는 것을 넘어, 개발자가 `main.rs`에서 `set_verso_path`, `set_verso_resource_directory`, `invoke_system`과 같은 명시적인 설정을 추가해야 한다는 점에서 초기 설정 및 빌드 프로세스에 직접적인 영향을 미칩니다. 이는 기존 `wry` 런타임 사용 시와는 다른 초기화 로직을 요구하며, 개발자가 선택한 런타임에 따라 애플리케이션의 진입점을 다르게 구성해야 함을 의미합니다. 이러한 변경은 Tauri의 모듈화된 아키텍처를 잘 보여주지만, 동시에 개발자가 런타임 선택에 따른 추가적인 설정 부담을 가질 수 있음을 시사합니다. 특히, `versoview` 실행 파일의 자체 컴파일 필요성은 초기 진입 장벽으로 작용할 수 있으며, Tauri 팀의 "pre-built Verso executable" 계획이 이러한 문제를 해결하는 데 중요할 것입니다.

```rust
// Experimental Tauri Verso Integration 예시 (main.rs)
use tauri_runtime_verso::{
    INVOKE_SYSTEM_SCRIPTS, VersoRuntime, set_verso_path, set_verso_resource_directory,
};

fn main() {
    // versoview 실행 파일의 경로를 설정해야 합니다.
    set_verso_path("../verso/target/debug/versoview");
    // Verso/Servo의 리소스 디렉토리 경로를 설정하는 것이 권장됩니다.
    set_verso_resource_directory("../verso/resources");
    tauri::Builder::<VersoRuntime>::new()
        // 이 설정을 해야 일부 명령어가 작동합니다.
        .invoke_system(INVOKE_SYSTEM_SCRIPTS.to_owned())
        .run(tauri::generate_context!())
        .unwrap();
}
````

### API 확장 및 개선

#### 창 및 웹뷰 제어 강화

2025년 상반기에는 애플리케이션의 창과 웹뷰를 더욱 정교하게 제어할 수 있는 다양한 API가 추가되었습니다. Tauri 2.5.0에서는 창이 모니터 크기를 벗어나지 않도록 방지하는 **`preventOverflow`** 설정 옵션과 `WindowBuilder::prevent_overflow`, `WebviewWindowBuilder::prevent_overflow` 등의 API가 추가되었습니다. 또한, **`WindowSizeConstraints`** 인터페이스를 통해 `Window.setSizeConstraints` 및 `WebviewWindow.setSizeConstraints`를 사용하여 창의 최소/최대 크기 제약 조건을 더 세밀하게 제어할 수 있게 되었습니다. 웹뷰를 프로그래밍 방식으로 숨기거나 표시하는 **`Webview.hide`** 및 **`Webview.show`** 메서드도 도입되었습니다. Tauri 2.6.0의 `@tauri-apps/api`에서는 웹뷰의 자동 크기 조절 기능을 제어하는 **`setAutoResize`** API가 노출되었고, **`Monitor.workArea`** 필드가 추가되어 모니터의 작업 영역(dock, taskbar 등을 제외한 실제 사용 가능한 영역)을 얻을 수 있게 되었습니다.

`preventOverflow`, `setSizeConstraints`, `hide/show` 웹뷰 메서드, `setAutoResize`, `Monitor.workArea`와 같은 API 추가는 개발자가 애플리케이션의 창과 웹뷰를 더욱 동적으로, 그리고 사용자 환경에 적응적으로 제어할 수 있도록 돕습니다. 이는 다양한 화면 크기, 해상도, 그리고 OS별 UI 요소(독, 작업 표시줄)에 대응하는 반응형 디자인 구현을 용이하게 합니다. 특히 `Monitor.workArea`는 크로스 플랫폼 환경에서 UI 요소를 정확하게 배치하는 데 필수적인 정보를 제공합니다. 이러한 기능들은 사용자 경험을 향상시키고, 애플리케이션이 다양한 데스크톱 및 모바일 환경에서 더 일관되고 전문적인 모습을 보이도록 돕습니다. 개발자는 이제 하드코딩된 값 대신 동적인 정보를 사용하여 애플리케이션 레이아웃을 최적화할 수 있습니다.

#### 모바일 및 플랫폼별 기능 확장

모바일 플랫폼 지원은 Tauri 2.0의 핵심 목표 중 하나였으며, 2025년 상반기에도 이 분야의 기능 확장이 두드러졌습니다. Tauri 2.5.0에서는 iOS에 `WebviewWindowBuilder::with_input_accessory_view_builder` 및 `WebviewBuilder::with_input_accessory_view_builder`가 추가되어, 키보드 위에 나타나는 액세서리 뷰를 커스터마이징할 수 있게 되었습니다. 또한, macOS/iOS 웹뷰 빌드 시 링크 미리보기 기능을 비활성화하거나 활성화하는 옵션(**`WindowOptions::allowLinkPreview`**, **`WebviewOptions::allowLinkPreview`**)이 추가되었습니다.

`@tauri-apps/api` 2.5.0에서는 **배지(badging) API**가 추가되어 `Window`/`WebviewWindow::set_badge_count` (Linux, macOS, iOS), `Window`/`WebviewWindow::set_overlay_icon` (Windows), `Window`/`WebviewWindow::set_badge_label` (macOS) 등 플랫폼별 배지 기능을 제어할 수 있게 되었습니다.

`tauri-cli` 2.5.0에서는 iOS 및 macOS 번들 구성에 `CFBundleVersion`을 지정할 수 있는 기능이 추가되었습니다.

iOS 특정 기능(입력 액세서리 뷰, 링크 미리보기 제어)과 크로스 플랫폼 배지 API, `CFBundleVersion` 지원은 Tauri 2.0의 모바일 지원이 단순히 "작동하는" 수준을 넘어, 플랫폼별 사용자 경험을 세밀하게 조정하고 네이티브 앱과 같은 느낌을 줄 수 있는 방향으로 진화하고 있음을 나타냅니다. 이는 모바일 앱의 완성도를 높이는 데 필수적인 요소들입니다. 개발자들은 이제 Tauri를 사용하여 모바일 애플리케이션을 개발할 때, 각 플랫폼의 고유한 UI/UX 가이드라인을 더 잘 준수하고, 사용자에게 더 자연스러운 경험을 제공할 수 있게 되었습니다. 이는 Tauri의 모바일 시장 침투력을 높이는 데 기여할 것입니다.

#### 기타 API 개선

Tauri 2.0.4(2025년 상반기 이전 릴리스이나 v2 API 세트의 일부로 중요)에서는 프론트엔드로 전송되는 메시지를 가로챌 수 있는 `Builder::channel_interceptor`가 추가되어 `Builder::invoke_system` 인터페이스를 보완합니다. 또한, 이벤트 명령이 메인 스레드를 차단하지 않도록 비동기(`async`)로 표시되었습니다. Windows에서는 팝업 및 트레이 아이콘 메뉴에 사용되는 \*\*`HMENU`\*\*를 가져오는 `ContextMenu::hpopupmenu` 메서드가 추가되었습니다.

**`App::cleanup_before_exit`** 및 \*\*`AppHandle::cleanup_before_exit`\*\*가 추가되어 종료 전 정리 로직을 수동으로 호출할 수 있게 되었습니다. Tauri 2.0.0-beta.0(2025년 상반기 이전 릴리스이나 v2 이벤트 시스템의 핵심)에서는 `event` 모듈에 **`emitTo`** API가 추가되어 Rust의 `emit_to` 메서드와 동일하게 작동하며, `Window`, `Webview` 및 `WebviewWindow` 클래스에도 `emitTo` 메서드가 추가되었습니다.

`channel_interceptor`와 이벤트 명령의 비동기화는 Tauri의 핵심인 IPC(Inter-Process Communication) 및 이벤트 시스템의 유연성과 성능을 향상시킵니다. `channel_interceptor`는 디버깅, 로깅, 또는 보안 검사와 같은 미들웨어 계층을 구현할 수 있는 가능성을 열어주며, 이벤트 비동기화는 UI 응답성을 유지하는 데 필수적입니다. `emitTo`의 확장성은 다중 창/웹뷰 환경에서 특정 대상에게만 이벤트를 전송하는 시나리오를 지원하여, 애플리케이션의 복잡성을 관리하는 데 도움을 줍니다. 이러한 개선은 대규모 또는 고성능이 요구되는 Tauri 애플리케이션 개발에 중요한 영향을 미칩니다. 개발자는 이제 더 정교한 통신 흐름을 구축하고, 애플리케이션의 반응성을 저해하지 않으면서 백엔드 로직과 프론트엔드 UI 간의 상호작용을 최적화할 수 있습니다.

### 호환성 파괴 변경사항 및 마이그레이션 고려사항

2025년 상반기 동안 Tauri는 몇 가지 호환성 파괴 변경사항을 포함했습니다. Tauri 2.5.0에서는 `tauri-runtime`에서 실수로 노출되었던 `WebviewAttributes` 재내보내기가 제거되었습니다. 이전에 이 재내보내기에 의존하여 코드를 작성했던 개발자는 직접 `tauri-runtime` 크레이트에서 `WebviewAttributes`를 임포트하거나, 해당 API 사용 방식을 변경해야 합니다.

또한, Tauri 2.0.0-alpha.11(2025년 상반기 이전 변경이나 v2 마이그레이션에 지속적으로 영향)에서 트레이(Tray) 아이콘 및 메뉴 API에 대한 대규모 리팩토링이 있었습니다. 모든 메뉴 및 트레이 타입이 `tauri::menu` 및 `tauri::tray` 모듈에서 새로운 이름으로 내보내지게 되었으며, `tauri::Builder::system_tray`가 제거되고 `tauri::tray::TrayIconBuilder`를 `tauri::Builder::setup` 훅 내에서 사용해야 합니다. `tauri::Builder::menu`도 함수로 변경되었습니다. `tauri::Context` 메서드 이름도 변경되었고, `RunEvent::MenuEvent`, `RunEvent::TrayIconEvent`가 추가되었습니다. `App`/`AppHandle` 및 `Window`에 메뉴 및 트레이 관련 새로운 메서드들이 추가되었습니다. 이러한 변경은 Tauri v1에서 v2로 마이그레이션하거나, v2 초기 알파/베타 버전에서 2025년 상반기 버전으로 업데이트하는 개발자에게는 트레이 아이콘 및 메뉴 구현에 상당한 코드 변경을 요구합니다. 기존 코드는 더 이상 컴파일되지 않을 가능성이 높지만, 새로운 API는 더 유연하고 강력한 기능을 제공합니다.

마지막으로, `system-tray` 기능 플래그가 `tray-icon`으로 이름이 변경되었습니다. `Cargo.toml` 파일에서 해당 기능 플래그를 사용하는 경우 이름을 업데이트해야 합니다. `Window#on_navigation` 클로저의 인자가 `Url` 대신 `&Url`을 받도록 변경되었습니다.

---

## 결론

2025년 상반기 동안 Tauri 프레임워크는 2.0 안정 버전 출시 이후 안정화와 기능 확장을 동시에 추구하는 시기를 보냈습니다. 2.2.5 버전에서 iOS 관련 주요 버그를 수정하며 안정성에 대한 초기 집중을 보여주었고, 이는 새로운 메이저 버전의 기반을 다지는 데 필수적이었습니다. 이후 2.5.0 및 2.6.0 버전에서는 모바일 지원을 대폭 강화하고, 창 및 웹뷰 제어 API를 확장하며, IPC 및 이벤트 시스템을 정교화하는 데 주력했습니다. 특히 `initialization_script_on_all_frames`, iOS 입력 액세서리 뷰, 플랫폼별 배지 API, 그리고 `preventOverflow`와 같은 기능들은 개발자가 더욱 정교하고 플랫폼 친화적인 애플리케이션을 구축할 수 있도록 지원합니다.

또한, Servo 기반의 Verso 런타임과의 실험적인 통합은 Tauri가 특정 웹뷰 기술에 대한 의존성을 줄이고, 렌더링 스택에 대한 더 많은 제어권을 확보하려는 장기적인 전략적 움직임을 보여줍니다. 이는 향후 Tauri 애플리케이션의 유연성과 번들 크기 최적화에 기여할 잠재력을 가지고 있습니다.

일부 호환성 파괴 변경사항, 특히 트레이 아이콘 및 메뉴 API의 대규모 리팩토링은 기존 Tauri v1 또는 초기 v2 사용자가 최신 버전으로 업데이트할 때 상당한 코드 수정이 필요할 수 있음을 의미합니다. 그러나 이러한 변경은 더 강력하고 유연한 API를 제공하여 장기적인 개발 편의성을 높이는 데 목적이 있습니다.

결론적으로, 2025년 상반기 Tauri의 변경사항은 프레임워크가 모바일 및 데스크톱 환경 모두에서 더욱 견고하고 기능이 풍부한 크로스 플랫폼 애플리케이션 개발을 지원하기 위해 지속적으로 진화하고 있음을 명확히 보여줍니다. 개발자들은 이러한 업데이트를 통해 향상된 성능, 확장된 기능, 그리고 더 나은 개발 경험을 기대할 수 있습니다.
