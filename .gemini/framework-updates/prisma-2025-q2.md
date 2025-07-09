# Prisma ORM 2025년 2분기 주요 업데이트 분석

## 1. 서론

본 보고서는 2025년 3월 1일부터 6월 30일까지 릴리즈된 Prisma ORM의 주요 패치노트와 변경사항을 한국인 개발자들이 쉽게 이해하고 실제 코딩에 적용할 수 있도록 상세히 설명하는 것을 목적으로 합니다. Prisma ORM은 Node.js 및 TypeScript 환경에서 데이터베이스 작업을 간소화하는 강력한 도구로, 성능, 개발자 경험(DX), 기능 확장성을 지속적으로 개선하기 위해 활발하게 업데이트됩니다.

본 보고서는 특히 개발자의 코드 동작에 직접적인 영향을 미치거나, 새로운 기능을 활용하기 위해 알아야 할 핵심적인 내용들을 중심으로 다룹니다. 또한, 이 기간 동안 발생한 주요 버그 및 그에 대한 해결책도 함께 제시합니다.

### 2. 2025년 2분기 Prisma ORM 주요 릴리즈 요약

2025년 2분기(3월 1일 ~ 6월 30일) 동안 Prisma ORM은 여러 중요한 버전 업데이트를 진행했습니다.

* 버전 번호 | 릴리즈 날짜 | 주요 변경사항 (간략 요약)
* 6.11.0 | 2025년 7월 1일 | (2분기 직후 릴리즈, 2분기 개발 내용 포함 가능성)
* 6.10.1 | 2025년 6월 18일 | 6.10.0에서 발생한 `Prisma.skip` 및 날짜 필터 버그 수정

3월 11일 6.5.0 버전부터 6월 18일 6.10.1 버전까지 약 3개월 반 동안 6개의 마이너 버전과 다수의 패치 버전이 릴리즈되었습니다. 이러한 빠른 릴리즈 주기는 Prisma 팀이 매우 활발하게 개발을 진행하고 있음을 시사하며, 새로운 기능 도입 및 버그 수정이 빠르게 이루어지고 있음을 나타냅니다.

---

## III. 개발자가 반드시 알아야 할 핵심 변경사항

이 섹션에서는 2025년 2분기 동안 Prisma ORM에 도입된 주요 기능 및 아키텍처 변경사항 중 개발자의 코딩 방식과 애플리케이션 동작에 직접적인 영향을 미치는 핵심 내용들을 상세히 설명합니다.

### 1. 쿼리 컴파일러 (Query Compiler) GA 전환 및 영향

Prisma Client의 기존 쿼리 엔진을 더 빠르고 유연한 TypeScript 기반 구현체로 대체하는 새로운 쿼리 컴파일러 아키텍처가 점진적으로 도입되고 있습니다. 이 변화는 Rust 기반의 네이티브 바이너리 의존성을 제거하여 배포를 간소화하고, 장기적인 플랫폼 유연성을 확보하는 것을 목표로 합니다.

2025년 2분기 로드맵에 따르면, 쿼리 컴파일러는 SQL 기반 First-Class Databases (FCDBs)에 대해 GA(General Availability)로 전환될 예정입니다. Prisma Postgres, Postgres, SQLite는 이미 Preview 단계였으며, GA로의 전환이 계획되어 있습니다. MySQL, MariaDB도 이 기간 내에 GA 지원이 목표입니다. MongoDB는 비SQL 특성상 다음 분기에 GA 지원이 예상됩니다. 또한, 6.10.0 릴리즈에서는 MS SQL Server 및 PlanetScale에 대한 Rust 엔진 없는 Prisma ORM (Query Compiler) 지원이 Preview 단계로 확장되었습니다.

Rust 엔진 제거는 성능 향상과 배포 간소화(네이티브 바이너리 불필요)라는 명확한 이점을 제공합니다. 그러나 이를 사용하기 위해서는 `previewFeatures`에 "queryCompiler" 및 "driverAdapters"를 설정해야 하며, `@prisma/adapter-pg`와 같은 해당 데이터베이스용 드라이버 어댑터를 설치하고 `PrismaClient` 인스턴스화 시 어댑터를 전달해야 합니다.

