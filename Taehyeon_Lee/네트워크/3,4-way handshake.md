# 3,4-way handshake

> 3-Way Handshake는 TCP의 접속
4-Way Handshake는 TCP의 접속 해제 과정
> 

### **포트(PORT) 상태 정보**

- CLOSED: 포트가 닫힌 상태
- LISTEN: 포트가 열린 상태로 연결 요청 대기 중
- SYN_RCV: SYNC 요청을 받고 상대방의 응답을 기다리는 중
- ESTABLISHED: 포트 연결 상태

### **플래그 정보**

- 6bits
- TCP Header에는 CONTROL BIT(플래그 비트, 6bit)가 존재한다
- 각각의 bit는 "URG-ACK-PSH-RST-SYN-FIN"의 의미를 가진다.
    - 즉, 해당 위치의 bit가 1이면 해당 패킷이 어떠한 내용을 담고 있는 패킷인지를 나타낸다.
- SYN(Synchronize Sequence Number) / 000010
    - 연결 설정 Sequence Number를 랜덤으로 설정하여 세션을 연결하는 데 사용
    - 초기에 Sequence Number를 전송한다.
- ACK(Acknowledgement) / 010000
    - 응답 확인
    - 패킷을 받았다는 것을 의미한다
    - Acknowledgement Number 필드가 유효한지를 나타낸다.
    - 양단 프로세스가 쉬지 않고 데이터를 전송한다고 가정하면 최초 연결 설정 과정에서 전송되는 첫 번째 세그먼트를 제외한 모든 세그먼트의 ACK 비트는 1로 지정된다고 생각할 수 있다.
- FIN(Finish) / 000001
    - 연결 해제
    - 세션 연결을 종료시킬 때 사용되며, 더 이상 전송할 데이터가 없음을 의미한다.

## 3-Way Handshake

### 개념

- TCP 통신을 이용하여 데이터를 전송하기 위해, 네트워크 연결(Connection Establish)하는 과정
- 양쪽 모두 데이터 전송준비되었음을 보장
- TCP/IP 프로토콜을 이용하여, 통신을 하는 응용 프로그램이 데이터를 전송하기 전에
- 정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션을 수립하는 과정을 의미

### 기본 메커니즘

- PAR (Positive Acknowledgement with Re-transmission)을 통해 신뢰적인 통신을 제공한다

> PAR을 사용하는 기기는 ACK를 받을 때까지 데이터 유닛을 재전송한다
> 
1. 송신자가 수신자에게 데이터유닛(세그먼트)를 보냄
2. 수신자가 수신한 세그먼트가 손상된 것을 확인
3. 수신자는 해당 세그먼트를 삭제한다
4. 송신자는 ACK를 받지 못하였기에 다시한번 세그먼트를 수신자에게 보낸다
5. 1~5과정을 송신자가 ACK를 받을 때까지 반복한다
6. 송신자가 ACK를 받고 다시 수신자에게 ACK를 보내며, 연결되었음을 보장한다

## 작동 방식

- **SYN(Synchronization)**
    - 연결요청
    - 세션을 설정하는데 사용되며 초기에 시퀀스 번호를 보낸다
- **ACK(Acknowledgement)**
    - 보낸 시퀀스 번호에 TCP 계층에서의 길이 또는 양을 더한 것과 같은 값을 ACK에 포함하여 전송
    - 동기화 요청에 대한 답변 : `Client의 Sequence Number+1`을 하여 ACK로 돌려줍니다.

### **Step 1 (SYN)**

**클라이언트는 서버와 connection을 연결하기 위해 SYN을 보낸다. (seq : x)**

