# UnoCSS 2025년 1분기 주요 업데이트 분석

## 서론: UnoCSS 최신 변경사항 심층 분석

이 보고서는 혁신적인 Atomic CSS 엔진인 UnoCSS의 최신 릴리즈(v66.1.3부터 v66.3.2까지)에 포함된 핵심적인 변경사항들을 분석합니다. 이 보고서의 목표는 개발자들이 현재 진행 중이거나 새로 시작하는 프로젝트에 최신 변경사항을 자신 있게 적용하고, 코드의 안정성을 확보하며, 새로운 기능들을 온전히 활용할 수 있도록 실용적이고 상세한 가이드를 제공하는 것입니다.

사용자의 요청은 2025년 1분기 릴리즈 분석이었으나, 공식 릴리즈 노트가 2025년 5월과 6월에 집중되어 있어 현재 개발 환경에 가장 직접적인 영향을 미치는 최신 데이터들을 기반으로 분석을 진행했습니다.

최근 UnoCSS 업데이트 동향은 세 가지 핵심적인 흐름으로 요약할 수 있습니다.

1. `preset-wind4`의 부상: Tailwind CSS v4의 변화에 발맞추기 위한 차세대 프리셋.
2. 상태 기반 스타일링의 혁신: `aria-*` 및 `data-*`와 같은 HTML 속성을 기반으로 한 동적 변형(Variant) 기능 강화.
3. 개발 생태계와의 완벽한 통합: Vite 7과 같은 최신 빌드 도구 즉각 지원 및 Svelte, Nuxt 등 주요 프레임워크와의 연동성 개선.

이러한 변화들은 UnoCSS가 단순히 유틸리티 클래스를 생성하는 엔진에 머무르지 않고, 최신 프론트엔드 개발 트렌드를 이끌어가는 핵심 플레이어로 자리매김하려는 전략적 의지를 보여줍니다.

---

## 핵심 요약: 개발자가 즉시 알아야 할 필수 변경사항

* `preset-wind4`의 공식 등장: 새로운 프로젝트 시작 시 `preset-wind4` 사용을 최우선으로 고려해야 합니다. 이 프리셋은 Tailwind CSS v4와의 호환성을 목표로 하며, 내장 CSS Reset, 새로운 테마 구조, 향상된 색상 모델(oklch) 등 중요한 변화를 포함합니다.
* Vite 7 공식 지원: 최신 버전의 Vite(v7.x)를 사용하는 프로젝트에서 UnoCSS를 아무런 호환성 문제 없이 사용할 수 있습니다.
* 새로운 상태 기반 변형(Variant) 대거 추가: `has-aria-[disabled=true]:`, `data-[state=open]:`과 같은 새로운 변형들이 추가되었습니다. 이제 JavaScript로 클래스를 동적으로 제어하는 대신, HTML 요소의 속성 값에 따라 직접 스타일을 적용할 수 있어 코드 가독성과 유지보수성을 향상시킵니다.
* 기존 `preset-wind`의 지원 중단(Deprecated): `@unocss/preset-wind`와 `@unocss/preset-uno` 패키지는 공식적으로 지원이 중단되고 `@unocss/preset-wind3`로 이름이 변경되었습니다. 기존 프로젝트의 `package.json` 파일에서 해당 의존성을 찾아 `@unocss/preset-wind3`로 반드시 업데이트해야 합니다.
* 웹폰트 최적화 기능 추가 (`subsets` 옵션): `@unocss/preset-web-fonts` 프리셋의 `fontsource` 프로바이더에서 `subsets` 옵션이 지원되기 시작했습니다. 특히 한글 폰트를 사용하는 경우, `subsets: ['korean']` 옵션을 통해 필요한 글리프만 선택적으로 로드하여 웹사이트의 초기 로딩 속도를 크게 개선할 수 있습니다.

---

## preset-wind4 집중 분석: 차세대 프리셋으로의 전환

최근 UnoCSS 업데이트의 가장 큰 화두는 `preset-wind4`의 등장과 빠른 발전입니다. 이 프리셋은 Tailwind CSS v4의 기능을 모방하는 것을 넘어, UnoCSS의 철학에 맞게 재해석하고 개선한 차세대 표준입니다.