### 2. 새로운 `prisma-client` 제너레이터 도입 및 `prisma-client-js` Deprecation

`prisma-client-js`의 현대적인 대안으로 ESM(ECMAScript Modules) 호환성을 갖춘 새로운 `prisma-client` 제너레이터가 도입되었습니다. 이 새로운 제너레이터는 번들러나 ESM-only 런타임에 의존하는 최신 환경에 더 적합한 깔끔하고 예측 가능한 코드베이스를 제공합니다. 주요 변경사항으로는 ESM 호환 출력, 여러 파일로 분할된 생성 코드, 그리고 `package.json` 및 기타 `node_modules` 컨벤션 미생성이 있습니다.

2025년 2분기 로드맵에 따라 Early Access에서 Preview를 거쳐 GA로 전환될 예정이며, GA에 도달하면 기존 `prisma-client-js` 제너레이터는 사용 중단(deprecated)될 계획입니다. 6.10.0 릴리즈에서는 새로운 `prisma-client` 제너레이터의 엔트리포인트가 변경되었습니다.

### 3. `multiSchema` 및 `views` 기능 GA 전환

`multiSchema` (다중 스키마 지원) 및 `views` (데이터베이스 뷰 지원) 기능이 Preview 단계에서 GA(General Availability)로 전환될 예정입니다. 이 기능들은 이미 프로덕션 환경에서 널리 사용되고 있었으며, 이번 GA 전환을 통해 API가 최종 확정되고 문서화가 완료될 것입니다.

`multiSchema`와 `views`가 Preview에서 GA로 전환되는 것은 Prisma ORM이 단순 CRUD를 넘어, 더 복잡하고 실제 엔터프라이즈 환경에서 요구되는 데이터베이스 패턴을 안정적으로 지원하는 방향으로 성숙하고 있음을 보여줍니다.

### 4. `prisma.config.ts` 설정 및 `.env` 파일 로딩 변경

Prisma는 더 이상 `.env` 파일을 자동으로 로딩하지 않습니다. 이러한 변경은 `prisma.config.ts` 파일을 통해 환경 구성을 직접 관리하도록 유도하며, Prisma 7에서는 `.env` 자동 로딩 기능이 완전히 제거될 예정입니다. 6.5.0 릴리즈부터 `prisma.config.ts` 파일이 Prisma Studio에서도 지원되어, 드라이버 어댑터가 활성화된 데이터베이스에 Prisma Studio를 통해 연결할 수 있게 되었습니다.

`.env` 파일 자동 로딩 제거는 Prisma가 환경 설정의 책임을 애플리케이션 또는 개발자에게 명시적으로 위임하려는 경향을 보여줍니다.

### 5. `prisma migrate --allow-reset` 옵션 제거

이전 버전에서 `prisma migrate` 명령이 데이터베이스에 깔끔하게 적용될 수 없을 때, 데이터 손실의 위험이 있는 프롬프트가 표시되었습니다. Prisma 6.5.0부터 이 프롬프트가 완전히 제거되었으며, 대신 적절한 오류 메시지와 함께 명령이 종료됩니다. 데이터베이스를 리셋해야 하는 경우, 이제 `prisma migrate reset` 명령을 명시적으로 실행해야 합니다.

이 변경은 개발자의 의도치 않은 데이터 손실을 방지하고, 데이터베이스 리셋과 같은 파괴적인 작업에 대해 개발자의 명시적인 동의를 얻으려는 Prisma의 강한 의지를 보여줍니다.

### 6. `$on` 및 `$extends` 메서드 체이닝 가능

