# HTTP

[노션](https://night-gecko-97e.notion.site/HTTP-b114415e2f4b40bbae1d77c05cb752eb)

## HTTP

---

클라이언트와 서버 간의 통신을 위한 통신규칙 세트,프로토콜

OSI 7계층에서 7계층에 해당하는 Application Layer에 대응하는 프로토콜이다.

### 특징

1. 암호화 되지 않은 데이터를 전송한다.
2. TCP/IP 방식 위에서 동작한다.
    1. HTTP/3 부터는 UDP 기반의 QUIC이라는 프로토콜을 사용한다.
3. 기본적으로 80번 포트를 사용한다.

## 버전별 HTTP

---

### HTTP/0.9

최초 버전,이후 버전과의 구별을 위해 네이밍됨.

- 요청

  단일라인(한줄)로 구성된다.

  지원되는 메서드가 GET만 있기에 별도의 메서드 표기를 하지 않는다.

  전체 URL을 포함하지 않는다.

  →Why?) 당시의 웹서비스는 매우 단순한 형태였기에 한번 서버에 연결되면 이후에는 URL을 포함하지 않아도 원하는 경로로 찾아갈수 있었다.

- 응답

  파일 내용 자체로 구성.

  응답헤더가 없음.

  전송할수 있는 파일 형식이 HTTP 한종류 밖에 없다.

  상태코드가 없음.

  → 문제 발생 시 별도의 HTML 파일을 생성하고 해당 파일 내용으로 문제에 대한 설명을 넣어 놓는다.

    ```html
    <HTML>
       <HEAD><TITLE>Error</TITLE></HEAD>
       <BODY>
          <H1>Request Failed</H1>
          <P>The requested resource is not available or an error occurred.</P>
       </BODY>
    </HTML>
    ```


### HTTP/1.0 - 확장성

- 요청과 응답에 헤더가 도입된다.

  메타데이터의 전송을 통해 확장성이 늘어나게 되었다.