### preset-wind3에서 preset-wind4로의 마이그레이션

`preset-wind4`로의 전환은 단순한 버전 업그레이드가 아니며, 몇 가지 중요한 변경사항(Breaking Changes)에 대한 이해가 필요합니다.

1. Breaking Change 1: 테마 키(Theme Key) 구조 변경
    `preset-wind4`는 테마 설정의 명확성과 일관성을 높이기 위해 `uno.config.ts` 파일의 `theme` 객체 구조를 변경했습니다. 기존 `preset-wind3` 설정을 그대로 사용할 경우 스타일이 적용되지 않거나 오류가 발생할 수 있습니다.

    `preset-wind3` vs `preset-wind4` 테마 키 매핑

    | preset-wind3 Key | preset-wind4 Key | 비고 (Notes) |
    |---|---|---|
    | `fontFamily` | `font` | 폰트 관련 설정이 `font` 키 아래로 통합되었습니다. |
    | `fontSize` | `text` 객체 내부로 이동 | `theme: { text: { size: { ... } } }` 형태로 구조가 변경되었습니다. |
    | `lineHeight` | `text` 객체 내부로 이동 또는 `leading` | `theme: { text: { leading: { ... } } }` 형태로 구조가 변경되었습니다. |
    | `borderRadius` | `radius` | 이름이 더 직관적으로 변경되었습니다. |
    | `breakpoints` | `breakpoint` | 단수형으로 변경되었습니다. |
    | `verticalBreakpoints` | `verticalBreakpoint` | 단수형으로 변경되었습니다. |
    | `easing` | `ease` | 이름이 더 직관적으로 변경되었습니다. |

    이러한 변경은 개발자가 테마를 더 체계적으로 관리할 수 있도록 돕기 위한 것입니다. 마이그레이션 시, 기존 `theme` 설정을 하나씩 검토하며 새로운 키에 맞게 수정하는 작업이 필요합니다.

2. Breaking Change 2: 내장된 CSS Reset
    `preset-wind4`는 Tailwind CSS v4와 마찬가지로 브라우저 기본 스타일을 초기화하는 CSS Reset 규칙을 내장하고 있습니다.

    * 과거 (preset-wind3 + 별도 reset): `import '@unocss/reset/tailwind.css'`와 같이 CSS Reset 파일을 직접 `import`해야 했습니다.
    * 현재 (preset-wind4): 이러한 `import` 구문은 더 이상 필요하지 않으며, `uno.config.ts` 파일 내에서 이 기능을 직접 제어해야 합니다.

        ```typescript
        // uno.config.ts
        import { defineConfig, presetWind4 } from 'unocss'

        export default defineConfig({
          presets: [
            presetWind4({
              preflights: {
                reset: true, // 이 옵션을 통해 reset 스타일 적용 여부를 제어
              },
            }),
          ],
        })
        ```

    이 변경으로 인해 설정이 `uno.config.ts` 파일로 중앙화되어 프로젝트의 전반적인 스타일 관리가 더 용이해졌습니다.

### 새로운 변형(Variants) 상세 가이드: 더 스마트해진 UI 스타일링

이번 업데이트에서 가장 주목할 만한 발전 중 하나는 UI의 '상태(state)'에 직접 반응하는 변형(Variant) 기능의 대폭적인 확장입니다. 개발자는 더 이상 UI 상태 변화를 감지하여 JavaScript로 클래스 이름을 동적으로 바꾸는 코드를 작성할 필요가 없습니다. 대신, `data-state="open"`이나 `aria-expanded="true"`와 같이 의미론적으로 올바른 HTML 속성을 관리하는 데만 집중하면, CSS가 해당 상태를 감지하여 자동으로 스타일을 적용합니다.

