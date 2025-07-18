# SvelteKit 25H1 핵심 변경사항

## 서론

### 개요: 2025년 상반기 SvelteKit 변경사항 보고서 목적

본 보고서는 2025년 상반기(1월~6월) 동안 프레임워크 SvelteKit에 도입된 주요 변경사항들을 심층적으로 분석하고 정리합니다. 특히 개발자의 코딩 구현 방식에 직접적인 영향을 미치는 핵심 업데이트에 초점을 맞춰, SvelteKit을 활용하는 개발팀이 변화를 효과적으로 이해하고 적용할 수 있도록 실질적인 정보를 제공하는 것을 목표로 합니다.

SvelteKit은 Svelte 5의 혁신적인 반응성 모델인 'Runes'를 기반으로 빠르게 발전하고 있으며, 2025년 상반기는 SvelteKit이 더욱 강력하고 유연한 풀스택 웹 프레임워크로 진화하는 중요한 시기였습니다. Svelte 5는 이미 2024년 12월부터 많은 UI 라이브러리에서 지원되기 시작했으며, 2025년 1월에도 지속적인 개선이 이루어졌습니다.

### SvelteKit의 지속적인 발전 방향

2025년 상반기 SvelteKit의 변화는 크게 세 가지 핵심 방향성을 보여줍니다.

1. **Svelte 5의 안정화 및 기능 확장:** 새로운 반응성 프리미티브와 구문 도입을 통해 개발자가 더욱 효율적이고 직관적으로 코드를 작성할 수 있도록 지원합니다.
2. **빌드 및 런타임 성능 최적화:** 애플리케이션의 로딩 속도와 전반적인 사용자 경험을 향상시키는 데 기여합니다.
3. **개발자 경험(DX) 개선:** 개발 도구와 언어 서비스의 발전을 통해 개발 과정의 생산성과 편의성을 증대시키는 데 중점을 둡니다.

이러한 변화들은 SvelteKit이 단순한 UI 라이브러리를 넘어, 현대 웹 애플리케이션 개발을 위한 포괄적인 풀스택 솔루션으로서의 입지를 더욱 공고히 하고 있음을 보여줍니다.

## 핵심 변경사항

### Svelte Core 및 언어 도구 개선

Svelte 코어 및 관련 언어 도구의 발전은 개발자가 Svelte 애플리케이션을 작성하고 디버깅하는 방식에 직접적인 영향을 미칩니다.

#### Attachment 도입 및 Actions 대체

2025년 6월, Svelte는 'Attachments'라는 새로운 기능을 도입했습니다. `Attachments`는 기존의 `Actions`보다 더 유연하고 현대적인 버전으로 설계되었습니다. 이는 컴포넌트와 DOM 요소 간의 상호작용 방식을 개선하여, 개발자가 더욱 강력하고 재사용 가능한 UI 로직을 구현할 수 있도록 합니다.

기존에 `Actions`를 사용하여 구현했던 기능들은 `fromAction` 유틸리티를 통해 `Attachments`로 쉽게 변환할 수 있어, 레거시 코드와의 호환성을 유지하면서 새로운 API로의 마이그레이션 경로를 제공합니다. 이 변화는 컴포넌트의 행위를 정의하는 패러다임의 전환을 의미하며, 개발자는 더 추상적이고 모듈화된 방식으로 UI 로직을 구성할 수 있게 됩니다.

#### Generics 지원 확대 (Snippets, JSDoc)

타입 시스템의 강화는 대규모 애플리케이션 개발에서 코드의 견고성과 유지보수성을 높이는 데 필수적입니다. Svelte 5.30.0 버전에서는 `Snippets`에 제네릭(`Generics`) 사용이 허용되어, 타입 추론과 타입 힌트 기능이 크게 향상되었습니다.

또한, Svelte 언어 도구는 JSDoc에 제네릭 속성을 지원하게 되었습니다. 이러한 변화는 특히 복잡한 컴포넌트나 재사용 가능한 유틸리티 함수를 작성할 때 타입 안전성을 보장하고, 개발자가 코드의 의도를 명확히 파악할 수 있도록 돕습니다. 이는 개발 과정에서 발생할 수 있는 타입 관련 오류를 줄이고, 코드 가독성을 높이는 데 기여합니다.

#### 새로운 타입 및 구문 지원

