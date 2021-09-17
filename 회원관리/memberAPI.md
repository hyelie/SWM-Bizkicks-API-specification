## 회원관리

### 1) 회원가입

`/member/signup`

Method : **POST**

Description : 클라이언트가 아이디, 비밀번호, 휴대폰 번호, 회사 코드를 전송하면 서버에서 회원가입 절차 진행 및 ID에 대해 중복검사 진행 및 결과 리턴.

Request : id, 비밀번호, 휴대폰 번호, 회사 코드를 POST로 전송함.

Request example)

```json
http body
{
  "id" : "root" (string),
  "name" : "사용자" (string),
  "password" : "password" (string),
  "license" : true (boolean),
  "user_role" : "ROLE_USER" (string),
  "phone_number" : "01012345678" (string),
  "company_code" : "code" (string)
}
```

Response : 통신 결과 및 메시지 리턴.

Response example)

```json
HTTP/1.1 201 Created
{
	"msg" : "Success" (string)
}

HTTP/1.1 400 Bad Request
{
"timestamp": "2021-09-17T17:42:27.0850648" (datetime),
"status": 400 (number),
"error": "BAD_REQUEST" (string),
"code": "PARAMETER_NOT_VALID" (string),
"msg": "입력 정보가 유효하지 않습니다." (string)
}

HTTP/1.1 409 Conflict
{
	"timestamp": "2021-08-09T21:53:57.1167114" (datetime),
	"status": 409 (number),
	"error": "CONFLICT" (string),
	"code": "ID_DUPLICATED" (string),
	"msg": "아이디가 중복되었습니다." (string)
}
```

Returns:

- 201 Create (Success)
- 400 Bad Request (Parameter not valid)
- 409 Conflicted (id duplicated)

Validation:

- user_role : ROLE_USER, ROLE_MANAGER 2개의 값을 가질 수 있음. ROLE_USER는 일반 사용자(임직원), ROLE_MANAGER는 관리자(복지 담당자)임.
- license : true or false



<br>

### 2) 로그인

`/member/login`

Method : **POST**

Description : 클라이언트가 아이디, 비밀번호를 전송하면 서버는 해당 값이 저장된 값인지 검사 후 결과 리턴.

Request : ID, PW를 POST로 전송함.

Request example)

```json
http body
{
  "id" : "root" (string),
  "password" : "password" (string)
}
```

Response : 실패 시 에러 코드 및 메시지 리턴. 성공 지 type, access Token, refresh Token, access token 만료 시간을 리턴받음. access token은 authorization hearer에 붙여넣으면 됨.

Response example)

```json
HTTP/1.1 200 OK
{
	"grantType": "bearer" (string),
	"accessToken": "eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJ1c2VyMSIsImF1dGgiOiJST0xFX01BTkFHRVIiLCJleHAiOjE2MzE4NjkyNDl9.oWP7kfI1i2eERSpU34Tqiv36x-ZxGtE_4zqVBUxl7k3JRIqpnli0QNqwyzrIPQAcu0xlU0K-bQvlo5alhjIPKA" (string),
	"accessTokenExpiresIn": 1631869249795 (number),
	"refreshToken": "eyJhbGciOiJIUzUxMiJ9.eyJleHAiOjE2MzI0NzIyNDl9.bUJsRxw_OXauLAsSqwlnv8s6Ce2EXpJZit5SRqCmy2_G0aol5Diphijz2Rl1BFB5YS8eHzoErbZodtU5gFWUmQ" (string)
}

HTTP/1.1 401 Unauthorized
{
	"timestamp": "2021-08-09T21:48:32.9523621" (datetime),
	"status": 401 (number),
	"error": "UNAUTHORIZED" (string),
	"code": "NO_UNAUTHORIZED" (string),
	"msg": "자격 증명에 실패했습니다." (string)
}
```

Returns:

- 200 OK (Success)
- 401 Unauthorized

<br>

### 3) 사용자 정보

`/member/me`

Method : **GET**

Description : 클라이언트가 authorization header에 access token을 담아 전송하면 해당 사용자의 정보를 리턴.

Request : authorization header에 access token을 담아 전송. access token 앞에 "Bearer "를 붙임.

Request example)

