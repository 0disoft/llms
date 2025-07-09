# Flame 엔진 2025년 2분기 주요 업데이트 분석

## 서론

본 보고서는 2025년 3월 1일부터 6월 30일까지 릴리즈된 Flame 엔진의 모든 패치노트를 심층 분석하여, 한국인 개발자들이 코딩 시 반드시 알아야 할 중요한 변경사항을 명확하고 이해하기 쉬운 언어로 정리하여 제공합니다. 특히 기존 코드의 작동 방식에 영향을 미치는 호환성 파괴 변경(Breaking Changes)과 함께 주요 기능 개선 및 버그 수정 사항들을 중점적으로 다룹니다.

### 조사 대상 Flame 버전

2025년 2분기 동안 Flame 엔진은 총 8개의 버전이 릴리즈되었습니다.

* Flame v1.30.0: 2025년 6월 27일 릴리즈
* Flame v1.29.0: 2025년 5월 20일 릴리즈
* Flame v1.28.1: 2025년 5월 1일경 릴리즈
* Flame v1.28.0: 2025년 5월 1일경 릴리즈
* Flame v1.27.0: 2025년 4월 1일경 릴리즈
* Flame v1.26.1: 2025년 4월 1일경 릴리즈
* Flame v1.26.0: 2025년 4월 1일경 릴리즈
* Flame v1.25.0: 2025년 3월 1일경 릴리즈

이러한 빠른 릴리즈 주기는 Flame 엔진의 기능 확장 및 버그 수정 속도를 높이는 긍정적인 효과를 가져오지만, 동시에 기존 코드와의 호환성을 깨는 변경(breaking changes)이 발생할 가능성도 높아집니다.

### 주요 변경사항 요약

2025년 2분기 Flame 엔진 업데이트는 다음과 같은 주요 경향을 보입니다.

* 핵심 시스템 개선 및 호환성 파괴 변경: `Vector2`의 32비트 전환, 컴포넌트 계층 구조 및 생명 주기 관리 방식 변경(`parent` 참조 유지), `HasGameRef`의 `HasGameReference`로의 변경 등 엔진의 근간을 이루는 부분에서 중요한 호환성 파괴 변경이 있었습니다.
* 고급 렌더링 및 물리 엔진 통합 강화: Post Process API, Render Context API 도입 및 `flutter_shaders`와의 연동 강화를 통해 보다 복잡하고 시각적으로 풍부한 게임 구현을 위한 기반이 마련되었습니다.
* 개발 편의성 및 생산성 향상: Layout Components와 같은 UI/레이아웃 관리 기능, `FunctionEffect`와 같은 유연한 이펙트 추가, `TimerComponent` 및 `SpriteAnimation` 개선 등 개발자들이 게임 로직을 더 쉽고 효율적으로 구현할 수 있도록 돕는 기능들이 다수 추가되었습니다.
* 코드 품질 및 문서화 노력: 새로운 린트 패키지 적용을 통한 코드 스타일 및 품질 향상, 문서 구조 개선 및 특정 API 동작에 대한 명확화 등 코드 품질 유지 및 개발자 이해도 향상을 위한 노력이 지속되었습니다.

표: 2025년 2분기 Flame 엔진 주요 호환성 파괴 변경사항

| 버전 | 변경사항 설명 | 영향 | 개발자 조치 |
|---|---|---|---|
| v1.30.0 | `testGolden` 함수의 `prepare` 인자로 `WidgetTester` 전달 방식 변경 | 기존 골든 테스트 코드 컴파일 오류 발생 가능성. | `prepare` 콜백 시그니처를 `WidgetTester`를 받도록 수정. |
| v1.29.0 | 부모 제거 후에도 자식 컴포넌트가 `parent` 참조를 유지하도록 변경 | `onRemove` 등에서 `parent`가 `null`이 될 것을 가정한 기존 로직에 예상치 못한 동작 발생 가능성. | `onRemove` 콜백 등에서 `parent` 참조 로직 검토 및 수정. |
| v1.28.0 | 셰이더 및 Forge2D 호환성을 위해 Flame에서 32비트 `Vector2` 사용 | `Vector2`를 직접 조작하거나 Forge2D와 연동하는 코드에서 데이터 정밀도 및 메모리 사용 방식에 미묘한 차이 발생 가능성. | `Vector2` 관련 연산 및 외부 라이브러리 연동 코드 테스트 및 필요시 수정. |
| v1.28.0 | `HasGameRef` 믹스인을 `HasGameReference` 믹스인으로 사용하도록 Deprecate | `HasGameRef`를 사용하던 모든 컴포넌트에서 경고 발생 및 향후 버전에서 작동하지 않을 수 있음. | `HasGameRef`를 `HasGameReference`로 변경. |

---

## 버전별 상세 패치노트

이 섹션에서는 2025년 2분기 동안 릴리즈된 각 Flame 버전의 주요 변경사항을 상세히 설명합니다.

### Flame v1.30.0 (2025년 6월 27일 릴리즈)

Flame v1.30.0은 골든 테스트 방식의 중요한 변경과 함께 렌더링 정확도 및 컴포넌트 스폰 기능에 대한 개선이 이루어졌습니다.

