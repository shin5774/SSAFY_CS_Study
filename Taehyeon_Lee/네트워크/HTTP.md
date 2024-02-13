Notion : https://insidious-curler-f3b.notion.site/HTTP-32a76b10651f4a36b1b5e9e00efec41c?pvs=4

# HTTP

## http 정의

> HTTP(Hypertext Transfer Protocol)는 **서버와 클라이언트가 TCP/IP 통신 위에서 메시지를 교환하기 위해 사용되는 프로토콜이다.**
> 

![Untitled](../img/client_server.png)

- 애플리케이션 레벨의 프로토콜(7계층)로 **TCP/IP위에서 작동**한다.
- HTTP는 **어떤 종류의 데이터든지 전송**할 수 있도록 설계돼 있다.
- HTTP로 보낼 수 있는 데이터는 **HTML문서, 이미지, 동영상, 오디오, 텍스트 문서** 등 여러종류가 있다.
- 하이퍼텍스트 기반으로(Hypertext) 데이터를 전송하겠다(Transfer) = **링크 기반으로 데이터에 접속**하겠다는 의미이다.
    - 하이퍼텍스트 : 사용자가 한 문서에서 다른 문서로 즉시 접근할 수 있는 텍스트를 뜻합니다. 하이퍼 링크를 이용하여, 문서간의 이동이 가능하며, 문서들이 비선형적으로 존재합니다.

## http의 특징-stateless

무상태(stateless)란?

- 서버가 클라이언트의 상태를 저장하지 않는다

### 서버의 확장성이 높다(장점)

서버가 클라이언트의 상태를 저장하지 않음 → 서버 확장 용이

<Stateful server인 경우>

클라이언트가 사과를 구매한다는 요청을 서버에게 보낼때를 가정해보자

1. A 클라이언트 → 서버 : 사과 2개 구매할게요
2. 서버에 A가 “사과”를 “2개” 구매 요청한 것을 저장
3. 서버 → A 클라이언트 : 구매수단을 선택해주세요
4. A클라이언트 → 서버 : 신용카드로 결제할게요
5. 서버가 기존에 저장된 정보인“A클라이언트”가”사과”,”2개”를 이용하여 “신용카드”로 결제 프로세스를 진행한다

서버 확장을 위해서 다음 2가지 중 1가지를 선택할 수 있다

1. 모든 서버가 A클라이언트의 요청 정보에 대해 공유
2. A는 계속 같은 서버와 통신

하지만 이 2가지 모두 서버의 확장성이 용이하다고 볼 수 없다

<stateless server인 경우>

1. A 클라이언트 → 서버 : 사과 2개 구매할게요
2. 서버 → A 클라이언트 : 구매수단을 선택해주세요
3. A클라이언트 → 서버 : “사과 2개를 신용카드”로 결제할게요

클라이언트가 요청을 보낼 때, 서버에서 처리에 필요한 모든 데이터를 함께 보낸다.

서버가 클라이언트의 정보를 저장하지 않기 때문에 어떤 서버가 요청을 받더라도 같은 응답을 클라이언트에게 보낼 수 있다

### stateless의 한계점

stateless server에도 한계가 존재한다

1. 로그인한 사용자의 경우, 로그인 했다는 상태를 서버에 유지할 필요가 있다
2. 웹 브라우저의 쿠키, 서버의 세션을 사용하여 상태를 유지할 수 있다

## http의 특징-connectionless

서버는 클라이언트의 요청에 응답을 한 후 연결을 바로 종료한다

### connectionless의 한계점

1. TCP/IP 연결을 매 요청마다 새로 맺어야 한다
    1. 이는 반복적인 3-way handshake과정이 필요하여, 연속된 요청에도 매번 연결을 위한 시간과 자원이 소모된다
2. 사이트 요청시, html, css, js, image 등 많은 자원을 다운로드하며, **자원마다 연결**을 생성하여 추가적인 네트워크 리소스와 시간이 소요된다

