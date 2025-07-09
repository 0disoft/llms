# Flutter 2025년 1분기 주요 업데이트 분석

## I. 서론: 2025년 Flutter의 진화와 1분기 핵심 요약

2025년은 Flutter 프레임워크가 단순한 크로스플랫폼 UI 툴킷을 넘어, 차세대 애플리케이션 개발의 핵심 동력으로 자리매김하려는 야심 찬 비전을 실행하는 원년입니다. Flutter 팀의 2025년 로드맵은 성능, 개발자 경험, 플랫폼 통합이라는 세 가지 축을 중심으로 전개됩니다.

Flutter의 2025년 비전은 다음과 같은 핵심 목표를 포함합니다:

* 성능의 비약적 도약: 임펠러(Impeller) 렌더링 엔진을 모든 주요 플랫폼의 기본 렌더러로 채택하여 셰이더 컴파일 끊김(jank) 현상을 제거합니다.
* 개발자 생산성 극대화: AI 활용 코드 분석, 실시간 위젯 성능 히트맵, 그리고 강화된 Dart 4(베타) 언어 도입을 통해 생산성을 높입니다.
* 완벽한 플랫폼 패리티: WebAssembly(WASM)를 통한 웹 성능, Apple Silicon 및 Linux ARM64 데스크톱 지원 강화를 통해 모든 플랫폼에서 일관된 고품질 경험을 제공합니다.
* AI 네이티브 앱 개발: Vertex AI, Firebase ML Kit와 같은 AI 서비스를 손쉽게 통합할 수 있는 네이티브 SDK와 위젯을 제공합니다.

이러한 거대한 비전을 실현하기 위한 첫 단추가 2025년 1분기 업데이트입니다. 이번 업데이트는 신기능 추가보다는 프레임워크의 내실을 다지고 미래를 위한 기반을 공고히 하는 '전략적 통합' 과정에 초점을 맞춥니다.

해당 기간 동안 개발자들이 주목해야 할 주요 릴리즈는 다음과 같습니다:

* Flutter 3.27.2 (2025년 1월 13일): 버그 수정에 중점을 둔 마이너 패치 릴리즈입니다.
* Flutter 3.29 및 Dart 3.7 (2025년 2월 12일): 이번 분기의 가장 핵심적인 메이저 릴리즈로, 파괴적 변경(breaking changes)과 중요한 동작 변경을 다수 포함합니다.

본 보고서는 위 릴리즈, 특히 Flutter 3.29와 Dart 3.7에 포함된 모든 중요 변경 사항을 심층적으로 분석하여 성공적인 마이그레이션 및 기능 활용 가이드를 제공합니다.

---

## II. Dart 3.7 업데이트: Flutter 개발 생산성을 좌우하는 언어 및 도구 변경점

Flutter 애플리케이션의 근간을 이루는 Dart 언어의 업데이트는 모든 Flutter 개발자에게 직접적인 영향을 미칩니다. Dart 3.7 SDK는 언어의 표현력을 높이고, 코드 스타일을 통일하며, 웹 플랫폼의 미래 방향성을 명확히 하는 중요한 변경점들을 담고 있습니다.

### 2.1. 핵심 변경: 와일드카드 변수(`_`) 도입과 잠재적 코드 충돌 해결 가이드

Dart 3.7에서 가장 주목해야 할 언어적 변화는 `_` (밑줄)의 의미가 재정의된 것입니다.

