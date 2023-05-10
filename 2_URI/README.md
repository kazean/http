# 2. URI와 웹 브라우저 요청 흐름
- URI
- 웹 브라우저 요청 흐름

## 1. URI(Uniform Resource Identifier)
- URI? URL? URN?
> "URI는 로케이터(Locator), 이름(Name) 또는 둘 다 추가로 분류될 수 있다"
> URI = URL, URN
### URI 단어뜻
- Uniform: 리소스 식별하는 통일된 방식
- Resource: 자원
- Identifier: 다른 항목과 구분하는데 필요한 정보
### URL, URN 단어 뜻
- URL - Locator: 리소스가 있는 위치를 지정
- URN - Name: 리소스에 이름을 부여
> 위치는 변할수 있지만, 이름은 변하지 않는다, urn:isbn:8960777331  
!URN 이름만으로 실제 리소스를 찾을 수 있는 방법이 보편화 되지 않음  
URI = URL과 같은 의미로 부여

### URL 전체 문법
- scheme://[userinfo@]host[:port][/path][?query][#fragment]
> https://www.google.com/search?q=hello&hl=ko
- 프로토콜(https), 호스트명(www.google.com), 포트(443), 패스(/search), 쿼리 파라미터(q=hello&hl=ko)
### URL - scheme
주로 프로토콜 사용: http, https, ftp
> 포트는 생략 가능
### URL - userinfo
사용자 정보를 포함해서 인증
> !거의 사용 X
### URL - host
호스트명, 도메인명, IP 주소
### URL - port
포트
> 일반적으로 생략, 생략시 http: 80, https: 443
### URL - path
리소스 경로(path), 계층적 구조
### URL - query
- key=value 형태
- ?로 시작, &로 추가가능
- query parameter, query string 등으로 불림
### URL - fragment
- html 내부 북마크 등에 사용
- 서버에 전송하는 정보 아님

## 2. 웹 브라우저 요청 흐름
- HTTP 요쳥 메세지
```
GET /search?q=hello&hl=ko HTTP/1.1
HOST: www.google.com
```
- HTTP 응답 메세지
```
HTTP/1.1 200 OK
Content-Type:
Content-Length:
<html>
 ~ 
</html>
```