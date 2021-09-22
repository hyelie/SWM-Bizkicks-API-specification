## 관리자 대시보드

### 1) 이용 지역 통계

`/dashboard/location?from=x&to=y`

Method : **GET**

Description : 클라이언트가 조회 시작 기간, 조회 종료 기간을 전송하면 서버에서 헤더에 있는 아이디가 관리자 아이디인지 유효성 검사를 진행한 후 조회 시작 기간부터 조회 종료 기간까지 해당 관리자가 속한 고객 법인의 킥보드 이용 결과 목록을 리턴.

Request : 사용자의 정보가 담긴 Authorization header, query parameter로 조회 시작 기간, 조회 종료 기간을 전송함.

Request example)

```json
/dashboard/location?from=2020-12-22&to=2021-01-22
```

Response : 성공 시 서울시 지역별 이용량 수를 리턴함. 실패 시 실패 사유를 메시지로 리턴.

Response example)

```json
HTTP/1.1 200 OK
{
  "location" : [
		{
			"강남구" : 15 (number),
			"서초구" : 20 (number),
		  "관악구" : 6 (number),
		}
  ] (json list)
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

HTTP/1.1 403 Forbidden
{
	"timestamp": "2021-08-09T21:50:40.2363793" (datetime),
	"status": 403 (number),
	"error": "FORBIDDEN" (string),
	"code": "NOT_ALLOWED" (string),
	"msg": "허가되지 않은 접근입니다." (string)
}
```

Validation:

- from : YYYY-MM-DD (date)
- to : YYYY-MM-DD (date)

Returns:

- 200 OK
- 400 Bad Request (parameter not valid)
- 401 Unauthorized (user status logout)
- 403 Forbidden (not allowed)

Note:

- 헤더에 명시된 사용자가 "관리자"여야만 요청할 수 있음.

Default:

- from : 최근 계약 갱신일.
- to : default 값은 조회하는 시점 날짜

<br>

### ~~2) 전체 이용시간 - 얘 굳이 필요함..?~~

`/dashboard/total-time?from=x&to=y`

Method : **GET**

Description : 클라이언트가 조회 시작 기간, 조회 종료 기간을 전송하면 서버에서 헤더에 있는 아이디가 관리자 아이디인지 유효성 검사를 진행한 후 조회 시작 기간부터 조회 종료 기간까지 해당 관리자가 속한 고객 법인의 킥보드 전체 이용 시간을 리턴.

Request : 사용자의 정보가 담긴 Authorization header, query parameter로 조회 시작 기간, 조회 종료 기간을 전송함.

Request example)

```json
/dashboard/totaltime?2020-12-22&to=2021-01-22
```

Response : 성공 시 해당 고객 법인의 킥보드 전체 이용 시간을 리턴. 실패 시 실패 사유를 메시지로 리턴.

Response example)

```json
HTTP/1.1 200 OK
{
	"time" : 12:20:40 (time)
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

HTTP/1.1 403 Forbidden
{
	"timestamp": "2021-08-09T21:50:40.2363793" (datetime),
	"status": 403 (number),
	"error": "FORBIDDEN" (string),
	"code": "NOT_ALLOWED" (string),
	"msg": "허가되지 않은 접근입니다." (string)
}
```

Validation:

- from : YYYY-MM-DD (date)
- to : YYYY-MM-DD (date)

Returns:

- 200 OK
- 400 Bad Request (parameter not valid)
- 401 Unauthorized (user status logout)
- 403 Forbidden (not allowed)

Note:

- 헤더에 명시된 사용자가 "관리자"여야만 요청할 수 있음.

Default:

- from : 최근 계약 갱신일.
- to : default 값은 조회하는 시점 날짜

<br>

### 3) 시간별 이용량

`/dashboard/time?from=x&to=y`

Method : **GET**

Description : 클라이언트가 조회 시작 기간, 조회 종료 기간을 전송하면 서버에서 헤더에 있는 아이디가 관리자 아이디인지 유효성 검사를 진행한 후 조회 시작 기간부터 조회 종료 기간까지 해당 관리자가 속한 고객 법인의 시간별 킥보드 이용 시간을 리턴.

Request : 사용자의 정보가 담긴 Authorization header, query parameter로 조회 시작 기간, 조회 종료 기간을 전송함.

Request example)

```json
/dashboard/time?2020-12-22&to=2021-01-22
```

Response : 성공 시 해당 고객 법인의 킥보드 시간별 이용 시간을 리턴, 실패 시 실패 사유를 메시지로 리턴.

Response example)

```json
HTTP/1.1 200 OK
{
  "statistics" : [
		{
			0 : 12:20:40 (time),
			1 : 06:10:27 (time),
			...
		}
	] (json list)
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

HTTP/1.1 403 Forbidden
{
	"timestamp": "2021-08-09T21:50:40.2363793" (datetime),
	"status": 403 (number),
	"error": "FORBIDDEN" (string),
	"code": "NOT_ALLOWED" (string),
	"msg": "허가되지 않은 접근입니다." (string)
}
```

Validation:

- from : YYYY-MM-DD (date)
- to : YYYY-MM-DD (date)

Returns:

- 200 OK
- 400 Bad Request (parameter not valid)
- 401 Unauthorized (user status logout)
- 403 Forbidden (not allowed)

Note:

- 헤더에 명시된 사용자가 "관리자"여야만 요청할 수 있음.

Default:

- from : 최근 계약 갱신일.
- to : default 값은 조회하는 시점 날짜

<br>

### 4) 전체 이용내역

`/dashboard/usage?from=x&to=y&page=z&unit=w`