* 변경 내용: 지역 변수나 매개변수의 이름으로 `_`를 사용하면, 해당 변수는 더 이상 값을 바인딩하지 않으며 읽을 수 없는 '와일드카드'로 취급됩니다. 이는 패턴 매칭에서 `_`가 사용되던 방식과 일관성을 맞추기 위한 변경입니다.
* 변경 이유: 콜백 함수 등에서 사용하지 않는 여러 매개변수를 처리할 때 발생하던 이름 충돌 문제를 해결합니다. 이제 사용하지 않는 매개변수를 모두 `_`로 통일하여 간결하게 표현할 수 있습니다. (예: `(_, _, int importantValue) => ...`)
* 마이그레이션 가이드 (파괴적 변경): 이 변경은 기존 코드에 직접적인 영향을 미칠 수 있습니다. 만약 기존 코드에서 `_`를 변수 이름으로 선언하고 그 값을 사용했다면, Dart 3.7 환경에서는 컴파일 오류가 발생합니다.
  * 변경 전 (Dart 3.7에서 오류 발생):

        ```dart
        list.forEach((_) {
          print(_.toString()); // 오류: The getter 'toString' isn't defined for the type 'void'.
        });
        ```

  * 변경 후 (수정된 코드):

        ```dart
        list.forEach((element) {
          print(element.toString()); // 명시적인 이름으로 변경
        });
        ```

  * 이러한 문제를 쉽게 찾을 수 있도록 Dart 분석기에는 `unnecessary_underscores`라는 새로운 린트(lint) 규칙이 추가되었습니다.

### 2.2. 새로운 `dart format` 스타일 적용 및 프로젝트 단위 설정법

Dart 3.7에는 완전히 새로 작성된 코드 포매터(`dart format`)가 포함되어, 더욱 일관되고 정돈된 코드 스타일을 강제합니다.

* 변경 내용: 새로운 포매터는 인수 목록 등에서 후행 쉼표(trailing comma)를 자동으로 추가하거나 제거하여, 코드를 수직으로 정렬하는 스타일을 기본으로 채택합니다.
* 적용 방식: 이 새로운 포매팅 스타일은 `pubspec.yaml` 파일에 명시된 SDK 제약 조건에 따라 자동으로 적용됩니다. SDK 버전이 3.7 미만으로 설정된 프로젝트는 기존 스타일을 유지하며, 3.7 이상으로 설정된 프로젝트는 새로운 스타일이 적용됩니다.
* CI/CD 환경에서의 주의사항: `dart format`은 각 파일의 언어 버전을 확인하기 위해 `package_config.json` 파일을 참조합니다. 따라서 CI/CD 파이프라인과 같은 자동화 환경에서는 반드시 `dart format` 실행 전에 `dart pub get` 명령을 먼저 실행해야 합니다.
* 새로운 기능: 이제 `analysis_options.yaml` 파일에서 프로젝트 전체의 페이지 너비(page width)를 설정할 수 있으며, `// dart format off`와 `// dart format on` 주석을 사용하여 특정 코드 블록에 대한 자동 포매팅을 비활성화할 수 있습니다.

### 2.3. 지원 중단: 웹 플랫폼 라이브러리 마이그레이션 로드맵

Dart 3.7은 Flutter 웹의 미래인 WebAssembly(WASM)로의 전환을 가속화하기 위한 전략적인 결정을 내렸습니다.

* 변경 내용: 기존의 웹 관련 라이브러리 7개(`dart:html`, `dart:indexed_db`, `dart:js`, `dart:js_util`, `dart:web_audio`, `dart:web_gl`, `dart:web_sql`)가 공식적으로 지원 중단(deprecated)되었습니다.
* 변경 이유: 분편화되어 있던 웹 API를 현대적이고 WASM과 호환되는 단일 API 세트로 통합하기 위함입니다. 앞으로 모든 브라우저 API와의 상호작용은 `package:web`을, 일반적인 JavaScript와의 상호작용은 `dart:js_interop`을 사용하는 것이 권장됩니다.
* 향후 계획: 지원 중단된 라이브러리들은 2025년 말에 완전히 제거될 예정입니다. 따라서 Flutter 웹 프로젝트를 진행 중인 개발자들은 지금부터 새로운 라이브러리로의 마이그레이션을 계획하고 실행해야 합니다. 신규 웹 프로젝트는 처음부터 `package:web`과 `dart:js_interop`을 사용해야 합니다.

