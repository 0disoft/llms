# Prisma ORM 2025년 1분기 주요 업데이트 분석

## 1. 서론

### 보고서의 목적 및 범위

본 보고서는 2025년 1월 1일부터 2025년 3월 31일까지 릴리즈된 Prisma ORM의 패치 노트를 심층 분석하여, 개발자가 코딩 시 반드시 인지하고 적용해야 할 주요 변경사항들을 명확하고 이해하기 쉽게 정리하는 것을 목표로 합니다. 본 보고서는 애플리케이션 개발에 사용되는 Prisma ORM의 2025년 1분기 릴리즈에 집중합니다.

### 2025년 1분기 Prisma ORM 주요 릴리즈 개요

Prisma ORM은 일반적으로 약 2주마다 새로운 버전을 릴리즈하는 빠른 개발 주기를 가지고 있습니다. 2025년 1분기에는 Prisma ORM 6.5.0 버전이 2025년 3월 11일에 릴리즈되었으며, 6.6.0 버전은 4월 11일에 릴리즈되었으나 해당 버전의 많은 핵심 커밋이 3월에 이루어졌습니다.

또한, Prisma ORM 5.0.0은 2023년 7월 11일에 릴리즈되었지만, 해당 버전에서 도입된 주요 변경사항들이 개발자가 구버전에서 최신 Prisma ORM 버전(예: 6.x.x)으로 업그레이드할 때 호환성 문제에 직면할 가능성이 높으므로 본 보고서에 포함됩니다.

---

## 2. 개발자가 반드시 알아야 할 주요 변경사항: 호환성 및 코드 수정

이 섹션에서는 2025년 1분기 Prisma ORM 릴리즈(주로 6.5.0 및 6.6.0의 3월 커밋)와 Prisma ORM 5.0.0에서 도입된, 개발자가 코드를 수정하거나 환경을 업데이트해야 하는 필수적인 변경사항들을 상세히 설명합니다.

### 2.1. 주요 API 및 코드베이스 변경

Prisma ORM은 성능 최적화, 코드 일관성 강화, 그리고 오래되거나 비권장되는 API의 정리를 통해 더욱 견고하고 예측 가능한 ORM으로 발전하고 있습니다.

* JSON 프로토콜 일반 가용성 및 `jsonProtocol` 프리뷰 기능 제거
  * Prisma ORM 5.0.0부터 `jsonProtocol` 프리뷰 기능이 정식 기능(GA)으로 전환되었습니다. `schema.prisma` 파일의 `generator client` 블록에 `jsonProtocol`을 `previewFeatures`로 명시했었다면, 이제 이 설정을 제거해야 합니다.
* 배열 단축키 지원 중단
  * Prisma ORM 5.0.0부터 특정 연산자(`OR`, `in`, `notIn`, PostgreSQL JSON path 필드 단축키, Scalar list 단축키, MongoDB Composite list 단축키)에 대한 "배열 단축키" 사용이 더 이상 지원되지 않습니다. 이제 배열 값을 명시적으로 배열 `[]`로 래핑해야 합니다.
* `rejectOnNotFound` 매개변수 제거
  * 더 이상 사용되지 않는 `rejectOnNotFound` 매개변수가 Prisma ORM 5.0.0에서 완전히 제거되었습니다.
* `runtime/index.js` 파일 제거 및 API 임포트 변경
  * Prisma ORM 5.0.0부터 생성된 클라이언트에서 `runtime/index.js` 파일이 제거되었습니다. 이제 `@prisma/client/runtime`에서 공개 API를 임포트하던 코드는 `@prisma/client`에서 `Prisma` 객체를 통해 접근해야 합니다. (예: `new Prisma.Decimal()` 대신 `new Decimal()` 사용)
* `beforeExit` 훅 제거
  * Prisma ORM 5.0.0에서 라이브러리 엔진의 `beforeExit` 훅이 제거되었습니다. 이 훅을 사용하여 종료 관련 작업을 수행하던 코드는 더 이상 작동하지 않습니다. NestJS 사용자의 경우, 커스텀 `enableShutdownHooks` 메서드를 제거하고 NestJS 내장 `app.enableShutdownHooks()`를 사용해야 합니다.
