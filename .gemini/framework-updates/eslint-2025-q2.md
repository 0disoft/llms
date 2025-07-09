# 2025년 2분기 ESLint 및 @typescript-eslint 주요 변경사항 분석 보고서: 한국인 개발자를 위한 필수 업데이트

## 서론: 2025년 2분기 ESLint 및 @typescript-eslint 주요 변경사항 개요

2025년 2분기(3월 1일 ~ 6월 30일) 동안 ESLint와 @typescript-eslint는 개발 워크플로우와 코드 품질에 영향을 미칠 중요한 업데이트를 릴리즈했습니다. ESLint는 v9.23.0부터 v9.30.0까지의 버전을, @typescript-eslint는 v8.33.1부터 v8.35.1까지의 업데이트를 선보였습니다. 이러한 업데이트의 핵심은 TypeScript 지원 강화, 개발자 경험 개선, 설정 및 자동 수정 기능 향상, 그리고 새로운 규칙 도입에 있습니다.

---

이 분기의 주요 변경사항은 TypeScript 코드를 효율적으로 린팅하고, 설정 관리 복잡성을 줄이며, 잠재적인 런타임 오류를 사전에 방지하는 데 중점을 둡니다. 특히 TypeScript 문법 지원이 ESLint 핵심 규칙들로 대폭 확장되어 일관된 린팅이 가능해졌습니다. 자동 수정 시 규칙 충돌을 감지하여 경고를 제공하는 기능이 추가되어 디버깅이 용이해졌습니다. 새로운 `no-unassigned-vars` 규칙은 `undefined` 변수 사용 오류를 방지하며, `eslint.config.js`에서 `basePath` 속성을 사용하여 특정 디렉토리에만 설정을 적용할 수 있게 되어 대규모 프로젝트 관리가 수월해졌습니다. @typescript-eslint에서는 특정 유틸리티 함수의 사용 중단이 발표되어 코드 마이그레이션이 필요할 수 있습니다.

---

다음 표는 2025년 2분기 동안 릴리즈된 ESLint 및 @typescript-eslint의 주요 버전을 요약하고, 각 버전의 가장 중요한 변경사항을 간략하게 설명합니다. 이 표를 통해 개발자는 자신의 프로젝트에 어떤 버전이 영향을 미치는지 빠르게 식별할 수 있습니다. ESLint와 @typescript-eslint가 TypeScript 지원 강화, 설정 유연성 증대, 개발자 경험 개선이라는 명확한 방향성을 가지고 지속적으로 발전하고 있음을 파악할 수 있습니다. 특히, TypeScript 지원이 여러 릴리즈에 걸쳐 점진적으로 추가되는 패턴은 ESLint가 TypeScript 생태계를 핵심적으로 포용하려는 장기적인 전략을 보여줍니다.

| 버전                      | 릴리즈 날짜   | 주요 변경사항 요약                                           |
| :------------------------ | :------------ | :----------------------------------------------------------- |
| ESLint v9.23.0            | 2025-03-21    | TypeScript 문법 지원 확장 (3개 규칙), 충돌하는 자동 수정 감지 |
| ESLint v9.25.0            | 2025-04-18    | `no-restricted-properties`의 `allowObjects` 옵션, TypeScript 문법 지원 확장 (4개 규칙) |
| ESLint v9.26.0            | 2025-05-02    | 마이너 릴리즈, 새로운 기능 및 버그 수정                       |
| ESLint v9.27.0            | 2025-05-16    | `@eslint/mcp` 서버 별도 패키지 분리, `ESLINT_FLAGS` 환경 변수, 새 규칙 `no-unassigned-vars`, `no-array-constructor` 자동 수정 가능, TypeScript 문법 지원 확장 (2개 규칙) |
| ESLint v9.28.0            | 2025-05-30    | 마이너 릴리즈, 새로운 기능 및 버그 수정                       |
| @typescript-eslint v8.34.0 | 2025-06-09    | `getSourceFileOfNode` 함수 사용 중단(Deprecated)              |
| ESLint v9.29.0            | 2025-06-13    | 마이너 릴리즈, 새로운 기능 및 버그 수정                       |
| @typescript-eslint v8.35.0 | 2025-06-23    | `no-base-to-string` 규칙에 `checkUnknown` 옵션 추가           |
| ESLint v9.30.0            | 2025-06-27    | `basePath` 속성 도입, `v10_config_lookup_from_file` 플래그 안정화, `no-duplicate-imports`의 `allowSeparateTypeImports` 옵션 |
| @typescript-eslint v8.35.1 | 2025-06-30    | `eslint-plugin`에서 Prettier 제거                             |