* 호환성 파괴 기능 (BREAKING FEAT): `testGolden` 함수의 `prepare` 인자로 `WidgetTester` 전달 방식 변경.
* 수정사항 (FIX):
  * `angleTo`, `lookAt`, `absoluteAngle`, `angle` 세터가 부모 컴포넌트의 변환(회전 등)을 올바르게 고려하고, 각도 값을 `[-pi, pi]` 범위로 정규화하여 반환하도록 수정.
  * `Delay` 이펙트가 `SpeedEffectControllers`와 함께 사용될 때 올바르게 작동하지 않던 버그 수정.
  * 컴포넌트 제거 시 `remove` 메서드에 의도된 부모를 명시적으로 전달하도록 개선.
  * `super.onDispose` 호출 시점을 컴포넌트 생명 주기 마지막으로 조정하고, `setState` 호출 전에 `mounted` 상태를 확인하는 로직이 추가.
  * 32비트 벡터를 사용하여 각진 선 교차 계산이 올바르게 작동하도록 수정.
  * `PostProcessComponent`가 동적으로 크기를 조정하도록 수정.
* 새로운 기능 (FEAT):
  * `SpawnComponent`에 `target` 인자가 추가.
  * `SpawnComponent`에 `spawnCount` 인자가 추가.
  * `RasterSpriteComponent.fromImage` 생성자가 추가.
  * 스프라이트 렌더링 시 발생할 수 있는 고스트 라인(ghost lines) 및 그래픽 아티팩트(graphical artifacts) 문제를 해결하기 위한 `measure` 기능이 구현.

### Flame v1.29.0 (2025년 5월 20일 릴리즈)

Flame v1.29.0은 컴포넌트 계층 구조 관리 방식의 중요한 변화와 함께 내부 데이터 무결성 및 문서화 개선에 중점을 두었습니다.

* 호환성 파괴 기능 (BREAKING FEAT): 부모가 컴포넌트 트리에서 제거된 후에도 자식 컴포넌트가 `parent` 참조를 유지하도록 변경.
* 수정사항 (FIX):
  * `component.children`에서 `ReadOnlyOrderedset`만 노출하도록 수정.
  * 후처리(postprocess) 단계에서 그림(picture)이 올바르게 해제되지 않던 문제 수정.
  * 항목 제거 전에 후처리 목록을 구체화(materialize)하도록 수정.
* 문서화 (DOCS): 문서 구조가 업데이트되고 `RowComponent` 및 `ColumnComponent`에 대한 문서가 추가.

### Flame v1.28.1 (2025년 5월 1일경 릴리즈)

Flame v1.28.1은 플랫폼 호환성 개선과 렌더링 정확도 향상, 그리고 오버레이 제어 기능 추가에 초점을 맞춘 유지보수 릴리즈입니다.

* 리팩토링 (REFACTOR): `dart:io` 사용을 `defaultTargetPlatform`으로 대체.
* 수정사항 (FIX):
  * `flutter_shaders`에서 프래그먼트 셰이더(fragment shader) 확장이 추가.
  * `Viewport`의 자식 컴포넌트에서 `onParentResize` 콜백을 호출할 때 `virtualSize`를 사용하도록 수정.
* 새로운 기능 (FEAT): 오버레이(`overlays`)를 토글하는 메서드가 추가.

### Flame v1.28.0 (2025년 5월 1일경 릴리즈)

Flame v1.28.0은 엔진의 근간을 이루는 중요한 호환성 파괴 변경과 함께 고급 렌더링 및 물리 엔진 통합을 위한 기반을 마련하는 데 집중했습니다.

* 호환성 파괴 수정 (BREAKING FIX): 셰이더 및 Forge2D와의 호환성을 위해 Flame에서 32비트 `Vector2` 사용.
* 수정사항 (FIX):
  * 컴포넌트의 `Priority` 변경이 즉시 렌더링/업데이트 순서에 반영되지 않던 문제 수정.
  * `HasGameRef` 믹스인을 `HasGameReference` 믹스인으로 사용하도록 Deprecate.
  * `SpriteButton`의 `label` 속성이 `nullable`하도록 수정.
  * 내부 컴포넌트 관리 방식의 일부로 `ordered_set`이 업데이트되고 `ComponentSet`이 제거.
* 새로운 기능 (FEAT):
  * Post Process API가 추가.
  * `LineSegment`에 헬퍼 메서드(`translate`, `inflate`, `deflate`, `spread`)가 추가.
  * `SpriteButton`의 비활성화(`disabled`) 상태가 지원.
  * Render Context API가 추가.
  * `TimerComponent`에 `tickCount` 속성이 추가.
  * `SpriteAnimation` 컴포넌트가 제거될 때 애니메이션을 초기 상태로 리셋하는 기능이 추가.

### Flame v1.27.0 (2025년 4월 1일경 릴리즈)

Flame v1.27.0은 코드베이스 정리, 품질 개선, 그리고 이펙트 시스템의 유연성 확장에 초점을 맞추었습니다.