### connectionless의 한계 극복 - **Persistent Connection**

http 통신을 이용하여, html, css, js, image등의 여러 자원을 응답받아야하는 경우, 여러번의 3-way handshake가 필요하다는 한계가 존재하였다

이를 해결하는 방법 중 하나로 **HTTP Persistent Connection**이 있다

1. HTTP1.0 기반에서 “Connection: keep-alive**”**을 지원하는 클라이언트는 서버에게 요청을 보낼 때, 요청 메시지 헤더에 “Connection: keep-alive”를 추가한다.
2. **Persistent Connection**을 지원하는 서버는 응답 메시지 헤더에 “Connection: keep-alive”을 포함하므로써, 응답 이후 연결을 끊지 않겠다는 것을 클라이언트에게 알린다

HTTP 1.1기반에서는 요청 메시지 헤더에 “Connection: keep-alive”을 명시하지 않아도 자동으로 서버에서 "**Persistent Connection”**로 연결한다

만약, “**Persistent Connection”을 원치 않는 경우에는 요청 헤더에 “Connection: close”을 포함하므로써 “Persistent Connection”을 사용하지 않을 수 있다.**

Persistent Connection을 피해야 하는 경우

- Persistent Connection이 계속 늘어나면, 서버에 연결된 클라이언트의 TCP 연결이 늘어나며, 서버 자원이 고갈되어, 추가적인 클라이언트에 접속에 대처할 수 없다
- 클라이언트의 접속이 잦은 페이지(ex. 메인 페이지)에서는 서버의 가용성을 고려하여 Persistent Connection을 사용하지 않는 것을 고려해야한다

Persistent Connection이 필요한 경우

- 서버의 단일 시간 내의 TCP연결을 줄이는 효과, 한 클라이언트의 지속적인 TCP연결 횟수를 감소시키는 효과를 얻음으로써, 서버의 CPU자원과 메모리를 절약할 수 있다
- 네트워크 혼잡 및 지연을 줄일 수 있다
- 복수 개의 HTTP 요청과 응답을 병렬적으로 동시에 처리하기 위해서 HTTP Pipelining 기술을 사용하기 위해서 필요하다
    - Pipelining 은 HTTP/1.1 이전버전 에서 3개의 요청을 보낼때(요청-응답) -> (요청-응답) -> (요청-응답) 의 순차적으로 진행되어 느린 부분을 성능적으로 개선하기 위해 등장한 방법이다.
    - 서버의 응답에 대한 처리를 미루는 것으로 구현된다

## http 동작 방식

http는 서버/클라이언트 모델을 따르며, 클라이언트에서 요청(request)하면, 서버는 요청을 처리하여(response)한다.

- 클라이언트(Client)
    - 서버에 요청하는 클라이언트 sw(Chrome, Safari..)가 설치된 컴퓨터를 의미
    - URI를 이용하여 서버에 접속 및 데이터 요청 가능
- 서버(Server)
    - 클라이언트의 요청을 받아, 요청을 해석하고, 응답하는 sw(Apache, nginx, IIS)가 설치된 컴퓨터

## Request message

클라이언트가 서버에 요청을 보낼 때 사용되는 메시지로 3가지 부분으로 나누어진다

- start line
- headers
- body

### <start line>

HTTP Request Message의 시작 라인으로 3가지로 구성된다

- HTTP Method : 요청의 의도, GET, POST, PUT, DELETE
- request target : 메시지가 전송되는 목표 주소
- http version

```java
GET /test.html HTTP/1.1
[HTTP Method] [Request target] [HTTP version]
```

### <headers>

- Request Message에 대한 추가 정보를 담고 있는 부분
- key-value 형태로 구성

```java
// http 헤더 예시
GET /home.html HTTP/1.1
Host: developer.mozilla.org
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101 Firefox/50.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/ *;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://developer.mozilla.org/testpage.html
Connection: keep-alive
Upgrade-Insecure-Requests: 1
If-Modified-Since: Mon, 18 Jul 2016 02:36:04 GMT
If-None-Match: "c561c68d0ba92bbeb8b0fff2a9199f722e3a621a"
Cache-Control: max-age=0
```

