---

지금까지의 대화를 참고해 아래 템플릿에 채워야 할 부분들을 정리해줘. 필요하다면 너가 문서 형식을 임의적으로 필요한만큼 추가하거나 수정해도 상관없고 아까 답변과 다르게 생각이 바뀌어 변경사항이 있다면 수정해도 좋음.
 기능구현시 사용하면 좋은 라이브러리나 서비스도 너가 알아서 추천해도 좋아. 답변시 내가 마크다운 문법 형식을 눈으로 볼수 있게끔 하고, 너가 답변한 것에 복사 붙여넣기 버튼도 내가 사용할 수 있게 해줘. 그래야 .md 파일에 쉽게 붙여넣을수 있으니까. 마크다운 형식 작성시 볼드체(**)는 사용하지마. 필요없는 부분은 적당히 생략하고 작성해도 좋아. "개발 환경", "코딩 규칙 및 스타일
", "AI 지식 베이스 및 문서화", "LLM과의 협업" 항목은 고정 템플릿으로 사용하려 하므로 수정할 필요 없이 그대로 출력하면 돼.

---

# 프로젝트 가이드라인

* 이 문서는 LLM이 프로젝트의 맥락을 정확히 이해하고, 더 효율적이며 프로젝트에 특화된 도움을 제공하도록 돕기 위해 작성되었습니다.

## 프로젝트 개요 및 목표

* 프로젝트 이름: [프로젝트 영어 이름]
* 목표: 이 프로젝트의 주요 목적과 달성하고자 하는 바를 간략하게 설명합니다.
* 핵심 가치: 사용자에게 제공하고자 하는 가장 중요한 경험이나 가치는 무엇인지 설명합니다. (예: '직관적인 인터페이스', '개인화된 쇼핑 경험', '신뢰할 수 있는 상품', '초고속 로딩')
* 디자인 톤 앤 매너: 전반적인 디자인 톤 앤 매너는 무엇인지 설명. (예: '미니멀리즘', '모던하고 깔끔한', '따뜻하고 아늑한', '생동감 넘치는')
* 대상 사용자: 이 서비스가 타겟으로 하는 주요 사용자층은 누구인지 설명합니다. (예: '20-30대 패션에 민감한 여성', '친환경 제품에 관심 있는 가족 단위 구매자')
* 주요 경쟁 서비스: 이 프로젝트가 경쟁하게 될 주요 서비스나 웹사이트는 무엇이며, 이들과 차별화되는 점은 무엇인지 설명합니다.
* 비즈니스 모델: 서비스의 주요 수익 모델은 무엇인지 설명합니다. (예: '상품 판매 수수료', '광고', '프리미엄 멤버십')
* 향후 확장 계획 (Roadmap): 1단계 출시 후 계획하고 있는 추가 기능이나 서비스 확장 아이디어 설명. (예: '모바일 앱 개발', '판매자 입점 기능', '글로벌 배송 확대'). 또한 어떤 기술적 스케일업/스케일아웃 전략을 염두에 두고 있는지 설명합니다.
* 프로젝트 깃헙 레포지토리: `https://github.com/0disoft/[프로젝트 영어 이름]`

---

## 기술 스택 및 설정 전략

* 개발 유형: [선택된 개발 유형]
* 프론트엔드/풀스택 프레임워크: [선택된 프레임워크]
  * 핵심 기능 라이브러리/서비스: (예: 상태 관리 라이브러리, 폼 관리 라이브러리)
* 스타일링 툴: [선택된 스타일링 툴]
* 백엔드: [선택된 백엔드/접근 방식]
  * 핵심 기능 라이브러리/서비스: (예: 인증 라이브러리, 파일 업로드 라이브러리)
* 런타임 환경 및 패키지 매니저: [선택된 런타임 및 패키지 매니저]
* 데이터베이스: [선택된 데이터베이스/BaaS]
  * ORM/데이터베이스 클라이언트: [선택된 ORM/클라이언트, 예: Prisma]