1. `has-[attribute]` 변형
    이 변형은 특정 자식 요소나 속성을 가진 부모 요소를 기준으로 다른 자식 요소나 부모 자신을 스타일링할 때 사용됩니다.
    * 사용 예시 (아코디언 컴포넌트): `aria-expanded="true"` 속성을 가진 버튼이 내부에 있을 때, 전체 컨테이너(`div`)의 배경색을 변경하고 아이콘을 회전시키는 경우.

        ```html
        <div class="p-2 rounded has-[[aria-expanded=true]]:bg-gray-100 dark:has-[[aria-expanded=true]]:bg-slate-800">
          <button
            aria-expanded="true"
            class="group w-full flex justify-between items-center"
          >
            <span>아코디언 질문</span>
            <svg class="transition-transform duration-300 group-aria-expanded:rotate-180">
              </svg>
          </button>
          <div class="mt-2 text-sm">
            아코디언 답변 내용입니다.
          </div>
        </div>
        ```

    * `has-[[aria-expanded=true]]`는 `aria-expanded="true"` 속성을 가진 자손 요소가 있을 경우, `div`의 배경색을 변경합니다. `group-aria-expanded:rotate-180`는 `group` 클래스가 있는 부모의 `aria-expanded` 상태에 따라 아이콘을 회전시킵니다.

2. `in-[attribute]` 변형
    이 변형은 자신이 특정 속성을 가진 부모 요소 내부에 위치할 때 자신의 스타일을 변경하는 데 사용됩니다. 이는 컨텍스트에 따라 스타일이 달라져야 하는 중첩된 컴포넌트에 이상적입니다.
    * 사용 예시 (활성화된 네비게이션 메뉴): `data-active-section="about"` 속성을 가진 `nav` 요소 안의 'About' 링크만 활성화 스타일을 적용하는 경우.

        ```html
        <nav data-active-section="about">
          <ul>
            <li><a href="/home" class="in-data-[active-section=home]:text-blue-500 in-data-[active-section=home]:font-bold">Home</a></li>
            <li><a href="/about" class="in-data-[active-section=about]:text-blue-500 in-data-[active-section=about]:font-bold">About</a></li>
            <li><a href="/contact" class="in-data-[active-section=contact]:text-blue-500 in-data-[active-section=contact]:font-bold">Contact</a></li>
          </ul>
        </nav>
        ```

    * 위 코드에서 `data-active-section`의 값이 'about'이므로 'About' 링크에만 활성화 스타일이 적용됩니다. 이 기능은 `preset-wind4`에서 `in-*`, `in-data-*`, `in-aria-*` 형태로 지원됩니다.

3. 중첩된 의사(Pseudo) 변형
    `$pseudo-aria-*` 및 `$pseudo-data-*`와 같은 새로운 변형은 `:not()`과 같은 의사 클래스와 결합하여 더욱 복잡하고 정교한 조건을 표현할 수 있게 해줍니다.
    * 사용 예시 (선택되지 않은 포커스 아이템): `data-focus-visible` 속성이 `true`이면서(focus-visible 상태), 동시에 `aria-selected` 속성이 `true`가 아닌 요소에만 특정 스타일을 적용하고 싶을 때.

        ```html
        <div
          data-focus-visible="true"
          aria-selected="false"
          class="[$pseudo-data-[focus-visible=true]:not([aria-selected=true])]:ring-2 [$pseudo-data-[focus-visible=true]:not([aria-selected=true])]:ring-blue-500"
        >
          포커스는 되었지만 선택되지는 않은 아이템
        </div>
        ```

    * 이 기능은 매우 구체적인 UI 상태를 타겟팅해야 하는 고급 컴포넌트 라이브러리나 디자인 시스템을 구축할 때 강력한 도구가 됩니다.

4. 새로운 규칙(Rules) 및 구문 개선
    `preset-wind4`는 새로운 유틸리티 규칙을 추가하고 기존 구문을 개선하여 개발 편의성을 높였습니다.
    * `mask` 규칙 추가: CSS의 `mask-image`, `mask-composite` 등의 속성을 제어하는 `mask-*` 유틸리티가 추가되었습니다.
    * `collapse` 가시성(visibility) 추가: `visibility: collapse` CSS 속성을 위한 `collapse` 유틸리티가 추가되었습니다.
    * 향상된 `font-` 및 `color` 구문: 폰트와 색상 관련 유틸리티의 구문 분석 능력이 향상되었습니다. 특히 색상 구문에서는 `color-mix()`와 같은 최신 CSS 함수의 색상 보간(color-interpolation) 메소드를 지원합니다.

### 주요 프리셋 및 통합(Integration) 업데이트

UnoCSS는 핵심 엔진뿐만 아니라, 다양한 프리셋과 외부 도구와의 통합 기능도 지속적으로 강화하고 있습니다.