- Host : 요청하는 호스트에 대한 호스트명 및 포트번호(필수)
- User-Agent : 클라이언트 SW 명칭 및 버전정보
- From : 클라이언트 사용자 메일 주소
- Cookie : 서버에 의해 Set-Cookie로 클라이언트에게 설정된 쿠키 정보
- Referer : 바로 직전에 머물렀던 웹 링크 주소
- If-Modified-Since : 제시한 일시 이후로만 변경된 리소스 취득 요청
- Authorization :  인증토큰을 서버로 보낼때 사용하는 헤더, “토큰의 종류 + 실제 토큰 문자” 전송
- Origin : 서버로 POST 요청을 보낼 때, 요청이 어느 주소에서 시작되었는지 나타냄

### <body>

HTTP Request가 전송하는 데이터를 담고있는 부분

전송하는 데이터가 없는 경우 비어있으며, POST요청일 경우 HTML 폼 데이터가 존재

## Response Message

request와 동일하게 세 부분으로 구분된다

- status line
- headers
- body

### <status line>

HTTP response에 대한 정보를 간략하게 나타내며, 세 부분으로 구분된다.

- HTTP version
- Status Code
- Status Text

```java
HTTP/1.1 200 OK
[HTTP version] [Status Code] [Status Text]
```

### <headers>

- Server : 웹서버의 정보
- Access-Control-Allow-Origin : 요청 Host와 응답 Host가 다르면 CORS 에러가 발생 → 서버에서 응답 메시지에 “Access-Control-Allow-Origin”헤더에 프론트 주소를 적어주면 에러가 발생하지 않음
- Allow : Allow 헤더는 CORS 요청 외에도 적용됨,
    - Allow : GET → GET 요청만 받으며, POST가 요청되면 405 Method Not Allowed 에러를 발생시킨다
- Content-Disposition : 응답 본문을 브라우저가 어떻게 표시해야 할지 알려주는 헤더 / inline : 웹페이지에 화면 표시, attachment : 다운로드
- Location : 300번대 응답이나 201 created 응답일 때 어느 페이지로 이동할지 알려주는 헤더
    - ex. HTTP/1.1 302 Found
    Location: /login
    - /login 주소로 리다이렉트 됨
- Content-Security-Policy : 다른 외부파일을 불러오는 경우, 차장할 소스와 불러올 소스를 명시한다

### CORS란?

CORS (Cross-Origin Resource Sharing)는 웹 브라우저에서 실행되는 스크립트가 다른 도메인의 자원에 접근할 수 있는 권한을 부여하는 보안 기능입니다.

일반적으로 웹 브라우저에서는 동일한 도메인에서 실행되는 스크립트만 다룰 수 있다

하지만 때로는 다른 도메인의 자원에 접근해야 하는 경우가 있다

이 경우 웹 브라우저에서는 보안 상의 이유로 이를 차단하며, CORS는 이러한 보안상의 이유로 인해 다른 도메인의 자원에 접근하는 것을 허용하기 위한 규약을 제공한다

## HTTP 상태 코드

서버에서 클라이언트로 보내는 응답 메시지에는 상태코드가 포함되어 있다

상태 코드는 3자리 숫자로 구성

첫번째 자리가 4와 5인 경우는 정상적인 상황이 아니기 때문에 사이트 관리자가 즉시 알아야 하는 정보이다

코드의 분류

- **1xx(정보) :** 요청을 받았으며 프로세스를 계속 진행합니다.
- **2xx(성공) :** 요청을 성공적으로 받았으며 인식했고 수용하였습니다.
- **3xx(리다이렉션) :** 요청 완료를 위해 추가 작업 조치가 필요합니다.
- **4xx(클라이언트 오류) :** 요청의 문법이 잘못되었거나 요청을 처리할 수 없습니다.
- **5xx(서버 오류) :** 서버가 명백히 유효한 요청에 대한 충족을 실패했습니다.

