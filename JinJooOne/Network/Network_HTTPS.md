# HTTPS

[노션](https://daisy-atmosphere-561.notion.site/HTTPS-fc6a686c68244700b13398f13aeb04fb?pvs=4)

### 서론

> 당당하게 말하겠다. 나는 보안을 싫어한다. 정확히는 보안 공부하는 걸 싫어한다. 보안 공부할거였으면… 대학원을 갔겠지 ㅡㅡ;;
>

### HTTPS

> HTTP + Security : HTTP의 보안 버전
>
- HTTPS는 **암호화 프로토콜 사용**하여 통신을 암호화

### Why Secure

1. 사이트에 보내는 정보를 탈취당해도 **보안성**을 챙기기 위해

![image](https://github.com/jinjoo-lab/SSAFY_CS_Study/assets/84346055/9d7a95cf-4c5a-4c7b-93b9-1663a0a56633)

- HTTP를 통해 특정 사이트의 ID와 Password를 전송한다 했을 때 중간에 해당 정보가 탈취된다면 보안에 심각한 문제가 발생한다.
- HTTPS를 사용할경우 사용자가 전송하는 정보가 암호화되기 때문에 **중간에 탈취당하더라도 보안성이 증가**한다.
1. **사이트 자체에 대한 검증**
    - 기관으로부터 검증된 사이트만 주소에 HTTPS 사용이 허가된다.
2. 검색 엔진 최적화
    - 자신이 개발한 사이트가 검색 엔진에 더 많이 노출되는 것은 경영 측면에서 굉장한 이득일 것이다.
    - 가장 대표적인 검색 엔진인 구글은 HTTPS 사이트에 대해 가산점 부여
        - 또한 구글에서 만든 AMP (가속화 모바일 페이지)는 HTTPS로만 만들 수 있다.

> HTTPS는 HTTP 프로토콜 자체를 암호화하는 것은 아니다. **HTTP의 Header와 Body를 암호화**하는 것이다.
>

### How

- HTTPS가 어떻게 사용자의 요청에 대해 보안성을 강화하는지 알아보자

### 대칭키

> **암호화**하는 키와 **복호화**하는 키가 동일한 암호 시스템
>
- 하나의 키로 암호화와 복호화를 수행

![image](https://github.com/jinjoo-lab/SSAFY_CS_Study/assets/84346055/bc0d4e32-0c1f-4257-98e8-899dd0cff4d5)

**장점**

- 암호화 방식이 빠르다. (대용량 데이터 암호화에 적합)

**단점**

- 키가 탈취당했을 경우 타격이 크다. (동일한 키로 암호화와 복호화가 수행되기 때문)
- 키에 대한 공유 방식의 고찰 필요

> HTTPS에서 보내는 메시지를 암호화시 **대칭키 방식을 사용**한다면 ?
>
- HTTP는 근본적으로 서버 - 클라이언트 구조이다. 이 구조에서 키를 사용할 경우 **서버에서 클라이언트에게 키를 전송하는 과정**이 필요하다.
    - 해당 과정에서 키가 탈취당한다면 심각한 보안 문제 발생

### 공개키

> 암호화와 복호화에 있어 서로 다른 키를 사용하는 방식 , 해당 키들을 **비밀키** 그리고 **공개키**라 명명한다.
>
- 암호화에 사용되는 공개키는 외부에 노출되어 있다.
- 복호화에 사용되는 비밀키는 공개되지 않는다.

![image](https://github.com/jinjoo-lab/SSAFY_CS_Study/assets/84346055/497e373f-f39e-4863-8012-40e6dbea45df)

**장점**

- 보안성 향상
    - 대칭키 방식과 비교하여 보안성 향상된다.
    - 키 공유 문제에 있어 사용자에게 공개키만 공유하면 되기 때문에 보안성 향상

**단점**

- 암호화와 복호화에 있어 더 복잡한 과정 수행 → 시간 + 비용 증가

> HTTPS에서 보내는 메시지 암호화시 **공개키 방식을 사용**한다면 ?
>
- 사이트에 오는 모든 요청과 응답에 대해 복잡한 암호화와 복호화 과정이 수행
- 서버에 부하가 심하다. → 많은 비용과 지연이 발생할 수 있다.

### MIX IT

> 어느 한쪽만 사용하는 것은 결국 각기 장점과 단점이 존재한다. HTTPS는 이러한 문제에 대해 공개키 방식과 대칭키 방식을 혼합해서 사용한다.
>

### SSL ( Secure Sockets Layer)

> 웹 서버와 웹 브라우저간의 보안을 위해 만든 프로토콜 : 공개키 / 개인키 방식 사용
>

### 동작 과정

> SSL은 **공개키 방식으로 대칭키 전달** + **대칭키로 암호화와 복호화하여 서버와 브라우저간 통신**
>
1. 클라이언트는 서버에 접속 요청

![image](https://github.com/jinjoo-lab/SSAFY_CS_Study/assets/84346055/378b0d64-4025-434d-9f6f-123f52018f4b)

1. 서버는 클라이언트에게 **공개키 전송**

![image](https://github.com/jinjoo-lab/SSAFY_CS_Study/assets/84346055/595ea6d3-cbd6-429a-b9ac-2bc8c85f4efa)

1. 클라이언트는 **전송받은 공개키로 자신의 대칭키 암호화** 하여 전송

![image](https://github.com/jinjoo-lab/SSAFY_CS_Study/assets/84346055/ccc95eb4-194b-4b04-8de2-1c3146d2fbaa)

1. 서버는 전송받은 **대칭키를 자신의 개인키로 복호화**

![image](https://github.com/jinjoo-lab/SSAFY_CS_Study/assets/84346055/c3573675-83f3-4c00-a64f-8bd0540f1ed9)

1. 해당 대칭키로 통신

![image](https://github.com/jinjoo-lab/SSAFY_CS_Study/assets/84346055/79c15fb3-efb0-4e52-8562-5135680d1859)

### 공개키에 대한 검증

- 이 과정에 있어 전송된 공개키가 올바른 공개키인지에 대한 확인이 필요하다.
- 해당 검증은 **CA (Certificate Authority)**라는 검증된 기관에서 수행한다.

![image](https://github.com/jinjoo-lab/SSAFY_CS_Study/assets/84346055/1b1460c4-ee01-4a21-a1c0-59852f49fa22)

### CA

1. 서버는 자신의 정보를 인증 기관에게 전달
2. CA는 서버 정보를 바탕으로 **서버 인증서** 생성
    - CA는 자신의 개인키로 서버의 정보를 암호화
3. CA는 암호화된 서버 인증서를 서버에 전송
4. 상대방 인증 시 서버는 클라이언트에게 서버 인증서 전송
    - 우리가 사용하는 브라우저에는 CA의 목록이 내장되어 있다.
    - 브라우저에는 CA의 공개키를 가지고 있다. **CA의 공개키로 서버 인증서를 복호화 !**
    - **복호화된 인증서에는 → 서버의 공개키가 포함되어 있다.**

### TLS HandShaking

> 위에서 CA를 통해 서버의 인증서에서 클라이언트가 공개키를 제공받는 방법을 기술했다. 이러한 과정은 구체적으로 SSL / TLS Handshaking 과정을 거치게 된다.
>

![image](https://github.com/jinjoo-lab/SSAFY_CS_Study/assets/84346055/c6750253-b836-4de9-b2ec-1414e2405e7a)

- **파란색으로 색칠된 과정**은 익숙한 **TCP 3-way Handshaking**이다.
- **노란색으로 색칠된 과정**이 **SSL Handshaking**이다.

**1 : Client Hello**

- 클라이언트는 서버에 **Cipher Suite 목록** , **Session ID**, **SSL Protocol Version** , **Random Byte** 전달
    - Cipher Suite의 알고리즘에 따라 데이터를 암호화한다.

![image](https://github.com/jinjoo-lab/SSAFY_CS_Study/assets/84346055/71dbd331-3b65-4938-a1b3-e1eca6d8bf5a)

**2 - 1 : Server Hello**

- 서버는 클라이언트에게 전달 받은 Cipher Suite 목록 중 한개를 선택하여 클라이언트에 전달

**2 - 2 : Certificate**

- 서버는 자신의 SSL 인증서를 Client에게 전달
- 인증서 내부에는 **서버의 공개키**가 포함
- 인증서에서 서버의 공개키를 얻는 방법은 위에서 기술한 CA 검증이다.

**3 : Client Key Exchange**

- 클라이언트가 자신의 대칭키를 서버의 공개키로 암호화후 전송

**4 : ChangeCipherSpec / Finished**

- 서버와 클라이언트 모두 대칭키를 획득하였고 통신 준비가 완료되었음을 알린다.

### TLS

> SSL의 업데이트 버전 : 현재는 TLS가 사용된다.
>

### 차이점

**HandShake**

- SSL 핸드셰이크 프로세스는 TLS 프로세스보다 단계가 더 많았다. TLS는 추가 단계를 제거하고 총 암호 그룹 수를 줄여서 프로세스 속도를 높였습니다.

**알림**

- SSL 및 TLS 프로토콜은 알림 메시지를 통해 오류와 경고를 전달.
    - SSL에는 경고와 치명적이라는 두 가지 알림 메시지 유형
        - 경고 알림은 오류가 발생했지만 연결을 계속할 수 있음
        - 치명적 알림은 연결을 즉시 종료해야 함.
    - SSL 알림 메시지는 암호화되지 않습니다.
- TLS에는 **닫기 알림이라는 추가 알림 메시지 유형
    - 닫기 알림은 세션 종료
    - TLS 알림은 추가 보안을 위해 암호화

**메시지 인증**

- SSL은 MAC을 사용
- TLS는 HMAC 사용