1. Vite 7 지원 및 영향
    UnoCSS v66.3.0부터 차세대 프론트엔드 빌드 도구인 Vite 7을 공식적으로 지원합니다. Vite 7은 Node.js 18 지원 중단 및 ESM-only 패키지로의 전환과 같은 중요한 변화를 포함하며, 이는 전체 JavaScript 생태계가 더 현대적인 개발 방식으로 나아가고 있음을 의미합니다. UnoCSS가 이러한 변화에 신속하게 대응했다는 사실은, 개발자들이 최신 기술 스택의 이점을 UnoCSS와 함께 온전히 누릴 수 있다는 확신을 줍니다.

2. `@unocss/preset-web-fonts` 기능 강화: 한글 폰트 최적화
    웹폰트는 웹사이트의 시각적 품질을 높이는 중요한 요소이지만, 특히 한글 폰트는 파일 크기가 매우 커서 페이지 로딩 성능에 큰 부담을 줍니다.
    * `fontsource` 프로바이더의 `subsets` 옵션: `@unocss/preset-web-fonts`의 `fontsource` 프로바이더에 `subsets` 옵션이 추가되었습니다.
    * 이를 통해 'korean' 글리프만 선택적으로 로드하여 수 메가바이트에 달하는 전체 폰트 파일 대신 수백 킬로바이트 크기의 서브셋 파일만 다운로드하게 됩니다. 이는 웹 성능 지표(LCP)를 직접적으로 개선하고 사용자 이탈률을 줄이는 데 결정적인 역할을 합니다.
    * 사용 예시 (Noto Sans KR 폰트의 한글 서브셋만 로드하기):
        * 먼저, 필요한 폰트소스를 설치합니다.
            `npm install @fontsource/noto-sans-kr`
        * 그 다음, `uno.config.ts` 파일을 다음과 같이 설정합니다.

            ```typescript
            // uno.config.ts
            import { defineConfig } from 'unocss'
            import presetWebFonts from '@unocss/preset-web-fonts'

            export default defineConfig({
              presets: [
                presetWebFonts({
                  provider: 'fontsource', // 폰트소스 프로바이더 사용
                  fonts: {
                    // 예시: Noto Sans KR 폰트의 'korean' 서브셋만 로드
                    sans: [
                      {
                        name: 'Noto Sans KR',
                        weights: ['400', '700'], // 필요한 폰트 두께 지정
                        subsets: ['korean'], // 이 부분이 핵심: 'korean' 서브셋만 로드하도록 지정
                      },
                    ],
                  },
                }),
                //... 다른 프리셋들
              ],
            })
            ```

    * 이 간단한 설정만으로도 웹사이트의 성능을 극적으로 향상시킬 수 있으므로, 한글 폰트를 사용하는 모든 프로젝트에 적용하는 것을 강력히 권장합니다.

3. 프레임워크별 업데이트
    UnoCSS는 특정 프레임워크와의 통합을 더욱 강화합니다.
    * Svelte (`@unocss/svelte-scoped`): 조건부 클래스 적용을 위해 `clsx` 라이브러리와 유사한 구문을 지원하기 시작했습니다. 이를 통해 여러 조건이 얽혀 있는 복잡한 클래스 바인딩을 훨씬 더 깔끔하고 가독성 높게 관리할 수 있습니다.
        * 사용 예시:

            ```html
            <script>
              let isBold = true;
              let hasError = false;
              const getUnderlineClass = () => 'underline';
            </script>

            <div class={`text-lg ${isBold ? 'font-bold' : ''} ${hasError ? 'text-red-500' : ''} ${getUnderlineClass()}`}>
              clsx-like syntax is powerful!
            </div>
            ```

    * Nuxt (`@unocss/nuxt`): Nuxt 모듈의 내부 동작이 개선되었습니다. 이제 프로젝트 빌드 시 필요한 경우에만 Vite 또는 Webpack 플러그인을 동적으로 가져오도록 최적화되었습니다. 이로 인해 불필요한 초기 로딩 시간이 줄어들고 프로젝트의 전반적인 빌드 성능이 향상되었습니다.

### 코드 안정성을 위한 중요 버그 수정 및 기타 변경사항