이전 버전의 Prisma ORM에서 `$on` 클라이언트 메서드는 `void`를 반환하여 `$on()`과 `$extends()` 호출을 체이닝할 수 없었습니다. Prisma 6.5.0부터 이 문제가 해결되어 `$on`이 수정된 Prisma Client를 반환함으로써, 메서드 체이닝이 가능해졌습니다.

이 작은 변경은 개발자 경험(DX)을 크게 향상시킵니다. 메서드 체이닝은 현대 JavaScript 개발에서 매우 일반적인 패턴이며, 이를 지원함으로써 Prisma Client의 API가 더욱 직관적이고 "fluent"해집니다.

---

## IV. 주요 버그 및 해결책

2025년 2분기 동안 Prisma ORM은 여러 중요한 개선사항과 함께 몇 가지 주목할 만한 버그와 회귀(regression)도 발생했습니다.

### 1. Prisma 6.10.1에서 `Prisma.skip`과 날짜 필터 버그

Prisma ORM 6.10.1 버전에서 `Prisma.skip`을 `gte` (greater than or equal to) 또는 `lte` (less than or equal to)와 같은 날짜 필터와 함께 사용할 때 런타임 오류(`Argument _ref is missing`)가 발생하는 버그가 보고되었습니다.

이 버그는 6.9.0에서 6.10.0/6.10.1로의 마이너 버전 업데이트에서 발생한 회귀입니다. 이는 Prisma의 빠른 개발 주기와 기능 추가 과정에서 기존 기능의 안정성이 영향을 받을 수 있음을 보여줍니다. 이 버그는 동적 날짜 필터링 로직에 의존하는 애플리케이션의 핵심 기능을 마비시킬 수 있습니다. 현재로서는 Prisma ORM을 6.9.0 버전으로 다운그레이드하거나, `Prisma.skip` 대신 조건부 로직을 사용하여 필터를 동적으로 구성하는 것이 임시 해결책이 될 수 있습니다.

### 2. Prisma 6.7.0에서 `queryCompiler` 및 `driverAdapters` 사용 시 `Module not found` 버그

Prisma ORM 6.7.0 버전에서 `queryCompiler` 및 `driverAdapters` Preview 기능을 활성화하고 Next.js(특히 Turbopack 사용 시)와 함께 사용할 때, `Module not found: Can't resolve./enums.js`와 같은 모듈을 찾을 수 없다는 런타임 오류가 발생합니다.

이 버그는 애플리케이션 시작 시 충돌을 일으키는 치명적인(Critical) 버그이며, 개발 및 프로덕션 환경 모두에서 발생합니다. 현재까지 명확한 워크어라운드는 보고되지 않았으며, Prisma 팀의 패치가 필요합니다.

### 3. Prisma 6.6.0에서 `prismaSchemaFolder` 관련 Breaking Change

Prisma ORM 6.6.0 버전에서 `prismaSchemaFolder` Preview 기능과 관련된 브레이킹 체인지가 발생했습니다. 이 변경으로 인해 `prisma/schema/schema.prisma`와 같은 폴더 구조를 사용하는 기존 프로젝트의 빌드 및 개발 환경이 중단되었습니다.

이 변경은 마이너 버전(6.6.0)에서 브레이킹 체인지가 발생하여 Semantic Versioning(SemVer) 규칙을 위반했습니다. 해결책으로는 Prisma 6.6.0에 맞게 스키마 파일 경로를 조정하거나, 6.5.0과 같은 이전 버전으로 다운그레이드하는 것이 임시 해결책이 될 수 있습니다.

### 4. Prisma 6.8.1에서 SQLite 상대 경로 및 커스텀 출력 버그

Prisma ORM 6.8.1 (및 6.7.0) 버전에서 NestJS 예제 프로젝트에 SQLite를 사용하고 Prisma Client의 `output` 경로를 커스텀(`../src/generated/prisma` 등)으로 설정했을 때, SQLite 데이터베이스 파일 경로가 상대 경로(`file:./dev.db`)이면 런타임에 데이터베이스를 인식하지 못하는 버그가 발생합니다.