* 생성된 타입 변경사항
  * Prisma ORM 5.0.0에서 생성된 타입 정의의 정확성이 향상되었습니다. `UserNullableRelationFilter`와 같이 nullable 관계에 대해 더 정확한 타입이 생성되고, 동일한 역관계 속성 이름을 가진 두 개의 외래 키가 있을 때의 이름 충돌 문제가 해결되었습니다.

### 2.2. CLI 도구 및 설정 변경

Prisma CLI는 "마법 같은" 암시적 동작에서 "명시적이고 예측 가능한" 설정으로 전환되고 있습니다.

* 더 이상 사용되지 않는 CLI 플래그 제거
  * Prisma ORM 5.0.0부터 `--preview-feature` (`db execute`, `db seed`, `db diff`), `--experimental` 및 `--early-access-feature` (`migrate`), `--force` / `-f` (`db push`), `--experimental-reintrospection` 및 `--clean` (`db pull`) 등 여러 CLI 플래그가 제거되었습니다. `db push --force`는 `db push --accept-data-loss`로 대체해야 합니다.
* `db push --force` 대체 및 `migrate reset` 동작 변경
  * `db push --force` 플래그가 제거되고 `db push --accept-data-loss`로 대체되었습니다. 이와 함께, Prisma ORM 6.5.0부터는 `migrate` 명령어가 데이터베이스에 깔끔하게 적용될 수 없다고 판단될 경우, 이전에 묻던 프롬프트가 제거되고 오류 메시지와 함께 종료됩니다.
* `experimentalFeatures` 속성 제거
  * `generator` block 내의 `experimentalFeatures` 속성이 제거되었으며, `previewFeatures`로 업데이트되어야 합니다.
* `migration-engine` 이름 변경
  * `prisma migrate` 및 `prisma db` 명령어를 담당하는 내부 엔진의 이름이 `migration-engine`에서 `schema-engine`으로 변경되었습니다.
* `prismaSchemaFolder` 관련 변경사항
  * Prisma ORM 6.6.0부터 `prismaSchemaFolder` 관련 주요 변경사항이 도입되었습니다. 이제 스키마 폴더의 경로를 항상 명시적으로 제공해야 합니다.
* 자동 `.env` 파일 로딩 제거
  * Prisma ORM 6.x.x 버전부터 `.env` 파일의 자동 로딩이 점진적으로 제거되고 있습니다.

### 2.3. 최소 버전 요구사항 업데이트

Prisma는 최신 기술 스택과의 호환성을 유지하고, 더 이상 지원되지 않는 구버전의 기술 부채를 줄이는 데 중점을 둡니다.

* 설명: Prisma ORM 5.0.0부터 다음과 같은 최소 버전 요구사항이 업데이트되었습니다:
  * Node.js: 최소 지원 버전이 16.13.0으로 변경되었으며, Node.js v16은 마지막 주요 버전이 될 예정입니다. Node.js LTS v18.x로의 업그레이드가 강력히 권장됩니다.
  * TypeScript: 최소 지원 버전이 4.7로 변경되었습니다.
  * PostgreSQL: 최소 지원 버전이 9.6으로 변경되었습니다.
  * Prisma Client 임베디드 SQLite: 임베디드 SQLite 버전이 3.35.4에서 3.41.2로 업그레이드되었습니다.
* 영향: 개발 환경의 Node.js, TypeScript, 데이터베이스 버전이 최소 요구사항을 충족하지 못하면 Prisma ORM을 사용할 수 없거나 예기치 않은 문제가 발생할 수 있습니다.
* 개발자 조치: 개발 및 배포 환경의 모든 구성 요소가 업데이트된 최소 버전 요구사항을 충족하는지 확인하고 필요에 따라 업그레이드를 수행해야 합니다.

### 2.4. 데이터베이스 공급자별 변경사항

Prisma는 특정 데이터베이스에 대한 최적화 및 정확한 타입 매핑을 위해 더 세분화된 공급자 정의를 요구하고 있습니다.