* 배포 전략 및 플랫폼: [선택된 배포 플랫폼]
  * CI/CD 파이프라인: (예: Vercel/Netlify 내장 CI/CD, GitHub Actions, Dagger)
* 지역화 (l10n) 전략: [선택된 지역화 방식]
  * 관련 라이브러리/서비스: (예: react-i18next, next-i18n, Transifex)
* API 클라이언트 (테스트 및 문서화): [선택된 API 클라이언트, 예: Bruno]
* 데이터 유효성 검사: [선택된 유효성 검사 라이브러리, 예: Zod]

---

## 개발 환경

* OS: 윈도우 11
* IDE: VS Code
* 터미널: Git Bash
* 쉘 프롬프트 커스터마이저: Starship

---

## 구현해야 할 페이지와 기능

* (이전 답변에서 정리된 내용을 포함하되, 기능별 우선순위나 MVP(Minimum Viable Product)에 포함될 기능과 이후 추가될 기능을 구분하여 명시하면 좋습니다.)
* 핵심 페이지:
  * [이전 답변의 필수 핵심 페이지 목록 포함]
* 주요 기능:
  * [이전 답변의 가치를 높이는 핵심 기능 목록 포함]
* 추가 고려 기능 (선택 사항):
  * 실시간 알림 (웹소켓 기반)
  * 다중 파일 업로드 기능
  * 소셜 공유 기능
  * AI 기반 상품 추천 고도화
  * 사용자 등급별 혜택 시스템

---

## 코딩 규칙 및 스타일

* 변수/함수명: 명확하고 의미를 쉽게 유추할 수 있는 이름을 사용합니다. (예: `getData` 대신 `fetchUserProfileData`)
* 코드 라인 길이: 한 줄의 최대 길이는 80자 (띄어쓰기 포함)를 지킵니다. (예외: 긴 URL, 코드 블록, 테이블, 제목)
* 함수 길이: 함수의 최대 길이는 50라인 (주석 및 줄바꿈 포함)을 지킵니다.
* 들여쓰기: 특별한 경우가 아니라면 2칸 공백을 사용합니다.
* 커밋 메시지: Gitmoji 형식을 사용한 영어 코멘트를 작성합니다.
* Single Source of Truth: 서비스 이름, API 엔드포인트, 주요 설정 값 등은 코드 곳곳에 하드코딩하지 않습니다. 모든 설정은 별도의 설정 파일이나 상수 모듈에 모아서, 딱 한 군데만 수정하면 전체에 적용되도록 만듭니다.
* 모듈성: 코드를 작고 응집도 높은 모듈로 분리하여 재사용성과 유지보수성을 높입니다.
* 가독성: 다른 개발자가 코드를 쉽게 이해할 수 있도록 작성합니다.
* 성능: 특히 고빈도 호출이 예상되는 API나 데이터 처리 로직에서는 성능을 최우선으로 고려합니다.
* 보안: 사용자 데이터 보호, 인증/인가, 입력 유효성 검사 등 보안을 항상 염두에 둡니다. (프로젝트 특성에 따른 추가 보안 고려사항 명시 가능: 예: XSS/CSRF 방지, SQL Injection 방지, 비밀번호 해싱)
* 데이터 정확성: 제공되는 데이터(특히 계산 결과)는 최신 법규와 공식 데이터를 기반으로 최대한의 정확성을 보장해야 합니다. 부동 소수점 오류 방지를 위한 정밀한 숫자 연산 라이브러리 사용을 권장합니다.
* 에러 처리: 모든 잠재적인 에러 상황에 대해 견고하고 명확한 에러 처리 로직을 구현합니다. (예: 서버 측 폼 액션에서 `fail` 함수를 사용하여 개발자에게 구체적인 오류 메시지 전달)
* 접근성 (A11y): 웹 애플리케이션 개발 시 웹 접근성 표준을 준수하여 모든 사용자가 쉽게 접근하고 이용할 수 있도록 합니다. (처음부터 강력한 접근성 보장)
* 주석 작성 규칙: 주석은 '왜' 이 코드가 존재하는지, 코드를 변경했다면 '왜' 변경했는지, 특정 비즈니스 논리나 기술적 절충안 등 코드만으로 설명하기 어려운 배경 정보를 제공하는데 사용합니다.
* 마크다운 작성 규칙: 다음은 자주 발생하는 마크다운 오류이므로 항상 유의하여 작성합니다.
  * `MD030/list-marker-space`: 리스트 마커 다음에는 정확히 한 칸의 공백을 사용합니다.
  * `MD022/blanks-around-headings`: 제목(Heading)의 위와 아래에는 항상 한 줄씩 비워둡니다.
  * `MD047/single-trailing-newline`: 파일의 마지막은 항상 하나의 개행 문자로 끝나야 합니다.
  * `MD024/no-duplicate-heading`: 한 문서 내에서 동일한 내용의 제목을 중복으로 사용하지 않습니다.
  * `MD007/ul-indent`: 중첩되지 않은 리스트는 2칸 들여쓰기를 사용합니다.