이 버그는 애플리케이션 충돌 및 데이터 접근 실패를 야기하는 "Critical" 심각도의 버그입니다. 해결책은 `schema.prisma` 파일 내 SQLite 데이터베이스의 `url`을 절대 경로로 변경하는 것입니다 (예: `url = "file:/Users/username/path/to/dev.db"`).

---

## V. 기타 중요한 개선사항 및 기능

2025년 2분기 동안 Prisma ORM은 개발자 경험(DX)을 크게 향상시키고 Prisma 생태계를 확장하는 다양한 기능들을 도입했습니다.

### 1. 로컬 Prisma Postgres 기능 대폭 개선

Prisma 6.8.0에서 `npx prisma dev` 명령을 통해 로컬 Prisma Postgres 인스턴스를 Docker 없이 PGLite 기반으로 실행할 수 있는 기능이 Early Access로 도입되었습니다. 6.9.0 릴리즈에서는 이 기능이 대폭 개선되어, `prisma dev` 호출 간 데이터베이스가 영구적으로 유지되고, 여러 로컬 인스턴스를 동시에 실행할 수 있게 되었으며, `prisma init` 명령이 기본적으로 로컬 Prisma Postgres 연결 문자열을 사용하도록 변경되었습니다. 6.10.0 릴리즈에서는 로컬 Prisma Postgres 인스턴스를 VS Code에서 시각적으로 관리할 수 있는 기능이 추가되었고, "Push to Cloud" 버튼을 통해 로컬 인스턴스를 클라우드에 쉽게 배포할 수 있게 되었습니다. 이제 로컬 Prisma Postgres 인스턴스에 `postgres://`로 시작하는 직접 TCP 연결 문자열을 사용하여 어떤 ORM이나 데이터베이스 도구(Drizzle, Kysely, DBeaver 등)로도 연결할 수 있습니다.

### 2. VS Code 확장 기능 및 Agent Mode 통합

Prisma 6.8.0부터 VS Code의 GitHub Copilot Agent Mode와 Prisma 확장이 통합되었습니다. 이를 통해 개발자는 IDE를 벗어나지 않고도 Copilot에게 Prisma CLI 명령(마이그레이션 실행, 데이터베이스 프로비저닝 등)을 요청할 수 있게 되었습니다. 6.9.0 릴리즈에서는 Prisma VS Code 확장에 Prisma Postgres를 관리할 수 있는 UI가 추가되었습니다. 6.10.0 릴리즈에서는 로컬 Prisma Postgres 인스턴스 관리 기능이 더욱 강화되었습니다.

### 3. 네이티브 Deno 지원

Prisma 6.8.0부터 Deno에 대한 네이티브 지원이 제공됩니다. 이는 Deno 런타임에서 Prisma ORM을 실행하기 위해 별도의 어댑터나 커스텀 빌드가 필요 없음을 의미합니다. 이전 Preview 기능이었던 `deno`는 이제 Deprecated 되었습니다.

### 4. PlanetScale 샤드 키 지원

Prisma ORM은 이제 PlanetScale에서 샤딩(sharding)을 네이티브로 지원합니다. 이는 Prisma 스키마에 `@shardKey` 및 `@@shardKey` 속성을 사용하여 모델의 필드를 샤드 키로 지정할 수 있음을 의미합니다.

### 5. `prisma migrate dev` 성능 향상

Prisma 6.10.0 릴리즈에서 `prisma migrate dev` 명령의 성능이 향상되었습니다. 특히 섀도우 데이터베이스와의 상호작용이 최적화되어, 일부 데이터베이스에서는 2배 빠른 속도를 보였습니다.

### 6. Prisma Postgres 백업 및 복원 기능 자동화

Prisma Postgres의 백업 및 복원 메커니즘이 크게 개선되었습니다. 이제 Prisma Console UI에서 몇 번의 클릭만으로 이전 백업을 쉽게 복원할 수 있게 되었습니다.

### 7. Prisma Postgres를 모든 ORM/도구와 연결 가능

