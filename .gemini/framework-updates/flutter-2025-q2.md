# Flutter 2025년 2분기 주요 업데이트 분석

## 서론: Flutter 2025년 2분기 핵심 업데이트 개요

이 보고서는 Flutter의 2025년 2분기(2025년 3월 1일부터 2025년 6월 30일까지)에 릴리즈된 주요 패치노트와 업데이트를 한국인 개발자가 이해하기 쉽게 정리하여 제공합니다. 이번 2분기에는 개발자 경험, 성능, 플랫폼 통합 및 새로운 기능 측면에서 중요한 변화들이 있었습니다. 특히 Flutter 3.32와 Dart 3.8의 공식 릴리즈는 주목할 만한 주요 사건입니다.

보고서의 목적은 해당 기간 동안 릴리즈된 Flutter 및 Dart의 모든 중요한 변경사항을 심층적으로 분석하고, 개발자가 코딩 시 반드시 알아야 할 핵심적인 내용들을 명확히 제시하는 것입니다.

2025년 2분기의 주요 릴리즈는 다음과 같습니다:

* Flutter 3.32 (2025년 5월 20일 릴리즈): Google I/O 2025에서 발표된 주요 안정화 버전으로, 웹 핫 리로드의 정식 지원, 새로운 UI 위젯 도입, 성능 최적화 및 AI 통합 기능이 핵심입니다.
* Dart 3.8 (2025년 5월 20일 릴리즈): Flutter 3.32와 함께 릴리즈된 Dart 언어의 업데이트로, 개발자 생산성 향상과 코드 품질 개선에 초점을 맞춘 기능들이 포함되었습니다.
* Flutter 3.31 Beta (2025년 3월 중): 웹 핫 리로드 기능이 베타로 도입되며 웹 개발 경험을 크게 향상시켰습니다.
* Flutter 3.29 Hotfixes (2025년 3월 이후): 2월에 릴리즈된 Flutter 3.29의 안정화 및 버그 수정이 2분기 내내 이루어졌습니다. 특히 Impeller 엔진 관련 안정화가 중요하게 다루어졌습니다.
* Syncfusion Flutter 2025 Volume 2 (2025년 2분기): 주요 서드파티 위젯 라이브러리인 Syncfusion도 새로운 기능과 라이선스 정책 변경을 발표했습니다.
* Sentry Flutter SDK 9.0 (2025년 6월 16일): 에러 모니터링 SDK의 주요 업데이트가 있었습니다.

Flutter 3.32와 Dart 3.8이 동시에 릴리즈된 점, 그리고 Flutter 팀이 Dart 언어 개선에 지속적으로 투자하고 있다는 사실은 Flutter가 Dart 언어와 긴밀히 통합된 완전한 개발 생태계를 지향하고 있음을 시사합니다.

또한, 웹 핫 리로드와 WASM 지원 개선, 레거시 HTML/JS 라이브러리 제거 노력 등 웹 관련 개선사항이 지속적으로 강조되고 있습니다. 이는 Flutter 팀이 웹을 모바일 및 데스크톱과 동등한 수준의 핵심 타겟으로 보고 있음을 의미합니다.

---

## 핵심 프레임워크 및 렌더링 엔진 개선사항

이 섹션에서는 Flutter 프레임워크 자체와 렌더링 엔진인 Impeller의 주요 개선사항을 다룹니다.

### Impeller 엔진의 안정화 및 성능 향상 (Android 포함)

Flutter의 차세대 렌더링 엔진인 Impeller는 2025년 2분기에 Android 플랫폼에서 기본 렌더링 엔진으로 완전히 채택되었습니다. Flutter 3.29부터는 iOS에서 Skia가 완전히 제거되었고, Impeller를 비활성화하는 `FLTEnableImpeller` 옵트아웃 플래그는 더 이상 사용할 수 없게 되었습니다.

Android에서는 Impeller Vulkan 드라이버가 제대로 작동하지 않는 기기에서 Impeller OpenGLES로 자동 폴백(fallback)되도록 개선되었습니다. Impeller Vulkan의 안정성 문제(깜빡임, 시각적 흔들림)가 수정되었고, 특정 SoC(MediaTek/PowerVR)에서는 Vulkan 사용이 비활성화되는 조치가 취해졌습니다.

