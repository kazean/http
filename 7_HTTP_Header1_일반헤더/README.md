# 7. HTTP 헤더1 - 일반 헤더
## 1. HTTP 헤더 개요
### HTTP 헤더
- `header-field = field-name":" OWS field-value OWS`
- field-name은 `대소문자 구문 없음`
### HTTP 헤더 - 용도
- HTTP 전송에 필요한 모든 부가 정보
- 필요시 임의의 헤더 추가 기능
### HTTP 헤더 - 분류 - RFC2616(과거)
- 헤더 분류
> General 헤더: 메세지 전체에 적용되는 정보, Connection: close  
Request 헤더: 요청정보  
Response 헤더: 응답정보  
`Entity 헤더`: 엔티티 바디 정보
### HTTP BODY - message body - RFC2616(과거)
- `메세지 본문(message body)은 엔티티 본문(entity body)`을 전달하는데 사용
- 실제 데이터
- 엔티티 헤더는 엔티티 본문의 데이터를 해석할 수 있는 정보 제공
### RFC723x 변화
- `엔티티(Entity) > 표현(Representation)`
- `Representation = representation Metadata + Representation Data`
### HTTP BODY - message body - RFC7230(최신)
- 메세지 본문을 통해 표현 데이터 전달
- `메세지 본문 = 페이로드(payload)`
- `표현은 실제 데이터`
- 표현 헤더

## 2. 표현
- Content-Type: 표현 데이터 형식
- Content-Encoding: 압축 방식
- Content-Language: 자연 언어
- Content-Length: 길이

## 3. 협상(콘텐츠 네고시에이션) - 클라이언트가 선호하는 표현 요청
- Accept: 클라이언트가 선호하는 미디어 타입
- Accept-Charset: 문자 인코딩
- Accept-Encoding
- Accept-Language
> 협상 헤더는 요청시에만 사용
### 협상과 우선순위1 - Quality Valeus(q)
- 0 ~ 1, 클수록 높은 우선순위
- 생략하면 1
- ex) Accept-Language: ko-KR, ko;q=0.9, en-US;q=0.8
### 협상과 우선순위2 - Quality Valeus(q)
- 구체적인 것이 우선한다
- ex) Accept: text/*, text/plain, text/plain;format=flowed, */*
1. text/plain;format=flowed
2. text/plain
3. text/*
4. */*
### 협상과 우선순위3 - Quality Valeus(q)
- 구체적인 것을 기준으로 미디어 타입을 맞춘다.

## 4. 전송 방식
- Trnasfer-Encoding
- Range, Content-Range
### 전송 방식 설명
- 단순 전송
- 압축 전송
- 분할 전송
- 범위 전송
### 단순 전송 - Content-Length
### 압축 전송 - Content-Encoding: gzip
### 분할 전송 - Transfer-Encoding: chucked
```
HTTP/1.1 200 OK
Content-Type: text/plain
Transfer-Encoding: chucked

5
Hello
5
World
0
\r\n
```
### 범위 전송 - Range(Req), Content-Range(Res)
- Request
```
GET /evnet
Range: byes=1001-2000
```
- Response
```
HTTP/1.1 200 OK
Content-Range: bytes=1001-2000/2000

qqweqeqeq131r31mr31o
```

## 5. 일반정보
- From: 유저 에이전트의 이메일 정보
- Referer: 이전 웹 페이지 주소
- User-Agent
- Server
- Data
### 특별한 정보
- Host: 요청한 호스트 정보(도메인), 한 IP 여러 도메인
- `Location`: 페이지 리다이렉션
> 201(Create): Location 값은 요청에 의해 생성된 리소스 URI  
3xx(Redirection): Location 값은 요청을 자동으로 리다이렉션하기 위한 대상 리소스
- Allow: 허용 가능한 HTTP 메서드
> 405(Method Not Allowed)에서 응답에 표함해야함
- Retry-After: 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간
> 날짜, 초단위 표기

## 6. 인증
- `Authorization`: 클라이언트 인증 정보를 서버에 전달
- `WWW-Authentication`: 리소스 접근시 필요한 인증 방법 정의
> 401(Unauthorized)

## 7. 쿠키
- `Set-Cookie`: 서버에서 클라리언트로 쿠키 전달(응답)
- `Cookie`: 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청시 서버로 전달
### Stateless
- HTTP는 무상태 프로토콜이다
### 쿠키
- 사용처
> 사용자 로그인 세션 관리, 광고 정보 트레킹
- 쿠키 정보는 항상 서버에 전송됨
> 네트워크 트래픽 추가 유발, 최소한의 정보만 사용
- 주의!
> 보안에 민간함 데이터는 저장하면 안됨
### 쿠키 - 생명주기 - Expire, max-age
- `Set-Cookie: expires=Sat, 26-Dec-2020 04:39:21 GMT`
- `Set-Cookie: max-age=3600 (초)`
- `세션 쿠키`: 만료 날짜 생략시 브라우저 종료시 까지만 유지
- `영속 쿠키`: 만료 날짜 지정
### 쿠키 - 도메인 Domain
- 명시: 명시한 문서 기준 도메인 + 서브 도메인 포함
- 생략: 현재 문서 기준 도메인만 적용
### 쿠키 - 경로
- 이 경로를 포함한 하위 경로 페이지만 쿠키 접근
- 일반적으로 path=/ 루트로 지정
### 쿠키 - 보안 Secure, HttpOnly, SameSite
- `Secure`: Https 만 전송
- `HttpOnly`: XSS 방비, 자바스크립트에서 접근 불가(document.cookie)
- `SameSite`: XSRF 공격 방지, 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송