- 요청

  ![출처: https://developer.mozilla.org/ko/docs/Web/HTTP/Overview](https://night-gecko-97e.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F8c7a5dcd-cd0c-4d92-a660-8919576b5006%2F095cc5f4-e5cf-4c68-81d2-eb76c392dd52%2FUntitled.png?table=block&id=b1d9292c-a036-4b0d-86e0-8709ae712e8e&spaceId=8c7a5dcd-cd0c-4d92-a660-8919576b5006&width=1390&userId=&cache=v2)

  출처: https://developer.mozilla.org/ko/docs/Web/HTTP/Overview

    1. 요청 헤더에 버전 정보가 추가됨.

- 응답

  ![출처https://developer.mozilla.org/ko/docs/Web/HTTP/Overview](https://night-gecko-97e.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F8c7a5dcd-cd0c-4d92-a660-8919576b5006%2Fdebd7761-da0f-45c7-8a75-da42d74708ed%2FUntitled.png?table=block&id=178efdab-2fe1-4ded-a022-74d2b10b1d3c&spaceId=8c7a5dcd-cd0c-4d92-a660-8919576b5006&width=1520&userId=&cache=v2)

  출처: https://developer.mozilla.org/ko/docs/Web/HTTP/Overview

    1. 상태 코드가 추가됨

       1.1버전에서 표준화시킴.

    2. Content-Type field 추가

       HTML 이외의 다른 파일들도 전송할수 있게됨.



### HTTP/1.1 - 표준화

- 연결의 재사용

  이전 버전에서와 다르게 요청마다 연결을 형성하는것이 아닌 이전 연결을 재사용할수 있게됨.

  각 헤더의 Connection field를 통해 해당 연결의 유지 여부를 상대에게 알려줌.

    ```html
    GET /index.html HTTP/1.1
    Host: example.com
    Connection: keep-alive
    ```

    - 파이프라이닝을 통한 다중 전송

      동일한 TCP 연결에서 여러 HTTP 요청을 순차적으로 보내고 서버도 이를 순차적으로 응답해주는 매커니즘.

      이를 통해 연결 설정 및 해제에 대한 Cost가 줄어서 네트워크 성능이 향상됨.


- 청크된 응답

  응답을 쪼개서 보낼수 있게 되었다.

    - 쪼개서 보내면 좋은점?

      준비되는 만큼 데이터를 전송하여 수신자는 해당 내용들 만으로 처리할수 있는 일을 수행하여 작업 속도를 향상 시킬수 있다.


- 캐시 제어 매커니즘의 표준화

  이전 버전에서 일관성이 부족한 캐시 제어 매커니즘을 표준화시킴.

    - Cache-Control

      해당 필드를 통해 캐싱 동작을 명확하게 제어할수 있다.

      (캐시의 유효시간,캐시의 적용여부, 검증과정 유무)

    - ETag와 If-None-Match

      서버에서 부여한 리소스의 고유 식별자를 나타내는 ETag(Entity Tag)를 사용해 실제 데이터 자체를 가져오는 것이 아닌 ETag 값만을 비교해 서버의 데이터와 캐시 데이터의 동일성을 체크할수 있다.

      이를 통해 전송되는 데이터의 양을 줄여서 성능을 향상시켜준다.

    - Vary

      특정 헤더의 값에 따라 서로 다른 버전의 캐시를 제공하기 위해 사용된다. 이를 통해 리소스의 다양성을 제공해준다.


- `컨텐츠 협상` 도입

  `컨텐츠 협상`이란 클라이언트와 서버간의 어떠한 표현을 사용할지 서로 협상하여 결정하는 매커니즘으로 주도성에 따라 `서버 주도` 와 `클라아언트 주도` 로 나뉜다.

  > 서버 주도
  >
  >
  > 클라이언트의 요청에 따라 적합한 표현을 서버가 결정하는 방식.
  >

  > 클라이언트 주도
  >
  >
  > 클라이언트가 원하는 표현을 요청하면 서버가 이에 맞게 제공해주는 방식.


- Host 헤더의 도입

  Host field에 적힌 주소를 통해서 동일한 IP에서 동작하는 다른 서버에 접근이 가능해졌다.


### HTTP/2 - 성능 향상

- 평문에서 이진 프로토콜로의 변화

  요청/응답을 Frame이라는 단위로 쪼개서 인코딩하고 이를 전송한다.

- 다중화 프로토콜

  한 TCP 연결에서 여러개의 요청과 응답을 처리할수 있게됨

    - 1.1의 파이프라이닝과의 차이점은?
        - 파이프라이닝

          순차 요청에 대한 응답이 순차적이 아닐수 있다.

          이로 인해 요청이 밀리는 `Head-of-Line Blocking`이 발생할수 있다.

          커넥션을 여러개 만드는 방식

        - 다중화 프로토콜

          커넥션 하나에 여러개의 스트림이 동시에 요청/응답 하는 방식

          병렬적인 자원 처리

- 헤더의 압축

  요청/응답 헤더는 중복된 정보가 많은 경우가 있다.

  (예를 들면, 같은 도메인으로 여러 요청을 보낼때의 도메인 필드)

  이때 이를 인식하고 인코딩 테이블을 통해 중복을 제거하여 압축시킨다.(HPACK 알고리즘을 사용함.)

- 서버 푸시 매커니즘의 도입

  서버가 클라이언트에게 앞으로 필요할것으로 보이는 리소스들을 미리 보내놓는 매커니즘.

  이로 인해 HTML 파싱을 통해 필요 리소스를 재요청하게 되어 발생하는 트래픽을 줄여준다.


### HTTP/3 - QUIC의 도입

기존의 TCP 프로토콜 사용에서 UDP 기반의 QUIC 프로토콜 사용으로 전환하였다.

[QUIC에 대하여](QUIC.md)

## HTTP의 메서드

---

### GET

리소스의 조회를 의미하는 메서드

- 특징
    1. Body를 사용하지 않고 쿼리 스트링을 통해 데이터를 전달함

       → Body를 사용할수는 있지만 서버가 따로 구성해줘야함.

    2. GET은 캐싱이 가능해서 조회시에 POST보다 유리하다.

### HEAD

GET과 동일하나 응답에 header만 존재함

- 어떤 목적으로 사용하나?

  데이터가 아닌 헤더 정보만을 필요로 할때 사용되며

  크게 리소스의 변경여부를 확인하거나, 캐시의 유효성을 확인할때 사용된다.


### POST

전달한 데이터의 처리/생성을 요청하는 메서드

- 특징
    1. Body를 사용하여 데이터를 전달함.
    2. 동일 요청이라도 다른 결과가 나올수 있다. (멱등이 아니다.)
    3. POST에 의해 서버의 상태가 변경될수 있다.

### PUT

리소스의 대체,없다면 생성을 요청하는 메서드

- POST와의 차이점
    1. 요청 주소의 차이

       PUT은 정확한 Entity를 지정해줘야 한다.

    2. 멱등성

       PUT은 동일한 요청에 대해 항상 같은 결과를 반환한다.


### DELETE

삭제를 요청하는 메서드

### CONNECT

서버의 터널 설정을 위한 메서드

클라이언트와 서버의 연결을 형성한다.

### OPTIONS

통신 가능 옵션 설명 메서드

서버에서 지원 가능한 HTTP 메서드가 응답으로 온다.

`CORS 정책`을 검사하기 위해 사용되는 요청이다.

### TRACE

리소스 경로에 따라 메시지 루프백 테스트 수행하는 메서드.

중간마다 방문하는 proxy,gateway에 들릴때마다 헤더가 변조가 된다.

서버는 이렇게 변조된 요청 헤더를 응답의 body에 담아서 보내준다.

이를 통해 클라이언트는 서버의 구조를 확인할수 있다.

```html
HTTP/1.1 200 OK
Date: [날짜]
Content-Type: message/http

TRACE /path/to/resource HTTP/1.1
Host: example.com
[other headers]
```

### PATCH

리소스의 일부분을 변경하는 메서드