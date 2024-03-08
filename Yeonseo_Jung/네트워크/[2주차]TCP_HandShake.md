# [2주차] TCP HandShake

---
## 플래그 정보
TCP Header에는 CONTROL BIT(플래그 비트, 6bit)가 존재하며, 각각의 bit는 "URG-ACK-PSH-RST-SYN-FIN"의 의미를 가진다.
즉, 해당 위치의 bit가 1이면 해당 패킷이 어떠한 내용을 담고 있는 패킷인지를 나타낸다.

### SYN(Synchronize Sequence Number) / 000010
연결 설정. Sequence Number를 랜덤으로 설정하여 세션을 연결하는 데 사용하며, 초기에 Sequence Number를 전송한다.
### ACK(Acknowledgement) / 010000
응답 확인. Client 혹은 Server로부터 받은 SYN에 1을 더한 값으로 SYN을 잘 받았다는 것을 의미한다.
Acknowledgement Number 필드가 유효한지를 나타낸다.
양단 프로세스가 쉬지 않고 데이터를 전송한다고 가정하면 최초 연결 설정 과정에서 전송되는 첫 번째 세그먼트를 제외한 모든 세그먼트의 ACK 비트는 1로 지정된다고 생각할 수 있다.
### FIN(Finish) / 000001
연결 해제. 세션 연결을 종료시킬 때 사용되며, 더 이상 전송할 데이터가 없음을 의미한다.


## TCP의 연결과정 3-way handshake
TCP 통신을 이용해 데이터를 전송하기 위해 네트워크 연결을 설정하는 과정으로, **데이터 전송 전에 먼저 정확한 전송을 보장하기 위해서 상대방 컴퓨터와 사전에 세션을 수립하는 과정**을 의미한다.

TCP 통신은 PAR(Positive Acknowledgement wit Re-transmission, 수신자가 수신 확인 응답 메시지를 근원지에 돌려보낸다)을 통해 신뢰적인 통신을 제공한다.  
PAR에서는 수신자가 데이터 유닛이 손상된 것을 확인(체크섬)하면 해당 세그먼트를 없애고, 발신자는 positive ack가 오지 않은 유닛을 다시 보내야 한다.