```json
http header
{
  "authorization" : "Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJ1c2VyMSIsImF1dGgiOiJST0xFX1VTRVIiLCJleHAiOjE2MzE4NjgyNjR9.k2QYcm0HCpRKFoUIjyBRmMyCptZVvPPEvObmbOBCmBXpdj0xZYsaxsaxSD5E4GRuHEzize3UDLeKTnHTS3PHoQ" (string)
}
```

Response : 실패 시 에러 코드 및 메시지 리턴. 성공 지 type, access Token, refresh Token, access token 만료 시간을 리턴받음. access token은 authorization hearer에 붙여넣으면 됨. access token이 변조된 경우 복호화에서 문제가 생기기 때문에 사용자가 존재하지 않는다는 에러 메시지가 나오게 됨.

Response example)

```json
HTTP/1.1 200 OK
{
	"memberId": "user1",
	"name": "사용자1",
	"userRole": "ROLE_USER",
	"license" : true,
	"phoneNumber": "0102372666",
	"customerCompanyName": "삼성",
}

HTTP/1.1 404 Not Found
{
	"timestamp": "2021-08-09T21:48:32.9523621" (datetime),
	"status": 404,
	"error": "NOT_FOUND",
	"code": "MEMBER_NOT_EXIST",
	"msg": "사용자가 존재하지 않습니다."
}
```

Returns:

- 200 OK (Success)
- 404 Not Found

Validation:

- user_role : ROLE_USER, ROLE_MANAGER 2개의 값을 가질 수 있음. ROLE_USER는 일반 사용자(임직원), ROLE_MANAGER는 관리자(복지 담당자)임.

<br>

### 4) 토큰 재발급

`/member/reissue`

Method : **POST**

Description : 클라이언트가 access token, refresh token을 전송하면 token을 재발급 해 줌.

Request : http body에 access token, refresh token을 전송함.

Request example)

```json
http body
{
	"grantType": "bearer" (string),
	"accessToken": "eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJ1c2VyMSIsImF1dGgiOiJST0xFX1VTRVIiLCJleHAiOjE2MzE4Njg2ODV9.f67AIBe_kNhvAErYhIC3B5tPVZiYb23lciPRFtJU21AD86ERYRHtT6fTHCgpDzboHg3ivfnFmbmM1fcv2fNjxQ" (string),
	"accessTokenExpiresIn": 1631868685202 (string),
	"refreshToken": "eyJhbGciOiJIUzUxMiJ9.eyJleHAiOjE2MzI0NzM0ODR9.M3D3MFpDLfMIvgWObmV2mpZk735vurJDyLgH4E2XQp4peYXZOmby3K9lOOKtcu_vGQUFr-Z4TaIt7LyvUv0TyQ" (string)
}
```

Response : 실패 시 에러 코드 및 메시지 리턴. 성공 지 type, access Token, refresh Token, access token 만료 시간을 리턴받음. access token은 authorization hearer에 붙여넣으면 됨.

Response example)

```json
HTTP/1.1 200 OK
{
	"memberId": "user1",
	"name": "사용자1",
	"userRole": "ROLE_USER",
	"license" : true,
	"phoneNumber": "0102372666",
	"customerCompanyName": "삼성",
}

HTTP/1.1 400 Bad Request
{
	"timestamp" : "2021-09-17T17:52:27.8201783" (datetime),
	"status" : 400 (number),
	"error" : "BAD_REQUEST" (string),
	"code" : "INVALID_REFRESH_TOKEN" (string),
	"msg" : "사용자 정보가 유효하지 않습니다." (string)
}

HTTP/1.1 400 Bad Request
{
	"timestamp" : "2021-09-17T17:52:27.8201783" (datetime),
	"status" : 400 (number),
	"error" : "BAD_REQUEST" (string),
	"code" : "INVALID_ACCESS_TOKEN" (string),
	"msg" : "사용자 정보가 유효하지 않습니다." (string)
}

HTTP/1.1 401 Unauthorized
{
	"timestamp" : "2021-09-17T17:52:27.8201783" (datetime),
	"status" : 401 (number),
	"error" : "UNAUTHORIZED" (string),
	"code" : "MEMBER_STATUS_LOGOUT" (string),
	"msg" : "사용자가 로그아웃 상태입니다." (string)
}

```

Returns:

- 200 OK (Success)
- 400 Bad Request (invalid refresh token)
- 401 Unauthorized
- 404 Not Found

<br>

### 5) 아이디 찾기

`/member/find/id`

