## 회원관리

### 1) 회원가입

`/members`

Method : **POST**

Description : 클라이언트가 아이디, 비밀번호, 휴대폰 번호, 회사 코드를 전송하면 서버에서 회원가입 절차 진행 및 ID에 대해 중복검사 진행 및 결과 리턴.

Request : id, 비밀번호, 휴대폰 번호, 회사 코드를 POST로 전송함.

Request example)

```json
http body
{
  "id" : "root" (string),
  "password" : "password" (string),
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
- 409 Conflicted (id duplicated)

<br>

### 2) 로그인

`/members/login`

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

Response : 통신 결과 및 메시지 리턴. 실패 시 실패 이유 리턴.

Response example)

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

Returns:

- 200 OK (Success)
- 401 Unauthorized ('ID does not exist' or 'Not valid password')

<br>

### 3) 아이디 찾기

`/members/find/id`

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

### 4) 비밀번호 찾기

`/members/find/password`

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

### 5) 운전면허 등록 - 직접 경찰정 API에 접근해도 될 듯.

`/members/license`

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

### 6) 비밀번호 변경

`/members/modify/password`

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