---

## AI 지식 베이스 및 문서화

LLM의 학습 데이터 한계(Knowledge Cutoff)를 극복하고, 프로젝트의 기술적 맥락과 최신 정보를 정확하게 파악하기 위해 이 중앙화된 지식 베이스를 관리합니다. 코드를 분석하거나 생성할 때, 항상 이 내부 지식 베이스의 문서를 가장 먼저 확인하십시오.

* 위치: LLM과 관련된 모든 문서는 `/.gemini/` 폴더 아래에 중앙화하여 관리합니다.

### 프레임워크 최신 정보 (`/.gemini/.changelogs/` 및 `/.gemini/.context7/`)

* 목적: LLM의 지식 격차를 해소하고, 최신 프레임워크 정보를 효과적으로 활용하기 위해 두 가지 유형의 문서를 관리합니다.
  * `/.gemini/.changelogs/`: 주요 프레임워크의 분기별(예: 25년 상반기, 하반기) 최신 업데이트 중, 반드시 알아야 할 핵심 변경 사항이나 주요 기능 추가 내용을 요약하여 저장합니다.
  * `/.gemini/.context7/`: `Context7` MCP를 통해 방대한 공식 문서를 검색하기 전, 로컬에서 자주 사용하는 기능의 일반적인 사용법이나 코드 스니펫을 빠르게 확인하기 위한 용도로 사용됩니다.
* LLM 활용 가이드:
  * 프레임워크의 최신 버전과 관련된 작업을 수행할 때는 먼저 `/.gemini/.changelogs/` 폴더를 확인하여 중요한 변경 사항(breaking changes 등)을 코드에 반영하십시오.
  * 특정 기능의 기본적인 사용법이나 예제가 필요할 때는 `/.gemini/.context7/` 폴더의 문서를 우선적으로 참조하여 빠르게 정보를 얻고, 더 상세한 내용이 필요할 경우에 `Context7` MCP를 활용하십시오.

---

## LLM과의 협업

* LLM이 문제 해결을 완료하고 사용자로부터 긍정적인 응답을 받으면, `.gemini/.commitmsg.txt` 파일에 Gitmoji 형식의 영어 커밋 메시지를 작성합니다.
* LLM이 작업을 완료하고 사용자로부터 긍정적인 응답을 받으면, `.gemini/tasks/` 폴더에 상세 작업 기록을 Markdown 파일로 작성합니다. 상세 형식은 `.gemini/tasks/task_record_example.md` 파일을 참조하십시오.
* LLM의 답변(응답)은 한국어로 합니다.
* 폴더명과 파일명은 영어로 작성해야 합니다.
* 변수명, 클래스명 등은 영어로 작성해야 합니다.
* 코드 주석은 한국어로 작성합니다.
* 오류 로그는 한국어로 작성합니다.
* 홈페이지에 표시되는 메시지나 서비스를 이용하게 될 사용자와의 상호작용 메시지는 페이지의 기본 언어 설정 및 다국어 지원 여부에 따라 적절한 언어(예: 영어 또는 한국어)로 작성합니다.
