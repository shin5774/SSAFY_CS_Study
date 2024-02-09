# Network

## Network

> 주소창에 www.google.com을 입력했을 때 일어나는 과정
>

### 전체 구상도

![image](https://github.com/jinjoo-lab/SSAFY_CS_Study/assets/84346055/42f35d1b-3310-4327-8624-bc7af7b3ad83)


1. 입력한 URL 주소 중 도메인 이름에 해당하는 google.com을 **DNS 서버**에서 검색
    - 이 과정에서 브라우저는 캐싱된 DNS 기록을 먼저 확인 → 존재할 경우 따로 요청하지 않고 IP 주소를 바로 반환
2. 전달 받은 IP 주소를 이용하여 웹 브라우저는 **웹 서버에게 HTML문서 요청**
    - 해당 HTTP 요청은 TCP/IP 프로토콜 사용하여 서버로 전송
3. **WAS 서버와 데이터베이스**에서 웹페이지 작업 처리
    - WAS → 웹 서버 → 웹 브라우저에게 결과 전달

### DNS (Domain Name System)

> 사람이 읽을 수 있는 도메인 이름을 IP 주소로 바꿔주는 시스템
>
- 컴퓨터는 [www.google.com](http://www.google.com) 이라는 URL 주소를 읽을 수 없다. 오직 숫자만 읽을 수 있다.
    - 즉 중간에서 [www.google.com](http://www.google.com) → IP 로 변환해주는 곳이 필요하다.
- DNS는 계층 구조를 가지며 분산 데이터베이스 시스템이다.

### DNS 구성 요소

- 도메인 네임 스페이스
    - DNS가 저장 관리하는 계층 구조
- Name Server (DNS Server)
    - Root DNS 서버
    - TLD 서버
    - SLD 서버
- Resolver
    - 사용자 요청을 Name Server로 전달

### DNS Server

- Root DNS 서버
    - ICANN이 직접 관리 (전세계에 13개)
    - TLD DNS 서버의 IP 주소를 저장
- TLD 서버
    - 도메인 등록 기관이 관리하는 서버
- SLD 서버
    - 개인 도메인과 IP 주소의 관계가 기록되는 서버 (도메인 / 호스팅 업체 서버)

![image](https://github.com/jinjoo-lab/SSAFY_CS_Study/assets/84346055/12e616a1-daf6-42d5-af3d-7d4ef94351a8)


### Resolver

- 리졸버는 웹 브라우저와 같은 DNS 클라이언트의 요청을 네임 서버로 전달하고 네임 서버로부터 정보(도메인 이름과 IP 주소)를 받아 클라이언트에게 제공하는 기능을 수행

### DNS Query

> DNS Query(쿼리)는 사용자가 도메인 이름을 입력하고 IP 주소를 얻기 위해 DNS 서버에 보내는 요청을 말한다. 이 요청은 DNS Resolver가 사용자 컴퓨터에서 생성하고 DNS 서버에 전송
>
- DNS 쿼리는 **재귀적** or **반복적**

![image](https://github.com/jinjoo-lab/SSAFY_CS_Study/assets/84346055/b9d7eea0-5a91-42d2-89fa-5a43e1084702)


### Recursive Query

- Client와 DNS recursor 통신에 사용되는 쿼리
- 실제 IP 주소를 반환합니다.
    - Recursive Query는 요청과 응답하는 과정을 포함하는 실제 반환 결과 값입니다.
    - DNS recursor는 요청과 응답 과정을 실행하는 서버입니다.

### Iterative Query

- DNS recursor과 Name Server 통신에 사용되는 쿼리
    - 반복적으로 쿼리를 보내서 결과물(IP 주소)를 알아내서 DNS recursor에게 IP 주소를 보냅니다.
    - DNS recursor에 이미 IP 주소가 캐시 되어있다면 이 과정은 생략합니다.
