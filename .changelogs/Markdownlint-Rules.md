# 마크다운 문서 작성 규칙

이 문서는 모든 규칙에 대한 설명, 규칙이 확인하는 내용, 규칙을 위반하는 문서의 예시 및 수정된 예시 버전을 포함합니다.

<a name="md001"></a>

## `MD001` - 제목 레벨은 한 번에 한 레벨씩만 증가해야 합니다

태그: `headings`

별칭: `heading-increment`

이 규칙은 마크다운 문서에서 제목 레벨을 건너뛸 때 트리거됩니다. 예를 들어:

```markdown
# 제목 1

### 제목 3

이 문서에서는 2번째 레벨 제목을 건너뛰었습니다.
```

여러 제목 레벨을 사용할 때, 중첩된 제목은 한 번에 한 레벨씩만 증가해야 합니다:

```markdown
# 제목 1

## 제목 2

### 제목 3

#### 제목 4

## 다른 제목 2

### 다른 제목 3
```

근거: 제목은 문서의 구조를 나타내며, 건너뛸 경우 혼란을 줄 수 있습니다 - 특히 접근성 시나리오에서 그렇습니다. 추가 정보: <https://www.w3.org/WAI/tutorials/page-structure/headings/>.

<a name="md003"></a>

## `MD003` - 제목 스타일

태그: `headings`

별칭: `heading-style`

매개변수:

- `style`: 제목 스타일 (`string`, 기본값 `consistent`, 값 `atx` / `atx_closed` / `consistent` / `setext` / `setext_with_atx` / `setext_with_atx_closed`)

이 규칙은 동일한 문서에서 다른 제목 스타일이 사용될 때 트리거됩니다:

```markdown
# ATX 스타일 H1

## 닫힌 ATX 스타일 H2 ##

Setext 스타일 H1
===============
```

이 문제를 해결하려면 문서 전체에서 일관된 제목 스타일을 사용하세요:

```markdown
# ATX 스타일 H1

## ATX 스타일 H2
```

`setext_with_atx` 및 `setext_with_atx_closed` 설정은 setext 스타일 제목(레벨 1과 2만 지원)이 있는 문서에서 레벨 3 이상의 ATX 스타일 제목을 허용합니다:

```markdown
Setext 스타일 H1
===============

Setext 스타일 H2
---------------

### ATX 스타일 H3
```

참고: 구성된 제목 스타일은 특정 스타일(`atx`, `atx_closed`, `setext`, `setext_with_atx`, `setext_with_atx_closed`)을 요구하거나, `consistent`를 통해 모든 제목 스타일이 첫 번째 제목 스타일과 일치하도록 요구할 수 있습니다.

참고: 텍스트 줄 바로 아래에 수평선을 배치하면 해당 텍스트가 레벨 2 setext 스타일 제목으로 바뀌어 이 규칙이 트리거될 수 있습니다:

```markdown
수평선이 뒤따르는 텍스트 줄은 제목이 됩니다
---
```

근거: 일관된 서식은 문서 이해를 더 쉽게 만듭니다.

<a name="md004"></a>

## `MD004` - 순서 없는 목록 스타일

태그: `bullet`, `ul`

별칭: `ul-style`

매개변수:

- `style`: 목록 스타일 (`string`, 기본값 `consistent`, 값 `asterisk` / `consistent` / `dash` / `plus` / `sublist`)

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 문서에서 순서 없는 목록 항목에 사용된 기호가 구성된 순서 없는 목록 스타일과 일치하지 않을 때 트리거됩니다:

```markdown
* 항목 1
+ 항목 2
- 항목 3
```

이 문제를 해결하려면, 문서 전체에서 목록 항목에 대해 구성된 스타일을 사용하세요:

```markdown
* 항목 1
* 항목 2
* 항목 3
```

구성된 목록 스타일은 모든 목록 스타일링이 특정 기호(`asterisk`, `plus`, `dash`)가 되도록 하거나, 각 하위 목록이 부모 목록과 다른 일관된 기호를 갖도록 하거나(`sublist`), 모든 목록 스타일이 첫 번째 목록 스타일과 일치하도록 할 수 있습니다(`consistent`).

예를 들어, 다음은 `sublist` 스타일에 대해 유효합니다. 가장 바깥쪽 들여쓰기는 별표를, 중간 들여쓰기는 더하기 기호를, 가장 안쪽 들여쓰기는 대시를 사용하기 때문입니다:

```markdown
* 항목 1
  + 항목 2
    - 항목 3
  + 항목 4
* 항목 4
  + 항목 5
```

근거: 일관된 서식은 문서 이해를 더 쉽게 만듭니다.

<a name="md005"></a>

## `MD005` - 동일한 수준의 목록 항목에 대한 일관성 없는 들여쓰기

태그: `bullet`, `indentation`, `ul`

별칭: `list-indent`

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 목록 항목이 동일한 수준으로 구문 분석되지만 들여쓰기가 동일하지 않을 때 트리거됩니다:

```markdown
* 항목 1
  * 중첩된 항목 1
  * 중첩된 항목 2
   * 잘못 정렬된 항목
```

일반적으로 이 규칙은 오타로 인해 트리거됩니다. 목록의 들여쓰기를 수정하여 해결하세요:

```markdown
* 항목 1
  * 중첩된 항목 1
  * 중첩된 항목 2
  * 중첩된 항목 3
```

순차적으로 정렬된 목록 마커는 일반적으로 모든 항목이 동일한 시작 열을 갖도록 왼쪽 정렬됩니다:

```markdown
...
8. Item
9. Item
10. Item
11. Item
...
```

이 규칙은 또한 모든 항목이 동일한 끝 열을 갖도록 목록 마커의 오른쪽 정렬을 지원합니다:

```markdown
...
 8. Item
 9. Item
10. Item
11. Item
...
```

근거: 이 규칙을 위반하면 콘텐츠가 제대로 렌더링되지 않을 수 있습니다.

<a name="md007"></a>

## `MD007` - 순서 없는 목록 들여쓰기

태그: `bullet`, `indentation`, `ul`

별칭: `ul-indent`

매개변수:

- `indent`: 들여쓰기 공백 수 (`integer`, 기본값 `2`)
- `start_indent`: 첫 번째 수준 들여쓰기 공백 수 (start_indented가 설정된 경우) (`integer`, 기본값 `2`)
- `start_indented`: 목록의 첫 번째 수준을 들여쓰기할지 여부 (`boolean`, 기본값 `false`)

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 목록 항목이 구성된 공백 수(기본값: 2)만큼 들여쓰기되지 않았을 때 트리거됩니다.

예시:

```markdown
* 목록 항목
   * 3칸 공백으로 들여쓴 중첩 목록 항목
```

수정된 예시:

```markdown
* 목록 항목
  * 2칸 공백으로 들여쓴 중첩 목록 항목
```

참고: 이 규칙은 부모 목록이 모두 순서 없는 목록인 경우에만 하위 목록에 적용됩니다 (그렇지 않으면 정렬된 목록의 추가 들여쓰기가 규칙을 방해합니다).

`start_indented` 매개변수를 사용하면 목록의 첫 번째 수준을 0에서 시작하는 대신 구성된 공백 수만큼 들여쓰기할 수 있습니다. `start_indent` 매개변수를 사용하면 첫 번째 수준의 목록을 나머지 목록과 다른 공백 수로 들여쓰기할 수 있습니다 (`start_indented`가 설정되지 않은 경우 무시됨).

근거: 2칸 공백으로 들여쓰면 목록 마커 뒤에 단일 공백을 사용할 때 중첩 목록의 내용이 부모 목록 내용의 시작 부분과 일치하게 됩니다. 4칸 공백으로 들여쓰는 것은 코드 블록과 일관성이 있으며 편집기에서 구현하기가 더 간단합니다. 또한, 이는 4칸 공백 들여쓰기를 요구하는 다른 마크다운 파서와의 호환성 문제가 될 수 있습니다. 추가 정보: [마크다운 스타일 가이드][markdown-style-guide].

참고: 호환성 정보는 [Prettier.md](Prettier.md)를 참조하세요.

[markdown-style-guide]: https://cirosantilli.com/markdown-style-guide#indentation-of-content-inside-lists

<a name="md009"></a>

## `MD009` - 후행 공백

태그: `whitespace`

별칭: `no-trailing-spaces`

매개변수:

- `br_spaces`: 줄 바꿈을 위한 공백 수 (`integer`, 기본값 `2`)
- `list_item_empty_lines`: 목록 항목의 빈 줄에 공백 허용 (`boolean`, 기본값 `false`)
- `strict`: 불필요한 줄 바꿈 포함 (`boolean`, 기본값 `false`)

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 예기치 않은 공백으로 끝나는 모든 줄에서 트리거됩니다. 이 문제를 해결하려면 줄 끝에서 후행 공백을 제거하세요.

참고: 들여쓰기 및 펜스 코드 블록에서는 일부 언어에서 필요하기 때문에 후행 공백이 허용됩니다.

`br_spaces` 매개변수를 사용하면 명시적인 줄 바꿈을 삽입하는 데 일반적으로 사용되는 특정 수의 후행 공백에 대해 이 규칙에 예외를 둘 수 있습니다. 기본값은 하드 브레이크(\<br> 요소)를 나타내는 2개의 공백을 허용합니다.

참고: 이 매개변수가 적용되려면 `br_spaces`를 2 이상의 값으로 설정해야 합니다. `br_spaces`를 1로 설정하면 0과 동일하게 작동하여 후행 공백을 허용하지 않습니다.

기본적으로 이 규칙은 허용된 수의 공백이 사용될 때 하드 브레이크를 생성하지 않더라도(예: 단락 끝에서) 트리거되지 않습니다. 이러한 경우도 보고하려면 `strict` 매개변수를 `true`로 설정하세요.

```markdown
텍스트 텍스트 텍스트
텍스트[공백 2개]
```

목록 항목 내의 빈 줄을 들여쓰기 위해 공백을 사용하는 것은 일반적으로 필요하지 않지만 일부 파서에서는 필요합니다. 이를 허용하려면 `list_item_empty_lines` 매개변수를 `true`로 설정하세요 (`strict`가 `true`인 경우에도).

```markdown
- 목록 항목 텍스트
  [공백 2개]
  목록 항목 텍스트
```

