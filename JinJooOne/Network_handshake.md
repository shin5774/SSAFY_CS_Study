# TCP 3 - 4 way handshake

### TCP 연결 관리

> TCP의 가장 큰 특징으로는 **신뢰적인 연결**이며 **연결 지향형**이라는 것이다.
>
- TCP 프로토콜안에서 호스트(클라이언트)에서 운영되는 프로세스가 다른 호스트(서버)의 프로세스와 연결 설정 시 해당 과정에 집중해보자
- **TCP 세그먼트 코드 비트**
    - 연결의 제저 정보가 기록된다. 모두 0이 초깃값이며 활성 시 1로 변경된다.연결에 대한 비트는 SYN과 ACK가 사용된다.

![image](https://github.com/jinjoo-lab/SSAFY_CS_Study/assets/84346055/468328e7-49f4-4a1a-87de-35c852290a31)

## 3-way handshake

> TCP / IP 프로토콜을 이용해 통신하는 프로그램이 **데이터를 주고 받기 전에 연결(세션)을 수립하는 과정**
>
- **SYN : 연결 요청 비트**
- **ACK : 확인 응답 비트**
- 순서 번호(ISN) / 일련 번호
    - 각각의 TCP 연결에 대해 고유한 값으로 사용되며 연결을 초기화하는데 사용
    - 생성한 데이터 스트림의 첫 번째 바이트의 순서 번호 (seq)
- 확인 응답 번호
    - 수신 측이 몇 번째 데이터를 수신했는지 송신 측에 알려주는 역할

![image](https://github.com/jinjoo-lab/SSAFY_CS_Study/assets/84346055/6c8863ed-f943-45b2-876a-00eca4cc5245)

### 요약

1. Client → Server ( TCP SYN )
2. Server → Client ( TCP SYN ACK )
3. Client → Server ( TCP ACK ) ****

![image](https://github.com/jinjoo-lab/SSAFY_CS_Study/assets/84346055/ca0f3fad-f1c9-489b-aeee-d59084838389)

**1단계 : Client → Server**

- 애플리케이션 계층의 데이터를 포함하지 않는다.
- 클라이언트 측 TCP는 서버 TCP에게 **TCP 세그먼트를 전송**
    - **SYN 플래크 비트를 1로 설정**한다. ( **SYN 세그먼트** )
    - 추가적으로 최초의 순서 번호(client_isn)을 선택하고 세그먼트의 순서 번호 필드에 넣고 전송

**2단계 : Server → Client**

- 서버는 전송 받은 데이터에서 TCP SYN 세그먼트 추출
- 연결에 TCP 버퍼와 변수를 할당한다.
- 클라이언트 **TCP로 연결 승인 세그먼트를 전송** ( **SYNACK 세그먼트** )
    - SYN 비트는 1로 설정
    - TCP 세그먼트 헤더의 확인 응답 필드는 client_isn + 1로 설정
    - 서버는 자신의 최초 순서 번호(server_isn)을 선택 TCP 세그먼트 헤더에 값 추가

**3단계 : Client → Server**

- 연결 승인 세그먼트를 수신하면 클라이언트는 연결에 버퍼와 변수를 할당
- 서버에 **연결 확인 세그먼트** 전송
    - 세그먼트 헤더의 확인 응답 필드 : server_isn + 1
    - SYN 비트 0으로 설정

### 순서 번호가 필요한 이유 ?

- 데이터의 순서 번호 보장
- 중복 데이터 식별
- **Flow Control**
- **Connection Management**:
    - 순서 번호는 연결 초기화 및 해제 과정에서 사용
    - 3-way handshake에서 초기 순서 번호를 교환하고, 연결이 종료될 때는 FIN 패킷을 교환하면서 마지막 데이터의 순서 번호를 알림
- Reliability and Retransmission
    - 데이터 전송의 신뢰성 보장 + 누락된 데이터 재전송

---

## 4-way handshake

> TCP 프로토콜에서 **연결(세션)을 종료하기 위해** 수행되는 작업
>
- TCP 연결에 참여하는 두 프로세스 중 하나가 연결을 끝낼 수 있다.
    - 연결이 끝날 때 호스트의 자원(버퍼 & 변수)는 회수된다.

### 요약

1. Client → Server (FIN) : 연결 종료 요청
2. Server → Client (ACK) : 연결 종료 응답
3. Server → Client (FIN) : 연결 종료 요청
4. Client → Server (ACK) : 연결 종료 응답

![image](https://github.com/jinjoo-lab/SSAFY_CS_Study/assets/84346055/d1e37583-2c35-4506-8eb3-ddf8b04b4f79)

**1단계 : Client → Server**

- 클라이언트 TCP가 서버 프로세스에게 **TCP 세그먼트 전송 (연결 종료 요청)**
    - 세그먼트 헤더에 **FIN 플래크 비트를 1로 설정**
    - **FIN_WAIT**

**2단계 : Server → Client**

- 세그먼트를 수신 , 서버는 클라이언트에게 **확인 응답 세그먼트 전송**
    - **CLOSE_WAIT**

**3단계 : Server → Client**

- FIN 비트가 1로 설정된 **종료 세그먼트 전송**
    - **LAST_ACK**

**4단계 : Client → Server**

- 서버의 종료 세그먼트에 **확인 응답 세그먼트 전송**
    - **TIME_WAIT**
- **TIME_WAIT**

  Client는 Server로부터 FIN을 수신하더라도 일정시간(디폴트 240초) 동안 세션을 남겨놓고 잉여 패킷을 기다리는 과정을 거치게 되는데 이 과정을 **"TIME_WAIT"** 라고 합니다. 일정시간이 지나면, 세션을 만료하고 연결을 종료시키며, **"CLOSE"** 상태로 변화


### 참고

[[Network] TCP와 UDP의 구조와 특징](https://devlog-wjdrbs96.tistory.com/288)

[[Network] TCP와 UDP의 구조와 특징](https://devlog-wjdrbs96.tistory.com/288)
