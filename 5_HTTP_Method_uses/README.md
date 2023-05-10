# 5. HTTP 메서드 활용
- 클라리언트에서 서버로 데이터 전송
- HTTP API 설계 예시
## 1. 클라이어느에서 서버로 전송 - 데이터 전달 방식 크게 2가지
1. 쿼리 파라미터를 통한 데이터 전송
- GET
- 주로 정렬 필터(검색)
2. 메세지 바디를 통한 데이터 전송
- POST, PUT, PATCH
- 회원 가입, 상품 주문, 리소스 등록, 리소스 변경
### 클라이언트에서 서버로 데이터 전송 - 4가지 상황
1. 정적 데이터 조회: 이미지, 정적 텍스트 문서
2. 동적 데이터 조회: 주로 검색, 게시판 목록에서 정렬 필터
3. HTML Form을 통한 데이터 전송: 회원 가입, 상품 주문, 데이터 변경
4. HTML API를 통한 데이터 전송: ~, 서버 to 서버

### 정적 데이터 조회 - 쿼리 파라미터 미사용
- 이미, 정적 텍스트 문서
- GET
- 정적 데이터는 일반적으로 쿼리 파라미터 없이 리소스 경로로 단순하게 조회 가능

### 동적 데이터 조회 - 쿼리 파라미터 사용
- GET, 정렬 필터

### HTML Form - POST 전송 - 저장/ GET 전송 - 조회
```
POST /save HTTP/1.1
Host: localhost:8080
Content-Type: application/x-www-form-uriencoded

useranem=kim&age=20
```
> `Content-Type: application/x-www-form-uriencoded`
### HTML Form - multipart/form-data
```
POST /save HTTP/1.1
Host: localhost:8080
Content-Type: mutlipart/form-data; boundary=-----XXX
Content-Length: 10457

-----XXX
Content-Disposition: from-data; name="username"

kim
-----XXX
Content-Disposition: from-data; name="age"

20
-----XXX
Content-Disposition: from-data; name="file1"; filename="intro.png"
Content-Type: image/png

109238a9o0p3eqwokjasd09ou3oirjwoe9u34ouief...
```
> `Content-Type: mutlipart/form-data; boundary=-----XXX`  
`Content-Disposition: from-data; name="username"`
### HTML Form 데이터 전송 - 정리
- HTML Form submit시 `POST 전송`
> 회원 가입, 상품 주문, 데이터 변경
- `Content-Type: application/x-www-uriencoded 사용`
> form의 내용을 메세지 바디를 통해서 전송(key=value, 쿼리 파라미터 형식)  
전송 데이터를 uri encoding처리
- HTML Form은 `GET 전송`도 가능
- `Content-Type: multipart/form-data`
- cf: HTML Form 전송은 GET, POST만 지원

## 2. HTTP API 데이터 전송
```
POST /members HTTP/1.1
Content-Type: application/json
{
    "username": "young",
    "age": 20
}
```
> `Content-Type: application/json`
### HTTP API 데이터 전송 - 정리
- `서버 to 서버`: 백엔드 시스템 통신
- 앱 클라이언트
- 웹 클라이언트: AJAX, React, VueJS
- POST, PUT, PATCH: 메세지 바디를 통해 데이터 전송
- GET: 조회, 쿼리 파라미터로 데이터 전달
- `Content-Type: application/json 주로 사용`
### HTTP API 설계 예시
- HTTP API - `컬랙션`
> POST 기반 등록, ex) 회원 관리 API 제공
- HTTP API - `스토어`
> PUT 기반 등록, ex) 정적 컨텐츠 관리, 원격 파일 관리
- `HTTP FORM 사용`
> GET, POST만 지원, ex) 웹 페이지 회원 관리

### 회원 관리 시스템 - API 설계 - POST 기반 등록
- 회원 목록 /members > GET
- 회원 등록 /members > POST
- 회원 조회 /members/{id} > GET
- 회원 수정 /members/{id} > PATCH, PUT POST
- 회원 삭제 /members/{id} > DELETE
### 회원 관리 시스템 - POST - 신규 자원 등록 특징
- 클라이언트는 등록될 리소스의 URI를 모른다
- 서버가 새로 등록된 URI를 생성해준다.
- `컬렉션`
> 서버가 리소의 URI를 생성하고 관리

### 파일 관리 시스템 - API 설계 - PUT 기반 등록
- 파일 목록 /files > GET
- 파일 등록 /files/{filename} > PUT
- 파일 조회 /files/{id} > GET
- 파일 삭제 /files/{id} > DELETE
- 파일 대량 등록 /fiels > POST
### 파일 관리 시스템 - PUT - 신규 자원 등록 특징
- 클라이언트가 리소스 URI를 알고 있어야 한다
- 클라이언트가 직접 리소스의 URI를 지정한다.
- `스토어`

### HTML FORM 사용
- HTML FORM은 GET, POST만 지원
- AJAX같은 기술을 사용해서 해결 가능 > 회원 API 참고
- 여기서는 순수 HTML, HTML FORM 이야기
### HTML FORM 사용 - GET POST
- 회원 목록 /members > GET
- 회원 등록 폼 /members/new > GET
- 회원 등록 /member/new, /members > POST
- 회원 조회 /members/{id} > GET
- 회원 수정 폼 /members/edit/{id} > GET
- 회원 수정 /members/edit/{id}, /members/{id} > PATCH, PUT POST
- 회원 삭제 /members/{id} > DELETE
### HTML FORM 사용 - 정리
- GET, POST만 지원
- `컨트롤 URI`
> GET, POST만 지원하므로 제약이 있음  
이런 제약을 해결하기 위해 동사로 된 리소스 경로 사용  
/new, /edit, /delete가 컨트롤 URI
HTTP 메서드로 해결하기 애매한 경우 사용

### 정리
- HTTP API - 컬렉션
> POST 기반 등록  
서버가 리소스 URI 결정
- HTTP API - 스토어
> PUT 기반 등록  
클라이언트가 리소스 URI 결정
- HTML FORM 사용
> 순수 HTML + HTML form 사용  
GET, POST만 지원