---

## ESLint 핵심 기능 및 규칙 업데이트

### TypeScript 문법 지원 확장

ESLint는 2025년 2분기 동안 핵심 규칙들에 대한 **TypeScript 문법 지원을 꾸준히 확장**했습니다. 이는 TypeScript 코드를 린팅할 때 JavaScript와 동일한 엄격함과 일관성을 유지할 수 있도록 돕는 매우 중요한 변화입니다. 과거에는 TypeScript 린팅을 위해 `@typescript-eslint` 플러그인에 전적으로 의존해야 했지만, 이제 ESLint 코어 자체에서 TypeScript 문법을 이해하고 처리할 수 있는 규칙들이 늘어나고 있습니다. 이는 ESLint가 TypeScript 생태계를 적극적으로 포용하며, 사용자들에게 더 통합적이고 효율적인 린팅 경험을 제공하려는 의지를 보여줍니다. 개발자 입장에서는 `@typescript-eslint`와의 의존성을 줄이고, 코어 규칙만으로도 더 많은 TypeScript 코드를 커버할 수 있게 되어 설정 복잡성을 줄이고 린팅 성능을 개선할 여지가 생깁니다.

---

새롭게 TypeScript 문법 지원이 추가된 핵심 규칙들은 다음과 같습니다:

* **v9.23.0 (2025년 3월 21일 릴리즈)**: `class-methods-use-this`, `default-param-last`, `no-useless-constructor`.
* **v9.25.0 (2025년 4월 18일 릴리즈)**: `no-empty-function` (새로운 TypeScript 관련 옵션 포함), `no-invalid-this`, `no-loop-func`, `no-unused-expressions`.
* **v9.27.0 (2025년 5월 16일 릴리즈)**: `max-params` (새로운 `countVoidThis` 옵션 포함), `no-unassigned-vars` (새 규칙, TypeScript ambient declaration 미보고).
이러한 규칙들을 TypeScript 파일에 적용하려면 반드시 `parser` 옵션으로 `@typescript-eslint/parser` 또는 호환 가능한 파서를 설정해야 합니다.

---

| 규칙 이름                   | TypeScript 지원 추가 ESLint 버전     |
| :-------------------------- | :----------------------------------- |
| `class-methods-use-this`    | v9.23.0                              |
| `default-param-last`        | v9.23.0                              |
| `no-useless-constructor`    | v9.23.0                              |
| `no-empty-function`         | v9.25.0 (새로운 TS-specific 옵션 포함) |
| `no-invalid-this`           | v9.25.0                              |
| `no-loop-func`              | v9.25.0                              |
| `no-unused-expressions`     | v9.25.0                              |
| `max-params`                | v9.27.0 (새로운 `countVoidThis` 옵션 포함) |
| `no-unassigned-vars`        | v9.27.0 (새 규칙, TS ambient declaration 미보고) |

---

### 새로운 규칙 및 기존 규칙 개선

ESLint는 2025년 2분기에 새로운 규칙을 도입하고 기존 규칙에 유용한 옵션 및 기능을 추가하여 린팅의 정확성과 유연성을 높였습니다. **`no-unassigned-vars` 규칙 도입 (ESLint v9.27.0)**은 `let` 또는 `var`로 선언되었으나 값이 할당되지 않은 채 코드에서 읽히는 변수를 보고하여 잠재적인 프로그래밍 오류를 방지합니다. **`no-restricted-properties` 규칙의 `allowObjects` 옵션 추가 (ESLint v9.25.0)**는 특정 속성을 전역적으로 제한하면서도 특정 객체에 대해서는 해당 속성 사용을 허용할 수 있게 합니다.

---