근거: 줄 바꿈을 만드는 데 사용되는 경우를 제외하고 후행 공백은 목적이 없으며 콘텐츠 렌더링에 영향을 주지 않습니다.

<a name="md010"></a>

## `MD010` - 하드 탭

태그: `hard_tab`, `whitespace`

별칭: `no-hard-tabs`

매개변수:

- `code_blocks`: 코드 블록 포함 (`boolean`, 기본값 `true`)
- `ignore_code_languages`: 무시할 펜스 코드 언어 (`string[]`, 기본값 `[]`)
- `spaces_per_tab`: 각 하드 탭에 대한 공백 수 (`integer`, 기본값 `1`)

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 들여쓰기에 공백 대신 하드 탭 문자를 포함하는 모든 줄에서 트리거됩니다. 이 문제를 해결하려면 모든 하드 탭 문자를 공백으로 바꾸세요.

예시:

<!-- markdownlint-disable no-hard-tabs -->

```markdown
일부 텍스트

	* 목록 항목을 들여쓰기하는 데 사용된 하드 탭 문자
```

<!-- markdownlint-restore -->

수정된 예시:

```markdown
일부 텍스트

    * 대신 목록 항목을 들여쓰기하는 데 사용된 공백
```

코드 블록 및 스팬에 대해 이 규칙을 제외할 수 있는 옵션이 있습니다. 그렇게 하려면 `code_blocks` 매개변수를 `false`로 설정하세요. 마크다운 도구에서 탭 처리가 일관되지 않을 수 있으므로(예: 4칸 공백 대 8칸 공백 사용) 코드 블록과 스팬은 기본적으로 포함됩니다.

코드 블록을 스캔할 때(예: 기본적으로 또는 `code_blocks`가 `true`인 경우), `ignore_code_languages` 매개변수를 무시해야 하는 언어 목록으로 설정할 수 있습니다(즉, 하드 탭이 필수는 아니지만 허용됨). 이렇게 하면 문서에 하드 탭이 필요한 언어의 코드를 더 쉽게 포함할 수 있습니다.

기본적으로 이 규칙 위반은 탭을 1개의 공백 문자로 바꾸어 수정됩니다. 다른 수의 공백을 사용하려면 `spaces_per_tab` 매개변수를 원하는 값으로 설정하세요.

근거: 하드 탭은 다른 편집기에서 일관되지 않게 렌더링되는 경우가 많으며 공백보다 작업하기 더 어려울 수 있습니다.

<a name="md011"></a>

## `MD011` - 반전된 링크 구문

태그: `links`

별칭: `no-reversed-links`

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 링크로 보이는 텍스트가 발견되었지만 구문이 반전된 것으로 보일 때 트리거됩니다(`[]`와 `()`가 반전됨):

```markdown
(잘못된 링크 구문)[https://www.example.com/]
```

이 문제를 해결하려면 `[]`와 `()`를 서로 바꾸세요:

```markdown
[올바른 링크 구문](https://www.example.com/)
```