* CockroachDB에 `cockroachdb` 공급자 필수화
  * Prisma ORM 5.0.0부터 CockroachDB 데이터베이스에 연결할 때 `cockroachdb` 공급자(`provider = "cockroachdb"`)가 필수가 되었습니다. 이전에는 `postgresql` 공급자를 사용할 수 있었으나 더 이상 허용되지 않습니다.
  * `postgresql` 공급자를 사용하여 CockroachDB에 연결하던 기존 프로젝트는 `cockroachdb` 공급자로 변경해야 합니다.

표 1: 주요 Breaking Changes 및 필요한 조치

| 변경사항 | 관련 Prisma ORM 버전 | 영향 | 개발자 조치 |
|---|---|---|---|
| `jsonProtocol` 프리뷰 기능 제거 | 5.0.0 | `previewFeatures` 설정 시 경고/오류 발생 가능 | `schema.prisma`에서 `previewFeatures = ["jsonProtocol"]` 제거 |
| 배열 단축키 지원 중단 | 5.0.0 | `OR`, `in`, `notIn` 등 배열 기반 쿼리 런타임 오류 | 단일 요소도 `[]`로 감싸도록 코드 수정 |
| `rejectOnNotFound` 매개변수 제거 | 5.0.0 | 해당 매개변수 사용 코드 작동 중단 | `null` 결과에 대한 명시적 오류 처리 로직 추가 |
| `runtime/index.js` 제거 및 API 임포트 변경 | 5.0.0 | `@prisma/client/runtime` 임포트 오류 | `import { Prisma } from '@prisma/client';`로 변경 후 `Prisma` 객체 통해 접근 |
| `beforeExit` 훅 제거 | 5.0.0 | 해당 훅 사용 코드 작동 중단 | Node.js 네이티브 종료 훅(`process.on('exit',...)`) 사용 |
| 생성된 타입 변경 | 5.0.0 | nullable 관계 및 이름 충돌 해결로 인한 타입 오류 | Prisma Client 재생성 후 새로운 타입 정의에 맞춰 코드 수정 |
| 더 이상 사용되지 않는 CLI 플래그 제거 | 5.0.0 | 기존 스크립트/CI/CD 파이프라인 오류 | 제거된 플래그 제거, `db push --force`는 `db push --accept-data-loss`로 대체 |
| `migrate reset` 프롬프트 제거 | 6.5.0 | 데이터 손실 경고 프롬프트 사라짐 | 데이터베이스 리셋 시 `prisma migrate reset` 명시적 실행 필요 |
| `experimentalFeatures` 속성 제거 | 5.0.0 | 설정 오류 | `schema.prisma`에서 `experimentalFeatures`를 `previewFeatures`로 변경 |
| `migration-engine` 이름 변경 | 5.0.0 | 엔진 파일 이름 참조 시 오류 | `migration-engine` 참조를 `schema-engine`으로 변경 |
| `prismaSchemaFolder` 명시적 경로 요구 | 6.6.0 | 암시적 스키마 폴더 위치 사용 시 오류 | `--schema` 옵션, `package.json`, `prisma.config.ts`에 스키마 경로 명시 |
| 자동 `.env` 파일 로딩 제거 | 6.x.x (Prisma 7에서 완전 제거 예정) | CLI 명령어 실행 시 환경 변수 문제 발생 가능 | `prisma.config.ts`로 환경 설정 관리 또는 `.env` 파일 명시적 로딩 |
| CockroachDB `cockroachdb` 공급자 필수 | 5.0.0 | `postgresql` 공급자 사용 시 연결 오류 | `schema.prisma`에서 `provider = "cockroachdb"`로 변경 |

---

## 3. 개발 생산성을 향상시키는 주요 신규 기능 및 개선사항

이 섹션에서는 2025년 1분기 Prisma ORM 릴리즈에서 도입되거나 크게 진전된, 개발자 생산성과 애플리케이션 성능을 향상시키는 주요 기능들을 설명합니다.

* 새로운 `prisma-client` 제너레이터 (ESM 호환성 및 출력 변경)
  * Prisma ORM 6.6.0 (2025년 4월 11일 릴리즈이나 3월에 주요 커밋이 이루어짐)에서 새로운 `prisma-client` 제너레이터가 Early Access로 도입되었습니다. 이는 기존 `prisma-client-js`의 현대적인 대안으로, ESM(ECMAScript Modules) 호환성을 제공하고, 생성된 코드를 여러 파일로 분할하며, `package.json` 및 `node_modules` 컨벤션이 더 이상 생성되지 않습니다.
