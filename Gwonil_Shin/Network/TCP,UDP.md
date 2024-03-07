# TCP와 UDP

[노션](https://night-gecko-97e.notion.site/TCP-UDP-818576cf5115408d839fffc1856c42d2)

## Transport Layer

---

OSI 7 Layer의 4계층으로 서로 다른 Host에서 동작하는 Application Process 간의 **논리적 통신**을 제공한다. 계층의 프로토콜로는 TCP와 UDP가 있다.

> **논리적 통신**이란 Application의 관점에서 Host들이 직접 연결되어있는 것처럼 보이지만 실제 물리적으로 연결되어 있지 않고 논리적으로 연결되어 있는 것을 의미한다.
>

## TCP(Transmission Control Protocol)

---

전송을 제어하고 인터넷 상에서 데이터를 메시지의 형태로 보내기 위해 사용되는 프로토콜로 일련의 [옥텟](#octet)을 안정적이고 순서대로 에러없이 교환하도록 해준다.

Unreliable network에서 reliable network를 보장한다.

### TCP Packet Header

![Untitled](https://night-gecko-97e.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F8c7a5dcd-cd0c-4d92-a660-8919576b5006%2F0ac58b1b-6c44-436d-afe6-2c37b2d5a089%2FUntitled.png?table=block&id=25ef4561-a2bc-4b44-93e1-0f3bb1cb9352&spaceId=8c7a5dcd-cd0c-4d92-a660-8919576b5006&width=1920&userId=&cache=v2)


### 특징

- **연결 지향 방식**이기에 호스트 간 연결을 설정한 후 데이터를 주고 받는다.

  [TCP Handshake](TCP_Handshake.md)

- 제어 기능을 통해 데이터 전송의 신뢰성과 네트워크에서의 다양한 상황에 대비할수 있도록 한다.

  [TCP의 제어기능](TCP의_제어기능.md)

- [Checksum을 통해 오류를 검출한다.](#checksum)
- 위와 같은 복잡한 과정에 의해 UDP에 의해 속도가 느리다.
- 전이중,점대점 방식이다.
    - 점대점 (Point to Point)

      두 지점간의 직접적인 통신으로 연결이 2개의 종단점을 가지고 있는것을 의미한다.

    - 전이중 (Full-Duplex)

      전송이 양방향으로 동시에 일어날 수 있음을 의미한다.

- 데이터의 순서를 보장하고,전송 데이터 크기의 제한이 없다.

## UDP(User Datagram Protocol)

---

데이터를 데이터그램 단위로 처리하는 프로토콜

### UDP Header

![untitle](https://night-gecko-97e.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F8c7a5dcd-cd0c-4d92-a660-8919576b5006%2F4e0b0369-d77d-48c8-ad21-49448696859c%2FUntitled.png?table=block&id=aeecabe8-f678-479e-8b44-7628bc040556&spaceId=8c7a5dcd-cd0c-4d92-a660-8919576b5006&width=1920&userId=&cache=v2)

### 특징

- **비 연결형 서비스**로 **데이터그램** 방식을 제공하여서 데이터의 전송순서가 바뀔수 있음
- 송신에 대한 확인 작업을 거치지 않는다.
- [Checksum을 통해 오류를 검출한다.](#checksum)
- 신뢰성이 UDP에 비해 낮다
- 전송만 하는 과정만 있기에 TCP보다 속도가 빠르다.
- N:N 통신이 가능하다.

## 표로 보는 TCP와 UDP

---

|  | TCP | UDP |
| --- | --- | --- |
| 연결 | 연결O  | 연결X |
| 수신 여부 확인 | 확인함 | 확인안함 |
| 전송 순서 | 순서 보장 | 순서 미보장 |
| 통신 방식 | 1:1 | N:N (N≥1) |
| 신뢰성 | 높음 | 낮음 |
| 속도 | 느림 | 빠름 |
| 전송 크기 | 제한 있음 | 제한 없음 |

## Checksum

---

- 전통적인 방식

  계산에 사용되는 Header 및 Data를 16bit 단위로 분할하여 비트 합을 구하고 1의 보수를 취하여 계산한다.

  비트 합을 구하는 과정에서 Carry가 발생한다면 Wrap around를 적용한다.

    - 비트 합 구하기
        1. Pseudo Header 생성

           12바이트의 길이로 일부 IP Header를 참조해서 만들어진다.

           | 필드명 | 크기(바이트) | 설명 |
                       | --- | --- | --- |
           | Source IP | 4 | 출발지 IP |
           | Destination IP | 4 | 목적지 IP |
           | Reserved | 1 | 항상 0이다. |
           | 프로토콜 | 1 | IP Header에 있는 프로토콜 필드의 값 (TCP:6 ,UDP:17) |
           | TCP/UDP 길이 | 2 | TCP/UDP Header 길이 (TCP Header의 Data Offset*4/ UDP는 8로 고정)+ Data의 길이(IP Header의 Total Length- IHL*4) |
        2. 비트 합 구하기
            1. Pseudo Header와 TCP/UDP segment를 16bit 단위로 나눈다.

               (이때 TCP/UDP segment에서 checksum 영역은 0으로 설정한다.)

            2. 이후 나눈 값들을 checksum에 더해준다.

               ([Carry는 모두 Wrap Around를 적용해준다.](#carry--wrap-around))

    - 1의 보수 적용

      구해진 비트 합에 1의 보수를 적용하여 checksum을 구해준다.

      (1과 0을 바꿔주는 연산)

- CRC(Cyclic Redundancy Checking, 순환 중복검사)

  `차후 추가 예정`


## QUIC는 뭘까?

---

범용 목적의 전송계층 프로토콜로 Google을 통해 개발이 진행된 UDP 기반의 인터넷 연결

(차후 추가 예정)

## 용어 정리

---

### Octet

컴퓨팅에서 8개의 비트가 한데 모인 것을 의미한다. 해당 문서에서는 데이터,메시지,세그먼트로 이해하면 된다.

### Carry & Wrap around

자리 올림이 된 비트를 다음 자리에서 계산하는것을 Carry라고 한다.

Carry가 맨 왼쪽 bit에서 일어난 경우 해당 Carry를 맨 오른쪽으로 가져와 계산하는 로직을 Wrap around라고 한다.

ex) 100+100⇒ 000+001⇒001