**`no-useless-escape` 규칙의 `allowRegexCharacters` 옵션 추가 (ESLint v9.27.0)**는 정규 표현식 리터럴 내에서 특정 문자에 대한 불필요한 이스케이프를 허용할 수 있게 합니다. **`no-array-constructor` 규칙의 자동 수정(autofix) 기능 추가 (ESLint v9.27.0)**는 대부분의 문제를 자동으로 수정하여 개발자의 수동 작업 부담을 줄여 생산성을 높입니다. **`no-duplicate-imports` 규칙의 `allowSeparateTypeImports` 옵션 추가 (ESLint v9.30.0)**는 동일한 모듈에서 값 임포트와 타입 임포트를 분리하여 사용하는 경우 중복으로 보고하지 않도록 설정할 수 있게 하여 TypeScript 프로젝트에서 임포트 관리를 더욱 유연하게 만듭니다.

---

**CSS 린팅 `use-baseline` 규칙 개선 (2025년 4월)**은 Baseline 연도 지정, 선택자 지원, 그리고 `color-mix`, `conic-gradient` 등 더 많은 CSS 함수에 대한 지원을 포함하여 ESLint의 CSS 린팅 기능을 강화했습니다. 이 규칙은 기본적으로 경고를 발생시키며, 오류를 발생시키려면 명시적으로 설정해야 합니다. 이러한 개선은 개발자가 ESLint를 통해 더 안전하고, 더 현대적이며, 더 표준을 준수하는 코드를 작성할 수 있도록 돕습니다.

---

### 충돌하는 자동 수정 감지 기능 (ESLint v9.23.0)

`--fix` 옵션을 사용하여 ESLint를 실행할 때, 두 개 이상의 규칙이 코드의 특정 부분에 대해 서로 다른 수정 방식을 제안하는 경우, **ESLint v9.23.0부터는 이러한 규칙 충돌을 감지하고 명확한 경고를 발생**시킵니다. 이 기능은 단순한 오류 보고를 넘어선 개발자 경험(DX) 개선에 중점을 둔 업데이트입니다. 명확한 경고 메시지를 통해 ESLint는 '왜' 자동 수정이 실패했는지 알려줌으로써, 개발자가 문제를 능동적으로 해결하고 린팅 설정을 최적화할 수 있도록 돕습니다. 이는 린팅 프로세스의 투명성을 높이고, 개발자가 린팅 도구를 더 효과적으로 활용하도록 유도합니다.

개발자가 취해야 할 조치: 충돌 경고가 발생하면, 해당 규칙들의 설정을 검토하여 하나의 일관된 스타일을 따르도록 조정해야 합니다. 예를 들어, `@stylistic/semi`와 `@stylistic/js/semi`가 충돌한다면, 둘 중 하나를 비활성화하거나 동일한 설정으로 맞춰야 합니다. 이러한 개선을 통해 개발자는 자동 수정 실패에 대한 불필요한 디버깅 시간을 줄일 수 있으며, 린팅 설정의 일관성을 유지하는 데 도움이 됩니다. 이는 대규모 프로젝트나 여러 개발자가 참여하는 환경에서 린팅 규칙 관리의 효율성을 크게 향상시킬 것입니다.

| 규칙 이름                    | 변경 내용                                             | 개발자에게 미치는 영향                                       |
| :--------------------------- | :---------------------------------------------------- | :----------------------------------------------------------- |
| `no-unassigned-vars`         | 새 규칙 도입                                          | `let`/`var`로 선언 후 미할당 변수 사용 시 오류 보고. 런타임 `undefined` 오류 방지. |
| `no-restricted-properties`   | `allowObjects` 옵션 추가                              | 특정 객체에 대한 속성 사용 허용. 유연한 규칙 적용 가능.     |
| `no-useless-escape`          | `allowRegexCharacters` 옵션 추가                      | 정규식 내 불필요한 이스케이프 허용. 특정 패턴 유지 가능.     |
| `no-array-constructor`       | 자동 수정 기능 추가                                   | 대부분의 문제 자동 수정 가능. 코드 수정 시간 단축, 생산성 향상. |
| `no-duplicate-imports`       | `allowSeparateTypeImports` 옵션 추가                  | TypeScript `import type`과 일반 `import` 분리 허용. TS 임포트 관리 유연성 증대. |
| `use-baseline` (CSS 린팅)    | Baseline 연도 지정, 선택자 지원, 더 많은 CSS 함수 지원 | 최신 웹 표준 준수 및 브라우저 호환성 강화.                 |