---

## III. Flutter 3.29: 프레임워크 핵심 변경 사항 심층 분석

Flutter 3.29 릴리즈는 프레임워크의 안정성과 장기적인 발전 가능성을 높이기 위해 여러 중요한 변경 사항을 도입했습니다.

### 3.1. v1 Android 임베딩 API 완전 제거 및 v2 마이그레이션

오랫동안 지원 중단 상태였던 레거시 v1 Android 임베딩 API가 Flutter 3.29에서 완전히 제거되었습니다. 이는 네이티브 안드로이드 코드와 상호작용하는 모든 Flutter 프로젝트에 영향을 미칠 수 있는 가장 중요한 파괴적 변경입니다.

* 변경 내용: `io.flutter.app.*` 또는 `io.flutter.view.*` 패키지에 속한 모든 클래스가 프레임워크에서 삭제되었습니다.
* 변경 이유: v1 임베딩은 성능이 낮고 API가 복잡했으며, 기존 안드로이드 앱과의 통합이 어려웠습니다. 이를 제거함으로써 Flutter 팀은 유지보수 비용을 줄이고, 모든 앱이 더 현대적이고 강력하며 안정적인 v2 임베딩을 사용하도록 보장합니다.
* 마이그레이션 가이드: 자신의 프로젝트가 영향을 받는지 확인하려면 안드로이드 관련 Java/Kotlin 코드를 검토하여 v1 클래스 임포트 구문이 있는지 확인해야 합니다.
  * 주요 v1 클래스 및 v2 대체:

        | 제거된 v1 클래스 | 새로운 v2 클래스 | 핵심 마이그레이션 노트 |
        |---|---|---|
        | `io.flutter.app.FlutterActivity` | `io.flutter.embedding.android.FlutterActivity` | 가장 일반적인 마이그레이션 대상입니다. |
        | `io.flutter.app.FlutterFragmentActivity` | `io.flutter.embedding.android.FlutterFragmentActivity` | 프래그먼트를 사용하는 경우 해당 클래스로 변경해야 합니다. |
        | `io.flutter.app.FlutterApplication` | 없음 (제거됨) | 이제 안드로이드의 기본 `android.app.Application`을 직접 상속받아야 합니다. |
        | `io.flutter.view.FlutterView` | `io.flutter.embedding.android.FlutterView` | v1의 `FlutterView`는 v2의 `FlutterView`로 대체됩니다. |
        | `io.flutter.view.FlutterMain` | `io.flutter.embedding.engine.loader.FlutterLoader` | Flutter 엔진 초기화 로직이 변경되었습니다. |
        | `io.flutter.app.FlutterPluginRegistry` | 없음 (제거됨) | v2에서는 `FlutterPlugin` 인터페이스를 통해 플러그인이 자동으로 등록됩니다. |

### 3.2. WebGoldenComparator 지원 중단과 통합 테스트 로직

Flutter 웹의 골든 파일 테스트(golden file testing) 방식이 다른 플랫폼과 통일되었습니다.

* 변경 내용: 웹 전용 골든 테스트 비교 클래스였던 `WebGoldenComparator`가 지원 중단되었습니다. 이제 웹을 포함한 모든 플랫폼의 골든 테스트는 표준 `goldenFileComparator`를 사용합니다.
* 변경 이유: 레거시 HTML 웹 렌더러 제거에 따른 직접적인 결과입니다. 모든 웹 렌더링이 `CanvasKit`과 `SkWasm`으로 통합됨에 따라 더 이상 별도의 테스트 비교 클래스가 필요 없게 되었습니다.
* 마이그레이션 가이드: 대부분의 개발자는 별도의 조치가 필요 없습니다. 하지만 `WebGoldenComparator`를 상속받아 커스텀 비교 로직을 구현했다면, 해당 로직을 `GoldenFileComparator`를 상속받는 클래스로 이전해야 합니다. 마이그레이션은 기존 `compareBytes`와 `updateBytes` 메소드의 이름을 각각 `compare`와 `update`로 변경하고 파라미터 순서를 조정하는 방식으로 이루어집니다.