TCP의 연결은 다음의 과정으로 이루어진다.
![image](https://github.com/yeondori/SSAFY_CS_Study/assets/93027942/5fa7c379-551c-40d8-9b0c-7b5f7b0e0091)

#### STEP 1. SYN
클라이언트에서 서버로 SYN(접속 요청) 전송 (seq : x)  

Sequence Number는 임의의 랜덤 숫자로 지정하고, SYN 플래그 비트는 1로 설정한 세그먼트를 전송한다

클라이언트는 `CLOSED` -> `SYN_SENT` 상태  
서버는 `LISTEN` 상태

#### STEP 2. SYN + ACK
서버는 SYN(x)을 받고 클라이언트로 SYN + ACK(요청 수락) 전송 (seq: y, ACK: x+1)  
접속 요청을 받은 서버가 요청을 수락했으며, 접속 요청 프로세스인 클라이언트도 포트를 열어달라는 메시지 전송(SYN-ACK signal bits set)

   ACK Number필드는 Sequence Number + 1로 지정하고 SYN과 ACK 플래그 비트를 1로 설정한 새그먼트를 전송

서버는 `SYN_RECEIVED` 상태  

#### STEP 3. ACK
클라이언트는 SYN(y), ACK(x+1)를 받고 서버로 ACK(y+1) 전송  
클라이언트가 서버로 수락을 확인하는 ACK를 보내면 연결을 맺음. (ACK) 이때 데이터 전송이 가능하다

클라이언트는 `ESTABLISED` 상태  
서버는 `SYN_RCV` -> ACK -> `ESTABLISHED` 상태  

### full-duplex
1,2 단계에서는 클라이언트 -> 서버 방향에 대한 시퀀스 번호를 설정하고 이를 승인하며, 2,3 단계에서는 서버 -> 클라이언트 방향에 대한 시퀀스 번호를 설정하고 승인하면서 전이중 통신이 구축된다.  
송수신이 동시에 가능

---

## TCP의 연결 해제 과정 4-way handshake
4-Way Handshake은 연결을 해제 (Connecntion Termination)하는 과정이며, FIN 플래그를 이용한다.
TCP는 두 가지 연결 해제 방식이 있다.

1. **Abrupt connection release(갑작스런 연결 해제)**   
   한 TCP 엔티티가 연결을 강제로 닫는 경우  
   한 사용자가 두 데이터 전송 방향을 모두 닫는 경우

2. **Graceful connection release (정상적인 연결 해제)**  
   서로가 모두 커넥션을 닫을 때까지 연결되어 있음 

### Abrupt connection release(갑작스런 연결 해제) 작동 방식
Abrupt는 다음의 경우로 RST(TCP reset) 세그먼트가 전송되면 갑작스러운 연결 해제가 수행된다.  

      1. 존재하지 않는 TCP 연결에 대해 비SYN 세그먼트가 수신된 경우  
      2. 열린 커넥션에서 일부 TCP 구현은 잘못된 헤더가 있는 세그먼트가 수신된 경우 RST 세그먼트를 보내 해당 커넥션을 닫아 공격 방지  
      3. 리소스 부족, 원격 호스트 연결 불가, 응답 중지 등 일부 구현에서 기존 TCP 연결을 종료해야 하는 경우

### Graceful connection release (정상적인 연결 해제) 작동 방식
TCP의 연결 해제는 다음의 과정으로 이루어진다.
![image](https://github.com/yeondori/SSAFY_CS_Study/assets/93027942/c8d71da2-0602-4162-9ffa-e95a0b825c02)  

#### STEP 1. FIN(+ACK)  (Client -> Server)
close()를 실행한 클라이언트가 FIN을 보내고 FIN-WAIT-1 상태로 대기       
이때 FIN 패킷에는 실질적으로 ACK도 포함되어 있다(Half-close 기법)


##### Half-close
연결 종료시 완전히 종료가 아닌 반만 종료 하는 기법  
종료 요청자가 처음 보내는 FIN 패킷에 승인 번호를 함께 담아서 보내는데, 이는 "승인 번호까지는 처리되었으니 더 보낼 데이터가 있으면 보내" 라는 의미  
수신자가 남은 데이터를 모두 보내면 다시 요청자에게 FIN 을 보냄으로써 모든 데이터가 처리되었다는 신호를 보내고, 요청자는 나머지 반을 닫으며 안전하게 연결을 종료할 수 있게 된다.

즉, Client 가 데이터 전송을 마쳤다고 하더라도 Server는 보낼 데이터가 남아있을 수 있기 때문에 일단 FIN에 대한 ACK만 보내고 데이터를 모두 전송한 후에 자신도 FIN 메시지를 보낸다.

#### STEP 2. ACK (Server -> Client)

서버는 CLOSE-WAIT으로 바꾸고 응답 ACK 전달하고 자신의 통신이 끝날 때까지 대기(`TIME-WAIT` 상태)    
이때 ACK Number 필드는 Sequence Number + 1로 지정하고 ACK 플래그 비트를 1로 설정한 세그먼트 전송    

서버는 클라이언트에 응답을 보낸 후 `CLOSE-WAIT` 상태, 남은 데이터가 있다면 마저 전송한 후 close() 호출  
ACK를 받은 클라이언트는 상태를 `FIN-WAIT-2` 로 변경, 해당 포트에 연결된 애플리케이션에 close 요청    

#### STEP 3. FIN (Server -> Client)
close 요청을 받은 서버 어플리케이션은 종료 프로세스를 진행하고 FIN을 클라이언트로 보내 `LAST_ACK` 상태로 바꿈

#### STEP 4. ACK (Client -> Server)   
FIN을 받은 클라이언트는 ACK를 서버에 다시 전송하고 `TIME-WAIT`으로 상태 변경   
`TIME-WAIT`는 의도치 않은 에러로 연결이 데드락으로 빠지는 것을 방지하며, 일정 시간이 되면 `CLOSED`   
ACK를 받은 서버도 포트를 `CLOSED` 로 닫음

   
![image](https://velog.velcdn.com/images%2Faverycode%2Fpost%2F27fe1c27-49cc-40a0-acc0-bc1eef4dd6e4%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-18%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%202.04.20.png)   
---
# Reference
 [슬기로운 개발생활:티스토리](https://dev-coco.tistory.com/144)  
 [[네트워크] TCP/UDP와 3 -Way Handshake & 4 -Way Handshake](https://velog.io/@averycode/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-TCPUDP%EC%99%80-3-Way-Handshake4-Way-Handshake)