참고: [마크다운 엑스트라](https://en.wikipedia.org/wiki/Markdown_Extra) 스타일 각주는 이 규칙을 트리거하지 않습니다:

```markdown
(예시)[^1]의 경우
```

근거: 반전된 링크는 사용 가능한 링크로 렌더링되지 않습니다.

<a name="md012"></a>

## `MD012` - 연속된 여러 개의 빈 줄

태그: `blank_lines`, `whitespace`

별칭: `no-multiple-blanks`

매개변수:

- `maximum`: 연속된 빈 줄 수 (`integer`, 기본값 `1`)

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 문서에 연속된 여러 개의 빈 줄이 있을 때 트리거됩니다:

```markdown
여기에 일부 텍스트


여기에 더 많은 텍스트
```

이 문제를 해결하려면 문제가 되는 줄을 삭제하세요:

```markdown
여기에 일부 텍스트

여기에 더 많은 텍스트
```

참고: 이 규칙은 코드 블록 내에 연속된 여러 개의 빈 줄이 있는 경우에는 트리거되지 않습니다.

참고: `maximum` 매개변수를 사용하여 연속된 최대 빈 줄 수를 구성할 수 있습니다.

근거: 코드 블록을 제외하고 빈 줄은 목적이 없으며 콘텐츠 렌더링에 영향을 주지 않습니다.

<a name="md013"></a>

## `MD013` - 줄 길이

태그: `line_length`

별칭: `line-length`

매개변수:

- `code_block_line_length`: 코드 블록의 문자 수 (`integer`, 기본값 `80`)
- `code_blocks`: 코드 블록 포함 (`boolean`, 기본값 `true`)
- `heading_line_length`: 제목의 문자 수 (`integer`, 기본값 `80`)
- `headings`: 제목 포함 (`boolean`, 기본값 `true`)
- `line_length`: 문자 수 (`integer`, 기본값 `80`)
- `stern`: 엄격한 길이 검사 (`boolean`, 기본값 `false`)
- `strict`: 매우 엄격한 길이 검사 (`boolean`, 기본값 `false`)
- `tables`: 테이블 포함 (`boolean`, 기본값 `true`)

이 규칙은 구성된 `line_length`(기본값: 80자)보다 긴 줄이 있을 때 트리거됩니다. 이 문제를 해결하려면 줄을 여러 줄로 나누세요. 제목에 대해 다른 최대 길이를 설정하려면 `heading_line_length`를 사용하세요. 코드 블록에 대해 다른 최대 길이를 설정하려면 `code_block_line_length`를 사용하세요.

이 규칙은 구성된 줄 길이를 초과하는 공백이 없는 경우 예외가 있습니다. 이렇게 하면 긴 URL과 같은 항목을 중간에 강제로 끊지 않고도 포함할 수 있습니다. 이 예외를 비활성화하려면 `strict` 매개변수를 `true`로 설정하면 모든 줄이 너무 길 때 문제가 보고됩니다. 너무 길어서 수정할 수 있는 줄에 대해 경고하고 공백 없는 긴 줄을 허용하려면 `stern` 매개변수를 `true`로 설정하세요.

예를 들어 (정상적인 동작 가정):

```markdown
이 줄이 최대 길이라면
이 줄은 그 길이를 넘어서는 공백이 없으므로 괜찮습니다
이 줄은 그 길이를 넘어서는 공백이 있으므로 위반입니다
이-줄은-어디에도-공백이-없으므로-괜찮습니다
```

`strict` 모드에서는 위 마지막 세 줄이 모두 위반입니다. `stern` 모드에서는 위 중간 두 줄이 모두 위반이지만 마지막 줄은 괜찮습니다.

코드 블록, 테이블 또는 제목에 대해 이 규칙을 제외할 수 있는 옵션이 있습니다. 그렇게 하려면 `code_blocks`, `tables` 또는 `headings` 매개변수를 `false`로 설정하세요.

코드 블록은 문서 가독성에 대한 요구 사항인 경우가 많고 코드 규칙과 잠정적으로 호환되기 때문에 기본적으로 이 규칙에 포함됩니다. 그럼에도 불구하고 일부 언어는 짧은 줄에 적합하지 않습니다.

링크/이미지 참조 정의가 있는 줄과 링크/이미지만 있는 독립된 줄(강조를 사용할 수 있음)은 URL을 깨뜨리지 않고는 그러한 줄을 나눌 방법이 없는 경우가 많기 때문에 이 규칙에서 항상 제외됩니다(`strict` 모드에서도).

근거: 매우 긴 줄은 일부 편집기에서 작업하기 어려울 수 있습니다. 추가 정보: <https://cirosantilli.com/markdown-style-guide#line-wrapping>.

<a name="md014"></a>

## `MD014` - 출력 없이 명령어 앞에 달러 기호 사용

태그: `code`

별칭: `commands-show-output`

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 셸 명령어를 입력해야 하는 코드 블록이 표시되고, *모든* 셸 명령어 앞에 달러 기호($)가 붙는 경우에 트리거됩니다:

<!-- markdownlint-disable commands-show-output -->

```markdown
$ ls
$ cat foo
$ less bar
```

<!-- markdownlint-restore -->

이 상황에서는 달러 기호가 불필요하며 포함되어서는 안 됩니다:

```markdown
ls
cat foo
less bar
```

달러 기호가 앞에 붙은 명령어의 출력을 표시하는 것은 이 규칙을 트리거하지 않습니다:

```markdown
$ ls
foo bar
$ cat foo
Hello world
$ cat bar
baz
```

일부 명령어는 출력을 생성하지 않기 때문에, *일부* 명령어에 출력이 없는 것은 위반이 아닙니다:

```markdown
$ mkdir test
mkdir: created directory 'test'
$ ls test
```

근거: 필요하지 않을 때 달러 기호를 생략하면 복사/붙여넣기가 더 쉽고 덜 번거롭습니다. 자세한 내용은 <https://cirosantilli.com/markdown-style-guide#dollar-signs-in-shell-code>를 참조하세요.

<a name="md018"></a>

## `MD018` - atx 스타일 제목의 해시 뒤에 공백 없음

태그: `atx`, `headings`, `spaces`

별칭: `no-missing-space-atx`

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 atx 스타일 제목에서 해시 문자 뒤에 공백이 없을 때 트리거됩니다:

```markdown
#제목 1

##제목 2
```

이 문제를 해결하려면 제목 텍스트와 해시 문자 사이에 단일 공백으로 구분하세요:

```markdown
# 제목 1

## 제목 2
```

근거: 이 규칙을 위반하면 콘텐츠가 제대로 렌더링되지 않을 수 있습니다.

<a name="md019"></a>

## `MD019` - atx 스타일 제목의 해시 뒤에 여러 공백

태그: `atx`, `headings`, `spaces`

별칭: `no-multiple-space-atx`

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 atx 스타일 제목에서 제목 텍스트와 해시 문자를 구분하는 데 두 개 이상의 공백이 사용될 때 트리거됩니다:

```markdown
#  제목 1

##  제목 2
```

이 문제를 해결하려면 제목 텍스트와 해시 문자 사이에 단일 공백으로 구분하세요:

```markdown
# 제목 1

## 제목 2
```

근거: 추가 공백은 목적이 없으며 콘텐츠 렌더링에 영향을 주지 않습니다.

<a name="md020"></a>

## `MD020` - 닫힌 atx 스타일 제목의 해시 안에 공백 없음

태그: `atx_closed`, `headings`, `spaces`

별칭: `no-missing-space-closed-atx`

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 닫힌 atx 스타일 제목에서 해시 문자 안에 공백이 없을 때 트리거됩니다:

```markdown
#제목 1#

##제목 2##
```

이 문제를 해결하려면 제목 텍스트와 해시 문자 사이에 단일 공백으로 구분하세요:

```markdown
# 제목 1 #

## 제목 2 ##
```

참고: 이 규칙은 제목의 양쪽 중 한쪽에 공백이 없으면 발생합니다.

근거: 이 규칙을 위반하면 콘텐츠가 제대로 렌더링되지 않을 수 있습니다.

<a name="md021"></a>

## `MD021` - 닫힌 atx 스타일 제목의 해시 안에 여러 공백

태그: `atx_closed`, `headings`, `spaces`

별칭: `no-multiple-space-closed-atx`

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 닫힌 atx 스타일 제목에서 제목 텍스트와 해시 문자를 구분하는 데 두 개 이상의 공백이 사용될 때 트리거됩니다:

```markdown
#  제목 1  #

##  제목 2  ##
```

이 문제를 해결하려면 제목 텍스트와 해시 문자 사이에 단일 공백으로 구분하세요:

```markdown
# 제목 1 #

## 제목 2 ##
```

참고: 이 규칙은 제목의 양쪽에 여러 공백이 포함된 경우 발생합니다.

근거: 추가 공백은 목적이 없으며 콘텐츠 렌더링에 영향을 주지 않습니다.

<a name="md022"></a>

## `MD022` - 제목은 빈 줄로 둘러싸여야 합니다

태그: `blank_lines`, `headings`

별칭: `blanks-around-headings`

매개변수:

- `lines_above`: 제목 위의 빈 줄 (`integer|integer[]`, 기본값 `1`)
- `lines_below`: 제목 아래의 빈 줄 (`integer|integer[]`, 기본값 `1`)

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 제목(모든 스타일) 앞이나 뒤에 최소 한 줄의 빈 줄이 없을 때 트리거됩니다:

```markdown
# 제목 1
일부 텍스트

더 많은 텍스트
## 제목 2
```

이 문제를 해결하려면 모든 제목 앞뒤에 빈 줄이 있도록 하세요(제목이 문서의 시작이나 끝에 있는 경우 제외):

```markdown
# 제목 1

일부 텍스트

더 많은 텍스트

## 제목 2
```

`lines_above` 및 `lines_below` 매개변수를 사용하여 각 제목 위 또는 아래에 다른 수의 빈 줄(0 포함)을 지정할 수 있습니다. 어느 매개변수에든 `-1` 값을 사용하면 모든 수의 빈 줄이 허용됩니다. 각 제목 수준 위 또는 아래의 줄 수를 개별적으로 사용자 지정하려면 값이 제목 수준 1-6에 해당하는 `number[]`를 지정하세요(순서대로).

참고: `lines_above` 또는 `lines_below`가 하나 이상의 빈 줄을 요구하도록 구성된 경우 [MD012/no-multiple-blanks](md012.md)도 사용자 지정해야 합니다. 이 규칙은 지정된 수 이상의 빈 줄이 있는지 확인합니다. 추가 빈 줄은 무시됩니다.

근거: 미적인 이유 외에도 `kramdown`을 포함한 일부 파서는 이전에 빈 줄이 없는 제목을 구문 분석하지 않고 일반 텍스트로 구문 분석합니다.

<a name="md023"></a>

## `MD023` - 제목은 줄의 시작 부분에서 시작해야 합니다

태그: `headings`, `spaces`

별칭: `heading-start-left`

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 제목이 하나 이상의 공백으로 들여쓰기될 때 트리거됩니다:

```markdown
일부 텍스트

  # 들여쓴 제목
```

이 문제를 해결하려면 모든 제목이 줄의 시작 부분에서 시작하도록 하세요:

```markdown
일부 텍스트

# 제목
```

블록 인용과 같은 시나리오는 줄의 시작을 "들여쓰기"하므로 다음도 올바릅니다:

```markdown
> # 블록 인용 안의 제목
```

근거: 줄의 시작 부분에서 시작하지 않는 제목은 제목으로 구문 분석되지 않고 대신 일반 텍스트로 나타납니다.

<a name="md024"></a>

## `MD024` - 동일한 내용의 여러 제목

태그: `headings`

별칭: `no-duplicate-heading`

매개변수:

- `siblings_only`: 형제 제목만 확인 (`boolean`, 기본값 `false`)

이 규칙은 문서에 동일한 텍스트를 가진 여러 제목이 있는 경우 트리거됩니다:

```markdown
# 일부 텍스트

## 일부 텍스트
```

이 문제를 해결하려면 각 제목의 내용이 다른지 확인하세요:

```markdown
# 일부 텍스트

## 더 많은 텍스트
```

`siblings_only` 매개변수가 `true`로 설정된 경우, 다른 부모를 가진 제목에 대한 중복이 허용됩니다(변경 로그에서 흔히 볼 수 있듯이):

```markdown
# 변경 로그

## 1.0.0

### 기능

## 2.0.0

### 기능
```

근거: 일부 마크다운 파서는 제목 이름을 기반으로 제목에 대한 앵커를 생성합니다. 동일한 내용을 가진 제목은 해당 기능에 문제를 일으킬 수 있습니다.

<a name="md025"></a>

## `MD025` - 동일한 문서에 여러 최상위 제목

태그: `headings`

별칭: `single-h1`, `single-title`

매개변수:

- `front_matter_title`: 프런트 매터에서 제목과 일치하는 RegExp (`string`, 기본값 `^\s*title\s*[:=]`)
- `level`: 제목 수준 (`integer`, 기본값 `1`)

이 규칙은 최상위 제목이 사용 중일 때(파일의 첫 번째 줄이 h1 제목임) 문서에 두 개 이상의 h1 제목이 사용될 때 트리거됩니다:

```markdown
# 최상위 제목

# 또 다른 최상위 제목
```

수정하려면 문서의 제목 역할을 하는 단일 h1 제목이 있도록 문서를 구성하세요. 후속 제목은 하위 수준 제목(h2, h3 등)이어야 합니다:

```markdown
# 제목

## 제목

## 또 다른 제목
```

참고: `level` 매개변수를 사용하여 h1이 외부에서 추가되는 경우 최상위 수준을 변경할 수 있습니다(예: h2로).

[YAML](https://en.wikipedia.org/wiki/YAML) 프런트 매터가 있고 `title` 속성을 포함하는 경우(블로그 게시물에 일반적으로 사용됨), 이 규칙은 이를 최상위 제목으로 처리하고 후속 최상위 제목에 대한 위반을 보고합니다. 프런트 매터에서 다른 속성 이름을 사용하려면 `front_matter_title` 매개변수를 통해 정규식 텍스트를 지정하세요. 이 규칙에서 프런트 매터 사용을 비활성화하려면 `front_matter_title`에 `""`를 지정하세요.

근거: 최상위 제목은 파일의 첫 번째 줄에 있는 h1이며 문서의 제목 역할을 합니다. 이 규칙이 사용 중인 경우 문서에 두 개 이상의 제목이 있을 수 없으며 전체 문서는 이 제목 내에 포함되어야 합니다.

<a name="md026"></a>

## `MD026` - 제목의 후행 구두점

태그: `headings`

별칭: `no-trailing-punctuation`

매개변수:

- `punctuation`: 구두점 문자 (`string`, 기본값 `.,;:!。，；：！`)

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 지정된 일반 또는 전각 구두점 문자 중 하나가 줄의 마지막 문자인 모든 제목에서 트리거됩니다:

```markdown
# 이것은 제목입니다.
```

이 문제를 해결하려면 후행 구두점을 제거하세요:

```markdown
# 이것은 제목입니다
```

참고: `punctuation` 매개변수를 사용하여 제목 끝에서 구두점으로 간주되는 문자를 지정할 수 있습니다. 예를 들어, 느낌표로 끝나는 제목을 허용하도록 `".,;:"`로 변경할 수 있습니다. `?`는 FAQ 스타일 문서의 제목에서 흔히 사용되기 때문에 기본적으로 허용됩니다. `punctuation` 매개변수를 `""`로 설정하면 모든 문자가 허용되며 규칙을 비활성화하는 것과 같습니다.

참고: `&copy;`, `&#169;` 및 `&#x000A9;`와 같은 [HTML 엔터티 참조][html-entity-references]의 후행 세미콜론은 이 규칙에서 무시됩니다.

근거: 제목은 완전한 문장이 아닙니다. 추가 정보: [헤더 끝의 구두점][end-punctuation].

[end-punctuation]: https://cirosantilli.com/markdown-style-guide#punctuation-at-the-end-of-headers
[html-entity-references]: https://en.wikipedia.org/wiki/List_of_XML_and_HTML_character_entity_references

<a name="md027"></a>

## `MD027` - 블록 인용 기호 뒤에 여러 공백

태그: `blockquote`, `indentation`, `whitespace`

별칭: `no-multiple-space-blockquote`

매개변수:

- `list_items`: 목록 항목 포함 (`boolean`, 기본값 `true`)

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 블록 인용에 블록 인용(`>`) 기호 뒤에 두 개 이상의 공백이 있을 때 트리거됩니다:

```markdown
>  이것은 잘못된 들여쓰기가 있는 블록 인용입니다
>  하나만 있어야 합니다.
```

수정하려면 불필요한 공백을 제거하세요:

```markdown
> 이것은 올바른 들여쓰기가 있는 블록 인용입니다.
> 들여쓰기.
```

블록 인용 내에서 의도한 목록 들여쓰기를 유추하는 것은 어려울 수 있습니다. `list_items` 매개변수를 `false`로 설정하면 정렬된 목록 및 정렬되지 않은 목록 항목에 대해 이 규칙이 비활성화됩니다.

근거: 일관된 서식은 문서 이해를 더 쉽게 만듭니다.

<a name="md028"></a>

## `MD028` - 블록 인용 내 빈 줄

태그: `blockquote`, `whitespace`

별칭: `no-blanks-blockquote`

이 규칙은 두 개의 블록 인용 블록이 빈 줄 외에는 아무것도 없이 분리될 때 트리거됩니다:

```markdown
> 이것은 블록 인용입니다
> 바로 뒤에

> 이 블록 인용이 옵니다. 안타깝게도
> 일부 파서에서는 이것들이 동일한 블록 인용으로 처리됩니다.
```

이 문제를 해결하려면 바로 옆에 있는 모든 블록 인용 사이에 텍스트가 있는지 확인하세요:

```markdown
> 이것은 블록 인용입니다.

그리고 지미도 말했습니다:

> 이것도 블록 인용입니다.
```

또는 동일한 인용문이어야 하는 경우 빈 줄의 시작 부분에 블록 인용 기호를 추가하세요:

```markdown
> 이것은 블록 인용입니다.
>
> 이것은 동일한 블록 인용입니다.
```

근거: 일부 마크다운 파서는 하나 이상의 빈 줄로 분리된 두 개의 블록 인용을 동일한 블록 인용으로 처리하는 반면, 다른 파서는 별도의 블록 인용으로 처리합니다.

<a name="md029"></a>

## `MD029` - 정렬된 목록 항목 접두사

태그: `ol`

별칭: `ol-prefix`

매개변수:

- `style`: 목록 스타일 (`string`, 기본값 `one_or_ordered`, 값 `one` / `one_or_ordered` / `ordered` / `zero`)

이 규칙은 '1.'로 시작하지 않거나 숫자 순서대로 증가하는 접두사가 없는 정렬된 목록에 대해 트리거됩니다(구성된 스타일에 따라 다름). 첫 번째 접두사 또는 모든 접두사에 '0.'을 사용하는 덜 일반적인 패턴도 지원됩니다.

스타일이 'one'으로 구성된 경우 유효한 목록 예시:

```markdown
1. 이것을 하세요.
1. 저것을 하세요.
1. 완료.
```

스타일이 'ordered'로 구성된 경우 유효한 목록 예시:

```markdown
1. 이것을 하세요.
2. 저것을 하세요.
3. 완료.
```

```markdown
0. 이것을 하세요.
1. 저것을 하세요.
2. 완료.
```

스타일이 'one_or_ordered'로 구성된 경우 세 가지 예시 모두 유효합니다.

스타일이 'zero'로 구성된 경우 유효한 목록 예시:

```markdown
0. 이것을 하세요.
0. 저것을 하세요.
0. 완료.
```

모든 스타일에 대해 유효하지 않은 목록 예시:

```markdown
1. 이것을 하세요.
3. 완료.
```

이 규칙은 균일한 들여쓰기를 위해 정렬된 목록 항목의 0-접두사를 지원합니다:

```markdown
...
08. 항목
09. 항목
10. 항목
11. 항목
...
```

참고: 이 규칙은 두 목록 항목 사이에 부적절하게 들여쓴 코드 블록(또는 유사한 것)이 나타나 목록을 두 개로 "깨는" 다음과 같은 경우에 대해 위반을 보고합니다:

<!-- markdownlint-disable code-fence-style -->

~~~markdown
1. 첫 번째 목록

```text
코드 블록
```

1. 두 번째 목록
~~~

수정 방법은 의도한 대로 이전 목록 항목의 일부가 되도록 코드 블록을 들여쓰는 것입니다:

~~~markdown
1. 첫 번째 목록

   ```text
   코드 블록
   ```

2. 여전히 첫 번째 목록
~~~

<!-- markdownlint-restore -->

근거: 일관된 서식은 문서 이해를 더 쉽게 만듭니다.

<a name="md030"></a>

## `MD030` - 목록 마커 뒤 공백

태그: `ol`, `ul`, `whitespace`

별칭: `list-marker-space`

매개변수:

- `ol_multi`: 여러 줄로 된 정렬된 목록 항목의 공백 (`integer`, 기본값 `1`)
- `ol_single`: 한 줄로 된 정렬된 목록 항목의 공백 (`integer`, 기본값 `1`)
- `ul_multi`: 여러 줄로 된 정렬되지 않은 목록 항목의 공백 (`integer`, 기본값 `1`)
- `ul_single`: 한 줄로 된 정렬되지 않은 목록 항목의 공백 (`integer`, 기본값 `1`)

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 목록 마커(예: '`-`', '`*`', '`+`' 또는 '`1.`')와 목록 항목 텍스트 사이의 공백 수를 확인합니다.

확인되는 공백 수는 사용 중인 문서 스타일에 따라 다르지만 기본값은 모든 목록 마커 뒤에 공백 1개입니다:

```markdown
* 푸
* 바
* 바즈

1. 푸
1. 바
1. 바즈

1. 푸
   * 바
1. 바즈
```

문서 스타일은 정렬되지 않은 목록 항목과 정렬된 목록 항목 뒤의 공백 수를 독립적으로 변경할 수 있으며, 목록의 모든 항목 내용이 단일 단락으로 구성되는지 또는 여러 단락(하위 목록 및 코드 블록 포함)으로 구성되는지에 따라 변경할 수 있습니다.

예를 들어, <https://cirosantilli.com/markdown-style-guide#spaces-after-list-marker>의 스타일 가이드에서는 목록의 모든 항목이 단일 단락에 맞는 경우 목록 마커 뒤에 공백 1개를 사용해야 하지만, 목록 내에 여러 단락의 내용이 있는 경우 2개 또는 3개의 공백(정렬된 목록 및 정렬되지 않은 목록의 경우 각각)을 사용하도록 지정합니다:

```markdown
* 푸
* 바
* 바즈
```

vs.

```markdown
*   푸

    두 번째 단락

*   바
```

또는

```markdown
1.  푸

    두 번째 단락

1.  바
```

이 문제를 해결하려면 선택한 문서 스타일에 맞는 올바른 수의 공백이 목록 마커 뒤에 사용되었는지 확인하세요.

근거: 이 규칙을 위반하면 콘텐츠가 제대로 렌더링되지 않을 수 있습니다.

참고: 호환성 정보는 [Prettier.md](Prettier.md)를 참조하세요.

<a name="md031"></a>

## `MD031` - 펜스 코드 블록은 빈 줄로 둘러싸여야 합니다

태그: `blank_lines`, `code`

별칭: `blanks-around-fences`

매개변수:

- `list_items`: 목록 항목 포함 (`boolean`, 기본값 `true`)

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 펜스 코드 블록 앞이나 뒤에 빈 줄이 없을 때 트리거됩니다:

````markdown
일부 텍스트
```
코드 블록
```

```
다른 코드 블록
```
더 많은 텍스트
````

이 문제를 해결하려면 모든 펜스 코드 블록 앞뒤에 빈 줄이 있도록 하세요(블록이 문서의 시작이나 끝에 있는 경우 제외):

````markdown
일부 텍스트

```
코드 블록
```

```
다른 코드 블록
```

더 많은 텍스트
````

`list_items` 매개변수를 `false`로 설정하여 목록 항목에 대해 이 규칙을 비활성화하세요. 목록에 대해 이 동작을 비활성화하는 것은 코드 펜스를 포함하는 [타이트한](https://spec.commonmark.org/0.29/#tight) 목록을 만들어야 하는 경우 유용할 수 있습니다.

근거: 미적인 이유 외에도 kramdown을 포함한 일부 파서는 앞뒤에 빈 줄이 없는 펜스 코드 블록을 구문 분석하지 않습니다.

<a name="md032"></a>

## `MD032` - 목록은 빈 줄로 둘러싸여야 합니다

태그: `blank_lines`, `bullet`, `ol`, `ul`

별칭: `blanks-around-lists`

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 목록(모든 종류) 앞이나 뒤에 빈 줄이 없을 때 트리거됩니다:

```markdown
일부 텍스트
* 목록 항목
* 목록 항목

1. 목록 항목
2. 목록 항목
***
```

위의 첫 번째 경우, 텍스트가 순서 없는 목록 바로 앞에 옵니다. 위의 두 번째 경우, 주제 구분선이 정렬된 목록 바로 뒤에 옵니다. 이 규칙 위반을 수정하려면 모든 목록 앞뒤에 빈 줄이 있도록 하세요(목록이 문서의 맨 처음이나 끝에 있는 경우 제외):

```markdown
일부 텍스트

* 목록 항목
* 목록 항목

1. 목록 항목
2. 목록 항목

***
```

다음 경우는 이 규칙 위반이 **아닙니다**:

```markdown
1. 목록 항목
   더 많은 항목 1
2. 목록 항목
더 많은 항목 2
```

들여쓰기되지 않았지만 "더 많은 항목 2" 텍스트는 [느슨한 연속 줄][lazy-continuation]이라고 하며 두 번째 목록 항목의 일부로 간주됩니다.

근거: 미적인 이유 외에도 kramdown을 포함한 일부 파서는 앞뒤에 빈 줄이 없는 목록을 구문 분석하지 않습니다.

[lazy-continuation]: https://spec.commonmark.org/0.30/#lazy-continuation-line

<a name="md033"></a>

## `MD033` - 인라인 HTML

태그: `html`

별칭: `no-inline-html`

매개변수:

- `allowed_elements`: 허용된 요소 (`string[]`, 기본값 `[]`)

이 규칙은 마크다운 문서에서 원시 HTML이 사용될 때마다 트리거됩니다:

```markdown
<h1>인라인 HTML 제목</h1>
```

이 문제를 해결하려면 원시 HTML을 포함하는 대신 '순수' 마크다운을 사용하세요:

```markdown
# 마크다운 제목
```

참고: 특정 HTML 요소를 허용하려면 `allowed_elements` 매개변수를 사용하세요.

근거: 마크다운에서는 원시 HTML이 허용되지만, 이 규칙은 문서에 "순수" 마크다운만 포함하기를 원하는 사용자나 마크다운 문서를 HTML 이외의 것으로 렌더링하는 사용자를 위해 포함되었습니다.

<a name="md034"></a>

## `MD034` - 베어 URL 사용됨

태그: `links`, `url`

별칭: `no-bare-urls`

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 URL이나 이메일 주소가 꺾쇠괄호 없이 나타날 때마다 트리거됩니다:

```markdown
자세한 내용은 https://www.example.com/을 방문하거나 user@example.com으로 이메일을 보내세요.
```

이 문제를 해결하려면 URL이나 이메일 주소 주위에 꺾쇠괄호를 추가하세요:

```markdown
자세한 내용은 <https://www.example.com/>을 방문하거나 <user@example.com>으로 이메일을 보내세요.
```

URL이나 이메일 주소에 비 ASCII 문자가 포함된 경우 꺾쇠괄호가 있어도 의도한 대로 처리되지 않을 수 있습니다. 이러한 경우 [퍼센트 인코딩](https://en.m.wikipedia.org/wiki/Percent-encoding)을 사용하여 URL 및 이메일에 필요한 구문을 준수할 수 있습니다.

참고: 링크로 변환하지 않고 베어 URL이나 이메일을 포함하려면 코드 스팬으로 묶으세요:

```markdown
클릭할 수 없는 링크: `https://www.example.com`
```

참고: 다음 시나리오는 바로 가기 링크일 수 있으므로 이 규칙을 트리거하지 않습니다:

```markdown
[https://www.example.com]
```

참고: 다음 구문은 중첩된 링크가 바로 가기 링크일 수 있으므로(우선 순위가 높음) 이 규칙을 트리거합니다:

```markdown
[텍스트 [바로 가기] 텍스트](https://example.com)
```

이를 피하려면 내부 괄호를 모두 이스케이프 처리하세요:

```markdown
[링크 \[텍스트\] 링크](https://example.com)
```

근거: 꺾쇠괄호가 없으면 일부 마크다운 파서에서 베어 URL이나 이메일이 링크로 변환되지 않습니다.

<a name="md035"></a>

## `MD035` - 수평선 스타일

태그: `hr`

별칭: `hr-style`

매개변수:

- `style`: 수평선 스타일 (`string`, 기본값 `consistent`)

이 규칙은 문서에서 일관되지 않은 스타일의 수평선이 사용될 때 트리거됩니다:

```markdown
---

- - -

***

* * *

****
```

이 문제를 해결하려면 모든 곳에서 동일한 수평선을 사용하세요:

```markdown
---

---
```

구성된 스타일은 모든 수평선이 특정 문자열을 사용하도록 하거나 모든 수평선이 첫 번째 수평선과 일치하도록 할 수 있습니다(`consistent`).

근거: 일관된 서식은 문서 이해를 더 쉽게 만듭니다.

<a name="md036"></a>

## `MD036` - 제목 대신 강조 사용

태그: `emphasis`, `headings`

별칭: `no-emphasis-as-heading`

매개변수:

- `punctuation`: 구두점 문자 (`string`, 기본값 `.,;:!?。，；：！？`)

이 검사는 제목을 사용해야 하는 곳에 강조된(즉, 굵게 또는 기울임꼴) 텍스트를 사용하여 섹션을 구분하는 경우를 찾습니다:

```markdown
**내 문서**

Lorem ipsum dolor sit amet...

_다른 섹션_

Consectetur adipiscing elit, sed do eiusmod.
```

이 문제를 해결하려면 강조된 텍스트 대신 마크다운 제목을 사용하여 섹션을 나타내세요:

```markdown
# 내 문서

Lorem ipsum dolor sit amet...

## 다른 섹션

Consectetur adipiscing elit, sed do eiusmod.
```

참고: 이 규칙은 전체가 강조된 텍스트로 구성된 한 줄 단락을 찾습니다. 일반 텍스트 내에서 사용되는 강조, 여러 줄로 된 강조 단락 또는 구두점(일반 또는 전각)으로 끝나는 단락에서는 발생하지 않습니다. 규칙 MD026과 유사하게 구두점으로 인식되는 문자를 구성할 수 있습니다.

근거: 제목 대신 강조를 사용하면 도구가 문서 구조를 유추하는 것을 방해합니다. 추가 정보: <https://cirosantilli.com/markdown-style-guide#emphasis-vs-headers>.

<a name="md037"></a>

## `MD037` - 강조 마커 안의 공백

태그: `emphasis`, `whitespace`

별칭: `no-space-in-emphasis`

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 강조 마커(굵게, 기울임꼴)가 사용되었지만 마커와 텍스트 사이에 공백이 있을 때 트리거됩니다:

```markdown
여기에 ** 굵은 ** 텍스트가 있습니다.

여기에 * 기울임꼴 * 텍스트가 있습니다.

여기에 __ 굵은 __ 텍스트가 더 있습니다.

여기에 _ 기울임꼴 _ 텍스트가 더 있습니다.
```

이 문제를 해결하려면 강조 마커 주위의 공백을 제거하세요:

```markdown
여기에 **굵은** 텍스트가 있습니다.

여기에 *기울임꼴* 텍스트가 있습니다.

여기에 __굵은__ 텍스트가 더 있습니다.

여기에 _기울임꼴_ 텍스트가 더 있습니다.
```

근거: 강조는 별표/밑줄이 공백으로 둘러싸여 있지 않을 때만 구문 분석됩니다. 이 규칙은 공백으로 둘러싸인 곳을 감지하려고 시도하지만 저자가 강조된 텍스트를 의도한 것으로 보입니다.

<a name="md038"></a>

## `MD038` - 코드 스팬 요소 안의 공백

태그: `code`, `whitespace`

별칭: `no-space-in-code`

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 시작 또는 끝 백틱 옆에 불필요한 공백이 있는 내용이 포함된 코드 스팬에 대해 트리거됩니다:

```markdown
`일부 텍스트 `

` 일부 텍스트`

`   일부 텍스트   `
```

이 문제를 해결하려면 시작과 끝에서 추가 공백 문자를 제거하세요:

```markdown
`일부 텍스트`
```

참고: 단일 선행 *및* 후행 공백은 사양에서 허용되며 파서에 의해 잘려서 백틱으로 시작하거나 끝나는 코드 스팬을 지원합니다:

```markdown
`` `백틱` ``

`` 백틱` ``
```

참고: 입력에 단일 공백 패딩이 있는 경우 (불필요하더라도) 유지됩니다:

```markdown
` 코드 `
```

참고: 공백만 포함된 코드 스팬은 사양에서 허용되며 유지됩니다:

```markdown
` `

`   `
```

근거: 이 규칙 위반은 일반적으로 의도하지 않은 것이며 콘텐츠가 제대로 렌더링되지 않을 수 있습니다.

<a name="md039"></a>

## `MD039` - 링크 텍스트 안의 공백

태그: `links`, `whitespace`

별칭: `no-space-in-links`

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 링크 텍스트를 둘러싼 공백이 있는 링크에서 트리거됩니다:

```markdown
[ 링크 ](https://www.example.com/)
```

이 문제를 해결하려면 링크 텍스트를 둘러싼 공백을 제거하세요:

```markdown
[링크](https://www.example.com/)
```

근거: 일관된 서식은 문서 이해를 더 쉽게 만듭니다.

<a name="md040"></a>

## `MD040` - 펜스 코드 블록에는 언어가 지정되어야 합니다

태그: `code`, `language`

별칭: `fenced-code-language`

매개변수:

- `allowed_languages`: 언어 목록 (`string[]`, 기본값 `[]`)
- `language_only`: 언어만 필요 (`boolean`, 기본값 `false`)

이 규칙은 펜스 코드 블록이 사용되었지만 언어가 지정되지 않은 경우 트리거됩니다:

````markdown
```
#!/bin/bash
echo Hello world
```
````

이 문제를 해결하려면 코드 블록에 언어 지정자를 추가하세요:

````markdown
```bash
#!/bin/bash
echo Hello world
```
````

구문 강조 없이 코드 블록을 표시하려면 다음을 사용하세요:

````markdown
```text
코드 블록의 일반 텍스트
```
````

`allowed_languages` 매개변수를 구성하여 코드 블록이 사용할 수 있는 언어 목록을 지정할 수 있습니다. 언어는 대소문자를 구분합니다. 기본값은 `[]`이며 이는 모든 언어 지정자가 유효함을 의미합니다.

펜스 코드 블록의 정보 문자열에 추가 데이터가 있는 것을 방지할 수 있습니다. 그렇게 하려면 `language_only` 매개변수를 `true`로 설정하세요.

<!-- markdownlint-disable-next-line no-space-in-code -->
선행/후행 공백(예: `js `) 또는 기타 내용(예: `ruby startline=3`)이 있는 정보 문자열은 이 규칙을 트리거합니다.

근거: 언어를 지정하면 코드에 올바른 구문 강조를 사용하여 콘텐츠 렌더링이 향상됩니다. 추가 정보: <https://cirosantilli.com/markdown-style-guide#option-code-fenced>.

<a name="md041"></a>

## `MD041` - 파일의 첫 번째 줄은 최상위 제목이어야 합니다

태그: `headings`

별칭: `first-line-h1`, `first-line-heading`

매개변수:

- `allow_preamble`: 첫 번째 제목 앞에 콘텐츠 허용 (`boolean`, 기본값 `false`)
- `front_matter_title`: 프런트 매터에서 제목과 일치하는 RegExp (`string`, 기본값 `^\s*title\s*[:=]`)
- `level`: 제목 수준 (`integer`, 기본값 `1`)

이 규칙은 문서에 제목이 있는지 확인하기 위한 것이며 문서의 첫 번째 줄이 최상위([HTML][HTML] `h1`) 제목이 아닐 때 트리거됩니다:

```markdown
제목 없는 문서입니다
```

이 문제를 해결하려면 문서 시작 부분에 최상위 제목을 추가하세요:

```markdown
# 문서 제목

최상위 제목이 있는 문서입니다
```

GitHub의 프로젝트가 `README.md`의 제목에 이미지를 사용하는 것이 일반적이고 해당 패턴이 마크다운에서 잘 지원되지 않기 때문에 이 규칙에서는 HTML 제목도 허용됩니다. 예를 들어:

```markdown
<h1 align="center"><img src="https://placekitten.com/300/150"/></h1>

최상위 HTML 제목이 있는 문서입니다
```

경우에 따라 문서의 제목 제목 앞에 목차와 같은 텍스트가 올 수 있습니다. 이것은 접근성에 이상적이지는 않지만 `allow_preamble` 매개변수를 `true`로 설정하여 허용할 수 있습니다.

```markdown
서문 텍스트가 있는 문서입니다

# 문서 제목
```

[YAML][YAML] 프런트 매터가 있고 `title` 속성을 포함하는 경우(블로그 게시물에 일반적으로 사용됨), 이 규칙은 위반을 보고하지 않습니다. 프런트 매터에서 다른 속성 이름을 사용하려면 [정규식][RegExp]의 텍스트를 `front_matter_title` 매개변수를 통해 지정하세요. 이 규칙에서 프런트 매터 사용을 비활성화하려면 `front_matter_title`에 `""`를 지정하세요.

`level` 매개변수를 사용하여 `h1`이 외부에서 추가되는 경우 최상위 제목을 변경할 수 있습니다(예: `h2`로).

근거: 최상위 제목은 종종 문서의 제목 역할을 합니다. 추가 정보: <https://cirosantilli.com/markdown-style-guide#top-level-header>.

[HTML]: https://en.wikipedia.org/wiki/HTML
[RegExp]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions
[YAML]: https://en.wikipedia.org/wiki/YAML

<a name="md042"></a>

## `MD042` - 빈 링크 없음

태그: `links`

별칭: `no-empty-links`

이 규칙은 빈 링크가 발견될 때 트리거됩니다:

```markdown
[빈 링크]()
```

위반을 수정하려면 링크에 대한 대상을 제공하세요:

```markdown
[유효한 링크](https://example.com/)
```

빈 프래그먼트는 이 규칙을 트리거합니다:

```markdown
[빈 프래그먼트](#)
```

그러나 비어 있지 않은 프래그먼트는 그렇지 않습니다:

```markdown
[유효한 프래그먼트](#fragment)
```

근거: 빈 링크는 어디로도 연결되지 않으므로 링크로 작동하지 않습니다.

<a name="md043"></a>

## `MD043` - 필수 제목 구조

태그: `headings`

별칭: `required-headings`

매개변수:

- `headings`: 제목 목록 (`string[]`, 기본값 `[]`)
- `match_case`: 제목의 대소문자 일치 (`boolean`, 기본값 `false`)

이 규칙은 파일의 제목이 규칙에 전달된 제목 배열과 일치하지 않을 때 트리거됩니다. 파일 집합에 대한 표준 제목 구조를 적용하는 데 사용할 수 있습니다.

정확히 다음 구조를 요구하려면:

```markdown
# 제목
## 항목
### 세부 정보
```

`headings` 매개변수를 다음으로 설정하세요:

```json
[
    "# 제목",
    "## 항목",
    "### 세부 정보"
]
```

다음 구조와 같이 선택적 제목을 허용하려면:

```markdown
# 제목
## 항목
### 세부 정보 (선택 사항)
## 바닥글
### 참고 (선택 사항)
```

"0개 이상의 지정되지 않은 제목"을 의미하는 특수 값 `"*"` 또는 "1개 이상의 지정되지 않은 제목"을 의미하는 특수 값 `"+"`를 사용하고 `headings` 매개변수를 다음으로 설정하세요:

```json
[
    "# 제목",
    "## 항목",
    "*",
    "## 바닥글",
    "*"
]
```

프로젝트 이름과 같이 단일 필수 제목이 다양하도록 허용하려면:

```markdown
# 프로젝트 이름
## 설명
## 예시
```

"정확히 하나의 지정되지 않은 제목"을 의미하는 특수 값 `"?"`를 사용하세요:

```json
[
    "?",
    "## 설명",
    "## 예시"
]
```

오류가 감지되면 이 규칙은 첫 번째 문제가 있는 제목의 줄 번호를 출력합니다(그렇지 않으면 파일의 마지막 줄 번호를 출력합니다).

`headings` 매개변수는 단순성을 위해 "## 텍스트" ATX 제목 스타일을 사용하지만 파일은 지원되는 모든 제목 스타일을 사용할 수 있습니다.

기본적으로 문서의 제목 대소문자는 `headings`의 대소문자와 일치할 필요가 없습니다. 대소문자가 정확히 일치하도록 하려면 `match_case` 매개변수를 `true`로 설정하세요.

근거: 프로젝트는 유사한 콘텐츠 집합 전체에서 일관된 문서 구조를 적용하고자 할 수 있습니다.

<a name="md044"></a>

## `MD044` - 고유명사는 올바른 대소문자를 사용해야 합니다

태그: `spelling`

별칭: `proper-names`

매개변수:

- `code_blocks`: 코드 블록 포함 (`boolean`, 기본값 `true`)
- `html_elements`: HTML 요소 포함 (`boolean`, 기본값 `true`)
- `names`: 고유명사 목록 (`string[]`, 기본값 `[]`)

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 `names` 배열의 문자열 중 어느 하나라도 지정된 대소문자를 사용하지 않을 때 트리거됩니다. 프로젝트 및 제품 이름에 대한 표준 문자 대소문자를 적용하는 데 사용할 수 있습니다.

예를 들어, "JavaScript"라는 언어는 보통 'J'와 'S'를 모두 대문자로 쓰지만, 때로는 's'나 'j'가 소문자로 나타나기도 합니다. 올바른 대소문자를 적용하려면 `names` 배열에 원하는 문자 대소문자를 지정하세요:

```json
[
    "JavaScript"
]
```

때로는 특정 컨텍스트에서 고유명사가 다르게 대문자로 표기되기도 합니다. 이러한 경우 `names` 배열에 두 가지 형식을 모두 추가하세요:

```json
[
    "GitHub",
    "github.com"
]
```

코드 블록 및 스팬에 대해 이 규칙을 비활성화하려면 `code_blocks` 매개변수를 `false`로 설정하세요. HTML 요소 및 속성에 대해 이 규칙을 비활성화하려면 `html_elements` 매개변수를 `false`로 설정하세요(예: `a`/`href` 또는 `img`/`src`의 경로 일부로 고유명사를 사용하는 경우).

근거: 고유명사의 잘못된 대소문자 표기는 보통 실수입니다.

<a name="md045"></a>

## `MD045` - 이미지에는 대체 텍스트(alt text)가 있어야 합니다

태그: `accessibility`, `images`

별칭: `no-alt-text`

이 규칙은 이미지에 대체 텍스트 정보가 없을 때 위반을 보고합니다.

대체 텍스트는 일반적으로 다음과 같이 인라인으로 지정됩니다:

```markdown
![대체 텍스트](image.jpg)
```

또는 다음과 같이 참조 구문으로 지정됩니다:

```markdown
![대체 텍스트][ref]

...

[ref]: image.jpg "선택적 제목"
```

또는 다음과 같이 HTML로 지정됩니다:

```html
<img src="image.jpg" alt="대체 텍스트" />
```

참고: [HTML `aria-hidden` 속성][aria-hidden]을 사용하여 보조 기술에서 이미지를 숨기는 경우 이 규칙은 위반을 보고하지 않습니다:

```html
<img src="image.jpg" aria-hidden="true" />
```

대체 텍스트 작성에 대한 지침은 [W3C][w3c], [위키백과][wikipedia] 및 [기타 위치][phase2technology]에서 확인할 수 있습니다.

근거: 대체 텍스트는 접근성에 중요하며 이미지를 볼 수 없는 사람들을 위해 이미지의 내용을 설명합니다.

[aria-hidden]: https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-hidden
[phase2technology]: https://www.phase2technology.com/blog/no-more-excuses
[w3c]: https://www.w3.org/WAI/alt/
[wikipedia]: https://en.wikipedia.org/wiki/Alt_attribute

<a name="md046"></a>

## `MD046` - 코드 블록 스타일

태그: `code`

별칭: `code-block-style`

매개변수:

- `style`: 블록 스타일 (`string`, 기본값 `consistent`, 값 `consistent` / `fenced` / `indented`)

이 규칙은 원치 않거나 다른 코드 블록 스타일이 동일한 문서에 사용될 때 트리거됩니다.

기본 구성에서 이 규칙은 다음 문서에 대한 위반을 보고합니다:

<!-- markdownlint-disable code-block-style -->

    일부 텍스트.

        # 들여쓴 코드

    더 많은 텍스트.

    ```ruby
    # 펜스 코드
    ```

    더 많은 텍스트.

<!-- markdownlint-restore -->

이 규칙 위반을 수정하려면 일관된 스타일(들여쓰기 또는 코드 펜스)을 사용하세요.

구성된 코드 블록 스타일은 특정(`fenced`, `indented`)하거나 모든 코드 블록이 첫 번째 코드 블록과 일치하도록 요구할 수 있습니다(`consistent`).

근거: 일관된 서식은 문서 이해를 더 쉽게 만듭니다.

<a name="md047"></a>

## `MD047` - 파일은 단일 개행 문자로 끝나야 합니다

태그: `blank_lines`

별칭: `single-trailing-newline`

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 파일 끝에 단일 개행 문자가 없을 때 트리거됩니다.

규칙을 트리거하는 예시:

```markdown
# 제목

이 파일은 개행 없이 끝납니다.[EOF]
```

위반을 수정하려면 파일 끝에 개행 문자를 추가하세요:

```markdown
# 제목

이 파일은 개행으로 끝납니다.
[EOF]
```

근거: 일부 프로그램은 개행으로 끝나지 않는 파일에 문제가 있습니다.

추가 정보: [파일 끝에 새 줄을 추가하는 요점은 무엇입니까?][stack-exchange]

[stack-exchange]: https://unix.stackexchange.com/questions/18743/whats-the-point-in-adding-a-new-line-to-the-end-of-a-file

<a name="md048"></a>

## `MD048` - 코드 펜스 스타일

태그: `code`

별칭: `code-fence-style`

매개변수:

- `style`: 코드 펜스 스타일 (`string`, 기본값 `consistent`, 값 `backtick` / `consistent` / `tilde`)

이 규칙은 문서에서 펜스 코드 블록에 사용된 기호가 구성된 코드 펜스 스타일과 일치하지 않을 때 트리거됩니다:

````markdown
```ruby
# 펜스 코드
```

~~~ruby
# 펜스 코드
~~~
````

이 문제를 해결하려면 문서 전체에서 구성된 코드 펜스 스타일을 사용하세요:

````markdown
```ruby
# 펜스 코드
```

```ruby
# 펜스 코드
```
````

구성된 코드 펜스 스타일은 사용할 특정 기호(`backtick`, `tilde`)이거나 모든 코드 펜스가 첫 번째 코드 펜스와 일치하도록 요구할 수 있습니다(`consistent`).

근거: 일관된 서식은 문서 이해를 더 쉽게 만듭니다.

<a name="md049"></a>

## `MD049` - 강조 스타일

태그: `emphasis`

별칭: `emphasis-style`

매개변수:

- `style`: 강조 스타일 (`string`, 기본값 `consistent`, 값 `asterisk` / `consistent` / `underscore`)

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 문서에서 강조에 사용된 기호가 구성된 강조 스타일과 일치하지 않을 때 트리거됩니다:

```markdown
*텍스트*
_텍스트_
```

이 문제를 해결하려면 문서 전체에서 구성된 강조 스타일을 사용하세요:

```markdown
*텍스트*
*텍스트*
```

구성된 강조 스타일은 사용할 특정 기호(`asterisk`, `underscore`)이거나 모든 강조가 첫 번째 강조와 일치하도록 요구할 수 있습니다(`consistent`).

참고: 단어 내 강조는 like_this_one과 같이 내부 밑줄이 포함된 단어에 대한 원치 않는 강조를 피하기 위해 `asterisk`로 제한됩니다.

근거: 일관된 서식은 문서 이해를 더 쉽게 만듭니다.

<a name="md050"></a>

## `MD050` - 강조 스타일

태그: `emphasis`

별칭: `strong-style`

매개변수:

- `style`: 강조 스타일 (`string`, 기본값 `consistent`, 값 `asterisk` / `consistent` / `underscore`)

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 문서에서 강조에 사용된 기호가 구성된 강조 스타일과 일치하지 않을 때 트리거됩니다:

```markdown
**텍스트**
__텍스트__
```

이 문제를 해결하려면 문서 전체에서 구성된 강조 스타일을 사용하세요:

```markdown
**텍스트**
**텍스트**
```

구성된 강조 스타일은 사용할 특정 기호(`asterisk`, `underscore`)이거나 모든 강조가 첫 번째 강조와 일치하도록 요구할 수 있습니다(`consistent`).

참고: 단어 내 강조는 like__this__one과 같이 내부 밑줄이 포함된 단어에 대한 원치 않는 강조를 피하기 위해 `asterisk`로 제한됩니다.

근거: 일관된 서식은 문서 이해를 더 쉽게 만듭니다.

<a name="md051"></a>

## `MD051` - 링크 프래그먼트는 유효해야 합니다

태그: `links`

별칭: `link-fragments`

매개변수:

- `ignore_case`: 프래그먼트의 대소문자 무시 (`boolean`, 기본값 `false`)
- `ignored_pattern`: 추가 프래그먼트를 무시하기 위한 패턴 (`string`, 기본값 ``)

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 링크 프래그먼트가 문서의 제목에 대해 자동으로 생성된 프래그먼트와 일치하지 않을 때 트리거됩니다:

```markdown
# 제목 이름

[링크](#fragment)
```

이 문제를 해결하려면 링크 프래그먼트를 기존 제목의 생성된 이름(아래 참조)을 참조하도록 변경하세요:

```markdown
# 제목 이름

[링크](#heading-name)
```

일관성을 위해 이 규칙은 프래그먼트가 문자를 소문자로 변환하는 [GitHub 제목 알고리즘][github-heading-algorithm]과 정확히 일치하도록 요구합니다. 따라서 다음 예는 위반으로 보고됩니다:

```markdown
# 제목 이름

[링크](#Heading-Name)
```

프래그먼트와 제목 이름을 비교할 때 대소문자를 무시하려면 `ignore_case` 매개변수를 `true`로 설정할 수 있습니다. 이 구성에서는 이전 예가 위반으로 보고되지 않습니다.

또는 일부 플랫폼에서는 `{#named-anchor}` 구문을 제목 내에서 사용하여 특정 이름(소문자, 숫자, `-` 및 `_`로만 구성)을 제공할 수 있습니다:

```markdown
# 제목 이름 {#custom-name}

[링크](#custom-name)
```

또는 `id` 속성이 있는 모든 HTML 태그 또는 `name` 속성이 있는 `a` 태그를 사용하여 프래그먼트를 정의할 수 있습니다:

```markdown
<a id="bookmark"></a>

[링크](#bookmark)
```

`a` 태그는 제목이 적절하지 않은 시나리오나 프래그먼트 식별자의 텍스트를 제어해야 하는 경우에 유용할 수 있습니다.

[HTML 링크는 `#top`으로 스크롤하면 문서의 맨 위로 이동합니다][html-top-fragment]. 이 규칙은 해당 구문을 허용합니다(일관성을 위해 소문자 사용):

```markdown
[링크](#top)
```

이 규칙은 또한 GitHub에서 [문서의 특정 콘텐츠를 강조 표시][github-linking-to-content]하는 데 사용하는 사용자 지정 프래그먼트 구문을 인식합니다.

예를 들어, 20번째 줄에 대한 이 링크:

```markdown
[링크](#L20)
```

그리고 19번째 줄에서 시작하여 21번째 줄로 이어지는 콘텐츠에 대한 이 링크:

```markdown
[링크](#L19C5-L21C11)
```

일부 마크다운 생성기는 문서를 빌드할 때 `figure-`와 같은 고정 접두사와 증가하는 숫자 카운터를 결합하여 제목을 동적으로 만들고 삽입합니다. 이러한 생성된 프래그먼트를 무시하려면 `ignored_pattern` [정규식][RegEx] 매개변수를 일치하는 패턴(예: `^figure-`)으로 설정하세요.

근거: [GitHub 섹션 링크][github-section-links]는 마크다운 콘텐츠가 GitHub에 표시될 때 모든 제목에 대해 자동으로 생성됩니다. 이렇게 하면 문서 내의 다른 섹션에 직접 쉽게 연결할 수 있습니다. 그러나 제목이 변경되거나 제거되면 섹션 링크가 변경됩니다. 이 규칙은 문서 내에서 깨진 섹션 링크를 식별하는 데 도움이 됩니다.

섹션 링크는 CommonMark 사양의 일부가 **아닙니다**. 이 규칙은 [GitHub 제목 알고리즘][github-heading-algorithm]을 적용합니다. 이 알고리즘은 제목을 소문자로 변환하고, 구두점을 제거하고, 공백을 대시로 변환하고, 고유성을 위해 필요에 따라 증가하는 정수를 추가합니다.

[github-section-links]: https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#section-links
[github-heading-algorithm]: https://github.com/gjtorikian/html-pipeline/blob/f13a1534cb650ba17af400d1acd3a22c28004c09/lib/html/pipeline/toc_filter.rb
[github-linking-to-content]: https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/creating-a-permanent-link-to-a-code-snippet#linking-to-markdown#linking-to-markdown
[html-top-fragment]: https://html.spec.whatwg.org/multipage/browsing-the-web.html#scrolling-to-a-fragment
[RegEx]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions

<a name="md052"></a>

## `MD052` - 참조 링크 및 이미지는 정의된 레이블을 사용해야 합니다

태그: `images`, `links`

별칭: `reference-links-images`

매개변수:

- `ignored_labels`: 무시된 링크 레이블 (`string[]`, 기본값 `["x"]`)
- `shortcut_syntax`: 바로 가기 구문 포함 (`boolean`, 기본값 `false`)

마크다운의 링크와 이미지는 사용 시점에 링크 대상이나 이미지 소스를 제공하거나, 다른 곳에 정의하고 참조용 레이블을 사용할 수 있습니다. 참조 형식은 단락 텍스트를 깔끔하게 유지하고 여러 위치에서 동일한 URL을 쉽게 재사용할 수 있어 편리합니다.

참조 링크와 이미지에는 세 가지 종류가 있습니다:

```markdown
전체: [텍스트][레이블]
축약: [레이블][]
바로 가기: [레이블]

전체: ![텍스트][이미지]
축약: ![이미지][]
바로 가기: ![이미지]

[레이블]: https://example.com/label
[이미지]: https://example.com/image
```

해당 레이블이 정의되어 있으면 링크나 이미지가 올바르게 렌더링되지만, 레이블이 없으면 대괄호가 있는 텍스트로 표시됩니다. 기본적으로 이 규칙은 "전체" 및 "축약" 참조 구문에 대해 정의되지 않은 레이블을 경고하지만, "바로 가기" 구문은 모호하기 때문에 경고하지 않습니다.

`[예시]`라는 텍스트는 바로 가기 링크일 수도 있고 대괄호 안의 "예시"라는 텍스트일 수도 있으므로, "바로 가기" 구문은 기본적으로 무시됩니다. "바로 가기" 구문을 포함하려면 `shortcut_syntax` 매개변수를 `true`로 설정하세요. 그렇게 하면 정의된 바로 가기 링크가 아니고 링크나 이미지 자체가 아닌 대괄호 안의 모든 텍스트에 대해 경고가 발생합니다.

이 규칙 위반을 수정하려면 정의되지 않은 링크 및 이미지 레이블을 정의하세요:

```markdown
[텍스트][레이블]
![텍스트][이미지]

[레이블]: https://example.com/label
[이미지]: https://example.com/image
```

또는, 사용하지 않는 레이블 정의를 문서에서 제거할 수 있습니다.

때로는 대괄호 안의 단어가 링크를 의도한 것이 아니므로 무시해야 합니다. `ignored_labels` 매개변수를 사용하여 이 규칙에서 무시해야 하는 대소문자를 구분하지 않는 문자열 배열을 지정할 수 있습니다. 예를 들어, `[x]`는 일부 프로젝트의 이슈 템플릿에서 사용되며 배열에 `"x"`를 추가하여 무시할 수 있습니다.

근거: 정의되지 않은 참조 링크와 이미지는 의도한 대로 렌더링되지 않고 대괄호로 묶인 텍스트로 표시됩니다.

<a name="md053"></a>

<a name="md053"></a>

## `MD053` - 링크 및 이미지 참조 정의는 사용해야 합니다

태그: `images`, `links`

별칭: `link-image-reference-definitions`

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 문서에 정의되어 있지만 사용되지 않는 링크 및 이미지 참조 정의에 대해 경고합니다. 사용하지 않는 참조 정의를 제거하면 마크다운 파일이 더 작아지고 유지 관리가 쉬워집니다.

다음 예제는 `unused-label`이 정의되었지만 문서에서 참조되지 않기 때문에 이 규칙을 트리거합니다:

```markdown
[link][used-label]

[used-label]: https://example.com/used
[unused-label]: https://example.com/unused
```

이 규칙 위반을 수정하려면 사용하지 않는 참조 정의를 제거하세요:

```markdown
[link][used-label]

[used-label]: https://example.com/used
```

근거: 사용하지 않는 링크 및 이미지 참조 정의는 문서에 불필요한 내용을 추가합니다.

<a name="md054"></a>

## `MD054` - 링크 및 이미지 스타일

태그: `images`, `links`

별칭: `link-image-style`

매개변수:

- `autolink`: 자동 링크 허용 (`boolean`, 기본값 `true`)
- `collapsed`: 축약된 참조 링크 및 이미지 허용 (`boolean`, 기본값 `true`)
- `full`: 전체 참조 링크 및 이미지 허용 (`boolean`, 기본값 `true`)
- `inline`: 인라인 링크 및 이미지 허용 (`boolean`, 기본값 `true`)
- `shortcut`: 바로 가기 참조 링크 및 이미지 허용 (`boolean`, 기본값 `true`)
- `url_inline`: 인라인 링크로 URL 허용 (`boolean`, 기본값 `true`)

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

마크다운의 링크와 이미지는 사용 시점에 링크 대상이나 이미지 소스를 제공하거나, 문서의 다른 곳에 정의된 레이블을 참조할 수 있습니다. 세 가지 참조 형식은 단락 텍스트를 깔끔하게 유지하고 여러 위치에서 동일한 URL을 쉽게 재사용할 수 있어 편리합니다.

기본적으로 이 규칙은 모든 링크/이미지 스타일을 허용합니다.

`autolink` 매개변수를 `false`로 설정하면 자동 링크가 비활성화됩니다:

```markdown
<https://example.com>
```

`inline` 매개변수를 `false`로 설정하면 인라인 링크 및 이미지가 비활성화됩니다:

```markdown
[link](https://example.com)

![image](https://example.com)
```

`full` 매개변수를 `false`로 설정하면 전체 참조 링크 및 이미지가 비활성화됩니다:

```markdown
[link][url]

![image][url]

[url]: https://example.com
```

`collapsed` 매개변수를 `false`로 설정하면 축약된 참조 링크 및 이미지가 비활성화됩니다:

```markdown
[url][]

![url][]

[url]: https://example.com
```

`shortcut` 매개변수를 `false`로 설정하면 바로 가기 참조 링크 및 이미지가 비활성화됩니다:

```markdown
[url]

![url]

[url]: https://example.com
```

이 규칙 위반을 수정하려면 링크 또는 이미지를 허용된 스타일을 사용하도록 변경하세요. 이 규칙은 링크 또는 이미지를 `inline` 스타일(선호)로 변환할 수 있거나 링크를 `autolink` 스타일(이미지를 지원하지 않으며 절대 URL이어야 함)로 변환할 수 있는 경우 위반을 자동으로 수정할 수 있습니다. 이 규칙은 링크 또는 이미지를 `full`, `collapsed` 또는 `shortcut` 참조 스타일로 변환해야 하는 시나리오를 수정하지 않습니다. 이는 참조 이름을 지정하고 문서에 삽입할 위치를 결정해야 하기 때문입니다.

`url_inline` 매개변수를 `false`로 설정하면 동일한 절대 URL 텍스트/대상과 제목이 없는 인라인 링크 사용이 방지됩니다. 이러한 링크는 자동 링크로 변환될 수 있기 때문입니다:

```markdown
[https://example.com](https://example.com)
```

`url_inline` 위반을 수정하려면 더 간단한 자동 링크 구문을 대신 사용하세요:

```markdown
<https://example.com>
```

근거: 일관된 서식은 문서를 이해하기 더 쉽게 만듭니다. 자동 링크는 간결하지만 길고 혼란스러울 수 있는 URL로 나타납니다. 인라인 링크 및 이미지는 설명 텍스트를 포함할 수 있지만 마크다운 형식에서 더 많은 공간을 차지합니다. 참조 링크 및 이미지는 마크다운 형식에서 읽고 조작하기 더 쉽지만 별도의 링크 참조 정의가 필요합니다.

<a name="md055"></a>

## `MD055` - 테이블 파이프 스타일

태그: `table`

별칭: `table-pipe-style`

매개변수:

- `style`: 테이블 파이프 스타일 (`string`, 기본값 `consistent`, 값 `consistent` / `leading_and_trailing` / `leading_only` / `no_leading_or_trailing` / `trailing_only`)

이 규칙은 [GitHub Flavored Markdown 테이블][gfm-table-055]이 선행 및 후행 파이프 문자(`|`) 사용에 일관성이 없을 때 트리거됩니다.

기본적으로(`consistent` 스타일), 문서의 첫 번째 테이블의 헤더 행은 문서의 모든 테이블에 적용되는 스타일을 결정하는 데 사용됩니다. 특정 스타일을 대신 사용할 수 있습니다(`leading_and_trailing`, `leading_only`, `no_leading_or_trailing`, `trailing_only`).

이 테이블의 헤더 행에는 선행 및 후행 파이프가 있지만, 구분자 행에는 후행 파이프가 없고, 셀의 첫 번째 행에는 선행 파이프가 없습니다:

```markdown
| Header | Header |
| ------ | ------
  Cell   | Cell   |
```

이러한 문제를 해결하려면 모든 행의 시작과 끝에 파이프 문자가 있는지 확인하세요:

```markdown
| Header | Header |
| ------ | ------ |
| Cell   | Cell   |
```

테이블 바로 뒤에 오는 텍스트(즉, 빈 줄로 구분되지 않음)는 테이블의 일부로 처리되며(사양에 따라) 이 규칙을 트리거하지 않을 수 있습니다:

```markdown
| Header | Header |
| ------ | ------ |
| Cell   | Cell   |
이 텍스트는 테이블의 일부이며 다음 줄은 비어 있습니다.

일부 텍스트
```

근거: 일부 파서는 선행 또는 후행 파이프 문자가 없는 테이블에 문제가 있습니다. 선행/후행 파이프를 사용하면 시각적 명확성을 제공하는 데 도움이 될 수도 있습니다.

[gfm-table-055]: https://github.github.com/gfm/#tables-extension-

<a name="md056"></a>

## `MD056` - 테이블 열 개수

태그: `table`

별칭: `table-column-count`

이 규칙은 [GitHub Flavored Markdown 테이블][gfm-table-056]의 모든 행에 동일한 수의 셀이 없을 때 트리거됩니다.

이 테이블의 두 번째 데이터 행에는 셀이 너무 적고 세 번째 데이터 행에는 셀이 너무 많습니다:

```markdown
| Header | Header |
| ------ | ------ |
| Cell   | Cell   |
| Cell   |
| Cell   | Cell   | Cell   |
```

이러한 문제를 해결하려면 모든 행에 동일한 수의 셀이 있는지 확인하세요:

```markdown
| Header | Header |
| ------ | ------ |
| Cell   | Cell   |
| Cell   | Cell   |
| Cell   | Cell   |
```

테이블의 헤더 행과 구분자 행은 동일한 수의 셀을 가져야 테이블로 인식됩니다(사양에 따라).

근거: 행의 추가 셀은 일반적으로 표시되지 않으므로 데이터가 손실됩니다. 행의 누락된 셀은 테이블에 구멍을 만들고 누락을 시사합니다.

[gfm-table-056]: https://github.github.com/gfm/#tables-extension-

<a name="md058"></a>

## `MD058` - 테이블은 빈 줄로 둘러싸여야 합니다

태그: `table`

별칭: `blanks-around-tables`

수정 가능: 일부 위반 사항은 도구로 수정할 수 있습니다.

이 규칙은 테이블 앞이나 뒤에 빈 줄이 없을 때 트리거됩니다:

```markdown
Some text
| Header | Header |
| ------ | ------ |
| Cell   | Cell   |
> Blockquote
```

이 규칙 위반을 수정하려면 모든 테이블 앞뒤에 빈 줄이 있도록 하세요(테이블이 문서의 맨 처음이나 끝에 있는 경우 제외):

```markdown
Some text

| Header | Header |
| ------ | ------ |
| Cell   | Cell   |

> Blockquote
```

테이블 바로 뒤에 오는 텍스트(즉, 빈 줄로 구분되지 않음)는 테이블의 일부로 처리되며(사양에 따라) 이 규칙을 트리거하지 않을 수 있습니다:

```markdown
| Header | Header |
| ------ | ------ |
| Cell   | Cell   |
이 텍스트는 테이블의 일부이며 다음 줄은 비어 있습니다.

Some text
```

근거: 미적인 이유 외에도 일부 파서는 앞뒤에 빈 줄이 없는 테이블을 잘못 구문 분석합니다.

<a name="md059"></a>

## `MD059` - 링크 텍스트는 설명적이어야 합니다

태그: `accessibility`, `links`

별칭: `descriptive-link-text`

매개변수:

- `prohibited_texts`: 금지된 링크 텍스트 (`string[]`, 기본값 `["click here","here","link","more"]`)

이 규칙은 링크에 `[여기 클릭](...)` 또는 `[링크](...)`와 같은 일반적인 텍스트가 있을 때 트리거됩니다.

링크 텍스트는 설명적이어야 하며 링크의 목적을 전달해야 합니다(예: `[예산 문서 다운로드](...)` 또는 `[CommonMark 사양](...)`). 이는 때때로 컨텍스트 없이 링크를 제공하는 화면 판독기에게 특히 중요합니다.

기본적으로 이 규칙은 소수의 일반적인 영어 단어/구문을 금지합니다. 해당 단어/구문 목록을 사용자 지정하려면 `prohibited_texts` 매개변수를 `string` 배열로 설정하세요.

참고: 영어 이외의 언어의 경우 `prohibited_texts` 매개변수를 사용하여 해당 언어에 대한 목록을 사용자 지정하세요. 이 규칙이 모든 언어에 대한 번역을 갖는 것이 목표는 아닙니다.

참고: 이 규칙은 마크다운 링크를 확인합니다. HTML 링크는 무시됩니다.

추가 정보: <https://webaim.org/techniques/hypertext/>

<!-- markdownlint-configure-file {
  "no-inline-html": {
    "allowed_elements": [
      "a"
    ]
  }
} -->