---

## ESLint 설정 및 환경 관련 변경사항

### `basePath` 속성을 통한 설정 객체 적용 범위 지정 (ESLint v9.30.0)

ESLint v9.30.0부터 설정 객체에 새로운 **`basePath` 속성을 포함**할 수 있게 되었습니다. 이 속성은 해당 설정 객체가 적용될 하위 디렉토리의 경로를 지정합니다. `files` 및 `ignores` 속성에 지정된 패턴은 이제 `basePath`로 지정된 디렉토리를 기준으로 평가됩니다. `basePath`의 도입은 이전에는 `files` 및 `ignores` 패턴이 프로젝트 루트를 기준으로만 작동하여, 특정 하위 프로젝트에만 적용되는 복잡한 린팅 규칙을 관리하기 어려웠던 문제를 해결합니다. 이는 모노레포나 복잡한 프로젝트 구조에서 특정 디렉토리(예: `packages/my-feature` 또는 `src/client`)에만 특정한 린팅 설정을 적용해야 할 때 매우 유용합니다.

---

### `ESLINT_FLAGS` 환경 변수를 통한 기능 플래그 설정 (ESLint v9.27.0)

이제 ESLint의 실험적 기능 플래그(feature flags)를 **`ESLINT_FLAGS` 환경 변수를 통해 설정**할 수 있습니다. 여러 플래그는 쉼표로 구분하여 지정합니다 (예: `export ESLINT_FLAGS="unstable_config_lookup_from_file,unstable_native_nodejs_ts_config"`). 이 변화는 ESLint가 엔터프라이즈급 사용 사례, 특히 자동화된 빌드 및 배포 환경(CI/CD)에서의 통합을 고려하고 있음을 보여줍니다. 환경 변수를 통한 플래그 관리는 코드베이스 내 설정 파일을 변경하지 않고도 빌드 환경에 따라 다른 동작을 유도할 수 있게 하여 CI/CD 파이프라인에서 ESLint의 동작을 더욱 유연하고 안전하게 제어할 수 있게 합니다.

---

### `eslint-suppressions.json` 파일 정렬 (ESLint v9.27.0)

린팅 억제(suppression) 정보를 담는 **`eslint-suppressions.json` 파일 내의 객체 키가 이제 자동으로 정렬**됩니다. 이 작은 변화는 개발자 생산성과 협업 효율성에 큰 영향을 미칩니다. 불필요한 `diff` 노이즈는 코드 리뷰의 피로도를 높이고, 실제 변경 사항을 파악하기 어렵게 만듭니다. `eslint-suppressions.json`과 같은 자동 생성되거나 빈번하게 수정되는 파일에서 불필요한 `diff`를 제거하는 것은, 개발자가 핵심적인 코드 변경에 집중할 수 있도록 돕습니다. 결과적으로 코드 리뷰가 더 효율적이 되고, 버전 관리 시스템에서의 충돌 가능성이 줄어들어 개발 팀의 협업이 개선됩니다.

---

### `@eslint/mcp` 서버의 별도 패키지 분리 (ESLint v9.27.0)

ESLint v9.26.0에서 코어에 추가되었던 MCP (Microsoft Code Platform) 서버가 ESLint v9.27.0부터 **`@eslint/mcp`라는 별도의 패키지로 추출**되었습니다. 이 변화는 ESLint 프로젝트의 모듈화 전략과 성능 최적화에 대한 의지를 보여줍니다. 모든 사용자가 MCP 서버를 필요로 하는 것은 아니므로, 이를 코어에서 분리함으로써 ESLint의 기본 설치 패키지 크기를 줄이고, 불필요한 의존성을 제거할 수 있습니다. MCP 서버는 이제 `npx @eslint/mcp@latest` 명령을 통해 시작할 수 있으며, 기존의 `--mcp` ESLint CLI 플래그는 내부적으로 이 새 명령을 실행하도록 유지됩니다. 이는 설치 시간 단축, 디스크 공간 절약, 그리고 잠재적으로 런타임 성능 개선으로 이어질 수 있습니다.

---

### 안정화된 `v10_config_lookup_from_file` 플래그 (ESLint v9.30.0)