### 3.3. `ThemeData.dialogBackgroundColor` 지원 중단과 `DialogTheme`으로의 이전

앱의 테마를 정의하는 방식이 더욱 모듈화되고 명시적으로 변경되었습니다.

* 변경 내용: `ThemeData.dialogBackgroundColor` 속성이 지원 중단되었습니다. 이제 다이얼로그의 배경색을 지정하려면 `ThemeData(dialogTheme: DialogTheme(backgroundColor:...))` 방식을 사용해야 합니다.
* 변경 이유: Flutter의 테마 시스템이 각 컴포넌트별 테마 객체(예: `DialogTheme`, `SliderTheme`)를 통해 스타일을 정의하도록 유도하여 테마 코드를 더 깔끔하고 발견하기 쉽게 만들어 유지보수성을 높이기 위함입니다.
* 마이그레이션 가이드: 마이그레이션은 매우 간단합니다. 기존 코드를 찾아 아래와 같이 수정하면 됩니다.
  * 변경 전:

        ```dart
        theme: ThemeData(
          dialogBackgroundColor: Colors.orange,
        ),
        ```

  * 변경 후:

        ```dart
        theme: ThemeData(
          dialogTheme: const DialogTheme(backgroundColor: Colors.orange),
        ),
        ```

### 3.4. 동작 변경: `ImageFilter.blur`의 기본 동작 변경과 시각적 영향

블러 효과를 적용하는 `ImageFilter.blur`의 기본 동작이 변경되어, 별도 설정 없이도 더 자연스러운 시각적 결과를 얻을 수 있게 되었습니다.

* 변경 내용: `ImageFilter.blur` 생성자의 `tileMode` 매개변수가 더 이상 `TileMode.clamp`를 기본값으로 갖지 않습니다. 대신 `null`을 기본값으로 하여, 렌더링 엔진이 컨텍스트에 가장 적합한 타일 모드를 자동으로 선택하도록 변경되었습니다.
* 변경 이유 및 시각적 영향: 기존 `clamp` 모드는 블러 처리된 이미지 가장자리에 부자연스러운 경계선을 유발할 수 있었습니다. 새로운 기본값은 대부분의 UI 요소에 `TileMode.decal`을, 백드롭 필터(backdrop filter)에 `TileMode.mirror`를 자동으로 적용하여 가장자리를 훨씬 부드럽고 자연스럽게 처리합니다.
* 개발자 조치: 이 변경으로 대부분 긍정적인 시각적 개선을 경험할 수 있습니다. 만약 기존 `clamp` 모드의 동작을 우회하기 위해 코드에 명시적으로 `tileMode`를 설정했다면, 해당 코드를 제거하고 엔진의 기본 동작을 따르는 것이 좋습니다.

### 3.5. 선택적 적용: Material 3 디자인 업데이트 - Slider 및 Progress Indicator

Slider와 ProgressIndicator 위젯이 최신 Material 3 디자인 명세에 맞춰 업데이트되었습니다. 하지만 기존 UI의 시각적 일관성을 해치지 않도록 이 변경은 기본적으로 비활성화되어 있습니다.

