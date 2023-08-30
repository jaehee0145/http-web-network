# http-web-network

## 섹션 1 인터넷 네트워크 
### 인터넷 통신
- 인터넷 망에서 컴퓨터들은 어떻게 통신할까? 
- 클라이언트와 서버 사이에 수많은 노드라고 하는 서버들을 거쳐 데이터가 전송된다.
- 이를 이해하려면 IP 프로토콜을 알아야 한다.

### IP(인터넷 프로토콜)
- 지정된 IP 주소에 패킷이라는 통신 단위로 데이터를 전달

- IP 프로토콜의 한계 
  - 비연결성 : 패킷을 받을 대상이 없거나 서비스 불능 상태인지 알 수 없다. 
  - 비신뢰성 : 패킷이 소실되거나 순서가 보장되지 않는다. 
  - 프로그램 구분 : 같은 IP를 사용하는 서버에서 통신하는 애플리케이션이 둘 이상이면?

### TCP, UDP
<img width="727" alt="Screenshot 2023-08-30 at 8 23 31 PM" src="https://github.com/jaehee0145/jaehee0145.github.io/assets/45681372/95d5ceb6-c213-40ad-a363-9c6a85691b3e">

- 예시
  1. 프로그램에서 메세지 생성
  2. socket 라이브러리를 통해서 전달
  3. OS 계층에서 TCP 정보 생성
  4. IP 패킷 생성 
  5. 랜카드를 통하면서 물리적인 정보를 포함해서 전송된다.

- TCP/IP 패킷 정보
  - 출발지 port
  - 목적지 port
  - 전송 제어
  - 순서
  - 검증 정보 등을 포함
- TCP 특징 : 전송제어 프로토콜(Transmission Control Protocol)
  - 연결지향 - 3way handshake
    1. SYN : 접속 요청 
    2. SYN + ACK
    3. ACK : 요청 수락 (최적화되어서 데이터 전송이 함께 됨)
    - 참고 : 논리적인 연결 상태 
  - 데이터 전달 보증
    - 클라이언트에서 서버로 데이터를 보내면 서버가 데이터에 대한 응답을 클라이언트에 보낸다.
  - 순서 보장
    - TCP 정보 안에 순서 정보가 있어서 순차적으로 데이터를 받지 못하면 재전송 요청을 보낸다.

- UDT (User Datagram Protocol 사용자 데이터그램 프로토콜)
  - TCP는 데이터 양도 많고 3 way handshake 때문에 전송 속도가 느림 
  - UDP
    - 아무것도 없어서 상대적으로 전송 속도가 빠름
    - port 정보를 사용

### PORT
- 하나의 클라이언트가 여러개의 서버와 통신해야 하면? 
- TCP/IP 패킷 정보에 출발지, 목적지 port가 포함되어 있다. 
- PORT : 같은 IP내에서 프로세스를 구분하는 정보
  - 0 ~ 65535 할당 가능
  - 0 ~ 1023 잘 알려진 포트라 사용하지 않는 것이 좋음
  - FTP : 20, 21
  - TELNET: 23
  - HTTP : 80
  - HTTPS : 443

### DNS
- IP는 기억하기 어렵고 변경될 수 있다. 
- DNS (Domain Name System)
  - 도메인 이름과 IP주소가 등록된 서버 
  - 클라이언트가 도메인 이름으로 요청을 보내면 IP가 변경되거나 몰라도 접속 가능 


## 섹션 2 URI와 웹 브라우저 요청 흐름
### URI
- URI : URL, URN 상위 개념 
  1. URL : Resource Locator
  2. URN : Resource Name 

- URI 뜻
  - Uniform 리소스를 식별하는 통일된 방식
  - Resource 자원, URI로 식별할 수 있는 모든 것 
  - Identifier 다른 항목과 구분하는데 필요한 정보

- URL 문법 
> scheme://[userinfo@]host[:port][/path][?query][#fragment]
> https://www.google.com:443/search?q=hello&hl=ko

- scheme : 주로 프로토콜 
  - http, https, ftp 등 
- user info : 거의 사용하지 않음
- host : 주로 도메인명, IP 주소
- port : 웹 브라우저에서는 생략 
  - http 80, https 443이 생략됨
- query : key = value 형태
- fragment : html 내부 북마크 

### 웹 브라우저 요청 흐름
<img width="714" alt="Screenshot 2023-08-30 at 9 13 15 PM" src="https://github.com/jaehee0145/jaehee0145.github.io/assets/45681372/7337423a-711f-4fbb-bb8c-85e213bbdddd">

1. 웹 브라우저에 URL 입력
2. DNS 서버에 IP를 조회하고 생략된 PORT는 scheme으로 추론
3. 웹 브라우저가 HTTP 요청 메시지 생성
4. socket 라이브러리를 통해 서버와 TCP/IP 연결 
5. HTTP 메세지 포함해 TCP/IP 패킷 생성
6. 웹 브라우저의 요청으로 만들어진 패킷이 서버에 도착
7. 서버에서 클라이언트로 HTTP 응답 메시지
8. 웹 브라우저가 응답을 HTML 렌더링해서 표시 

## 섹션 3 HTTP 기본
### 모든 것이 HTTP
- 거의 모든 형태의 데이터 전송 가능 
- HTTP 역사 
  - HTTP/1.1 1997년: 가장 많이 사용 - 가장 중요한 버전
- 기반 프로토콜 
  - TCP : HTTP/1.1, HTTP/2
  - UDP : HTTP/3

### 클라이언트 서버 구조
- 요청 - 응답 구조
  - 클라이언트는 서버에 요청을 보내고 응답을 대기
- 클라이언트와 서버 역할 분리

### Stateless 무상태 프로토콜
- 무상태 
  - 서버가 클라이언트의 상태를 보존하지 않는다.
  - 중간에 서버 장애가 발생해도 클라이언트가 필요한 정보를 보존해서 다른 서버에 요청
  - 서버 확장성이 높음
  - 단점은 클라이언트가 전송해야 하는 데이터가 많음
- 실무 한계
  - 예) 로그인
    - 일반적으로 브라우저 쿠키와 서버 세션등을 사용해서 상태 유지 
    - 상태 유지는 최소한만 사용

### Connectionless 비 연결성
- HTTP는 기본적으로 연결을 유지하지 않는 모델
- 일반적으로 초 단위 이하의 빠른 속도로 응답
- 서버 자원을 효율적으로 사용할 수 있음 
- 단점
  - TCP/IP 연결을 새로 맺어야 함 - 3way handshake 시간 추가
  - HTML 뿐 아니라 css, img 등 추가적인 자원
- 지금은 HTTP 지속 연결 Persistent Connections
  - HTTP 2, 3에서는 최적화


### HTTP 메시지
<img width="726" alt="Screenshot 2023-08-30 at 9 40 41 PM" src="https://github.com/jaehee0145/jaehee0145.github.io/assets/45681372/0c0d39de-3740-4e0c-9d7b-2d67c692db9c">

- start line
  - 요청 메시지 - request line 
    - HTTP 메서드 + 요청 대상(절대 경로) + HTTP 버전
  - 응답 메시지 - status line
    - HTTP 버전 + HTTP 상태코드 

- HTTP 헤더
  - HTTP 전송에 필요한 모든 부가 정보 
  - 메시지 바디 내용, 바디 크기, 압축, 인증, 요청, 클라이언트 정보 등

- 메시지 바디
  - 실제 전송할 데이터 
























