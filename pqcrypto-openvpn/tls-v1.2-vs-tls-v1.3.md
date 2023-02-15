---
description: '- key에 차이가 있다?'
---

# TLS v1.2 vs  TLS v1.3

* [https://www.a10networks.com/glossary/key-differences-between-tls-1-2-and-tls-1-3/](https://www.a10networks.com/glossary/key-differences-between-tls-1-2-and-tls-1-3/)

#### TLS(전송계층보안)

* 온라인 개인 정보 보호를 위한 기본 기술&#x20;
  * 암호화 프로토콜인 TLS(Transport Layer Security)
  * HTTPS&#x20;
    * HTTP를 통해 인터넷을 통해 데이터를 이동할 때 데이터를 암호화하고 연결을 인증함&#x20;
* 사용자가 웹사이트를 방문하면 브라우저가 사이트에서 TLS 인증서를 확인
  * 존재하는 경우 브라우저는 TLS Handshake를 수행하여 유효성을 확인하고 서버를 인증함&#x20;
* 두 서버 간에 링크가 설정되면 TLS 암호화 및 SSL 암호 해독을 통해 안전한 데이터 전송이 가능&#x20;

#### TLSv1.2 vs TLSv1.3

* 이전 버전에 비해 몇 가지 향상된 기능을 제공
  * 특히 더 빠른 TLS Handshake와 더 간단하고 안전한 암호화 제품군을 제공&#x20;
  * 0-RTT 키 교환은 TLS Handshake를 더욱 간소화함&#x20;

#### 더 빠른 TLS Handshake&#x20;

* TLS 암호화 및 SSL 암호 해독에는 CPU 시간이 필요하고 네트워크 통신에 대기 시간이 추가되어 성능이 다소 저하됨&#x20;
* TLSv1.2에서 초기 handshake가 일반 텍스트로 수행되었으므로 암호화 및 암호 해독이 필요했음&#x20;
* 일반적인 Handshake에 클라이언트와 서버 간에 교환되는 5\~7개의 패킷이 포함된다는 점을 감안&#x20;
  * 연결에 상당한 오버헤드를 추가했음&#x20;
* v1.3에서는 기본적으로 서버 인증서 암호화가 채택되어 TLS Handshake가 0\~3개의 패킷으로 수행될 수 있어&#x20;
  * 이 오버헤드를 줄이거나 제거하고 더 빠르고 응답성이 높은 연결을 허용함&#x20;





