# UI/UX 가이드라인

* **문서 요약**: 이 문서는 우리 프로젝트의 시각적, 경험적 일관성을 유지하기 위한 디자인 원칙과 컴포넌트 사용 규칙을 정의합니다.
* **관련 디자인 시스템**: [shadcn-svelte, Material Design 등 프로젝트에서 사용하는 시스템을 기입]

---

## 1. 핵심 디자인 원칙 (Core Design Principles)

* **단순성 (Simplicity)**: 불필요한 시각적 요소를 최소화하고, 사용자가 핵심 기능에 집중할 수 있도록 합니다.
* **일관성 (Consistency)**: 프로젝트 전체에서 컴포넌트, 색상, 아이콘 등을 일관되게 사용하여 사용자에게 예측 가능한 경험을 제공합니다.
* **접근성 (Accessibility)**: 모든 사용자가 불편함 없이 서비스를 이용할 수 있도록 웹 접근성 표준(WCAG)을 준수합니다.

## 2. 디자인 토큰 (Design Tokens)

### 2.1. 색상 (Colors)

우리 프로젝트는 라이트/다크 모드를 모두 지원하며, 모든 색상 변수는 **`[전역 CSS 파일 경로. 예: src/styles/app.css]`** 파일 내에서 CSS 변수로 정의하고 관리합니다. `:root`에는 라이트 모드 값을, `.dark`와 같은 테마 클래스 안에는 다크 모드 값을 정의합니다.

| 역할 (Role) | 설명 | CSS 변수 (Light/Dark) |
|---|---|---|
| **Background** | 페이지의 기본 배경색 | `var(--background)` |
| **Foreground** | 페이지의 기본 텍스트 색상 | `var(--foreground)` |
| **Card / Popover**| 배경과 구분되는 컨테이너 요소의 배경색 | `var(--card)` / `var(--popover)` |
| **Primary** | 핵심 상호작용 (주요 버튼, 활성화된 링크 등) | `var(--primary)` |
| **Secondary** | 보조 상호작용 (보조 버튼 등) | `var(--secondary)` |
| **Accent** | 강조 요소 (하이라이트, 특별 알림 등) | `var(--accent)` |
| **Muted** | 덜 중요한 텍스트 (부가 정보, 비활성화 상태) | `var(--muted-foreground)`|
| **Border** | 컴포넌트 테두리, 구분선 | `var(--border)` |
| **Input** | 입력창의 배경색 | `var(--input)` |
| **Ring** | 포커스를 받았을 때 나타나는 외곽선 색상 | `var(--ring)` |
| **Destructive** | 파괴적 액션 (삭제, 오류 메시지 등) | `var(--destructive)` |
| **Success** | 긍정적 피드백 (성공, 유효성 통과 등) | `var(--success)` |
| **Warning** | 경고 및 주의 환기 | `var(--warning)` |
| **Info** | 일반 정보 안내 | `var(--info)` |
| *... (기타 색상)* | 프로젝트 고유의 추가 색상 | |

*자세한 색상 값은 위에 명시된 전역 CSS 파일을 직접 참고하십시오.*

### 2.2. 타이포그래피 (Typography)

전역 타이포그래피 스타일(h1, p 등)은 **`[전역 CSS 파일 경로. 예: src/styles/app.css]`** 파일의 `@layer base` 내에 Tailwind CSS의 `@apply` 지시어를 사용하여 정의합니다. 이를 통해 마크다운 렌더링이나 일반 태그 사용 시에도 일관된 스타일을 유지할 수 있습니다.

**`@layer base` 적용 예시:**

```css
/* in: [전역 CSS 파일 경로] */
@layer base {
  h1 {
    @apply text-2xl font-bold tracking-tight;
  }
  h2 {
    @apply text-xl font-semibold tracking-tight;
  }
}
```

**기본 스타일 예시:**

| 사용처 | 권장 스타일 (Tailwind 클래스) | 설명 |
|---|---|---|
| 페이지 제목 | `text-2xl font-bold` | `h1` 태그에 기본 적용됩니다. |
| 섹션 부제목 | `text-lg font-semibold` | |
| 본문 | `text-base` | `p` 태그에 기본 적용됩니다. |

## 3. 컴포넌트 사용 가이드

### 3.1. 버튼 (Button)

* **기본 버튼 (`variant="default"`)**: 가장 일반적인 긍정적 액션에 사용합니다. (예: '저장하기')
* **파괴적 버튼 (`variant="destructive"`)**: 사용자 데이터를 삭제하는 등 되돌리기 어려운 작업에만 사용합니다. (예: '계정 삭제')
