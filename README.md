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
 > 1. 프로그램에서 메세지 생성
 > 2. socket 라이브러리를 통해서 전달
 > 3. OS 계층에서 TCP 정보 생성
 > 4. IP 패킷 생성 
 > 5. 랜카드를 통하면서 물리적인 정보를 포함해서 전송된다.

- TCP/IP 패킷에 포함된 정보
  - 출발지 port
  - 목적지 port
  - 전송 제어
  - 순서
  - 검증 정보 등
  
- TCP 특징 : 전송제어 프로토콜(Transmission Control Protocol)
  - 연결 지향 - 3way handshake
    1. SYN : 접속 요청 
    2. SYN + ACK
    3. ACK : 요청 수락 (최적화되어서 데이터 전송이 함께 됨)
    - 참고 : 논리적인 연결 상태 
  - 데이터 전달 보증
    - 클라이언트에서 서버로 데이터를 보내면 서버가 데이터에 대한 응답을 클라이언트에 보낸다.
  - 순서 보장
    - TCP 정보 안에 순서 정보가 있어서 순차적으로 데이터를 받지 못하면 재전송 요청을 보낸다.

- UDP (User Datagram Protocol 사용자 데이터그램 프로토콜)
  - TCP는 데이터 양도 많고 3 way handshake 때문에 전송 속도가 느림 
  - UDP
    - 아무것도 없어서 상대적으로 전송 속도가 빠름
    - port 정보를 사용
    - 최근에는 UDP를 최적화해서 사용하는 사례 많음

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
  - 서버나 클라이언트가 명시적으로 연결을 close하거나 정책적으로 close하기 전까지 connection 유지
  - HTTP 2, 3에서는 최적화


### HTTP 메시지
<img width="726" alt="Screenshot 2023-08-30 at 9 40 41 PM" src="https://github.com/jaehee0145/jaehee0145.github.io/assets/45681372/0c0d39de-3740-4e0c-9d7b-2d67c692db9c">

- start line
  - 요청 메시지 - request line 
    - HTTP 메서드 + 요청 대상(보통 절대 경로) + HTTP 버전
  - 응답 메시지 - status line
    - HTTP 버전 + HTTP 상태코드 

- HTTP 헤더
  - HTTP 전송에 필요한 모든 부가 정보 
  - 메시지 바디 내용, 바디 크기, 압축, 인증, 요청, 클라이언트 정보 등

- 메시지 바디
  - 실제 전송할 데이터 

## 섹션4 HTTP 메서드
### HTTP API
- API URI를 설계 : 리소스와 해당 리소스를 대상으로 하는 행위를 분리해야 한다.
- 예) 
  - 리소스 : 회원
  - 행위 : 조회, 등록, 수정, 삭제

### HTTP 메서드 - GET, POST
- HTTP 메서드 종류 
  - GET : 리소스 조회
  - POST : 요청 데이터를 담아서 처리
  - PUT : 리소스를 대체, 해당 리소스가 없으면 생성 
  - PATCH : 리소스 부분 변경
  - DELETE : 리소스 삭제

- GET 
  - 리소스 조회
  - 쿼리 파라미터, 쿼리 스트링을 통해서 데이터 전달
  - 메시지 바디를 사용할 수 있지만 실무에서는 권장하지 않음
- POST
  - 메시지 바디를 통해 요청 데이터 전달
  - 신규 리소스 등록
  - 단순한 데이터 생성, 변경을 넘어 프로세스를 처리할 때 사용
  - 다른 메서드로 처리하기 애매한 경우 사용

### HTTP 메서드 - PUT, PATCH, DELETE
- PUT
  - 리소스를 완전히 대체, 없으면 생성 
  - 클라이언트가 리소스 위치를 알고 URI 지정
    - POST와 큰 차이
- PATCH
  - 리소스 부분 변경 
- DELETE
  - 리소스 제거

### HTTP 메서드의 속성 
- 안전 Safe Methods
  - 호출해도 리소스를 변경하지 않는다.
  - GET
- 멱등 Idempotent
  - 몇번 호출하든 결과가 똑같다.
  - GET, PUT, DELETE
  - POST : 멱등이 아님 - 두번 호출하면 같은 결제가 중복 발생할 수 있음
  - 자동 복구 메커니즘에 활용됨
    - 클라이언트에서 같은 요청을 반복해도 될까? 
- 캐시 가능 Casheable
  - 응답 결과 리소스를 캐시해서 사용해도 되는가?
  - 스펙상 GET, HEAD, POST, PATCH
  - 실제로는 GET, HEAD 정도만 사용
    - POST, PATCH는 바디 내용까지 고려해야해서 

## 섹션5 HTTP 메서드 활용
### 클라이언트에서 서버로 데이터 전송
- 데이터 전달 방식 
1. 쿼리 파라미터 
  - GET, 주로 정렬 필터(검색어)
2. 메시지 바디 
   - POST, PUT, PATCH
   - 회원가입, 상품 주문, 리소스 등록, 리소스 변경 등 

- 클라이언트에서 서버로 데이터 전송하는 4가지 상황
1. 정적 데이터 조회
  - 이미지 등 정적 데이터는 쿼리 파라미터 없이 단순히 리소스 경로로 조회
2. 동적 데이터 조회
   - 주로 검색, 게시판 목록 조회할 때 정렬에 사용하는 필터 
   - 조회 조건을 줄여주는 필터, 조회 결과를 정렬하는 정렬 조건에 주로 사용
   - GET 쿼리 파라미터를 사용해서 데이터 전달
3. HTML Form 데이터 전송
   - HTML Form submit 하면 POST로 전송
   - Content-Type: application/x-www-form-unlencoded 사용
     - form의 내용을 메시지 바디를 통해서 key=value 형식으로 전달
   - HTML Form은 GET 전송도 가능
   - Content-Type: multipart/form-data
     - 파일 같은 바이너리 데이터 전송시 사용
4. HTTP API 전송
  - 서버 to 서버 간 통신
  - 앱 클라이언트와 통신
  - 웹 클라이언트와 통신 : React, VueJs 등
  - Content-Type: application/json을 주로 사용(사실상 표준)

## 섹션6 HTTP 상태코드
### 1xx(Informational) - 요청이 수신되어 처리중
  - 거의 사용하지 않음 

### 2xx(Successful) - 성공
- 200 OK
- 201 Created
- 202 Accepted
- 204 No Content
  - 요청이 성공했지만 응답 페이로드 본문에 보낼 데이터가 없음

### 3xx(Redirection) - 리다이렉션
- 요청을 완료하기 위해 유저 에이전트의 추가 조치 필요
- 302 메서드가 GET으로 변할 수 있음
- 303 메서드가 GET으로 변경됨
- 307 메서드가 변하면 안됨
- 303, 307을 권장하지만 이미 많은 애플리케이션에서 302를 사용

### 4xx(Client Error) - 클라이언트 오류
### 5xx(Server Error) - 서버 오류

## 섹션7 HTTP 헤더1 - 일반 헤더
## 섹션8 HTTP 헤더2 - 캐시와 조건부 요청





















