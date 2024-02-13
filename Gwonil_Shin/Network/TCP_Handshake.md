# TCP Handshake

[노션](https://night-gecko-97e.notion.site/TCP-Handshake-ded270efe30c41fa96cd395255cabd4a)

연결 지향 방식인 TCP 프로토콜에서 두 호스트간의 연결을 보장하는 과정에서 진행되는 연결과 해제 과정

## TCP 3-Way Handshake

---

두 호스트간의 통신을 시작하기 위해 서로의 연결을 형성하는 과정

### 진행과정

![Untitled](https://night-gecko-97e.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F8c7a5dcd-cd0c-4d92-a660-8919576b5006%2Fd7e59a59-bb4f-4a59-aa45-f967756acc69%2FUntitled.png?table=block&id=7a46ce73-0752-409b-b021-14194766924a&spaceId=8c7a5dcd-cd0c-4d92-a660-8919576b5006&width=1100&userId=&cache=v2)

1.  클라이언트 → 서버 (SYN)
    송신자가 TCP Header의 Sequence Number(x)를 넣고 SYN flag를 1로 설정하고 보냄
2. 서버 → 클라이언트 (SYN + ACK)

   서버가 SYN을 받아 클라이언트에게 받았다는 신호로 SYN + ACK 패킷을 보냄

   ACK Number 필드를 x+1로 지정하고 Sequence Number는 임의로 지정한다.(y) SYN flag와 ACK flag를 1로 표시한다.

3. 클라이언트 → 서버 (ACK)

   클라이언트는 서버의 응답을 받아 ACK을 서버로 보낸다.

   ACK Number 필드를 y+1로 설정.

- 초기 Sequence Number의 ISN을 0이 아닌 난수로 생성하고 설정하는 이유
    1. 보안 강화

       ISN을 파악하기 어렵게 만들어서 공격자가 이를 악용하지 않도록 함

    2. 이전 통신과의 구별

       통신마다 포트는 특정 범위 내에서 사용하고 재사용이 된다.

       따라서 이전 통신에서 사용한 포트 쌍이 다시 사용될 가능성이 있다.

       만약 ISN이 순차적인 숫자로 적용된다면 이전 Connection에서 온 패킷으로 인식될수 있기 때문.


## TCP 4-Way Handshake

---

통신을 완료한 두 호스트간의 연결을 끊는 과정

### 진행과정

![Untitled](https://night-gecko-97e.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F8c7a5dcd-cd0c-4d92-a660-8919576b5006%2F2fdbaf96-dd30-48f6-915d-96ee2d4484fe%2FUntitled.png?table=block&id=5ba5c913-4991-441b-92a2-8ae4d93353d3&spaceId=8c7a5dcd-cd0c-4d92-a660-8919576b5006&width=1430&userId=&cache=v2)

1. 클라이언트 → 서버 (FIN)
   연결을 종료하고자 하는 클라이언트는 서버에서 FIN flag를 세팅후 전송함. 이후 클라이언트는 FIN_WAIT_1 상태로 바뀜.
2. 서버 → 클라이언트 (ACK)

   FIN을 받은 서버는 CLOSE_WAIT 상태로 변경이되고 받은 FIN에 대한 응답인 ACK을 보내줌.

   ACK을 받은 클라이언트는 FIN_WAIT_2 상태로 바뀌며 SERVER가 보내는 FIN을 기다림

    - 서버가 클라이언트에게 FIN과 같이 ACK을 안보내는 이유

      서버가 클라이언트에게 보낼 데이터가 아직 남았을수도 있기에 이를 다보내기 위해 ACK을 같이 안보냄.

3. 서버 → 클라이언트 (FIN)

   서버가 연결을 종료하기 위해 FIN 패킷을 보냄. 그리고 서버는 LAST_ACK상태로 변경됨.

4. 클라이언트 → 서버 (ACK)

   FIN을 받은 클라이언트는 TIME_WAIT 상태로 바뀌며 받은 FIN 패킷에 대응하는 ACK을 서버로 보냄.

   이후 서버는 이를 받고 CLOSED상태로 변경됨.

   일정 시간이 지난후 클라이언트도 CLOSED 상태로 변경됨.

   이를 마지막으로 두 호스트간의 연결이 끊어짐.

    - 클라이언트가 TIME_WAIT로 바뀌는 이유

      서버가 FIN이전에 전송한 패킷이 FIN보다 늦게 와서 발생할수 있는 에러를 방지하기 위함이다.

      이때 TIME_WAIT 시간은 2MSL로 알려져 있다.

      > MSL(Mean Segment Lifetime)은 TCP 세그먼트가 네트워크에서 유지될수 있는 평균시간을 의미한다. 일반적으로 1분에서 2분정도이다.