* 변경 내용: Slider, LinearProgressIndicator, CircularProgressIndicator의 모양, 트랙 간 간격, 애니메이션 등이 새로운 M3 디자인 가이드에 따라 변경되었습니다.
* 적용 방식 (Opt-in): 이 새로운 디자인은 개발자가 명시적으로 선택해야 적용됩니다. `year2023: false` 플래그를 설정하여 활성화할 수 있습니다. 이 '옵트인(opt-in)' 방식은 프레임워크 업데이트로 인해 기존 앱의 디자인이 예기치 않게 변경되는 것을 방지합니다.
* 마이그레이션 가이드: 새로운 디자인을 적용하는 방법은 두 가지입니다.
  * 개별 위젯 단위 적용:

        ```dart
        Slider(
          year2023: false, // 이 플래그를 false로 설정하여 M3 디자인 활성화
          value: _currentValue,
          onChanged: (value) { /* ... */ },
        )
        ```

  * 애플리케이션 전체 적용: `MaterialApp`의 `theme` 속성에서 컴포넌트별 테마를 설정합니다.

        ```dart
        MaterialApp(
          theme: ThemeData(
            sliderTheme: const SliderThemeData(year2023: false), // 전체 앱에 M3 Slider 적용
            progressIndicatorTheme: const ProgressIndicatorThemeData(year2023: false), // 전체 앱에 M3 Progress Indicator 적용
          ),
          //...
        )
        ```

---

## IV. 결론: 2025년 1분기 업데이트 요약 및 개발자 권장 조치

2025년 1분기의 Flutter 및 Dart 업데이트는 프레임워크의 '현대화', '통합 및 안정화', 그리고 '개발자 및 사용자 경험 향상'이라는 세 가지 핵심 주제로 요약할 수 있습니다. 레거시 기술 부채를 과감히 청산하고, 언어와 도구의 기반을 견고히 다졌으며, 더 나은 개발 패턴을 제시했습니다. 이러한 변화는 단기적으로 개발자의 마이그레이션 노력을 요구하지만, 장기적으로는 Flutter 생태계 전체를 더 강력하고 생산적인 방향으로 이끄는 필수적인 과정입니다.

모든 Flutter 개발자는 자신의 프로젝트가 최신 상태를 유지하고 안정적으로 동작하도록 다음의 권장 조치를 따르는 것이 좋습니다.

* [ ] (최우선 - 코드 건전성) `pubspec.yaml` 파일의 SDK 제약 조건을 `sdk: '>=3.7.0 <4.0.0'`으로 업데이트하고 `dart pub get` 명령을 실행합니다.
* [ ] (2순위 - 파괴적 변경) 코드베이스 전체에서 `_`를 변수처럼 사용한 부분을 검색하여 의미 있는 이름(예: `element`, `item`)으로 변경합니다.
* [ ] (3순위 - 안드로이드) 커스텀 안드로이드 코드가 있는 경우, Java/Kotlin 파일에서 `io.flutter.app.*` 또는 `io.flutter.view.*` 임포트 구문을 찾아 본 보고서의 3.1절에 제시된 마이그레이션 표를 참고하여 v2 API로 전환합니다.
* [ ] (4순위 - 테스트) 커스텀 `WebGoldenComparator`를 사용하고 있었다면, 이를 표준 `GoldenFileComparator`를 사용하도록 마이그레이션합니다.
* [ ] (5순위 - 테마) `ThemeData.dialogBackgroundColor`를 사용한 모든 부분을 찾아 `DialogTheme`을 사용하는 방식으로 수정합니다.
* [ ] (6순위 - 웹) 신규 웹 관련 개발 시에는 `package:web`과 `dart:js_interop`을 사용합니다. 기존 `dart:html` 기반 코드는 점진적인 마이그레이션 계획을 수립합니다.
* [ ] (선택 사항 - UI 개선) 앱의 UI를 검토하고, 더 현대적인 디자인을 적용하기 위해 새로운 Material 3 Slider 및 ProgressIndicator 스타일을 옵트인(`year2023: false`)하는 것을 고려합니다.

결론적으로, 2025년 1분기 업데이트는 Flutter의 성숙도를 한 단계 끌어올리는 중요한 이정표입니다. 개발자들은 이러한 변화의 흐름을 이해하고 적극적으로 대응함으로써, 올 한 해 동안 펼쳐질 Flutter의 더욱 강력하고 혁신적인 기능들을 맞이할 준비를 마칠 수 있을 것입니다.