### 2xx번대

**200(성공)**

- 서버가 클라이언트의 요청을 정확히 처리하고 응답
- 주로 페이지를 요청받고, 클라이언트에게 제공했다는 의미로 사용

**201(작성됨)**

- 성공적으로 요청 되었으며 서버가 새로운 리소스를 작성했다는 의미
- 위의 200번 상태코드가 단순히 페이지를 제공했다는 의미라면 201은 클라이언트의 요청에 의해 **서버에서 새로운 리소스를 작성**되었다는 의미를 내포

**202(허용됨)**

- 서버가 요청을 접수했지만 아직 처리하지 않았다
- 클라이언트가 비동기 처리를 요청했을 경우 주로 사용된다
- 비동기로 요청이 전달되었을때,
    - 서버에서 처리할 작업이 긴 시간이 소요 예상 → 서버에서는 **202라는 코드**를 먼저 응답하고 → 처리를 모두 끝냈을때 다시 다른 상태코드로 응답을 보내주겠다는 의미

**204(콘텐츠 없음)**

- 서버가 요청을 성공적으로 처리했지만 콘텐츠를 제공하지 않는다.
- 예를들어 클라이언트에 삭제요청을 하여 성공적으로 삭제한 경우, 클라이언트에게 전달할 컨텐츠가 없는 경우를 의미한다
- ResponseBody는 null이 아니라, 아예 존재하지 않는다

### 3xx번대

**300(여러 선택항목)**

서버가 요청에 따라 여러 조치를 선택할 수 있다. 서버가 사용자 에이전트에 따라 수행할 작업을 선택하거나, 요청자가 선택할 수 있는 작업 목록을 제공한다.

**말그대로 요청에 한개 이상의 응답이 가능할때** 전송되는 상태코드이다. 사용자는 반드시 제시된 방법 중 한개를 선택 하여야 하지만 표준화된 구현 방법은 존재하지 않는다.

**301(영구 이동)**

요청한 페이지를 새 위치로 영구적으로 이동했다. GET 또는 HEAD 요청에 대한 응답으로 이 응답을 표시하면 요청자가 자동으로 새 위치로 전달된다.

브라우저는 301응답을 받으면 요청받은 Redirect URL로 이동하는 동작을 수행한다.

### 4xx번대

**400(잘못된 요청)**

서버가 요청의 구문을 인식하지 못했다.

클라이언트가 잘못된 요청을 보냈을 경우 응답하는 코드이다 보통 RequestBody에 **어떠한 부분에서 잘못된 요청으로 해석했는지 이유**를 담아서 응답하는 경우가 많다.

**401(권한 없음)**

이 요청은 인증이 필요하다. 

서버는 로그인이 필요한 페이지에 대해 이 요청을 제공할 수 있다.

설명과 같이 **로그인과 같은 기능을 활용하는 경우에 자주 사용**이 되며 권한이 필요한 리소스에 접근하거나 요청을 하는 경우에 현재 인증이 되어있지 않다라고 알려주는 상태코드이다. 백엔드 설정에 따라 401을 응답할경우 로그인이 필요하다고 판단하여 자동으로 로그인페이지로 **Redirection** 해주는 경우가 많다.

**403(금지됨)**

서버가 요청을 거부하고 있다. 

사용자가 리소스에 대한 필요 권한을 갖고 있지 않다.

- **401코드는 사용자가 누구인지 인증이 안됨**
- **403코드는 사용자가 누구인지 인증 여부를 따지지 않고 무조건 해당 리소스의 접근을 금지하는 것을 의미한다**

**404(찾을 수 없음)**

서버가 요청한 페이지(Resource)를 찾을 수 없다. 예를 들어 서버에 존재하지 않는 페이지에 대한 요청이 있을 경우 서버는 이 코드를 제공한다.