* 쿼리 컴파일러 (성능 및 배포 개선)
  * 새로운 쿼리 컴파일러(Query Compiler)는 Prisma Client의 레거시 쿼리 엔진을 더 빠르고 유연한 TypeScript 구현으로 대체합니다. 이는 쿼리 실행 성능을 향상시키고 배포를 간소화(더 이상 네이티브 바이너리가 필요 없음)하며 장기적인 플랫폼 유연성을 확보합니다. Q1 2025 로드맵에 따르면, Preview 단계로 Postgres, MySQL, SQLite, MongoDB, MariaDB를 지원합니다.
* 드라이버 어댑터 개선 (간소화된 API 및 D1/Turso 지원)
  * Prisma ORM 6.6.0에서 드라이버 어댑터 사용 API가 개선되어 더욱 간소화되었습니다. 이제 Prisma 드라이버 어댑터를 JS-네이티브 드라이버의 옵션과 함께 직접 인스턴스화할 수 있습니다.
  * `prisma db push`, `prisma db pull`, `prisma migrate diff`에 대한 D1 네이티브 마이그레이션 지원의 첫 번째 Early Access 버전을 도입했습니다.
* `multiSchema` 및 `views` 기능 일반 가용성
  * `multiSchema` 및 `views` 기능은 오랫동안 Preview 상태였으며 프로덕션 환경에서 널리 사용되어 왔습니다. Q2 2025 로드맵에 따르면 이 기능들이 GA로 승격될 예정입니다.
* `prisma.config.ts` 파일 지원
  * Prisma ORM 6.5.0부터 `prisma.config.ts` 파일이 Prisma Studio를 포함하여 Prisma 설정 관리를 위한 새로운 옵션으로 도입되었습니다. 이를 통해 TypeScript를 통해 Prisma 설정을 정의할 수 있어 타입 안정성을 확보하고 더 복잡한 로직으로 설정을 구성할 수 있습니다.
* `$on` 및 `$extends` 메서드 체이닝 가능
  * Prisma ORM 6.5.0 이전 버전에서는 `$on` 클라이언트 메서드가 `void`를 반환하여 `$on()`과 `$extends()` 호출을 체이닝할 수 없었습니다. 6.5.0 버전에서 이 문제가 해결되어 `$on`이 이제 수정된 Prisma Client를 반환하므로 체이닝이 가능해졌습니다.

표 2: Prisma ORM 최소 버전 요구사항

| 구성 요소 | 이전 최소 버전 | 새 최소 버전 (Prisma ORM 5.0.0 기준) | 권장 사항 |
|---|---|---|---|
| Node.js | 14.17.0 | 16.13.0 | LTS v18.x 이상으로 업그레이드 권장 (v16은 마지막 지원 버전) |
| TypeScript | 4.3 | 4.7 | 최신 안정 버전 유지 |
| PostgreSQL | 9.5 | 9.6 | 현재 지원되는 최신 버전으로 업그레이드 권장 |
| Prisma Client 임베디드 SQLite | 3.35.4 | 3.41.2 | 원시 SQLite 쿼리 사용 시 SQLite 변경 로그 확인 |

표 3: 주요 신규 기능 및 개발 이점

