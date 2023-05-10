# 8. HTTP 헤더2 - 캐시와 조건부 요청
## 1. 캐시 기본 동작
### 캐시가 없을 때
- !데이터가 변경되지 않아도 계속 네트워크를 통해서 데이터를 다운로드 받아야 한다.
> !인터넷 네트워크는 매우 느리고 비싸다, 브라우저 로딩 속도가 느리다, 느린 사용자 경험
### 캐시 적용
- `cache-control: max-age=60`
- 캐시 덕분에 캐시 가능 시간동안 네트워크를 사용하지 않아도 된다
> 비싼 네트워크 사용량을 줄일 수 있다, 브라우저 로딩 속도가 매우 빠르다, 빠른 사용자 경험
### 캐시 적용 - 캐시 시간 초과 
- 캐시 유효 시간이 초과하면, 서버를 통해 데이터를 다시 조회하고, 캐시를 갱신한다.

## 2. 검증 헤더와 조건부 요청1
### 캐시 시간 초과
1. 서버에서 기존 데이터를 변경함
2. 서버에서 기존 데이터를 변경하지 않음
> 변경하지 않았을 경우에 저장해 두었던 캐시를 재사용 할 수 있다.
### 검증 헤더 추가
- `(Server) Last-Modified: 2020년 11월 10일 10:00:00`
### 캐시 시간 초과 후 요청
- `(Client) if-modified-since: 2020년 11월 10일 10:00:00`
> [변경X] (Server) `304 Not Modified,` cache-control, Last-Modifiend, message body(X)
> [변경] (Server) `200 OK`, cache-control, Last-Modifiend, message body
### 검증 헤더와 조건부 요청 - 정리
- 304 Not Modifed + 헤더 메타 정보만 응답(바디X)
- 응답 헤더 정보로 캐시의 메타 정보를 갱신
- 클라이언트는 캐시에 저장되어 있는 데이터 재활용

## 3. 검증 헤더와 조건부 요청2
### 검증 헤더와 조건부 요청
- 검증 헤더
> `Etag, Last-Modified`
- 조건부 요청 헤더
> `if-None-Match, if-modified-since`
>> 조건이 만족하면(변경 되었으면): 200 OK  
조건이 만족하지 않으면 304 Modified
### 검증 헤더와 조건부 요청 - Last-Modified, If-Modified-Since 단점
- 1초 미만 단위로 캐시 조정이 불가능
- 날짜 기반의 로직 사용
- 데이터를 수정해서 날짜가 다르지만, 같은 데이터를 수정해서 데이터 결과가 똑같은 경우
- 서버에서 별도의 캐시 로직을 관리하고 싶은 경우
### 검증 헤더와 조건부 요청 - Etag, If-None-Match
- `Etag(Entity Tag)`
- 데이터가 변경되면 이 이름을 바꾸어서 변경함(Hash를 다시 생성)
### 검증 헤더와 조건부 요청 - Etag, If-None-Match 정리
- 진짜 단순하게 ETag만 서버에 보내서 같으면 유지, 다르면 받기!
- 캐시 제어 로직을 서버에서 완전히 분리
- 클라이언트는 단순히 이 값을 서버에 제공

## 4. 캐시와 조건부 요청 헤더
### 캐시 제어 헤더
- Cache-Control
- Pragma: 캐시 제어(하위 호환) HTTP/1.0이하
- Expires: 캐시 유효 기간(하위 호환)
### Cache-Control
- Cache-Control: max-age
> 캐시 유효 시간, 초단위
- `Cache-Control: no-cache`
> 데이터는 캐시해도 되지만, 항상 원(origin) 서버에 검증하고 사용
- `Cache-Control: no-store`
> 데이터에 민감정보가 있으므로 저장하면 안됨
### Pragma - 캐시 제어 (하위 호환)
- `Pragma: no-cache`
- HTTP 1.0 하위 호환
### Expires - 캐시 만료일 지정 (하위 호환)
- 지금은 더 유연한 Cache-Control: max-age 권장
- Cache-Control: max-age와 함께 사용하면 Expires는 무시
### 검증 헤더와 조건부 요청 데이터
- 검증 헤더(Validator)
> `Etag, Last-Modified`
- 조건부 요청 하데
> `If-Match, If-None-Match`  
`If-Modified-Since, If-Unmodified-Since`

## 5. 프록시 캐시
### 원 서버 직접 접근 - origin 서버
### 프록시 캐시 도입
### Cache-Contorol - 캐시 지시어(directives) - 기타
- Cache-Control: public
> 응답이 public 캐시에 저장되어도 됨
- Cache-Control: private
> 응답이 해당 사용자만을 위한 것임
- Cache-Control: s-maxage
> 프록시 캐시에만 적용되는 max-age
- Age: 60(HTTP 헤더)
> 오리진 서버에서 응답 후 프록시 캐시 내에 머문 시간(초)

## 6. 캐시 무효화
### Cache-Control - 확실한 캐시 무효화 응답
- `Cache-Control: no-cache, no-store, must-revalidate`
- `Pragma:no-cache`
### Cache-Control - 캐시 지시어(directives) - 확실한 캐시 무효화
- Cache-Control: no-cache
> 데이터는 캐시해도 되지만, 항상 원 서버에 검증하고 사용
- Cache-Control: no-store
> 민감정보 포함 저장 X
- Cache-Control: must-revalidate
> 캐시 만료후 최초 조회시 원 서버에 검증해야함  
원 서버 접근 실패시 반드시 오류가 발생해야함 - 504(Gateway Timeout)
- Pragma: no-cache
### no-cache vs. must-revalidate
- no-cache: 캐시 서버에 설정에 따라서 프록시 캐시 데이터를 응답하여도됨
- must-revalidate: 원 서버 접근 실패시 504