성능 향상 측면에서는 `BackdropGroup` 위젯과 `BackdropFilter.grouped` 생성자를 통해 여러 블러 필터 사용 시 성능이 향상되었고, `ImageFilter.shader` 생성자를 통해 위젯에 커스텀 셰이더를 적용할 수 있게 되어 애니메이션 및 렌더링 기능이 강화되었습니다. 또한, Conic curves가 더 이상 근사치로 처리되지 않고 직접 테셀레이션(tessellation)되어 렌더링 품질이 향상되었으며, 부분 재그리기(partial repaint)가 최적화되었습니다.

### 웹용 핫 리로드 정식 지원 및 개발 생산성 증대

Flutter 웹 개발에서 가장 오랫동안 기다려온 기능 중 하나인 핫 리로드가 Flutter 3.31 베타(2025년 3월)를 거쳐 Flutter 3.32에서 공식적으로 지원됩니다.

이는 `flutter run -d chrome --web-experimental-hot-reload` 명령어를 통해 실험적으로 활성화할 수 있었으며, VS Code에서 "Dart: Flutter Hot Reload On Save" 설정을 활성화하거나, Run/Debug 패널의 ⚡ 아이콘으로 트리거할 수 있습니다.

이 기능은 Dart Development Compiler (DDC)를 사용할 때 가능하며, 웹 애플리케이션 개발의 반복 속도(iteration time)를 획기적으로 단축합니다.

### 새로운 UI 위젯 및 기능 (Squircle, Expansible 등)

Flutter 3.32는 새로운 위젯과 기존 위젯의 기능을 확장했습니다.

* Squircles (스쿼클): 사각형과 원의 중간 형태인 '스쿼클'을 UI 디자인에 적용할 수 있는 새로운 API(`RoundedSuperellipseBorder`, `ClipRSuperellipse`, `Canvas.drawRSuperellipse`, `Canvas.clipRSuperellipse`, `Path.addRSuperellipse`)가 도입되었습니다.
* Expansible 위젯: 확장 및 축소 가능한 콘텐츠를 더 쉽게 만들 수 있는 `Expansible` 위젯이 도입되었습니다. 이 위젯은 기존 `ExpansionTile`의 내부 로직에 통합되어 사용됩니다.
* `CarouselController`: `animateToIndex` 메서드가 추가되어 캐러셀의 부드러운 인덱스 기반 내비게이션이 가능해졌습니다.
* `TabBar`: `onHover` 및 `onFocusChange` 콜백이 추가되어 다양한 상태에서 위젯의 외관을 더 세밀하게 제어할 수 있게 되었습니다.
* `SearchAnchor`/`SearchAnchor.bar`: `viewOnOpen` 및 `onOpen` 콜백이 추가되어 열기/닫기 이벤트를 더 잘 관찰하고 처리할 수 있습니다.
* Syncfusion 차트: `enableDirectionalZooming` 속성을 통해 차트에서 수평, 수직 또는 양방향으로 확대/축소할 수 있는 기능이 추가되었습니다.

### 접근성 및 사용자 경험 개선 (슬라이더, 드롭다운 메뉴 등)

여러 위젯의 접근성과 사용자 경험이 향상되었습니다.

* Slider/RangeSlider: 개별 라벨 스타일 커스터마이징을 위한 `onLabelCreated` 콜백이 도입되었으며, 기존 `labelFormatterCallback`은 사용 중단되었습니다. 키보드 접근성(TAB 키로 이동, 화살표 키로 값 조정)이 지원됩니다. `RangeSlider`의 썸(thumb) 오버레이가 하나만 표시되도록 수정되었고, 썸이 트랙의 양 끝에 도달할 수 있도록 개선되었습니다.
* DropdownMenu: 메뉴 너비가 텍스트 필드보다 작을 수 있도록 수정되었고, `RenderFlex` overflow 오류 예시가 업데이트되었습니다.
* Form 위젯: 스크린 리더 활성화 시 첫 번째 에러만 알리도록 변경되었습니다.
* 텍스트 필드: `onTapUpOutside` 동작을 커스터마이징할 수 있게 되었습니다.
* FormField: 에러 메시지를 텍스트뿐만 아니라 어떤 위젯으로도 생성할 수 있게 되었습니다.
* Selectable Text: 버그가 줄고 웹에서 성능이 향상되었습니다.
* iOS Voice Control: 불필요한 라벨 표시가 줄어들어 사용자 경험이 향상되었습니다.

---

## Dart 언어 및 개발자 경험 혁신