Svelte는 개발자가 더욱 유연하게 컴포넌트를 작성할 수 있도록 새로운 구문과 타입 정의를 지속적으로 추가하고 있습니다. Svelte 5.31.0에서는 클래스 생성자 내부에 상태 필드(`State fields`)를 선언할 수 있게 되어, 객체 지향 프로그래밍 패턴과의 통합이 더욱 용이해졌습니다.

또한, Svelte 5.17.5에서는 `<svelte:boundary>` 내부에 `const` 태그가 허용되었고, Svelte 5.18.0에서는 `<template>` 요소가 어떤 자식 요소든 포함할 수 있게 되었습니다. `svelte/elements` 모듈에서 `ClassValue` 타입이 노출되어, CSS 클래스 조작 시 타입 안전성을 확보할 수 있게 되었습니다. 이러한 개선사항들은 개발자가 Svelte 컴포넌트 내에서 더욱 다양한 JavaScript 패턴을 활용하고, UI 로직을 구조화하는 데 더 많은 자유를 제공합니다.

#### XHTML 준수 및 CSP 호환성 강화

Svelte 5.33.0 버전부터 Svelte는 XHTML을 준수하게 되었으며, 새로운 `fragments: 'html' | 'tree'` 옵션은 더 넓은 콘텐츠 보안 정책(CSP) 호환성을 추가합니다. XHTML 준수는 웹 표준을 따르는 애플리케이션을 구축하는 데 중요하며, `fragments` 옵션은 개발자가 애플리케이션의 보안 수준을 더욱 세밀하게 제어할 수 있도록 합니다. 이는 잠재적인 크로스-사이트 스크립팅(XSS) 공격과 같은 보안 취약점을 줄이는 데 기여하며, 규제가 엄격한 환경에서 SvelteKit 애플리케이션을 배포할 때 중요한 고려사항이 됩니다.

#### 개발자 도구 및 경험 개선

개발자 경험(DX)은 프레임워크의 채택과 생산성에 직접적인 영향을 미칩니다. Svelte 언어 도구는 저장 시 누락된 임포트를 자동으로 추가하는 기능을 지원하게 되어, 개발자가 수동으로 임포트 문을 관리하는 번거로움을 줄였습니다.

언어 서버(`language-server-0.17.9`)는 의미론적 문서 하이라이팅을 제공하여, VSCode와 같은 편집기에서 코드의 의미를 기반으로 더 정확한 하이라이팅을 가능하게 합니다. 또한, `$inspect.trace`는 이제 소스 파일 이름을 로깅에 포함하여, 로그 메시지의 출처를 쉽게 파악하고 디버깅 효율성을 높입니다.

`eslint-plugin-svelte v3`의 출시와 Svelte CLI(버전 0.7.0)의 코드 생성 업그레이드 또한 Svelte 5 개발 환경을 더욱 직관적이고 효율적으로 만들었습니다. 이러한 도구 개선은 개발 워크플로우를 간소화하고, 오류를 조기에 발견하며, 전반적인 코딩 경험을 향상시킵니다.

### SvelteKit 런타임 및 빌드 최적화

SvelteKit은 빌드 시간 단축과 런타임 성능 향상을 위해 지속적인 최적화를 진행하고 있습니다.

#### Vite 7 및 Rolldown 지원

2025년 6월 20일, SvelteKit 2.22.0 버전은 Vite 7 및 Rolldown에 대한 실험적 지원을 추가했습니다. Rolldown은 컴파일 속도를 향상시킬 것으로 예상되지만, 초기 단계에서는 추가적인 트리 쉐이킹(`tree-shaking`)이 구현되기 전까지 번들 크기가 커질 수 있습니다.

이 기능을 사용하려면 `vite-plugin-svelte@^6.0.0-next.0` 및 `vite@^7.0.0-beta.0`이 필요합니다. 이 변화는 SvelteKit 프로젝트의 빌드 성능에 큰 영향을 미칠 잠재력을 가지고 있으며, 대규모 애플리케이션의 빌드 시간을 단축하는 데 핵심적인 역할을 할 것입니다. 개발팀은 빌드 속도와 번들 크기 간의 균형을 면밀히 모니터링해야 합니다.

#### SSR 및 클라이언트 사이드 코드 실행 유연성