ESLint v9.12.0에서 도입된 실험적인 설정 파일(`eslint.config.js`) 해석 방식이 `basePath` 속성 추가와 함께 최종적으로 **안정화되었습니다**. 이에 따라 `unstable_config_lookup_from_file` 플래그의 이름이 `v10_config_lookup_from_file`로 변경되었습니다. 이 플래그의 안정화 및 이름 변경은 ESLint가 **`flat config` (새로운 설정 방식)를 미래의 표준으로 확고히 정립**하고 있음을 나타내는 중요한 변화입니다. `v10_config_lookup_from_file`이라는 이름은 다음 메이저 버전인 ESLint v10에서 이 방식이 기본이 될 것임을 명시적으로 예고합니다. 개발자들은 기존 `.eslintrc.*` 방식에서 `eslint.config.js` 기반의 `flat config`로 전환을 준비해야 함을 의미합니다.

---

## @typescript-eslint 플러그인 업데이트

@typescript-eslint 플러그인 또한 2025년 2분기 동안 TypeScript 개발자를 위한 중요한 업데이트를 릴리즈했습니다. 이 업데이트들은 TypeScript 코드의 린팅 정확도를 높이고, 특정 사용 패턴에 대한 유연성을 제공하며, 라이브러리 내부의 개선을 통해 전반적인 안정성을 강화하는 데 초점을 맞추고 있습니다.

---

### 주요 기능 및 개선사항

**`no-base-to-string` 규칙에 `checkUnknown` 옵션 추가 (v8.35.0, 2025년 6월 23일)**: 이 옵션은 `unknown` 타입의 값에 대해 `toString()` 메서드 사용을 검사할 수 있도록 하여, 잠재적인 런타임 오류를 방지하는 데 도움을 줍니다. **`eslint-plugin`에서 Prettier 제거 (v8.35.1, 2025년 6월 30일)**: `eslint-plugin` 패키지에서 Prettier 관련 의존성이 제거되었습니다. 이는 각 도구의 책임 영역을 명확히 하고, 불필요한 의존성을 줄여 패키지 경량화에 기여합니다. **EcmaVersion에 2026/17 추가 (v8.34.1, 2025년 6월 16일)**: TypeScript 파싱 엔진이 최신 ECMAScript 버전 지원을 업데이트하여 미래의 JavaScript 문법에 대한 호환성을 강화했습니다.

---

### `getSourceFileOfNode` 함수 Deprecation (사용 중단 권고) (v8.34.0, 2025년 6월 9일)

`type-utils` 내의 `getSourceFileOfNode` 함수가 **사용 중단(deprecated)되었습니다**. 라이브러리에서 특정 API를 `deprecated`하는 것은 해당 API가 더 이상 권장되지 않거나, 더 나은 대안이 있거나, 내부 구조 변경으로 인해 유지보수가 어려워졌음을 의미합니다. 이는 `@typescript-eslint`가 내부적으로 코드 베이스를 개선하고, 더 효율적이거나 안전한 API 디자인으로 전환하고 있음을 시사합니다. 이는 해당 함수를 직접 사용하던 플러그인 개발자나 고급 사용자에게 영향을 미치며, 대체 방법을 찾아 코드를 업데이트해야 할 수 있습니다.

---

## 개발자를 위한 실질적인 조언 및 권장사항

2025년 2분기 ESLint 및 @typescript-eslint의 업데이트는 코드 품질 향상과 개발 효율성 증대를 위한 중요한 기회를 제공합니다. 이러한 변화를 효과적으로 활용하고 잠재적인 문제를 방지하기 위해 다음과 같은 실질적인 조언과 권장사항을 따르는 것이 중요합니다.

---

### 업데이트 적용 전후 고려사항 및 테스트 방법

* **점진적 업데이트**: 프로덕션 환경에 바로 적용하기 전에 개발/스테이징 환경에서 충분히 테스트하는 것이 필수적입니다.
* **의존성 업데이트**: ESLint 및 @typescript-eslint 패키지를 최신 버전으로 업데이트한 후, `npm install` 또는 `yarn install`과 같은 패키지 관리자 명령을 다시 실행하여 모든 의존성이 올바르게 설치되었는지 확인하십시오.
* **린팅 실행 및 경고/오류 확인**: 업데이트 후 `npx eslint` 또는 `npm run lint` 명령을 실행하여 새로운 경고나 오류가 발생하는지 면밀히 확인하십시오. 특히 `--fix` 옵션을 사용하여 자동 수정 기능이 올바르게 작동하는지 테스트하고, 새로운 "충돌하는 자동 수정 감지" 경고가 발생하는지 주시하십시오.
* **버전 관리 시스템 활용**: 업데이트 전 현재 린팅 설정을 버전 관리 시스템에 커밋하여, 문제가 발생할 경우 쉽게 롤백할 수 있도록 준비하십시오. 이는 안전한 업데이트 경로를 확보하는 가장 기본적인 방법입니다.