- 송신자가 최초로 데이터를 전송할 때,
- [Sequence Number를 임의의 랜덤 숫자로 지정하고, SYN 플래그 비트를 1로 설정한 세그먼트를 전송한다.](https://www.notion.so/TCP-c47a503dc8d74746bc126eaf8558d916?pvs=21)
- PORT 상태
    - Client : `CLOSED`→ `SYN_SENT`  변함
    - Server : `LISTEN`

### **Step 2 (SYN + ACK)**

**서버가 SYN(x)을 받고, 클라이언트로 받았다는 신호인 ACK와 SYN 패킷을 보냄 (seq : y, ACK : x + 1)**

- 접속요청을 받은 Q가 요청을 수락했으며, 접속 요청 프로세스인 P도 포트를 열어달라는 메세지를 전송 (SYN-ACK signal bits set)
- ACK Number필드를 Sequence Number + 1 로 지정하고 SYN과 ACK 플래그 비트를 1로 설정한 새그먼트 전송 (`Seq=y, Ack=x+1, SYN, ACK`)
- PORT 상태
    - Client : `CLOSED`
    - Server : `SYN_RCV`

### **Step 3 (ACK)**

**클라이언트는 서버의 응답은 ACK(x+1)와 SYN(y) 패킷을 받고, ACK(y+1)를 서버로 보냄**

- 마지막으로 접속 요청 프로세스 P가 수락 확인을 보내 연결을 맺음 (ACK)
- 이때, 전송할 데이터가 있으면 이 단계에서 데이터를 전송할 수 있다.
- PORT 상태
    - Client : `ESTABLISED`
    - Server : `SYN_RCV` ⇒ ACK ⇒ `ESTABLISED`

### full-duplex 통신의 구성

- Step 1, 2에서는 P→Q 방향에 대한 연결 파라미터(시퀀스 번호)를 설정하고 이를 승인한다.
- Step 2, 3에서는 Q→P 방향에 대한 연결 파라미터(시퀀스 번호)를 설정하고 이를 승인한다.

⇒ 이를 통해 full-duplex 통신이 구축된다.

## **4-Way Handshake**

## 개념

4-Way Handshake은 연결을 해제 (Connecntion Termination)하는 과정이다. 

- FIN 플래그 사용
- `FIN` (finish) : 세션을 종료시키는데 사용되며, 더 이상 보낸 데이터가 없음을 나타낸다.

## **Termination의 종류**

1. Graceful connection release(정상적인 연결 해제)
    
    정상적인 연결 해제에서는 양쪽이 커넥션이 서로 모두 커넥션을 닫을 때까지 연결되어 있다.
    
2. Abrupt connection release(갑작스런 연결 해제)
    1. 갑자기 한 TCP 엔티티가 연결을 강제로 닫는 경우
    2. 한 사용자가 두 데이터 전송 방향을 모두 닫는 경우

### Step1 [Client -> FIN -> Server]

- Client가 연결을 종료하겠다는 FIN플래그를 server에 전송한다
- 보낸 후에 FIN-WAIT-1 상태로 변한다.

### **Step2 [Server-> ACK -> Client]**

- FIN 플래그를 받은 Server는 확인메세지인 ACK를 Client에게 보낸다.
- 그 후 Server는 CLOSE-WAIT상태로 변한다.
- Client도 마찬가지로 Server에서 종료될 준비가 됐다는 FIN을 받기위해 FIN-WAIT-2 상태가 된다.

### **Step3 [Server -> FIN -> Client]**

- Close준비가 다 된 후 Server는 Client에게 FIN 플래그를 전송한다.

### **Step4 [Client -> ACK-> Server]**

- Client는 해지 준비가 되었다는 정상응답인 ACK를 Server에게 보내준다.
- 이 때, Client는 TIME-WAIT 상태로 변경된다.

### Client가 Time-WAIT상태로 변경되는 이유

- **TIME-WAIT 상태는 의도치않은 에러로 인해 연결이 데드락으로 빠지는 것을 방지**하기 위함
- 만약 에러로 인해 종료가 지연되다가 타임이 초과되면 CLOSED 상태로 변경된다.