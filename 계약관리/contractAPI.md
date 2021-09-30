## 계약관리

### 1) 종량제 가격

`/manage/measuredrate-price`

Method : **GET**

Description : 클라이언트가 요청하면 서버에서 헤더에 있는 아이디가 관리자 아이디인지 유효성 검사를 진행한 후 저장되어 있는 종량제 가격을 리턴.

Request : 사용자의 정보가 담긴 Authorization header를 전송함.

Response : 성공 시 종량제 가격 리턴. 실패 시 실패 사유를 메시지로 리턴.

Response example)

```json
HTTP/1.1 200 OK
{
	"price" : 15000 (number)
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

Returns:

- 200 OK
- 401 Unauthorized (user status logout)
- 403 Forbidden (not allowed)

Note:

- 헤더에 명시된 사용자가 "관리자"여야만 요청할 수 있음.

<br>

### 2) 전체 계약목록에서 킥보드 업체 목록 및 요약정보

`/manage/products`

Method : **GET**

Description : 클라이언트가 요청하면 서버에서 헤더에 있는 아이디가 관리자 아이디인지 유효성 검사를 진행한 후 전체 킥보드 업체 목록 및 요약 정보를 리턴.

Request : 사용자의 정보가 담긴 Authorization header를 전송함.

Response : 성공 시 전체 킥보드 업체 목록 및 요약정보 리턴. 실패 시 실패 사유를 메시지로 리턴.

Response example)

```json
HTTP/1.1 200 OK
{
	"list" : [
		{
			"company_name" : "씽씽" (string),
			"price_per_hour" : 10000 (number),
			"service_location" : t (string list),
			"insurance" : true (boolean),
			"helmet" : false (boolean)
		},
		{
			"company_name" : "킥고잉" (string),
			"price_per_hour" : 12000 (number),
			"service_location" : ["금천구", "강북구", "강서구"] (string list),
			"insurance" : false (boolean),
			"helmet" : true (boolean)
		},
		...
	] (json list)
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

Returns:

- 200 OK
- 401 Unauthorized (user status logout)
- 403 Forbidden (not allowed)

Note:

- 헤더에 명시된 사용자가 "관리자"여야만 요청할 수 있음.

<br>

### 3) 킥보드 업체 상세정보 (계약목록과 정액제 모델) 명세를 바꿔야할듯

`/manage/products/{company-name}`

Method : **GET**

Description : 클라이언트가 킥보드 업체 명을 전송하면 서버에서 헤더에 있는 아이디가 관리자 아이디인지 유효성 검사를 진행한 후 킥보드 업체 명에 해당하는 상세 정보를 리턴.

Request : 사용자의 정보가 담긴 Authorization header, path variable로 킥보드 업체 명을 전송함.

Request example)

```json
/manage/products/씽씽
```

Response : 성공 시 path variable에 해당하는 킥보드 업체의 상세 정보 html 리턴. 실패 시 실패 사유를 메시지로 리턴.

Response example)

```json
HTTP/1.1 200 OK
{
	"brand_name" : "씽씽" (string),
	"text" : "소개글" (string),
	"helmet" : true | false (boolean),
	"insurance" : true | false (boolean),
	"price_per_hour" : 10000 (number),
	"images" : [
		"wrvaseoran298nad",
		...
	] (base64 encoded string list)
	"service_location" : ["관악구", "서초구", ...] (string list)
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

HTTP/1.1 404 Not Found
{
	"timestamp": "2021-08-09T21:53:01.1151735" (datetime),
	"status": 404 (number),
	"error": "NOT_FOUND" (string),
	"code": "KICKBOARD_BRAND_NOT_EXIST" (string),
	"msg": "존재하지 않는 킥보드 브랜드입니다." (string)
}
```

Returns:

- 200 OK
- 401 Unauthorized (user status logout)
- 403 Forbidden (not allowed)
- 404 Not Found (not exist kickboard brand)

Note:

- 헤더에 명시된 사용자가 "관리자"여야만 요청할 수 있음.

<br>

### 4) 계약 조회 - 현재 계약 종류, 종료 기간, 계약 중인 킥보드 업체 목록 및 요약정보

`/manage/contracts`

Method : **GET**

Description : 클라이언트가 해당 API를 호출하면 서버에서 헤더에 있는 아이디가 관리자 아이디인지 유효성 검사를 진행한 후 해당 고객 법인의 계약 종류, 종료 기간, 계약중인 킥보드 업체 및 계약정보 리턴

Request : 사용자의 정보가 담긴 Authorization header를 전송함.

