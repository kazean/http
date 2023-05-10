# 4. HTTP 메서드
- HTTP API
- HTTP Method - GET, POST
- HTTP Method - PUT, PATCH, DELETE
- HTTP 메서드의 속성

## 1. HTTP API
요구사항: 회원 정보 관리 API를 만들어라
- 회원 목록 조회, 회원 조회, 회원 등록, 회원 수정, 회원 삭제
### API URI 설계
가장 중요한 것은 `리소스 식별`
- 리소스의 의미는 무엇?
> 회원이라는 개념 자체가 바로 리소스
- 리소스는 어떻게 식별하는게 좋을까?
> 회원이라는 리소스만 식별하면 된다
### API URI 설계 - 리소스 식별, URI 계층 구조 활용
- 회원 목록 조회: /members
- 회원 조회/등록/수성/삭제: /members/{id}
> 어떻게 구분하지?
>> URI는 리소스만 식별, 리소스와 해당 리소스를 대상으로 하는 행위를 분리

## 2. HTTP Method - GET, POST
### HTTP 메서드 종류 - 주요 메서드
- GET: 조회
> 메세지 바디 사용해서 데이터 전달할 수 있지만, 권장 X
- `POST`: `요청 데이터 처리, 주로 등록에 사용`
> 메세지 바디를 통해 서버로 요청 데이터 전달
>> 리소스 URI에 POST 요청이 오면 요청 데이터를 어떻게 처리할지 리소스 마다 따로 정해야 함
1. 새 리소스 생성(등록)
2. 요청 데이터 처리
3. 다른 메서드로 처리하기 애매한 경우
- `PUT`: 리로스를 대체, 해당 리소스가 없으면 생성
- `PATCH`: 리소스 부분 변경
- `DELETE`: 리소스 삭제
### HTTP 메서드 종류 - 기타 메서드
- HEAD, OPTIONS, CONNECT, TRACE

## 3. HTTP Method - PUT, PATCH, DELETE
### PUT
- 리소스 대체, 없으면 생성
- `클라이언트가 리소스를 식별`
> 클라이언트가 리소스 위치를 알고 URI 지정  
POST와 차이점  
주의 리소스를 완전히 대체한다
### PATCH
- 리소스 부분 변경
### DELETE

## 4. HTTP 메서드의 속성
- 안전(Safe Methods)
- 멱등(Idempotent Methods)
- 캐시가능(Cacheable Methods)
```
            안전    멱등    캐시가능
GET         O       O       O
HEAD        O       O       O
POST        X       X       O
PUT         X       O       X
DELETE      X       O       X
PATCH(*)    X       X       O   
```
### 안전(Safe)
- 호출해도 리소스를 변경하지 않는다, 다른 로그 같은 부분 고려 X
### 멱등(Idempotent)
- 한 번 호출하든 두번 호출하든 100번 호출하든 결과가 똑같다.
- 멱등 메서드
> POST: 멱등이 아니다! 두 번 호출하면 같은 결제가 중복해서 발생할 수 있다.
- 활용
> 자동 복구 메커니즘, 서버 TIMEOUT시 클라이언트가 같은 요청을 다시 해도 되는 판단 근거  
멱등은 외부 요인으로 중간에 리소스가 변경되는 것 까지느 고려하지 안흔다
### 캐시가능(Cacheable)
- 응답 결과 리소스를 캐시해서 사용해도 되는가?
- GET, HEAD, [POST, PATCH 가능하지만 대게 사용 X] 캐시가능