| 신규 기능 | 주요 이점 | 사용 참고사항 | 현재 상태 (Q1 2025 기준) |
|---|---|---|---|
| 새로운 `prisma-client` 제너레이터 | ESM 호환성, 경량화된 출력, 번들링 최적화 | `output` 경로 명시, `moduleFormat` 설정 (esm 또는 cjs) | Early Access (Q2 2025 GA 예정) |
| 쿼리 컴파일러 | 쿼리 성능 향상, 네이티브 바이너리 제거로 배포 간소화 | Postgres, MySQL, SQLite, MongoDB, MariaDB 지원 | Preview (Q2 2025 모든 SQL-style FCDB GA 예정) |
| 드라이버 어댑터 개선 (간소화된 API) | 드라이버 어댑터 인스턴스화 간소화, 개발자 경험 향상 | JS-네이티브 드라이버 옵션으로 직접 어댑터 인스턴스화 | 일반 가용성 |
| Cloudflare D1 및 Turso/LibSQL 마이그레이션 지원 | 서버리스/엣지 데이터베이스 통합 강화 | `prisma.config.ts`를 통해 CLI 연결 필요 | Early Access |
| `multiSchema` 및 `views` 기능 | 복잡한 데이터베이스 구조 관리, 데이터 추상화 및 보안 강화 | GA 전환 시 API 결정 및 문서화 완료 예정 | Preview (Q2 2025 GA 예정) |
| `prisma.config.ts` 파일 지원 | 타입 안전한 설정 관리, 드라이버 어댑터 연동 용이 | `defineConfig` 헬퍼 또는 `PrismaConfig` 유틸리티 타입 사용 | 일반 가용성 |
| `$on` 및 `$extends` 메서드 체이닝 | Prisma Client 확장성 및 코드 간결성 향상 | `$on` 호출 후 `$extends` 호출 가능 | 일반 가용성 |

---

## 4. 한국인 개발자를 위한 권장사항

### 업그레이드 전략 및 모범 사례

* 단계적 업그레이드 계획 수립: Prisma ORM 5.0.0의 주요 Breaking Change와 6.x.x 버전의 새로운 변화들을 고려하여, 한 번에 모든 것을 업데이트하기보다는 단계적으로 업그레이드를 진행하는 것이 좋습니다.
* 테스트 환경에서의 선행 검증: 모든 업그레이드는 반드시 개발 또는 스테이징 환경에서 충분한 테스트를 거쳐야 합니다. 특히 기존 쿼리에서 배열 단축키 사용 여부, `rejectOnNotFound` 처리 방식, `beforeExit` 훅 사용 여부 등을 중점적으로 검토해야 합니다.
* 공식 문서 및 마이그레이션 가이드 활용: Prisma 공식 웹사이트의 릴리즈 노트와 업그레이드 가이드를 참조하여 각 버전에 대한 상세한 마이그레이션 지침을 따르는 것이 중요합니다. 특히 Prisma 5.0.0 업그레이드 가이드는 필수적으로 검토해야 할 문서입니다.
* CI/CD 파이프라인 업데이트: 제거된 CLI 플래그(`--force`, `--experimental` 등)나 변경된 명령어(`db push --accept-data-loss`)가 CI/CD 스크립트에 사용되고 있는지 확인하고 수정합니다. `prismaSchemaFolder` 관련 변경사항 또한 빌드 및 배포 파이프라인에 영향을 줄 수 있으므로 주의 깊게 확인해야 합니다.

### 잠재적 문제 해결 가이드

* 타입 관련 오류: Prisma Client 재생성 후 발생하는 타입 오류는 주로 생성된 타입 정의의 변경(예: nullable 관계 타입) 때문입니다. IDE의 자동 완성 기능을 활용하고, 새로운 타입 정의에 맞춰 코드를 수정해야 합니다.
* CLI 명령어 오류: 제거되거나 변경된 CLI 플래그/명령어 사용 시 발생하는 오류는 해당 명령어를 최신 버전의 대체 명령어로 변경하여 해결합니다. 데이터베이스 리셋이 필요한 경우 `prisma migrate reset` 명령어를 명시적으로 실행해야 함을 기억해야 합니다.
* 성능 문제: JSON 프로토콜은 기본적으로 성능 향상을 가져오지만, 특정 복잡한 쿼리에서 예상치 못한 성능 저하가 발생할 수 있습니다. Prisma의 쿼리 로깅 기능을 활용하여 병목 현상을 진단하고 최적화하는 것이 필요합니다.
* 환경 변수 로딩 문제: `.env` 파일이 자동으로 로드되지 않아 발생하는 문제는 `prisma.config.ts`를 통해 환경 설정을 명시적으로 관리하거나, 애플리케이션 시작 시 `.env` 파일을 직접 로드하는 방식을 사용해야 합니다.

### 새로운 기능 활용 방안