Prisma Postgres는 이제 Prisma ORM을 사용하지 않고도 Drizzle, Kysely, TypeORM, psql 등 일반적인 PostgreSQL 직접 TCP 연결 문자열(`postgres://...`)을 통해 모든 ORM이나 데이터베이스 도구와 상호작용할 수 있게 되었습니다. 서버리스 환경을 위한 새로운 서버리스 드라이버 `@prisma/ppg` (Early Access)도 도입되었습니다. 6.10.0 릴리즈에서는 AI 도구가 원격 MCP 서버를 통해 Prisma Postgres 인스턴스를 관리할 수 있는 기능이 추가되었습니다.

---

## VI. 결론 및 권고사항

2025년 2분기 동안 Prisma ORM은 쿼리 컴파일러의 GA 전환, 새로운 `prisma-client` 제너레이터 도입, `multiSchema` 및 `views` 기능의 GA 전환 등 핵심 아키텍처 및 기능의 중대한 발전을 이루었습니다. 이는 성능 향상, 배포 간소화, 개발자 경험 개선, 그리고 서버리스/엣지 환경 지원 강화라는 장기적인 목표를 향한 Prisma의 강력한 의지를 보여줍니다.

동시에, `Prisma.skip` 버그 (6.10.1), `Module not found` 버그 (6.7.0), `prismaSchemaFolder` 관련 브레이킹 체인지 (6.6.0), SQLite 상대 경로 버그 (6.8.1) 등 몇 가지 중요한 버그와 회귀도 발생했습니다. 특히 마이너 버전에서의 브레이킹 체인지는 SemVer 규칙 위반으로, 개발팀의 업데이트 전략에 신중함을 요구합니다.

이러한 변화와 발생한 문제들을 종합적으로 고려할 때, Prisma ORM을 사용하는 개발팀에게 다음과 같은 권고사항이 제시됩니다.

* 업데이트 전 철저한 테스트: 새로운 버전으로 업데이트하기 전에 개발 및 스테이징 환경에서 충분한 통합 및 회귀 테스트를 수행하여 잠재적인 버그나 브레이킹 체인지를 조기에 발견해야 합니다.
* 변경 로그 및 공식 문서 면밀히 검토: 각 버전의 변경 로그(Release Notes)와 공식 문서를 꼼꼼히 확인하여 코드 수정이 필요한 변경사항이나 새로운 기능의 사용법을 숙지해야 합니다.
* 의존성 버전 관리 강화: `package.json`에서 `~` (패치 버전만 허용) 또는 특정 버전을 명시하여 의도치 않은 마이너 버전 업데이트로 인한 문제를 방지하는 것을 고려해야 합니다.
* Preview 기능 사용 시 주의: Preview 기능은 강력하지만, 아직 API가 확정되지 않았거나 예상치 못한 버그가 있을 수 있으므로, 프로덕션 환경에서는 신중하게 도입해야 합니다.
* 환경 변수 관리 방식 변경 준비: `.env` 파일 자동 로딩 제거에 대비하여 `prisma.config.ts`를 활용하거나 애플리케이션 레벨에서 환경 변수를 명시적으로 로딩하는 방식으로 전환을 준비해야 합니다.
* 로컬 개발 환경 적극 활용: 개선된 로컬 Prisma Postgres 기능과 VS Code 통합을 적극적으로 활용하여 개발 생산성을 높이고, 클라우드 의존성을 줄이는 개발 워크플로우를 구축하는 것을 권장합니다.

Prisma는 활발한 개발이 이루어지는 프로젝트이므로, 최신 정보와 상세한 가이드를 얻기 위해 Prisma 공식 문서(`https://www.prisma.io/docs`), 공식 블로그(`https://www.prisma.io/blog`), 그리고 GitHub 저장소(`https://github.com/prisma/prisma`)의 릴리즈 페이지 및 이슈 트래커를 주기적으로 확인하는 것이 매우 중요합니다.