Response : 성공 시 고객 법인의 계약 종류, 시작 날짜, 종료 날짜, 사용한 시간, 계약중인 킥보드 업체 목록 및 요약정보 리턴. 실패 시 실패 사유를 메시지로 리턴.

Response example)

```json
HTTP/1.1 200 OK - membership
{
  "type" : "membership" (string),
  "startdate" : "2020-12-31" (date),
  "duedate" : "2020-12-31" (date),
  "list" : [
		{
			"company_name" : "씽씽" (string),
			"service_location" : ["관악구", "서초구", "강남구"] (string list),
			"insurance" : true (boolean),
			"helmet" : false (boolean),
			"used_time" : 123 (number)
		},
		{
			"company_name" : "킥고잉" (string),
			"service_location" : ["금천구", "강북구", "강서구"] (string list),
			"insurance" : false (boolean),
			"helmet" : true (boolean),
			"used_time" : 123 (number)
		},
		...
  ] (json list)
}

HTTP/1.1 200 OK - plan

{
  "type" : "plan" (string),
  "startdate" : "2020-12-31" (date),
  "list" : [
		{
			"company_name" : "씽씽" (string),
			"price_per_hour" : 12000 (number),
			"service_location" : ["관악구", "서초구", "강남구"] (string list),
			"insurance" : true (boolean),
			"helmet" : false (boolean),
			"used_time" : 123 (number),
			"total_time" : 123(number)
		},
		{
			"company_name" : "킥고잉" (string),
			"price_per_hour" : 10000 (number),
			"service_location" : ["금천구", "강북구", "강서구"] (string list),
			"insurance" : false (boolean),
			"helmet" : true (boolean),
			"used_time" : 123 (number),
			"total_time" : 123(number)
		},
		...
  ] (json list)
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
<br>
'membership'

- type : "membership" 
- duedate : YYYY-MM-DD (date)
- list : 계약된 킥보드 회사 목록. element는 회사명, 시간당 금액, 서비스 지역, 보험 제공 여부, 헬멧 제공 여부, 사용시간을 가지고 있음.
<br>
'plan'

- type : "plan"
- startdate : YYYY-MM-DD (date)
- list : 계약된 킥보드 회사 목록. element는 회사명, 시간당 금액, 서비스 지역, 보험 제공 여부, 헬멧 제공 여부, 사용시간을 가지고 있음.

Returns:

- 200 OK (Success)
- 401 Unauthorized (user status logout)
- 403 Forbidden (Not allowed)

Note:

- 헤더에 명시된 사용자가 "관리자"여야만 요청할 수 있음.

<br>

### 6) 계약 추가

`/manage/contracts`

Method : **POST**

Description : 클라이언트가 계약 종류, 계약 시작날짜, 종료날짜를 전송하면 서버에서 헤더에 있는 아이디가 관리자 아이디인지 유효성 검사를 진행한 후 해당 고객 법인의 계약 정보를 남김.

Request : 사용자의 정보가 담긴 Authorization header, 계약 종류, 계약시작날짜, 계약종료날짜를 POST로 전송함.

Request example)

```json
http body - membership
{
  "type" : "membership" (string),
  "duedate" : "2020-12-31" (datetime),
  "startdate" : "2020-12-31" (datetime)
}

http body - plan
{
  "type" : "plan" (string),
  "startdate" : "2020-12-31" (datetime),
  "list" : [
	  {
		"brandname" : "씽씽" (string),
		"totaltime" : 30 (number),
	  },
	  {
		"brandname" : "지쿠터" (string),
		"totaltime" : 40 (number),
	  }
	  ...
  ]
}
```

Response : 통신 결과 및 메시지 리턴.

Response example)

```json
HTTP/1.1 201 Created
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
<br>

'membership'
- type : 계약 타입. "membership"
- date : YYYY-MM-DD
<br>

'plan'
- type : 계약 타입. "plan" 
- date : YYYY-MM-DD
- list : 계약할 킥보드 회사 목록, 업체별 구매시간

Returns:

- 200 OK (Success)
- 401 Unauthorized (user status logout)
- 403 Forbidden (Not allowed)

Note:

- 헤더에 명시된 사용자가 "관리자"여야만 요청할 수 있음.

<br>

### 8) 계약 갱신

`/manage/contracts`

Method : **PUT**

Description : 클라이언트가 계약 종류, 계약 시작날짜, 계약 종료날짜를 전송하면 서버에서 헤더에 있는 아이디가 관리자 아이디인지 유효성 검사를 진행한 후 해당 고객 법인의 계약 정보를 갱신함.