* `@apply` 지시어의 `noMerge` 메타 존중: CSS 내에서 `@apply` 지시어를 사용할 때, 일부 규칙들이 의도치 않게 병합되는 문제가 있었습니다. 이제 규칙 정의 시 `noMerge: true` 메타데이터가 설정된 경우, `@apply`가 이를 존중하여 규칙을 병합하지 않도록 수정되었습니다.
* `@screen` 변환기의 `preset-wind4` 지원: `@screen md { ... }`와 같이 미디어 쿼리를 작성할 수 있는 `@screen` 지시어가 `preset-wind4`에서도 정상 작동하도록 지원이 추가되었습니다. 단, `preset-wind4`에서 테마의 `breakpoints` 키가 `breakpoint`로 변경되었으므로, 이 기능을 사용하려면 `uno.config.ts`의 `theme.breakpoint` 객체를 명시적으로 정의해야 할 수 있습니다.
* 임의 값(Arbitrary Values) 정규식 개선: `w-[calc(100%-2rem)]`과 같이 대괄호 안에 복잡한 값을 사용하는 '임의 값' 구문에서, `rem`, `px`와 같은 단어 문자를 포함할 수 있도록 내부 정규식이 개선되었습니다.
* `ring` 오프셋 크기 규칙 수정: 포커스링 등에서 사용되는 `ring-offset-*` 유틸리티의 스타일이 잘못 생성되던 버그가 수정되었습니다.

---

## 결론: UnoCSS v4.0과 함께하는 미래

UnoCSS의 최근 업데이트는 단순한 기능 개선을 넘어, 프론트엔드 개발의 미래 방향성에 대한 명확한 비전을 제시합니다. 개발자는 이러한 변화를 이해하고 적극적으로 활용함으로써 더 효율적이고, 성능이 뛰어나며, 유지보수가 용이한 애플리케이션을 구축할 수 있습니다.

### 핵심 요약 재확인

* UnoCSS는 `preset-wind4`를 통해 Tailwind CSS v4와의 정렬을 가속화하며 미래 표준에 대비하고 있습니다.
* `has-aria-*`와 같은 새로운 상태 기반 변형은 JavaScript 의존성을 줄이고, 더 선언적이며 접근성 높은 UI 개발 패러다임을 제시합니다.
* Vite 7 지원과 `preset-web-fonts`의 `subsets` 기능 추가는 개발자 경험(DX)과 실제 프로덕션 환경의 사용자 경험(UX)을 모두 크게 향상시키는 핵심적인 업데이트입니다.

### 한국인 개발자를 위한 맞춤 권장사항

* 신규 프로젝트: 망설임 없이 `preset-wind4`로 시작하는 것을 적극 권장합니다. 초기에는 다른 프리셋과의 호환성 문제 등 사소한 불편함이 있을 수 있으나, 이는 UnoCSS가 나아갈 방향이며 장기적으로 더 나은 테마 관리와 확장성을 제공할 것입니다.
* 기존 프로젝트: 현재 `preset-wind`나 `preset-uno`를 사용하고 있다면, 패키지 지원 중단에 따른 잠재적 문제를 피하기 위해 안정적인 `@unocss/preset-wind3`로 즉시 마이그레이션하는 것이 좋습니다. `preset-wind4`로의 전환은 프로젝트의 규모와 복잡성, 그리고 마이그레이션에 필요한 리소스를 고려하여 신중하게 계획해야 합니다.
* 성능 최적화: 만약 서비스의 주 사용자가 한국인이거나 한국어 콘텐츠를 다룬다면, `@unocss/preset-web-fonts`의 `subsets` 옵션을 활용한 웹폰트 최적화는 선택이 아닌 필수입니다. 이는 사용자 경험에 가장 큰 영향을 미치는, 가장 효과적인 성능 개선 방법 중 하나입니다.
* 지속적인 학습: UnoCSS는 매우 빠르게 발전하는 프로젝트입니다. 공식 GitHub 릴리즈 페이지와 공식 문서 사이트를 주기적으로 방문하여 새로운 기능과 모범 사례를 꾸준히 익히는 것이 중요합니다. 이를 통해 UnoCSS의 잠재력을 최대한 활용하고 경쟁력 있는 개발자로 성장할 수 있을 것입니다.