SvelteKit은 서버 사이드 렌더링(SSR)과 클라이언트 사이드 코드 실행 간의 유연성을 강화하고 있습니다. SSR이 비활성화되고 페이지 옵션이 불리언 또는 문자열 리터럴일 경우, 클라이언트 사이드 코드가 유니버설 페이지/레이아웃의 최상위 레벨에서 실행될 수 있게 되었습니다 (`kit@2.21.0`).

2025년 3월에는 Svelte 및 SvelteKit의 SSR이 개선되었으며, Svelte 5.20.0에서는 `$props.id()`를 통해 SSR-안전한 ID 생성이 가능해졌습니다. 또한, Svelte 5.22.0은 `render` 함수에 `idPrefix` 옵션을 도입하여, 여러 Svelte 런타임이 한 페이지에 존재하는 드문 시나리오에서도 클라이언트 사이드 ID 생성을 더욱 안정적으로 만들었습니다.

#### Pnpm 10 지원 및 빌드 프로세스 개선

SvelteKit 2.16.0 버전에서는 `postinstall` 스크립트가 제거되어 `pnpm 10`을 지원하게 되었습니다. 이 변경으로 인해 사용자는 Vite를 처음 실행할 때 경고를 피하기 위해 `package.json`에 `"prepare": "svelte-kit sync"`를 추가해야 합니다.

또한, Rolldown을 사용하여 단일 및 인라인 앱을 번들링하기 위해 `manualChunks`를 사용하는 `chore` 변경사항이 있었습니다 (`kit@2.22.1`). 이러한 업데이트는 다양한 패키지 관리자 환경에서의 호환성을 높이고, 빌드 프로세스에 대한 미세한 제어를 가능하게 합니다.

#### 이미지 최적화 기능

이미지 최적화는 웹 성능에 큰 영향을 미치는 요소입니다. SvelteKit은 `sveltekit-image-optimize`와 같은 유틸리티를 통해 이미지 최적화 엔드포인트를 쉽게 생성할 수 있도록 지원합니다.

Vercel 배포 환경에서는 `@sveltejs/adapter-vercel`을 통해 이미지가 온디맨드로 최적화되어, 빌드 시간을 단축하고 런타임에 최적화된 `srcset`을 생성하여 성능을 보장합니다.

### 데이터 처리 및 라우팅 강화

SvelteKit은 데이터 흐름 관리와 라우팅 로직을 더욱 강력하고 유연하게 만들기 위한 업데이트를 진행했습니다.

#### 비동기 reroute 및 getRequestEvent

SvelteKit 2.18.0 및 2.19.0 버전에서는 `reroute` 함수를 비동기적으로 호출할 수 있게 되었으며, 필요에 따라 `fetch`를 사용하여 쿠키나 다른 요청 컨텍스트를 전달할 수 있는 옵션이 추가되었습니다.

또한, SvelteKit 2.20.0은 `$app/server`에서 현재 `RequestEvent`를 반환하는 새로운 함수 `getRequestEvent`를 도입했습니다. 이 함수는 `HandleServerError` 훅에서도 접근 가능하게 개선되어, 서버 에러 처리 시 더 풍부한 요청 컨텍스트를 활용할 수 있게 되었습니다.

#### 데이터 로딩 및 타입 유틸리티

SvelteKit 2.16.0에서는 `PageProps` 및 `LayoutProps` 타입이 제공되어, 페이지 및 레이아웃의 `load` 함수에서 반환되는 데이터에 대한 타입 정의를 명확히 할 수 있게 되었습니다.

또한, `+page.server.ts` 파일을 사용하여 서버에서 데이터를 가져오고, 이를 동일 폴더의 `+page.svelte` 파일에서 접근할 수 있는 기능은 데이터 흐름을 명확하게 분리하고 관리하는 데 도움을 줍니다. `$inspect.trace`에 소스 파일 이름 로깅이 포함된 것은 디버깅 효율성을 높입니다.

#### URL 및 라우팅 유틸리티

SvelteKit 2.18.0에 추가된 `normalizeUrl` 헬퍼 함수는 SvelteKit 내부 데이터를 포함할 수 있는 원시 URL을 정규화하는 방법을 제공합니다.

SvelteKit 2.16.0에서는 `goto()` 함수에서 사용자 정의 식별자를 무효화할 수 있는 기능이 추가되어, 특정 데이터 캐시를 수동으로 제어하며 페이지 이동 시 데이터 일관성을 유지할 수 있게 되었습니다. 또한, 해시 라우팅 관련 여러 버그 수정이 이루어졌습니다.

