# Routing

## 라우팅이란?

하나 이상의 네트워크에서 경로를 선택하는 프로세스이다

“패킷에 포함된 주소 등의 상세 정보”를 이용하여 목적지까지 데이터 또는 메시지를 체계적으로 다른 네트워크에 전달하는 경로 선택, 스위칭 하는 과정의 의미한다

이러한 인터넷 라우팅 결정은 라우터라는 특수한 네트워크 하드웨어에 의해 이루어진다

## 라우터란?

라우터는 패킷을 패킷 명시된 목적지로 전달하는 것을 담당하는 네트워크 하드웨어의 일부이다

라우터는 둘 이상의 IP 네트워크 또는 서브 네트워크를 연결하고 필요에 따라 이들간의 데이터 패킷을 전달한다.

라우터는 로컬 네트워크 연결을 설정하는데 사용되며, 보다 강력한 라우터는 인터넷 전체에서 작동하며, 데이터 패킷이 목적지에 도달하도록 돕는다

## 라우팅의 작동 방식

라우터는 “내부 라우팅 테이블”을 참조하여, 네트워크 경로를 따라 패킷을 라우팅 하는 방법을 결정한다.

라우팅 테이블에는 “라우터가 담당하는 모든 대상”에 도달하기 위해 택해야하는 경로가 기록되어있다.

### 작동 순서

1. 라우터가 패킷을 수신
2. 패킷의 헤더를 읽어 가려는 목적지를 확인
3. 라우팅 테이블의 정보를 기반으로 패킷을 라우팅할 위치를 결정

라우터는 수백만개의 패킷을 대상으로 초당 수백만번의 작업을 수행한다

패킷을 목적지로 이동할 때, 각기 다른 라우터에 의해 여러번 라우팅 될 수 있다

## 라우팅 테이블이란?

네트워크 상의 특정 목적지까지의 거리와 가는 방법등을 명시하고 있는 테이블이다.

라우터가 패킷을 어디로 전송할 지 경로를 결정하는 방법은 이 라우팅 테이블을 참조하여 결정한다.

라우팅 테이블은 동적일 수도 있고, 정적일 수도 있다.

### 정적 라우팅 테이블

- 네트워크 관리자는 정적 라우팅 테이블을 수동으로 설정한다
- 관리자가 테이블을 수동으로 업데이트하지 않는한 데이터 패킷이 네트워크를 통과하며 택하는 경로는 변하지 않는다
- 소규모 네트워크에서는 주로 정적 라우팅에 의존한다(동적 라우팅은 컴퓨팅 성능이 많이 요구됨)

### 동적 라우팅 테이블

- 라우팅 테이블이 자동으로 업데이트 된다
- 다양한 라우팅 프로토콜을 사용하여 최단 경로와 가장 빠른 경로를 결정한다
- 동적 라우팅에는 더 많은 컴퓨팅 성능이 요구된다
- 중간규모, 대규모 네트워크의 경우, 동적 라우팅이 훨씬 효율적이다

## 주요 라우팅 프로토콜

프로토콜 : 연결된 모든 컴퓨터가 데이터를 이해할 수 있도록 데이터 형식을 지정하는 표준화된 방법

라우팅 프로토콜

- 네트워크 경로를 식별하거나 알리는 데 사용되는 프로토콜
- 라우터들끼리 경로 정보를 교환하는 프로토콜

### IP(Internet Protocol)

- 각 데이터 패킷의 원본과 대상을 지정한다.
- 각 패킷의 IP 헤더를 검사하여, 패킷을 보낼 위치를 식별한다

### BGP(Border Gateway Protocol) - 동적

- 경계 게이트웨이 프로토콜
- 어떤 네트워크에서 어떤 IP주소를 제어하고, 어떤 네트워크가 서로 연결되었는지 알리는데 사용된다
- BGP는 동적 라우팅 프로토콜이다

### OSPF(Open Shortest Path First) - 동적

- 최단경로 우선 프로토콜
- 네트워크 라우터에서 패킷을 목적지로 보내는데 사용할 수 있는 가장 빠르고 짧은 경로를 동적으로 식별하는데 사용된다

### RIP(Routing Information Protocol) - 동적

- 라우팅 정보 프로토콜
- “홉 수”를 사용하여 한 네트워크에서 다른 네트워크로 최단 경로를 찾는다
- 홉 수란?
    - 패킷이 도중에 통과해야하는 라우터의 수를 의미한다
    - 패킷이 한 네트워크에서 다른 네트워크로 이동한 경우를 “홉”이라고 한다