Flutter의 성능과 개발자 생산성은 Dart 언어의 발전에 크게 의존합니다. Dart 3.8은 개발자의 코딩 방식을 개선하고, 오류 처리 및 디버깅을 용이하게 하는 중요한 기능들을 도입했습니다.

### Dart 3.8의 주요 변경사항 (2025년 5월 20일 릴리즈)

Dart 3.8은 개발자 편의성을 강화하는 여러 기능을 포함하여 릴리즈되었습니다.

* 널 인식 컬렉션 요소 (Null-Aware Collection Elements): 컬렉션 리터럴(리스트, 셋, 맵)에서 요소가 null이 아닌 경우에만 포함되도록 `?` 접두사를 사용할 수 있게 되었습니다.
* 크로스 컴파일 지원 (Linux 타겟): Windows, macOS, Linux 개발 환경에서 `dart compile exe` 또는 `dart compile aot-snapshot` 명령어를 사용하여 네이티브 Linux 바이너리를 컴파일할 수 있게 되었습니다.
* 문서 임포트 (Doc Imports): 코드에 실제 `import` 문을 추가하지 않고도 문서 주석 내에서 외부 요소를 참조할 수 있는 새로운 주석 기반 구문(`/// @docImport 'dart:async';`)이 도입되었습니다.
* 향상된 코드 포매터 (Trailing Comma 정책 변화 포함): Dart 3.8의 포매터는 "tall" 스타일을 지원하도록 재작성되었으며, 트레일링 콤마(trailing comma) 처리 방식이 변경되었습니다.

### 직접 네이티브 상호 운용성 (FFIgen, JNIgen) 초기 액세스 프로그램

Flutter는 Dart 코드와 네이티브 코드(Kotlin/Java, Swift/Objective-C) 간의 브리징을 직접 처리하는 코드 생성 솔루션인 FFIgen 및 JNIgen의 초기 액세스 프로그램을 시작했습니다. 기존의 메서드 채널(Method Channel) 방식과 달리, 이 솔루션들은 API를 동기적으로 호출할 수 있게 하고, 트리 쉐이킹(tree-shaking)을 지원하며, 더 많은 데이터를 플랫폼 레이어에 상주시킬 수 있습니다.

### Riverpod 통합 구문 제안 (상태 관리 간소화)

인기 있는 상태 관리 패키지인 Riverpod의 개발자 Rémi Rousselet이 코드 생성(code generation)을 제거하고 더 간결하고 통일된 프로바이더(provider) 구문(`StateNotifierProvider`와 같은 복잡한 타입 제거)을 제안했습니다.

### 글로벌 오류 캡처 및 스택 트레이스 개선

`PlatformDispatcher.onError`를 통해 전역 오류 캡처 기능이 도입되어 애플리케이션 전반의 예외를 더 잘 감지하고 처리할 수 있게 되었습니다. 또한, 비동기 디버깅을 위한 스택 트레이스 기능이 향상되어 오류 해결 워크플로우가 간소화되었습니다.

### 향상된 개발자 도구 (DevTools, IDE 지원)

Flutter DevTools의 Inspector가 개선되어 위젯 트리 가시성이 향상되고, 위젯 속성 뷰가 재설계되었으며, 핫 리로드 및 내비게이션 이벤트에 대한 자동 새로고침이 지원됩니다. 로깅 도구도 개선되어 로그 심각도, 카테고리, 존(zone), 아이솔레이트(isolate)와 같은 더 많은 메타데이터를 표시하고, 심각도 수준별 필터링을 지원하며, 성능과 초기 로드 시간이 크게 향상되었습니다.

Dart 3.8은 IntelliJ 및 VS Code에서 더 스마트한 코드 완성, `FutureBuilder` 및 `ValueListenableBuilder`에 대한 빠른 래핑, 클로저 매개변수 이름 변경, show/hide 조합자(combinator) 지원 등 새로운 코드 어시스트 기능을 제공합니다.

---

## 개발자가 반드시 알아야 할 변경사항 및 마이그레이션 가이드

이 섹션은 기존 Flutter 프로젝트에 직접적인 영향을 미치거나, 코딩 방식에 변화를 요구하는 주요 변경사항, 사용 중단(Deprecation), 그리고 제거된 기능들을 다룹니다.

### 주요 API 변경 및 사용 중단

