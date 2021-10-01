## 킥보드 이용

### 1) 킥보드 위치 목록 - 수정 必, 보류.

front에서 어떤 식으로 호출할지 몰라 보류.

`/kickboard/location`

Method : **GET**

Description : 클라이언트가 요청하면 서버에서 해당 사용자가 속해있는 법인이 계약한 킥보드 위도 경도 배터리 모델명 각 킥보드별 이전 사용자 이용 사진을 리턴함.

Request : 사용자의 정보가 담긴 Authorization header, 기업인증 코드를 전송함.

Request example)

```json
url
/kickboards/location
```

Response : 성공 시 해당 기업이 계약한 킥보드 브랜드 이름, 위도, 경도, 배터리, 모델명, 이전 사용자가 찍은 사진을 리턴, 실패 시 실패 사유를 메시지로 리턴.

Response example)

```json
HTTP/1.1 200 OK
{
	"list" : [
		{
		  "company_name" : "씽씽" (string),
		  "lat" : 37.566570 (number),
		  "lng" : 126.978442 (number),
			"battery" : 100 (number),
			"model" : "AAAAA" (string),
			"past_picture": "http:// ... /kickboard.jpg"
		},
		{
		  "company_name" : "킥고잉" (string),
		  "lat" : 37.55377475931086 (number),
		  "lng" : 126.96435101606421 (number),
			"battery" : 100 (number),
			"model" : "BBBBB" (string),
			"past_picture": "http:// ... /kickboard.jpg"
		},
		...
	](json list)
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

<br>

### 2) 킥보드 이용 내역 조회 - 개인

`/kickboard/consumption?from=x&to=y&page=z&unit=w`

Method : **GET**

Description : 클라이언트가 조회 시작 기간, 조회 종료 기간, 현재 페이지 수, 한 페이지에 들어갈 내역 개수를 전송하면 서버에서 조회 시작 기간부터 조회 종료 기간까지 한 페이지에 들어갈 내역 개수와 현재 페이지 수에 맞게 해당 사용자의 이용 내역을 리턴함.

Request : 사용자의 정보가 담긴 Authorization header, query parameter로 조회 시작 기간, 조회 종료 기간, 현재 페이지 수, 한 페이지에 들어갈 내역 개수를 전송함.

Request example)

```json
url
/kickboards/usages?from=2020-12-01&to=2021-01-01&page=1&unit=10
```

Response : 성공 시  조회 시작 기간부터 조회 종료 기간까지 한 페이지에 들어갈 내역 개수와 현재 페이지 수에 맞게, 현재 페이지, 유닛 개수, 해당 사용자의 이용 내역을 리턴, 실패 시 실패 사유를 메시지로 리턴.

Response example)

```json
HTTP/1.1 200 OK
{
  "page" : 1 (number),
  "unit" : 10 (number),
  "total_time" : 100 (number),
	"history" : 
	[
		{
			"brand" : 씽씽 (number),
			"depart_time" : "2020-10-10T14:20:15" (string, UTC),
			"arrive_time" : "2020-10-10T14:25:40" (string, UTC),
			"location_list" :
				[
					{
						"latitude" : 131.0 (number),
						"longitude" : 131.1 (number)
					},
					{
						"latitude" : 131.2 (number),
						"longitude" : 131.6 (number)
					},
					...
				] (list of json),
			"cycle" : 5000 (number)
		},
		{
			"brand" : 킥고잉 (number),
			"depart_time" : "2020-07-28T14:20:15" (string, UTC),
			"arrive_time" : "2020-07-28T14:25:40" (string, UTC),
			"location_list" :
				[
					{
						"latitude" : 131.0 (number),
						"longitude" : 131.1 (number)
					},
					{
						"latitude" : 131.2 (number),
						"longitude" : 131.6 (number)
					},
					...
				] (list of json),
			"cycle" : 5000 (number)
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

- datetime : YYYY-MM-DDTHH:mm:ss (utc)
- from : YYYY-MM-DD (date)
- to : YYYY-MM-DD (date)

Returns:

- 200 OK (Success)
- 401 Unauthorized (user status logout)
- 403 Forbidden (not allowed)

Note:

- 헤더에 명시된 사용자가 "사용자"여야만 요청할 수 있음.
- cycle : 위치 좌표 측정 주기. 단위 ms.

Default:

- page : 1
- unit : 10
- from : to값에 들어있는 달의 시작 날짜
- to : default인 경우 조회하는 시점 날짜

<br>

### 3) 킥보드 이용 내역 추가 - 개인

<br> 추가적으로, 킥보드 이용을 하려 할 때 기업의 사용시간이 over되었으면 사용하지 못하게 하는 기능도 필요하지 않을까?<br>

`/kickboard/consumption`

Method : **POST**

Description : 클라이언트가 출발시간, 도착시간, 이동거리, 위치정보, 위치정보 측정 주기를 담은 JSON과 사진 파일 2개를 form으로 전송하면 서버는 해당 이용 내역을 저장함.

Requset : 사용자의 정보가 담긴 Authorization header, multipart/form-data를 담은 content-type header, 출발시간, 도착시간, 이동거리, 위치정보, 위치정보 측정 주기를 POST로 전송함.

Header content type : multipart/form-data

Request example)

```json
http body
body content type : multipart/form-data

FILE
image : <사진>

application/json
detail : 
{
	"brand" : "킥고잉" (string),
	"depart_time" : "2020-10-10T14:20:15" (string, UTC),
	"arrive_time" : "2020-10-10T14:25:40" (string, UTC),
	"kickboard_id" : 1 (number),
	"location_list" :
	[
		{
			"latitude" : 131.0 (number),
			"longitude" : 131.1 (number)
		},
		{
			"latitude" : 131.2 (number),
			"longitude" : 131.6 (number)
		},
		...
	] (list of json),
	"cycle" : 5000 (number),
}
```

Response : 통신 결과 및 메시지 리턴.

Response example)

```json
HTTP/1.1 201 OK
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

HTTP/1.1 404 NOT FOUND
{
	"timestamp": "2021-08-09T21:50:40.2363793" (datetime),
	"status": 403 (number),
	"error": "NOT_FOUND" (string),
	"code": "CONTRACT_NOT_EXIST" (string),
	"msg": "계약이 존재하지 않습니다." (string)
}

HTTP/1.1 404 NOT FOUND
{
	"timestamp": "2021-08-09T21:50:40.2363793" (datetime),
	"status": 403 (number),
	"error": "NOT_FOUND" (string),
	"code": "KICKBOARD_NOT_EXIST" (string),
	"msg": "존재하지 않는 킥보드입니다." (string)
}
```

Validation:

- datetime 형식 : YYYY-MM-DDTHH:mm:ss

Returns:

- 200 OK (Success)
- 401 Unauthorized (user status logout)
- 403 Forbidden (not allowed)

Note:

- 헤더에 명시된 사용자가 "사용자"여야만 요청할 수 있다.
- cycle : 위치 좌표 측정 주기. 단위 ms.

<br>

### 4) 킥보드 이용 - 보류

가상으로 구현

<br>

### 5) 킥보드 리포트 - 보류

가상으로 구현

<br>