Request : 사용자의 정보가 담긴 Authorization header, 계약 종류, 킥보드 업체 이름 목록, 계약 기간을 PUT로 전송함.

Request example)

```json
http body - membership
{
  "type" : "membership" (string),
  "startdate" : "2020-12-31" (date),
  "duedate" : "2020-12-31" (date)
}

http body - plan
{
  "type" : "plan" (string),
  "list" : [
	  {
		  "brandname" : "씽씽" (string),
		  "totaltime" : 123 (number)
	  },
	  {
		  "brandname" : "지쿠터" (string),
		  "totaltime" : 123 (number)
	  }
	  ...
  ]
}
```

Response : 통신 결과 및 메시지 리턴.

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
<br>

'membership'
- type : 계약 타입. "membership"
- date : YYYY-MM-DD (date)
<br>

'plan'
- type : 계약 타입. "plan"
- list : 브랜드 이름, 업체별구매시간

Returns:

- 200 OK (Success)
- 401 Unauthorized (user status logout)
- 403 Forbidden (Not allowed)

Note:

- 헤더에 명시된 사용자가 "관리자"여야만 요청할 수 있음.
- 고객 법인의 계약된 킥보드 법인 중 일부 킥보드 법인을 삭제/추가하는 경우 해당 API를 이용.

<br>

### 10) 계약 삭제

`/manage/contracts`

Method : **DELETE**

Description : 클라이언트가 해당 API를 호출하면 서버에서 관리자 아이디인지 유효성 검사를 진행한 후 해당 고객 법인의 계약 정보를 삭제함.

Request : 사용자의 정보가 담긴 Authorization header을 전송함.

Response : 통신 결과 및 메시지 리턴.

Response example)

```json
HTTP/1.1 204 No Content - membership

HTTP/1.1 204 - plan

{
	list : ["씽씽", "지쿠터",...] 
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

Returns:

- 204 No Content (Success)
- 401 Unauthorized (user status logout)
- 403 Forbidden (Not allowed)

Note:

- 헤더에 명시된 사용자가 "관리자"여야만 요청할 수 있음.

<br>


### 12) 알림 확인

`/manage/alarms`

Method : **GET**

Description : 클라이언트가 해당 API를 호출하면 서버에서 관리자 아이디인지 유효성 검사를 진행한 후 해당 고객 법인이 등록해 둔 알림 목록을 리턴함.

Request : 사용자의 정보가 담긴 Authorization header을 전송함.

Response : 성공 시 알림 목록 리턴, 실패 시 메시지 리턴.

Response example)

```json
HTTP/1.1 200 OK
{
  "list" : [
		{
			"type" : "cost" (string) | "time" (string),
		  "value" : 2500 (number)
		},
		{
			"type" : "cost" (string) | "time" (string),
		  "value" : 2500 (number)
		},
		...
  ] (json list)
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

HTTP/1.1 404 Not Found
{
	"timestamp": "2021-08-09T21:53:01.1151735" (datetime),
	"status": 404 (number),
	"error": "NOT_FOUND" (string),
	"code": "COMPANY_NOT_EXIST" (string),
	"msg": "존재하지 않는 법인입니다." (string)
}
```

Validation:

- type : 알림 종류. "cost" 또는 "time" 2개 값 중 하나임.

Returns:

- 200 OK (Success)
- 401 Unauthorized (user status logout)
- 403 Forbidden (Not allowed)
- 404 Not Found (company not exist)

Note:

- 헤더에 명시된 사용자가 "관리자"여야만 요청할 수 있음.

<br>

### 13) 알림 수정

`/manage/alarms`

Method : **POST**

Description : 클라이언트가 알림 종류, 알림 조건 목록을 전송하면 서버에서 관리자 아이디인지 유효성 검사를 진행한 후 해당 고객 법인이 등록해 둔 알림 목록을 갱신함.

Request : 사용자의 정보가 담긴 Authorization header, 알림 종류&알림 조건의 목록을 전송함.

Requset example)

```json
http body
{
	"list" : [
		{
			"type" : "cost" (string) | "time" (string),
		  "value" : 2500 (number)
		},
		{
			"type" : "cost" (string) | "time" (string),
		  "value" : 2500 (number)
		},
		...
  ] (json list)
}
```

Response : 통신 결과 및 메시지 리턴.

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

- type : 알림 종류. "cost" 또는 "time" 2개 값 중 하나임.

Returns:

- 200 OK (Success)
- 401 Unauthorized (user status logout)
- 403 Forbidden (Not allowed)

Note:

- 헤더에 명시된 사용자가 "관리자"여야만 요청할 수 있음.

<br>