| 변경 유형 | 영향을 받는 API/위젯 | 새로운 API/대안 | 관련 릴리즈 | 설명/주의사항 |
|---|---|---|---|---|
| 사용 중단 | `SfSlider`, `SfRangeSlider`, `SfRangeSelector`의 `labelFormatterCallback` | `onLabelCreated` | Syncfusion Flutter 2025 Volume 2 | Syncfusion 슬라이더 위젯의 라벨 커스터마이징 로직을 `onLabelCreated`로 마이그레이션해야 합니다. |
| 스타일 변경 | `CircularProgressIndicator`, `LinearProgressIndicator`, `Slider`의 Material Design 3 스타일 | `year2023: false` 또는 `ProgressIndicatorThemeData.year2023: false`, `SliderThemeData.year2023: false` | Flutter 3.29 | Material 3의 최신 디자인을 적용하려면 명시적인 설정이 필요합니다. |
| 사용 중단 | `ThemeData.dialogBackgroundColor` | `DialogThemeData.backgroundColor` | Flutter 3.29 | 테마 관련 코드를 업데이트해야 합니다. |
| 사용 중단 | `ButtonStyleButton`의 `iconAlignment` | `ButtonStyle`에 통합 | Flutter 3.29 | `ButtonStyleButton`의 `iconAlignment` 속성이 `ButtonStyle`에 통합되면서 사용 중단되었습니다. |

### Flutter 3.29/3.32 관련 사용 중단 및 제거된 기능

* 스크립트 기반 Flutter Gradle 플러그인 적용 제거: Flutter 3.19부터 사용 중단되었던 스크립트 기반 Gradle 플러그인 적용 방식이 제거되었습니다. `android/build.gradle` 및 `android/app/build.gradle` 파일을 업데이트해야 합니다.
* 웹 HTML 렌더러 제거: Flutter 웹에서 HTML 렌더러가 제거되었습니다. 이제 Flutter 웹 앱은 기본적으로 CanvasKit 또는 WASM 기반 렌더링을 사용합니다.
* 웹 이미지 처리 (`webHtmlElementStrategy` 플래그): `webHtmlElementStrategy` 플래그를 통해 웹에서 `<img>` 요소를 사용하여 이미지를 표시하는 방식을 더 세밀하게 제어할 수 있게 되었습니다.
* Dart 스레딩 변경 (Android/iOS): Flutter 3.29부터 Android 및 iOS에서 Dart 코드가 애플리케이션의 메인 스레드에서 실행되며, 별도의 UI 스레드가 더 이상 존재하지 않습니다.
* 최소 iOS 및 macOS 버전 요구사항 업데이트: Flutter는 이제 최소 배포 대상(deployment target)으로 iOS 13.0 및 macOS 10.15를 요구합니다.

### 지원 중단된 주요 패키지 목록 및 대안 (2025년 4월 30일부)

| 패키지 이름 | 지원 중단일 | 권장 대안/조치 |
|---|---|---|
| `ios_platform_images` | 2025년 4월 30일 | 커뮤니티 포크 고려, 대체 패키지 탐색 |
| `css_colors` | 2025년 4월 30일 | 커뮤니티 포크 고려, 대체 패키지 탐색 |
| `palette_generator` | 2025년 4월 30일 | 커뮤니티 포크 고려, 대체 패키지 탐색 |
| `flutter_image` | 2025년 4월 30일 | 커뮤니티 포크 고려, 대체 패키지 탐색 |
| `flutter_adaptive_scaffold` | 2025년 4월 30일 | 커뮤니티 포크 고려, 대체 패키지 탐색 |
| `flutter_markdown` | 2025년 4월 30일 | 커뮤니티 포크 고려, 대체 패키지 탐색 |

### 코딩 시 주의사항 및 권장 사항 (성능, 안정성, 보안 관련)

* Flutter 3.29 핫 리스타트 버그 (iOS): 일부 개발자는 iOS에서 핫 리스타트가 멈추는 문제를 겪었다. 최신 베타 버전을 테스트하고 버그를 조기에 보고하는 것이 좋다.
* 보안 시리즈 (OWASP Top 10 for Flutter): Majid Hajian은 OWASP Mobile Top 10을 Flutter 앱에 적용하는 보안 시리즈를 시작했다. 민감한 데이터를 다루는 앱을 개발하는 경우, 관련 시리즈를 읽어볼 것을 권장한다.
* 일반적인 Flutter & Dart 실수: Majid Hajian은 재빌드 트랩, 메모리 누수, 아키텍처적 실수 등 15가지 흔한 실수를 공유했다. `DCM`과 같은 도구를 사용하여 문제를 포착하고, 유지보수성과 성능을 개선하기 위한 전략을 적용해야 한다.
* 리팩토링의 중요성: 새로운 기능을 추가하기 전에 리팩토링을 통해 기술 부채를 줄이고, 코드 리뷰를 빠르게 하며, 향후 변경을 용이하게 하는 것이 중요하다.