* 새로운 `prisma-client` 제너레이터 도입 고려: ESM 환경에서 개발 중이거나 번들링 최적화가 필요한 경우, 새로운 `prisma-client` 제너레이터(Early Access)를 시험적으로 도입하여 ESM 호환성 및 분할된 출력의 이점을 평가해볼 수 있습니다.
* 드라이버 어댑터 및 클라우드 네이티브 데이터베이스 탐색: Cloudflare D1, Turso/LibSQL과 같은 서버리스 데이터베이스에 관심이 있다면, 간소화된 드라이버 어댑터 API를 활용하여 엣지 컴퓨팅 환경에서의 Prisma 적용 가능성을 모색해볼 수 있습니다.
* `multiSchema` 및 `views` 활용 준비: 복잡한 데이터베이스 스키마를 관리하거나 데이터베이스 뷰를 활용하는 프로젝트의 경우, `multiSchema` 및 `views` 기능이 GA로 전환되면 이를 적극적으로 도입하여 데이터 모델링의 유연성과 효율성을 높일 수 있습니다.

---

## 5. 결론

2025년 1분기는 Prisma ORM에게 중요한 전환점이었습니다. Prisma 5.0.0에서 도입된 JSON 프로토콜, 배열 단축키 제거, `rejectOnNotFound` 및 `beforeExit` 훅 제거와 같은 핵심적인 API 변경사항들은 Prisma Client의 성능과 일관성을 크게 향상시켰습니다. 이러한 변화는 Prisma가 더욱 견고하고 예측 가능한 ORM으로 발전하려는 의지를 보여주며, 개발자들이 더욱 명시적이고 표준적인 Node.js/TypeScript 패턴을 따르도록 유도합니다.

또한, 6.5.0 및 6.6.0 버전에서는 `db push --accept-data-loss`로의 변경, `prismaSchemaFolder`의 명시적 경로 요구, 그리고 `prisma.config.ts` 파일 지원과 같은 CLI 및 설정 관련 개선이 이루어졌습니다. 이는 개발자에게 더 많은 제어권을 부여하고 예측 가능한 환경을 제공하며, "설정 코딩" 패러다임에 맞춰 타입 안정성과 유연성을 강화하려는 Prisma의 의도를 반영합니다. `migration-engine`이 `schema-engine`으로 이름이 변경된 것은 Prisma가 스키마 관리 및 마이그레이션 기능을 핵심적인 위치에 두려는 전략적 방향을 나타냅니다.

특히, 새로운 `prisma-client` 제너레이터의 Early Access 도입과 쿼리 컴파일러의 진전은 Prisma ORM이 ESM 호환성, 경량화된 배포, 그리고 궁극적으로 더 빠른 성능을 향해 나아가고 있음을 명확히 보여줍니다. 드라이버 어댑터의 간소화된 API와 Cloudflare D1, Turso/LibSQL 지원은 Prisma가 서버리스 및 엣지 컴퓨팅 환경에 대한 강력한 지원을 확대하고 있음을 시사하며, 이는 Prisma의 활용 범위를 확장하고 개발자들이 최신 아키텍처 패턴을 쉽게 채택할 수 있도록 돕습니다.

Prisma는 Semantic Versioning(SemVer)을 따르며, MAJOR 버전은 호환되지 않는 API 변경 시, MINOR 버전은 하위 호환성을 유지하며 기능 추가 시, PATCH 버전은 버그 수정 시 증가합니다. Early Access 및 Preview 기능은 MINOR 버전에서 도입될 수 있으며, GA로 승격될 때 MAJOR 버전 업데이트가 필요할 수 있습니다. 개발자들은 이러한 버전 관리 정책을 이해하고 지속적으로 업데이트를 확인하는 것이 중요합니다.

종합적으로 볼 때, 2025년 1분기 Prisma ORM의 변화는 단순히 기능 추가를 넘어, 아키텍처의 근본적인 현대화와 개발자 경험의 총체적인 개선을 목표로 하고 있습니다. 이는 Prisma가 빠르게 변화하는 웹 개발 생태계, 특히 클라우드 네이티브 및 엣지 컴퓨팅 트렌드에 적극적으로 대응하고 있음을 보여주며, Prisma ORM을 점점 더 복잡하고 분산된 환경에서 애플리케이션을 구축하는 개발자들을 위한 견고하고 미래 지향적인 솔루션으로 자리매김하게 합니다.
