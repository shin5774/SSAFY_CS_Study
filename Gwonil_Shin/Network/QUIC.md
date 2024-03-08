# QUIC

[노션](https://night-gecko-97e.notion.site/QUIC-0050d19c2a824fb38e2b5aa46366f28c)

## QUIC

---

UDP 상에 구현된 다중 전송 프로토콜로 HTTP 연결에 대해 빠른 설정과 낮은 대기시간을 제공하기 위해 설계되었다. (Google에서 개발함)

> 왜 TCP가 아닌 UDP 기반으로 하였을까?
>
>
> 이는 TCP 프로토콜이 OS 커널에 들어가있기 때문이다. TCP 기반으로 만들게 되어 TCP를 수정하게 된다면 이를 사용하기 위해 모든 네트워크 장비의 OS를 업데이트 해야한다.
>
> 이는 현실적으로 무리이기에 User-Level에 존재하는 UDP를 기반으로 만들었다.
>

### 핵심 개념

`RTT`를 줄여서 속도를 확보하자

⇒ 대역폭을 늘리는 것만으로는 속도 향상의 한계가 있다. 불필요한 왕복시간을 줄여서 속도를 높여보자!

> RTT(Round Trip Time)
>
>
> 패킷이 송신지부터 목적지까지 왕복하는데 걸리는 시간.
>

### 이전까지의 통신은? (By HTTPS)

![출처 : [https://medium.com/rate-labs/quic-프로토콜-구글-또-너야-932befde91a1](https://medium.com/rate-labs/quic-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C-%EA%B5%AC%EA%B8%80-%EB%98%90-%EB%84%88%EC%95%BC-932befde91a1)](https://night-gecko-97e.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F8c7a5dcd-cd0c-4d92-a660-8919576b5006%2F12a0b5cd-8b66-4dee-a523-ea88e4647947%2FUntitled.png?table=block&id=3d6309d8-fbee-4c79-a552-b55502b2ceff&spaceId=8c7a5dcd-cd0c-4d92-a660-8919576b5006&width=1440&userId=&cache=v2)

출처 : [https://medium.com/rate-labs/quic-프로토콜-구글-또-너야-932befde91a1](https://medium.com/rate-labs/quic-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C-%EA%B5%AC%EA%B8%80-%EB%98%90-%EB%84%88%EC%95%BC-932befde91a1)

연결 형성시 TCP 3-Way Handshake 와 TLS Handshake를 연속적으로 수행한다. 이때 총 3번의 RTT가 소요되게 된다.

QUIC 알고리즘은 이 RTT를 1회 줄이는것을 목표로 한다.

### QUIC의 연결 생성 과정

![출처 : [https://medium.com/rate-labs/quic-프로토콜-구글-또-너야-932befde91a1](https://medium.com/rate-labs/quic-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C-%EA%B5%AC%EA%B8%80-%EB%98%90-%EB%84%88%EC%95%BC-932befde91a1)](https://night-gecko-97e.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F8c7a5dcd-cd0c-4d92-a660-8919576b5006%2F9f3c8ba2-30be-482a-ae3d-337f532f7d4e%2FUntitled.png?table=block&id=611182a2-a2d6-4290-95cf-4582b2eda6e8&spaceId=8c7a5dcd-cd0c-4d92-a660-8919576b5006&width=1440&userId=&cache=v2)

출처 : [https://medium.com/rate-labs/quic-프로토콜-구글-또-너야-932befde91a1](https://medium.com/rate-labs/quic-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C-%EA%B5%AC%EA%B8%80-%EB%98%90-%EB%84%88%EC%95%BC-932befde91a1)

1. 최초 연결시 클라이언트는 서버에게 CHLO(ClientHello)를 보낸다.
2. 서버는 REJECT로 응답하면서 (1) server config(공개 키를 포함), (2) 인증서 체인 정보, (3) server config를 개인키로 서명한 정보, 그리고 (4) source address token이라고 해서 서버가 바라본 클라이언트의 IP주소를 시간과 함께 전달한다
3. 클라이언트가 Complete CHLO를 전송하고 서버가 SHLO(ServerHello)를 전송하는 과정에서 키교환 알고리즘을 통해 앞으로 암호화에 이용할 forward-secure key를 생성한다.
4. 이후 재연결시  source address token이 서버가 발급한 것이 맞는지 검증한다.
    1. 클라이언트의 IP가 변경되지 않았다면 요청의 유효성을 보증함
    2. 변경되었다면 서버는 REJECT를 보내면서 2번과정부터 다시수행.