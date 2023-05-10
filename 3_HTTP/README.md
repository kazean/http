# 3. HTTP
- 모든 것이 HTTP
- 클라이언트 서버 구조
- Stateful, Stateless
- 비 연결성(Connectionless)
- HTTP 메세지

## http 1.0과 1.1 차이
### HTTP 1.0
- 요청 및 응답에 대한 메타 데이터를 포함한 `헤더 필드` 제공(Status Code, Content-Type)
- Response: Content-Type > HTTP, Script, style-sheet 등 전송 가능
- Method
- Connection: 응답 직후 종료
### HTTP 1.0 vs 1.1
- 커넥션 유지(Psersistent Connection)
> 1 GET / 1 Connection > N GET / 1 Connection
- 파이프 라이닝(Pipelining)
> 한번에 여러 요청, 응답
- 호스트 헤더(Host Header)
> 버추얼 호스팅  
1 IP  / 1 Domain > 1 IP > N Domain
- 강력한 인증 절차(Improved Authentication Procedure)
> proxy-authentication, authorization: 프록시를 통한 인증
### HTTP 2.0
- Mutiplexed Stream
> 한 Connection에 동시에 여러 메세지를 주고 받을 수 있으며, Response 순서에 상관없이 stream으로 주고 받는다
- Stream Priority
- Server Push
> 클라이언트가 요청하지 않은 리소스 예상하여 전송(css, js) 
- Header Compression
> Header Table, Huffman Encoding 기법(HPAC)을 이용한 압축 방법  
이전 Header와 중복되는 내용과 필드를 재전송하지 않는다.

## 1. HTTP(HyperText Transfer Protocol)
### 모든 것이 HTTP
HTTP 메세지에 모든 것을 전송
- HTML, TEXT, IMAGE, JSON, ...
### HTTP 역사
- HTTP/0.9: GET, HTTP 헤더 X
- HTTP/1.0: 메서드, 헤더 추가
- HTTP/1.1: 가장 많이 사용
- HTTP/2.0: 성능 개선
- HTTP/3 진행중: TCP 대신 UDP 사용, 성능 개선
### 기반 프로토콜
- TCP: HTTP/1.1, HTTP/2
- UDP: HTTP/3
### HTTP 특징
- 클라이언트 서버 구조
- 무상태 프로토콜(Stateless), 비연결성
- HTTP 메세지
- 단순함, 확장 가능

## 2. 클라이언트 서버 구조
- Request, Response

## 3. 무상태 프로토콜 스테이스리스
- 서버가 클라이언트의 상태를 보존 X
> 장점: 서버 확장성이 높음 (Scale-Out)  
단점: 클라이언트가 추가 데이터 전송
### Stateful, Stateless 차이
- Stateful
> 상태 유지, 항상 같은 서버 유지
- Stateless
> 무상태, 서버 수평 확장 유리
### Stateless 실무 한계
- 모든 것을 무상태로 설계 할 수 없다.
> 브라우저 쿠키와 서버 세션 등을 사용해서 상태 유지, 상태 유지는 최소한만 사용

## 4. 비 연결성(Connectionless)
- 연결을 유지하는 모델/유지하지 않는 모델
### 비 연결성
- HTTP 는 기본이 연결을 유지하지 않는 모델
- 초단위 빠른 속도로 응답
- 한 시간동안 수천명이 사용해도 동시에 처리하는 요청은 수십개 이하로 매우 작음
- 서버 자원 효율적 사용가능
### 비 연결성 - 한계와 극복
- TCP/IP 연결을 새로 맺어야 함
> HTTP 지속 연결(Persistent Connections), HTTP/2.0,3 에서 더 많은 최적화
### HTTP Persistent Connections

## 5. HTTP 메세지
모든 것이 HTTP  
HTTP 요청/응답 메세지
- HTTP 메세지 구조
```
start-line

header 헤더

empty line (CRLF)

message body
```
### 시작 라인 - 요청 메세지
- start-line = request-line / status-line
- request-line = method SP request-target SP HTTP-version CRLF
### 시작 라인 - 요청 메세지 - HTTP 메서드
- GET, POST, PUT, DELETE
### 시작 라인  - 요청 메세지 - 요청 대상
- absolute-path[?query]
### 시작 라인  - 요청 메세지 - HTTP 버전
### 시작 라인  - 응답 메세지
- status-line = HTTP-version SP status-code SP reason-pharse CRLF

### HTTP 헤더
- headerfield = field=name":"OWS field-value OWS
- field-name은 대소문자 구문 없음
### HTTP 헤더 - 용도
HTTP 전송에 필요한 모든 부가정보
### HTTP 메세지 바디 - 용도
### 단순함 확장 가능
### HTTP 정리
- HTTP 메세지에 모든 것을 전송
- HTTP 역사 HTTP/1.1
- 클라이언트 서버 구조
- 무상태 프로토콜(Stateless)
- HTTP 메세지
- 단순함, 확장 가능
- 지금은 HTTP 시대