---

## 향후 전망 및 개발자 생산성 향상

Flutter의 2025년 2분기 업데이트는 단순히 현재의 개선을 넘어, 미래 개발 방향에 대한 중요한 신호를 보낸다.

### AI 통합의 현재와 미래 (AI 기반 코드 분석, 차트 생성 등)

Flutter는 AI 기능 통합에 적극적으로 나서고 있다. Syncfusion Flutter 2025 Volume 2는 AI 기반 차트 생성 샘플을 제공한다. Flutter 2025 로드맵에는 AI 지원 코드 분석 및 자동 리팩토링 기능, 실시간 위젯 성능 히트맵, 그리고 Vertex AI, Firebase ML Kit 및 오픈 모델 기본 지원이 포함될 예정이다. Google I/O 2025에서도 성능 최적화와 AI 및 머신러닝 도구 통합에 중점을 두어 개발자가 더 스마트하고 직관적인 앱을 쉽게 구축할 수 있도록 강조되었다.

### 커뮤니티 활동 및 학습 자료 활용

Flutter Heroes 2025 컨퍼런스는 Flutter가 단순한 툴킷이 아니라 혁신하고 지원하는 커뮤니티임을 보여주었다. Flutter 팀은 "How Flutter Works"라는 6부작 YouTube 시리즈를 통해 Dart 코드의 런타임 라이프사이클을 심층적으로 설명하며, 개발자들이 Flutter 내부 아키텍처를 깊이 이해할 수 있도록 돕는다. 또한, `pub.dev` 랜딩 페이지에 "Trending Packages" 섹션이 추가되어 최근 주목할 만한 성장세를 보이는 패키지를 쉽게 찾을 수 있게 되었다.

---

## 결론: 성공적인 Flutter 개발을 위한 핵심 요약

2025년 2분기는 Flutter와 Dart 생태계에 있어 중요한 진보를 이룬 시기였다. Impeller의 안정화와 웹 핫 리로드의 정식 지원은 개발자 경험과 앱 성능을 한 단계 끌어올렸으며, Dart 3.8의 기능들은 코드의 간결성과 효율성을 높였다. 직접 네이티브 상호 운용성 및 AI 통합과 같은 미래 지향적인 기능들은 Flutter의 잠재력을 더욱 확장하고 있다.

성공적인 Flutter 개발을 위해서는 다음 핵심 사항들을 반드시 숙지하고 적용해야 합니다.

* 최신 버전 유지 및 마이그레이션: 사용 중단된 API, 제거된 기능, 최소 버전 요구사항 변경에 대한 지속적인 관심과 코드 업데이트가 필수적이다.
* 성능 최적화 및 Impeller 활용: Impeller의 이점을 최대한 활용하여 부드럽고 반응성 높은 UI를 구현하고, 렌더링 관련 최적화 기법을 익히는 것이 중요하다.
* Dart 언어의 새로운 기능 활용: 널 인식 컬렉션 요소, 향상된 포매터 등 Dart 3.8의 기능을 적극적으로 사용하여 코드 품질과 개발 생산성을 높이는 것이 권장된다.
* 개발자 도구 적극 활용: DevTools의 개선된 기능들을 활용하여 디버깅 및 성능 분석 효율을 극대화해야 한다.
* 보안 및 안정성 강화: OWASP 가이드라인과 흔한 실수 방지 팁을 참고하여 견고하고 안전한 앱을 만드는 데 주력해야 한다.
* AI 및 네이티브 통합에 대한 관심: AI 기반 개발 도구 및 네이티브 상호 운용성 기술의 발전을 주시하며 미래 앱 개발의 기회를 모색해야 한다.
* 커뮤니티 참여 및 학습: 활발한 커뮤니티와 공식 학습 자료를 통해 지속적으로 지식을 업데이트하고, 문제 해결에 도움을 받는 것이 중요하다.

Flutter는 끊임없이 진화하는 프레임워크이며, 이러한 변화를 이해하고 적응하는 것이 성공적인 크로스플랫폼 앱 개발의 핵심이다.