* 수정사항 (FIX): 오래된 Deprecation 경고가 코드베이스에서 제거.
* 새로운 기능 (FEAT):
  * 새로운 린트(lint) 패키지로 업데이트.
  * `FunctionEffect`가 추가되어, 임의의 함수를 Flame의 이펙트로 실행할 수 있게 됨.

### Flame v1.26.1 (2025년 4월 1일경 릴리즈)

Flame v1.26.1은 엔진의 안정성 향상과 내부 컴포넌트 관리의 정확성을 높이는 데 중점을 둔 패치 릴리즈입니다.

* 수정사항 (FIX):
  * 컴포넌트의 `ordered_set`에서 동시 변이(concurrent mutation)를 유발하던 우선순위 재조정 버그 수정.
  * 이벤트 디스패처 클래스들이 노출.

### Flame v1.26.0 (2025년 4월 1일경 릴리즈)

Flame v1.26.0은 UI 및 텍스트 렌더링 기능 강화, 그리고 새로운 이펙트 추가를 통해 개발 편의성을 높이는 데 기여했습니다.

* 수정사항 (FIX): `RouterComponent`가 다른 컴포넌트 위에 올바르게 렌더링되지 않던 문제 수정.
* 새로운 기능 (FEAT):
  * 텍스트 렌더링 파이프라인에서 여러 스타일을 동시에 적용할 수 있는 사용자 정의 속성 구문이 지원.
  * 레이아웃 축소(`Layout shrinkwrap`) 및 레이아웃 컴포넌트(`Layout Components`)가 추가.
  * `RotateAroundEffect`가 추가.
* 문서화 (DOCS): `InlineTextNode` 문서의 누락된 참조 수정, `onRemove()` 동작에 대한 API 문서 명확화.

### Flame v1.25.0 (2025년 3월 1일경 릴리즈)

Flame v1.25.0은 텍스트 컴포넌트의 유연성을 향상시키는 데 중점을 두었습니다.

* 새로운 기능 (FEAT): `TextBoxComponent.boxConfig`에 대한 `setter`가 추가되었고, 문자 단위 빌드업을 건너뛸 수 있는 편의 메서드가 추가.

---

## 결론 및 권장사항

2025년 2분기 동안 Flame 게임 엔진은 `v1.25.0`부터 `v1.30.0`까지 총 8개의 버전이 릴리즈되는 등 매우 활발한 개발 활동을 보였습니다. 이러한 빠른 릴리즈 주기는 엔진의 기능 개선과 버그 수정이 신속하게 이루어지고 있음을 나타내며, 개발자들이 최신 기능을 활용하여 더 풍부한 게임을 만들 수 있는 기회를 제공합니다.

그러나 이 기간 동안 `Vector2`의 32비트 전환, `HasGameRef`의 `HasGameReference`로의 변경, 그리고 컴포넌트의 `parent` 참조 유지 방식 변경 등 엔진의 핵심 부분에서 여러 호환성 파괴 변경이 발생했습니다. 이러한 변경사항들은 기존 Flame 프로젝트를 최신 버전으로 업데이트할 때 개발자가 반드시 코드를 검토하고 수정해야 함을 의미합니다.

긍정적인 측면에서는 Post Process API와 Render Context API의 도입을 통해 Flame이 고급 렌더링 기능을 강화하고 있음을 확인할 수 있습니다. 또한 Layout Components와 `FunctionEffect` 같은 기능들은 게임 내 UI 구성 및 동적인 로직 구현의 편의성을 크게 향상시켜 개발 생산성을 높이는 데 기여할 것입니다.

Flame 개발자들에게 다음과 같은 권장사항을 제시합니다.

* 정기적인 패치노트 검토 및 업데이트 계획 수립: Flame 엔진은 빠르게 발전하고 있으므로, 최신 버전의 이점을 활용하고 누적된 호환성 파괴 변경으로 인한 부담을 줄이기 위해 정기적으로 패치노트를 검토하고 업데이트 계획을 세우는 것이 중요합니다.
* 호환성 파괴 변경에 대한 선제적 대응: `Vector2` 타입 변경, `HasGameRef` 믹스인 교체 등 주요 호환성 파괴 변경사항에 대해서는 업데이트 전에 해당 변경이 프로젝트에 미칠 영향을 면밀히 분석하고, 필요한 코드 수정 작업을 미리 계획해야 합니다.
* 새로운 기능 적극 활용: Post Process API, Layout Components, `FunctionEffect` 등 새롭게 추가된 강력한 기능들을 게임 개발에 적극적으로 활용하여 시각적 품질과 개발 편의성을 높이는 것을 고려해야 합니다.
* 테스트 프로세스 강화: 특히 메이저 업데이트 시에는 자동화된 테스트(단위 테스트, 통합 테스트, 골든 테스트 등)를 충분히 수행하여 변경사항으로 인한 예상치 못한 버그나 동작 오류를 사전에 발견하고 수정해야 합니다.

이러한 접근 방식을 통해 개발자들은 Flame 엔진의 지속적인 발전을 효과적으로 활용하면서도, 안정적이고 고품질의 게임을 지속적으로 개발할 수 있을 것입니다.
