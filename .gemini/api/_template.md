# [API 기능 이름] API

* **기능 요약**: [이 API가 수행하는 핵심 기능을 한 문장으로 요약합니다. 예: 사용자 정보 조회]
* **관련 기능 또는 목표**: [사용자 프로필 표시 기능, FEAT-123]

---

## 1. [엔드포인트 이름]

[이 특정 엔드포인트의 역할을 간략히 설명합니다. 예: ID로 특정 사용자 정보를 조회합니다.]

### 요청 (Request)

* **HTTP Method**: `GET`
* **URL**: `/api/users/{userId}`
* **인증**: `필수 (사용자 토큰)`

#### 요청 파라미터 (URL Parameters)

| 이름 | 타입 | 설명 |
|---|---|---|
| `userId` | `string` | 조회할 사용자의 고유 ID |

#### 쿼리 파라미터 (Query Parameters)

| 이름 | 타입 | 설명 | 필수 여부 |
|---|---|---|---|
| `include_details` | `boolean` | 응답에 추가적인 상세 정보를 포함할지 여부 | 아니오 |

#### 요청 본문 (Body)

* 없음

---

### 응답 (Response)

#### 성공 응답 (Success)

* **Status Code**: `200 OK`
* **타입 정의**: `UserApiResponse` (in `apps/frontend/src/types/api.ts`)
* **내용**:

```json
{
  "id": "a1b2c3d4-e5f6-7890-1234-567890abcdef",
  "name": "홍길동",
  "email": "user@example.com",
  "createdAt": "2025-07-08T12:00:00Z"
}
```

#### 오류 응답 (Error)

* **Status Code**: `403 Forbidden`
  * **내용**: `{"error": "자신의 정보만 조회할 수 있습니다."}`

* **Status Code**: `404 Not Found`
  * **내용**: `{"error": "해당 사용자를 찾을 수 없습니다."}`

---

### 권한 및 보안 정책 (Permissions & Security)

* 이 엔드포인트는 인증된 사용자만 호출할 수 있습니다.
* 사용자는 자신의 `userId`에 해당하는 정보만 조회할 수 있으며, 'admin' 역할을 가진 사용자는 모든 사용자 정보를 조회할 수 있습니다.
