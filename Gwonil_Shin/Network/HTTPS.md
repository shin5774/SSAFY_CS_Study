# HTTPS

[노션](https://night-gecko-97e.notion.site/HTTPS-3f471adb5cc14a8f92763f4bd3a29d58)

## HTTPS(Hypertext Transfer Protocol Secure)

---

HTTP의 암호화 되지 않은 데이터를 전송한다는 보안 취약점을 보완하기 위해 나온 프로토콜.

### 특징

1. `대칭키`와 `비대칭키`를 사용해 보안을 형성한다.
2. 기본적으로 443번 포트를 사용한다.



## SSL/TLS(Secret Socket Layer/ Transport Layer Securty)

---

HTTPS의 구성 계층중 하나로 HTTP(Application Layer)의 하단 레이어에서  데이터의 암호화 및 복호화를 실시하여 보안성을 강화해주는 역할을 한다.

### TLS Handshake

서로의 Certifiactes를 이용해서 Shared Secret Key를 교환하는 과정이다.

### 절차

![출처: https://www.ssl.com/article/ssl-tls-handshake-ensuring-secure-online-interactions/](https://night-gecko-97e.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F8c7a5dcd-cd0c-4d92-a660-8919576b5006%2Ff786063a-bb80-43e3-aa28-35a68b812292%2FUntitled.png?table=block&id=8c5dab67-b239-491b-a254-4420cd066544&spaceId=8c7a5dcd-cd0c-4d92-a660-8919576b5006&width=1200&userId=&cache=v2)

출처: https://www.ssl.com/article/ssl-tls-handshake-ensuring-secure-online-interactions/

1. ClientHello

   평문으로 SSL/TLS 프로토콜의 버전정보, 사용 가능한 암호화 알고리즘을 전송한다.

2. ServerHello

   클라이언트와 서버가 사용가능한 공통된 SSL/TLS 버전중 가장 최신의 버전 및 둘 다 사용가능한 가장 강력한 암호화 알고리즘 을 반환한다.

3. ServerKeyExchange

   서버는 이와 동시에 Public Key Certificate를 클라이언트에게 보내게 된다.

4. ClientKeyExchange

   클라이언트는 Certificate를 통해 public key를 얻는다. 이 과정에서 해당 certificate가 인증되었다는 것을 보증한다.

   이후 클라이언트는 MasterSecret을 만들기 위한 난수인 `PreMasterSecret`을 만들고 이를 public key로 암호화 하여 서버에게 보낸다.

   서버는 이를 본인의 private Key로 해독해 PreMasterSecret을 획득한다.

   서버와 클라이언트는 이를 통해 대칭키를 만들고 이후에는 해당 대칭키로 데이터의 암호화 및 복호화를 진행한다.

   > MasterSecret
   >
   >
   > 대칭키와 Record Protocol에서 사용되는 MAC key를 만드는데 사용되는 값이다.


### Record Protocol

![출처 : https://www.mysoftkey.com/security/ssl-protocol-overview/](https://night-gecko-97e.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F8c7a5dcd-cd0c-4d92-a660-8919576b5006%2Ff5f82fc6-02a3-4423-9148-f0c18ac7d8bf%2FUntitled.png?table=block&id=71178c63-c581-4a5a-ae85-a2f2b5397dcd&spaceId=8c7a5dcd-cd0c-4d92-a660-8919576b5006&width=770&userId=&cache=v2)

출처 : https://www.mysoftkey.com/security/ssl-protocol-overview/

암호화된 데이터를 주고 받는 프로토콜.

전체 데이터를 쪼개서 각각을 암호화하여 Record라는 단위로 주고받는다.