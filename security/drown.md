---
description: '- Decrypting RSA with Obsolete and Weakened eNcryption'
---

# DROWN?

* 취약한 구식 암호화법을 통한 RSA 복호화에서 따온 이름&#x20;
* SSLv2 취약점을 악용한 교차 프로토콜 공격&#x20;

#### 원리

* 최신 서버와 클라이언트들은 TLS 프로토콜을 이용하여 암호화 통신을 함&#x20;
  * 하지만 많은 서버들이 여전히 SSLv2도 지원을 하며
  * Default로 SSLv2 연결을 허용하지 않는 서버들도 관리자들이 어플리케이션을 최적화하는 도중 의도치 않게 설정이 변경되는 경우도 있음&#x20;
* DROWN 공격 : SSLv2를 지원하는 것만으로도 서버와 클라이언트에 큰 위협이 될 수 있음을 보여줌&#x20;



* DROWN 공격은 공격자가 특별히 제작한 악성 패킷을 서버로 보내는 방법을 이용하여 HTTPS 연결을 복호화하거나, 다른 서버에 인증서가 공유되어 있을 경우 MITM 공격을 실행할 수 있게 함&#x20;
*



[https://blog.alyac.co.kr/554](https://blog.alyac.co.kr/554)