Method : **GET**

Description : 클라이언트가 조회 시작 기간, 조회 종료 기간을 전송하면 서버에서 헤더에 있는 아이디가 관리자 아이디인지 유효성 검사를 진행한 후 조회 시작 기간부터 조회 종료 기간까지 한 페이지에 들어갈 내역 개수와 현재 페이시 수에 맞게 해당 관리자가 속한 고객 법인의 킥보드 이용 내역을 리턴.

Request : 사용자의 정보가 담긴 Authorization header, query parameter로 조회 시작 기간, 조회 종료 기간, 현재 페이지 수, 한 페이지에 들어갈 내역 개수를 전송함.

Request example)

```json
/dashboard/time?2020-12-22&to=2021-01-22&page=1&unit=10
```

Response : 성공 시 조회 시작 기간부터 조회 종료 기간까지 한 페이지에 들어갈 내역 개수와 현재 페이지 수에 맞게, 현재 페이지, 유닛 개수, 해당 고객 법인의 전체 킥보드 이용내역을 리턴, 실패 시 실패 사유를 메시지로 리턴.

Response example)

```json
HTTP/1.1 200 OK
{
  "page" : 1 (number),
  "unit" : 10 (number),
	"history" : 
	[
		{
			"depart_time" : "2020-10-10T14:20:15" (string, UTC),
			"arrive_time" : "2020-10-10T14:25:40" (string, UTC),
			"location_list" :
				[
					["131.0" (number), "131.1" (number)] (list),
					["131.1" (number), "131.2" (number)] (list),
					["131.4" (number), "131.7" (number)] (list),
					...
				] (list of list),
			"cycle" : 5000 (number)
		},
		{
			"depart_time" : "2020-07-28T14:20:15" (string, UTC),
			"arrive_time" : "2020-07-28T14:25:40" (string, UTC),
			"location_list" :
				[
					["129.0" (number), "134.1" (number)] (list),
					["129.1" (number), "134.2" (number)] (list),
					["129.4" (number), "134.7" (number)] (list),
					...
				] (list of list),
			"cycle" : 5000 (number)
		},
	  ...
	] (json list)
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

HTTP/1.1 403 Forbidden
{
	"timestamp": "2021-08-09T21:50:40.2363793" (datetime),
	"status": 403 (number),
	"error": "FORBIDDEN" (string),
	"code": "NOT_ALLOWED" (string),
	"msg": "허가되지 않은 접근입니다." (string)
}
```

Validation:

- from : YYYY-MM-DD (date)
- to : YYYY-MM-DD (date)
- datetime : YYYY-MM-DDTHH:mm:ss (UTC)

Returns:

- 200 OK
- 400 Bad Request (parameter not valid)
- 401 Unauthorized (user status logout)
- 403 Forbidden (not allowed)

Note:

- 헤더에 명시된 사용자가 "관리자"여야만 요청할 수 있음.

Default:

- from : 최근 계약 갱신일.
- to : default 값은 조회하는 시점 날짜
- page : 1
- unit : 10

<br>

### 5) 청구비용

`/dashboard/money?from=x&to=y`

Method : **GET**

Description : 클라이언트가 조회 시작 기간, 조회 종료 기간을 전송하면 서버에서 헤더에 있는 아이디가 관리자 아이디인지 유효성 검사를 진행한 후 조회 시작 기간부터 조회 종료 기간까지 해당 관리자가 속한 고객 법인의 계약 종류, 예상 청구 비용을 리턴.

Request : 사용자의 정보가 담긴 Authorization header, query parameter로 조회 시작 기간, 조회 종료 기간을 전송함.

Request example)

```json
/dashboard/time?2020-12-22&to=2021-01-22
```

Response : 성공 시 해당 해당 관리자가 속한 고객 법인의 계약 종류, 예상 청구 비용을 리턴, 실패 시 실패 사유를 메시지로 리턴.

Response example)

```json
HTTP/1.1 200 OK
{
	"type" : "measuredRate" (string) | "fixedCharge" (string),
  "list" : [
		{
			"company_name" : "씽씽" (string),
			"price_per_hour" : 10000 (number),
			"used_time" : 50 (number),
			"result" : 500000 (number)
		},
		{
			"company_name" : "킥고잉" (string),
			"price_per_hour" : 8000 (number),
			"used_time" : 70 (number),
			"result" : 560000 (number)
		},
		...
  ] (json list),
	"price" : 1060000 (number)
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

HTTP/1.1 403 Forbidden
{
	"timestamp": "2021-08-09T21:50:40.2363793" (datetime),
	"status": 403 (number),
	"error": "FORBIDDEN" (string),
	"code": "NOT_ALLOWED" (string),
	"msg": "허가되지 않은 접근입니다." (string)
}
```

Validaion:

- from : YYYY-MM-DD (date)
- to : YYYY-MM-DD (date)
- type : 계약 타입. "measuredRate" 또는 "fixedCharge" 2개 값 중 하나임.
- list : 계약된 킥보드 회사 목록. type이 measuredRate인 경우 list는 null, fixedCharge인 경우에는각 element는 회사명, 시간당 금액, 이용시간, 회사별 청구 비용을 가지고 있음.

Returns:

- 200 OK (Success)
- 400 Bad Request (parameter not valid)
- 403 Forbidden (Not allowed)
- 404 Not Found (Does not exist)

Note:

- 헤더에 명시된 사용자가 "관리자"여야만 요청할 수 있음.

Default:

- from : 최근 계약 갱신일.
- to : default 값은 조회하는 시점 날짜

<br>