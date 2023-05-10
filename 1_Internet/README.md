# 1. 인터넷 네트워크
- IP  
- TCP, UDP
- PORT
- DNS 
## 1. IP(Internet Protocol)
IP 주소 부여
### IP 역할
- 지정한 IP 주소에 데이터 전달
- 패킷(Packet)이라는 통신 단위로 데이터 전달
### IP 패킷정보
- 출발지 IP, 목적지 IP, 기타, 전송데이터
### IP 프로토콜의 한계
- 비연결성
- 비신뢰성
- 프로그램 구분 X

## 2. TCP(Transmission Control Protocol)/ UDP(User Datagram Protocol)
### 인터넷 프로토콜 스택의 4계층
- 애플리케이션 계층 - HTTP, FTP  
- 전송 계층 - TCP, UDP
- 인터넷 계층 - IP
- 네트워크 인터페이스 계층
### OSI 7 Layer
> Application, Pressentation, Session, Transport(TCP, UDP), Network(IP), DataLink, Physical
### TCP/IP 패킷 정보
- 출발지 IP, 목적지 IP, 기타  
출발지 PORT, 목적지 PORT, 전송 제어, 순서, 검증 정보
### TCP 특징
- 연결지향 - TCP 3 way handshake
- 데이터 전달 보증
- 순서 보장
### TCP 3 way handshake
1. syn
2. syn + ack
3. ack(데이터 전송 가능)
> 데이터 전달 보증, 순서 보장

### UDP 특징
- !연결지향, 데이터 전달 보증, 순서보장: X
- 데이터 전달 및 순서가 보장되지 않지만, 단순하고 빠름
- 정리
> IP와 거의 같다, + PORT, Checksum 정도만 추가  
애플리케이션에서 추가 작업 필요

## 3. PORT
한번에 둘이 상 연결해야 한다면?
> PORT
### TCP/IP 패킷정보
[IP Packet]  
출발지 IP, 목적지 IP, 기타  
    [TCP segment]
    전송 제어, 순서, 검증 정보  
    데이터
### PORT
- 0 ~ 65535: 할당 가능
- 0 ~ 1023: 잘 아려진 포트
> FTP: 20, TELNET: 23, HTTP: 80, HTTPS: 443

## 4. DNS
### DNS(Domain Name System)