### 반응성 및 상태 관리 개선

Svelte 5의 Runes 도입 이후, SvelteKit은 반응성 시스템의 안정성과 유연성을 지속적으로 개선하고 있습니다.

#### $effect 및 파생 상태 필드 안정화

SvelteKit 2.22.1에서는 `$effect` 내에서 `pushState` 또는 `replaceState`를 호출할 때 발생하는 무한 루프를 방지하는 수정이 이루어졌습니다.

파생된(`derived`) 상태 내에서 생성된 `SvelteSet`, `SvelteMap`, `SvelteDate`, `SvelteURL`, `SvelteURLSearchParams`에 대한 `unsafe_state_mutation` 제약이 완화되었습니다. Svelte 5.24.0에서는 파생 및 이펙트 내에서 생성된 상태를 자체 무효화 없이 로컬에서 읽고 쓸 수 있게 되어, "unsafe read" 경고의 발생이 크게 줄었습니다.

#### Writable derived 문 지원

Svelte 5.25.0 및 5.25.2 버전에서는 쓰기 가능한 `derived` 문이 지원되기 시작했습니다. 이는 파생된 상태를 특정 조건에서 직접 값을 할당하고 반응적으로 업데이트할 수 있게 합니다. 이 기능은 복잡한 계산된 상태를 관리하거나, 사용자 입력에 따라 파생된 값을 업데이트해야 하는 시나리오에서 코드의 간결성과 가독성을 높이는 데 기여합니다.

#### 성능 최적화를 위한 부분 평가

Svelte 5.27.0 및 5.28.0 버전에서는 특정 표현식이 런타임 성능 향상을 위해 부분적으로 평가되도록 개선되었습니다. 이는 Svelte 컴파일러가 런타임에 불필요한 계산을 줄이고, 애플리케이션의 전반적인 반응성과 속도를 향상시키는 데 기여합니다.

### 배포 및 인프라 관련 변경사항

SvelteKit은 다양한 배포 환경에서의 최적화와 인프라 통합을 강화하고 있습니다.

#### Vercel 어댑터 개선

`adapter-vercel@5.7.0` 버전부터는 각 라우트별로 심링크 함수를 생성하여, Vercel에 배포된 애플리케이션의 관찰 가능성을 향상시킵니다.

SvelteKit은 Vercel Functions를 통한 스트리밍, ISR(Incremental Static Regeneration), 'Skew Protection', 'Draft Mode' 등 Vercel의 다양한 기능을 지원하여 배포 경험과 성능을 크게 향상시킵니다.

#### Cloudflare 어댑터 통합 및 업데이트

기존의 두 가지 Cloudflare 어댑터가 `adapter-cloudflare@7.0.0` 버전에서 단일 `adapter-cloudflare`로 통합되었습니다. 이 어댑터는 Wrangler 4 지원, 개선된 `_headers` 및 `_redirects` 파일 처리, 그리고 Cloudflare Workers Static Assets 빌드 지원을 포함합니다.

#### Websockets 및 서버 컴포넌트 동향

2025년 3월에는 웹소켓(`Websockets`)에 대한 네이티브 지원이 테스트를 위해 공개되었습니다. 또한, SvelteKit이 조만간 서버 컴포넌트(`Server Components`)를 지원할 가능성에 대한 논의도 있었습니다. 이러한 동향은 SvelteKit이 미래의 웹 아키텍처 트렌드를 적극적으로 수용하고 있음을 보여줍니다.

## 결론

2025년 상반기 SvelteKit의 변화는 프레임워크가 Svelte 5의 'Runes' 기반 위에서 더욱 성숙하고 강력한 풀스택 웹 개발 솔루션으로 진화하고 있음을 명확히 보여줍니다. `Attachments` 도입, 제네릭 지원 확대, Vite 7 및 Rolldown 지원, 비동기 `reroute`와 같은 기능들은 개발 경험과 애플리케이션 성능을 크게 향상시킬 것입니다.

개발팀은 이러한 변화들을 면밀히 검토하고, 특히 새로운 API, 빌드 도구, 반응성 모델의 안정화에 주의를 기울여야 합니다. SvelteKit의 공식 블로그와 CHANGELOG를 지속적으로 확인하여 최신 업데이트를 파악하고, 새로운 기능들을 점진적으로 적용하는 전략이 권장됩니다.