### 5xx번대

**500(내부 서버 오류)**

서버에 오류가 발생하여 요청을 수행할 수 없다.

해당 요청은 서버에서 어딘가 예외가 발생하거나 더이상 작업을 실행할 수 없는 상황이 되었을때 응답하게 되는 코드이다

**503(서비스를 사용할 수 없음)**

서버가 오버로드되었거나 유지관리를 위해 다운되었기 때문에 현재 서버를 사용할 수 없다. 이는 대개 일시적인 상태이다.

## HTTP 메서드

GET : 리소스 조회

POST: 요청 데이터 처리, 주로 등록에 사용

PUT : 리소스를 대체(덮어쓰기), 해당 리소스가 없으면 생성

PATCH : 리소스 부분 변경 (PUT이 전체 변경, PATCH는 일부 변경)

DELETE : 리소스 삭제

<GET>

- 리소스 조회 메서드 (Read)만일 틀서버에 전달하고 싶은 데이터는 쿼리스트링를 통해서 전달
- 쿼리스트링 외에 메시지 바디를 사용해서 데이터를 전달할 수 있지만, 서버에서 따로 구성해야 되기 때문에 지원하지 않는 곳이 많아서 권장하지 않음
- 조회할 때 POST도 사용할 수 있지만, GET 메서드는 캐싱이 가능하기에 GET을 사용하는 것이 유리하다.

<POST>

- 전달한 데이터 처리/생성 요청 메서드 (Create)
- 메시지 바디(body)를 통해 서버로 요청 데이터 전달하면 서버는 요청 데이터를 처리하여 업데이트
- 전달된 데이터로 주로 신규 리소스 등록, 프로세스 처리에 사용
- 만일 데이터를 GET 하는데 있어, JSON으로 조회 데이터를 넘겨야 하는 애매한 경우 POST를 사용

<PUT>

- 리소스를 수정하는 메서드
- 요청 메시지에 리소스가 있으면, 덮어쓰고 없으면 새로 생성
- 클라이언트가 리소스의 구체적인 전체 경로를 서버에게 전달해야 함

<PATCH>

- 리소스 일부 부분을 변경하는 메서드
- PAHCH를 지원하지 않는 서버의 경우 POST 사용 가능

<DELETE>

- 리소스 제거하는 메서드

### 참고

[https://shlee0882.tistory.com/107](https://shlee0882.tistory.com/107)

[https://m42-orion.tistory.com/106#recentEntries](https://m42-orion.tistory.com/106#recentEntries)

[https://brunch.co.kr/@sangjinkang/4](https://brunch.co.kr/@sangjinkang/4)

[https://devroach.tistory.com/111](https://devroach.tistory.com/111)

[https://gmlwjd9405.github.io/2019/01/28/http-header-types.html](https://gmlwjd9405.github.io/2019/01/28/http-header-types.html)

[https://goddaehee.tistory.com/169](https://goddaehee.tistory.com/169)

[https://www.whatap.io/ko/blog/40/](https://www.whatap.io/ko/blog/40/)

[https://velog.io/@kjt407/주요-HTTP-상태코드-정리](https://velog.io/@kjt407/%EC%A3%BC%EC%9A%94-HTTP-%EC%83%81%ED%83%9C%EC%BD%94%EB%93%9C-%EC%A0%95%EB%A6%AC)

[https://duklook.tistory.com/209](https://duklook.tistory.com/209)

[https://inpa.tistory.com/entry/WEB-🌐-HTTP-메서드-종류-통신-과정-💯-총정리#http_method_종류](https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-HTTP-%EB%A9%94%EC%84%9C%EB%93%9C-%EC%A2%85%EB%A5%98-%ED%86%B5%EC%8B%A0-%EA%B3%BC%EC%A0%95-%F0%9F%92%AF-%EC%B4%9D%EC%A0%95%EB%A6%AC#http_method_%EC%A2%85%EB%A5%98)