---

### 새로운 기능 및 규칙을 활용한 코드 품질 향상 전략

* **TypeScript 지원 규칙 활성화**: TypeScript 코드를 린팅하는 경우, 새로 추가된 TypeScript 지원 규칙들을 활성화하여 코드의 안정성과 일관성을 높이십시오. 특히 `no-unassigned-vars`와 같이 잠재적 런타임 오류를 방지하는 규칙은 적극적으로 활용하는 것이 좋습니다.
* **`basePath` 활용**: 모노레포나 복잡한 프로젝트 구조를 가진 경우, `basePath` 속성을 사용하여 각 서브 프로젝트에 특화된 린팅 설정을 적용함으로써 설정을 간소화하고 관리 효율성을 높이십시오.
* **`ESLINT_FLAGS` 환경 변수**: CI/CD 환경에서 특정 기능 플래그를 일관되게 적용해야 할 경우, `ESLINT_FLAGS` 환경 변수를 활용하여 설정 관리의 유연성을 확보하십시오.
* **`no-duplicate-imports`의 `allowSeparateTypeImports`**: TypeScript 프로젝트에서 `import type`과 일반 `import`를 분리하여 사용하는 경우, 이 옵션을 활성화하여 불필요한 린팅 오류를 방지하고 코드 가독성을 유지하십시오.
* **CSS `use-baseline` 규칙**: 웹 컴포넌트나 프론트엔드 개발 시, `use-baseline` 규칙을 활용하여 최신 CSS 기능의 브라우저 호환성을 사전에 확인하고 표준을 준수하는 코드를 작성하십시오.

---

### 향후 ESLint 버전 변화에 대한 준비

**`v10_config_lookup_from_file` 플래그의 안정화는 ESLint v10에서 `flat config`가 기본 설정 방식으로 채택될 것임을 의미**합니다. 기존 `.eslintrc.*` 기반 설정을 사용하는 프로젝트는 점진적으로 `eslint.config.js` 기반의 `flat config`로 전환을 준비하는 것이 좋습니다. ESLint 공식 문서에서 제공하는 마이그레이션 가이드를 참고하여 미리 대비하십시오. `flat config`는 기존 설정 방식의 한계를 극복하고 더 강력하고 예측 가능한 설정 환경을 제공하므로, 미리 학습하고 적용 계획을 세우는 것이 장기적인 프로젝트 유지보수에 큰 도움이 될 것입니다.

---

## 결론

2025년 2분기 ESLint 및 @typescript-eslint의 업데이트는 TypeScript 지원 강화, 개발자 경험 개선, 그리고 설정 관리의 유연성 증대라는 명확한 방향성을 보여줍니다. 특히 **TypeScript 코어 규칙 지원 확장, 충돌하는 자동 수정 감지, 새로운 `no-unassigned-vars` 규칙 도입, 그리고 `basePath`를 통한 설정 관리 개선**은 개발자들이 더욱 효율적이고 안정적으로 코드를 관리할 수 있도록 돕는 중요한 변화입니다. 개발자들은 이러한 변경사항들을 숙지하고, 자신의 프로젝트에 맞게 ESLint 설정을 업데이트하며, 새로운 기능을 적극적으로 활용하여 코드 품질을 향상시킬 것을 권장합니다. 특히 ESLint v10에서 `flat config`가 기본 설정 방식으로 채택될 예정이므로, 이에 대한 사전 준비는 향후 원활한 전환을 위해 필수적입니다. 지속적인 ESLint 업데이트 적용은 현대 웹 개발 환경에서 코드의 일관성, 안정성, 그리고 유지보수성을 확보하는 데 핵심적인 역할을 할 것입니다.
