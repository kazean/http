# 6. HTTP 상태코드
### 상태 코드
클라이언트가 보낸 요청의 처리 상태를 응답에서 알려주는 기능
- 1xx(Informational): 요청이 수신되어 처리중
- 2xx(Successful): 요청 정상 처리
- 3xx(Redirection): 요청을 완료하려면 추사 행동이 필요
- 4xx(Client Error)
- 5xx(Server Error)
### 만약 모르는 상태 코드가 나타나면?
- 클라이언트는 상위 상태코드로 해석해서 처리
- 미래에 새로운 상태 코드가 추가되어도 클라이언트를 변경하지 않아도 됨

## 1. 1xx(Informational)
요청이 수신되어 처리중
- 거의 사용 X

## 2. 2xx - 성공
### 2xx (Successful)
클라이언트의 요청을 성공적으로 처리
- 200 OK
- 201 Created
- 202 Accepted
- 204 No Content
### 200 - OK
### 201 - Create
요청 성공해서 새로운 리소스가 생성됨
> Res > `Location: /members/100`
### 202 - Accepted
요청이 접수되었으나 처리가 완료 되지 않음
- 배치 처리
### 204 - No Content
서버가 요청을 성공적으로 수행했지만, 응답 페이로드 본문에 보낼 데이터가 없음
- ex) 웹 편집기 SAVE 버튼

## 3. 3xx - 리다이렉션
### 3xx (Redirection)
요청을 완료하기 위해 유저 에이전트의 추가 조치 필요
- 300 Multiple Choices
- 301 Moved Permanently
- 302 Found
- 303 See Other
- 304 Not Modifiend
- 307 Temporary Redirect
- 308 Permanent Redirect
### 리다이렉션 이해
웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, `Location` 위치로 자동 이동(리다이렉트)
### 리다이렉션 이해 - 종류
- `영구 리다이렉션`
- `일시 리다이렉션`
> `PRG`
- 특수 리다이렉션: 결과 대신 캐시를 사용
### 영구 리다이렉션
301, 308
- 리소스 URI가 영구적으로 이동
- 원래의 URL를 사용 X, 검색 에닌 등에서도 변경을 인지
- 301 Moved Permanently
> 리다이렉트 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있음(MAY)
- 308 Permanent Redirect
> 리다이렉트시 요청 메소드와 본문 유지(처음 POST를 보내면 리다이렉트도 POST)
### 일시 리다이렉션
302, 303, 307
- 리소스 URI가 일시적으로 변경
- 따라서 검색 엔진 등에서 URL을 변경하면 안됨
- 302 Found
> 리다이렉트 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있음(MAY)
- 307 Temporary Redirect
> 리다이렉트시 요청 메서드와 본문을 유지
- 303 See Other
> = 302

### PRG: POST/Redirect/GET
- POST로 주문후에 웹 브라우저 새로고침시 중복 주문 방지

### 그래서 뭘 써야하나요?
302, 307, 303
- 모호한 302 대신 303, 307 등장
- 현실
> 이미 많은 애플리케이션 라이브러리들이 302를 기본값으로 사용, 자동 리다이렉션시에 GET으로 변경해도 되면 그냥 302를 사용해도 큰 문제 없음

### 기타 리다이렉션
- 300 Multiple Choices: 안쓴다
- `304 Not Modified`: `캐시`를 목적으로 사용
> 304 응답은 응답에 메세지 바디를 포함하면 안된다(로컬 캐시)  
조건부 GET, HEAD 요청시 사용

## 4. 4xx - 클라이언트 오류, 5xx - 서버 오류
### 4xx (Client Error)
- 클라이언트의 요청에 잘모소딘 문법 등으로 서버가 요청을 수행할 수 없음
- 오류의 원인이 클라이언트에 있음
- 중요! 클라이언트가 이미 잘못된 요청, 똑같은 재시도는 실패함
### 400 Bad Request
클라이언트가 잘못된 요청을 해서 서버가 요청을 처리할 수 없음
### 401 Unauthorized
클라이언트가 해당 리소스에 대한 인증이 필요함
- `인증(Authentication)` 되지 않음
- 401 오류 발생시 응답에 WWW-Authentication 헤더와 함께 인증 방법을 설명
### 403 Forbidden
- 주로 인증 자격 증명은 있지만, `접근 권한`이 불충분한 경우
### 404 Not Found
- 요청 리소스를 찾을 수 없음
### 5xx (Server Error)
서버 오류
### 500 Internal Server Error
### 503 Service Unavailable
서비스 이용 불가
- 서버가 일시적인 과부하 또는 예정된 작업으로 잠시 요청을 처리할 수 없음
- Retry-After 헤더 필드로 얼마뒤에 복구되는지 보낼 수도 있음