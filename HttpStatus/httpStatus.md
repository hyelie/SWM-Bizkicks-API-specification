# Http Status 목록

## 성공 시

### 200 OK

Response example)

```json
// 리턴되는 값이 없을 시
HTTP/1.1 200 OK
{
	"msg" : "Success" (string)
}

// 리턴되는 값이 있을 시
HTTP/1.1 200 OK
{
	... (리턴되는 값)
}
```

<br>

### 201 Created

Response example)

```json
HTTP/1.1 201 Created
{
	"msg" : "Success" (string)
}
```

<br>

### 204 No Content

Response example)

```json
// 204의 경우 리턴되는 json이 존재하지 않음.
http/1.1 204 No Content
```

<br>

## 실패 시

400번대 이상의 오류 메시지의 경우 아래와 같은 양식으로 리턴 json이 전송됨.

```json
{
	"timestamp": "2021-08-09T21:42:54.3989507" (datetime),
	"status": "오류 번호" (number)
	"error": "HTTP 오류 코드" (string),
	"code": "오류 코드" (string),
	"msg": "오류 메시지" (string)
}
```

<br>

### 400 Bad Request

400번의 오류 코드는 2가지가 존재함.

- PARAMETER_NOT_VALID : http body에 담긴 정보 형식이 다르거나 유효하지 않을 때
- INVALID_TOKEN : 사용자 정보가 유효하지 않을 때

Response example)

```json
HTTP/1.1 400 Bad Request
{
	"timestamp": "2021-08-09T21:45:20.2723262" (datetime),
	"status": 400 (number),
	"error": "BAD_REQUEST" (string),
	"code": "PARAMETER_NOT_VALID" (string),
	"msg": "입력 정보가 유효하지 않습니다." (string)
}

HTTP/1.1 400 Bad Request
{
	"timestamp": "2021-08-09T21:45:20.2723262" (datetime),
	"status": 400 (number),
	"error": "BAD_REQUEST" (string),
	"code": "INVALID_TOKEN" (string),
	"msg": "사용자 정보가 유효하지 않습니다." (string)
}
```

<br>

### 401 Unauthorized

401번의 오류 코드는 3가지가 존재함.

- USER_STATUS_LOGOUT : 사용자가 로그아웃 된 상태일 때
- ID_NOT_EXIST : 아이디가 틀렸을 때
- PASSWORD_NOT_VALID : 비밀번호가 틀렸을 때

Response example)

```json
HTTP/1.1 401 Unauthorized
{
	"timestamp": "2021-08-09T21:48:32.9523621" (datetime),
	"status": 401 (number),
	"error": "UNAUTHORIZED" (string),
	"code": "USER_STATUS_LOGOUT" (string),
	"msg": "사용자가 로그아웃 상태입니다." (string)
}

HTTP/1.1 401 Unauthorized
{
	"timestamp": "2021-08-09T21:48:32.9523621" (datetime),
	"status": 401 (number),
	"error": "UNAUTHORIZED" (string),
	"code": "ID_NOT_EXIST" (string),
	"msg": "아이디가 존재하지 않습니다." (string)
}

HTTP/1.1 401 Unauthorized
{
	"timestamp": "2021-08-09T21:48:32.9523621" (datetime),
	"status": 401 (number),
	"error": "UNAUTHORIZED" (string),
	"code": "PASSWORD_NOT_VALID" (string),
	"msg": "비밀번호가 틀렸습니다." (string)
}
```

<br>

### 403 Forbidden

403번의 오류 코드는 1가지가 존재함.

- NOT_ALLOWED : 허가되지 않은 접근을 했을 때(임직원이 관리자 페이지 접근 등)

Response example)

```json
HTTP/1.1 403 Forbidden
{
	"timestamp": "2021-08-09T21:50:40.2363793" (datetime),
	"status": 403 (number),
	"error": "FORBIDDEN" (string),
	"code": "NOT_ALLOWED" (string),
	"msg": "허가되지 않은 접근입니다." (string)
}
```

<br>

### 404 Not Found

404번의 오류 코드는 3가지가 존재함.

- COMPANY_NOT_EXIST : 존재하지 않는 법인으로 조회했을 때
- KICKBOARD_BRAND_NOT_EXIST : 존재하지 않은 킥보드 브랜드를 조회했을 때
- PHONE_NUMBER_NOT_EXIST : 휴대폰 번호가 존재하지 않을 때

Response example)

```json
HTTP/1.1 404 Not Found
{
	"timestamp": "2021-08-09T21:53:01.1151735" (datetime),
	"status": 404 (number),
	"error": "NOT_FOUND" (string),
	"code": "COMPANY_NOT_EXIST" (string),
	"msg": "존재하지 않는 법인입니다." (string)
}

HTTP/1.1 404 Not Found
{
	"timestamp": "2021-08-09T21:53:01.1151735" (datetime),
	"status": 404 (number),
	"error": "NOT_FOUND" (string),
	"code": "KICKBOARD_BRAND_NOT_EXIST" (string),
	"msg": "존재하지 않는 킥보드 브랜드입니다." (string)
}

HTTP/1.1 404 Not Found
{
	"timestamp": "2021-08-09T21:53:01.1151735" (datetime),
	"status": 404 (number),
	"error": "NOT_FOUND" (string),
	"code": "PHONE_NUMBER_NOT_EXIST" (string),
	"msg": "휴대폰 번호가 존재하지 않습니다." (string)
}
```

<br>

### 409 Conflict

409번의 오류 코드는 2가지가 존재함.

- ID_DUPLICATED : 아이디가 중복된 경우
- DUPLICATED_RESOURCE : 데이터가 이미 존재해서 삽입하지 못하는 경우

Response example)

```json
HTTP/1.1 409 Conflict
{
	"timestamp": "2021-08-09T21:53:57.1167114" (datetime),
	"status": 409 (number),
	"error": "CONFLICT" (string),
	"code": "ID_DUPLICATED" (string),
	"msg": "아이디가 중복되었습니다." (string)
}

HTTP/1.1 409 Conflict
{
	"timestamp": "2021-08-09T21:53:57.1167114" (datetime),
	"status": 409 (number),
	"error": "CONFLICT" (string),
	"code": "DUPLICATED_RESOURCE " (string),
	"msg": "데이터가 이미 존재합니다." (string)
}
```