Method : **POST**

Description : 클라이언트가 휴대폰 번호를 전송하면 서버에서 해당 휴대폰 번호의 유무를 검사한 후 해당하는 아이디를 휴대폰 메시지로 날려줌.

Request : 휴대폰 번호를 POST로 전송함.

Request example)

```json
http body
{
  "phone_number" : "01012345678" (string)
}
```

Response : 통신 결과 및 메시지 리턴.

Response example)

```json
HTTP/1.1 200 OK
{
  "msg" : "Success" (string)
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

Returns:

- 200 OK (Success)
- 404 Not Found (phone number does not exist)

<br>

### 6) 비밀번호 찾기

`/member/find/password`

Method : **POST**

Description : 클라이언트가 휴대폰 번호를 전송하면 서버에서 해당 휴대폰 번호의 유무를 검사한 후 해당하는 비밀번호를 임의로 바꾼 후 휴대폰 메시지로 날려줌.

Request : 휴대폰 번호를 POST로 전송함.

Request example)

```json
http body
{
  "phone_number" : "01012345678" (string)
}
```

응답 : 통신 결과 및 메시지 리턴.

응답 예시)

```json
HTTP/1.1 200 OK
{
  "msg" : "Success" (string)
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

Returns:

- 200 OK (Success)
- 404 Not Found (phone number does not exist)

<br>

### 7) 운전면허 등록 - 직접 경찰정 API에 접근해도 될 듯.

`/member/license`

Method : **POST**

Description : 클라이언트가 생년월일, 이름, 면허번호, 식별번호를 전송하면 서버에서 해당 면허의 조회 결과를 리턴함.

Request : 생년월일, 이름, 면허번호, 식별번호를 POST로 전송함.

Request example)

```json
http body
{
  "date_of_birth" : "19990823" (string),
  "name" : "이름" (string),
  "license_number" : "123-45-123456-1" (string),
  "identify_number" : "qwerty123456" (string)
}
```

Response : 면허 코드, 결과, 의미 리턴, 실패 시 실패 사유 리턴.

응답 예시) - > 굳이 API로 안쓰고 직접 요청해도 될 듯.

```json
HTTP/1.1 200 OK
{
  "code" : "00" (string),
  "result" : "정상" (string),
  "meaning" : "이상없음" (string)
}

HTTP/1.1 400 Bad Request
{
	"timestamp": "2021-08-09T21:45:20.2723262" (datetime),
	"status": 400 (number),
	"error": "BAD_REQUEST" (string),
	"code": "PARAMETER_NOT_VALID" (string),
	"msg": "입력 정보가 유효하지 않습니다." (string)
}

HTTP/1.1 401 Unauthorized
{
	"timestamp": "2021-08-09T21:48:32.9523621" (datetime),
	"status": 401 (number),
	"error": "UNAUTHORIZED" (string),
	"code": "USER_STATUS_LOGOUT" (string),
	"msg": "사용자가 로그아웃 상태입니다." (string)
}
```

Returns:

- 200 OK (Success)
- 400 Bad Request (not valid parameter)
- 401 Unauthorized (user status logout)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/12e7a9a4-f2f7-4513-867e-0e74d326fbba/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/12e7a9a4-f2f7-4513-867e-0e74d326fbba/Untitled.png)

<br>

### 8) 비밀번호 변경

`/member/modify/password`

Method : **POST**

Description : 클라이언트가 새로운 비밀번호를 전송하면 서버에서 헤더에 있는 아이디에 해당하는 사용자의 비밀번호를 변경함.

Request : 사용자의 정보가 담긴 Authorization header, 새 비밀번호를 POST로 전송함.

Request example)

```json
http body
{
  "password" : "qwerty" (string)
}
```

Response : 통신 결과 및 메시지 리턴.

응답 예시)

```json
HTTP/1.1 200 OK
{
  "msg" : "Success" (string)
}

HTTP/1.1 401 Unauthorized
{
	"timestamp": "2021-08-09T21:48:32.9523621" (datetime),
	"status": 401 (number),
	"error": "UNAUTHORIZED" (string),
	"code": "USER_STATUS_LOGOUT" (string),
	"msg": "사용자가 로그아웃 상태입니다." (string)
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

Returns:

- 200 OK (Success)
- 401 Unauthorized (user status logout)
- 404 Not Found (phone number does not exist)

<br>