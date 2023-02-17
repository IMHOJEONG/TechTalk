---
description: '-'
---

# Index

* QVPN
* pq-crystals.org
* Dilithium
* NIST 양자내성암호 표준화
* CRYSTAL KYBER
  * CRYSTAL DILITHIUM
* Microsoft/PQCrypto-VPN
* 논문 / 테스트 소스 코드 => 이거로는 약간...



* 구현체를 가지고 컴파일해서 동작하는지 확인&#x20;



1. Dilithium을 허용하는지 빠르게 확인&#x20;
   1. Microsoft/PQCrypto-VPN에서 지원&#x20;
      1. Clone
   2. libpqcrypto => openssl => openvpn
   3. confluence update도 필요할 듯



* 되면 container => QVPN, client, server까지 해서 누르면 배포되게 하자&#x20;
  * 배포해서 성능 비교하고...



* python 모듈 체크
  * Server, Client 각자 개발&#x20;
* 키 교환 알고리즘 SHA384
* ecdh-curve



* Dependency&#x20;
  * docker 이미지에 실행 순서 지정 가능&#x20;
* Dilithium&#x20;
* Server가 Client를 생성할 때 생성키를 다 가지고 있어야 함&#x20;
* ldd







* openvpn을 깔고 덮어쓰는 방식&#x20;
  * 이미지를 가져다가 배포해야 함&#x20;
*   etc/ld.so.conf.d

    * 환경변수 때문에 => ldconfig


*   python 모듈 체크&#x20;

    * Server, Client 각자 개발&#x20;


* ovpn-data/pki
* easyrsa
* tls-auth를 해제?



* openvpn은 성능 시험을 어떻게 할까
* genconfig 파일 => openvpn.conf.j2&#x20;
  * 공인 IP 주소로 치환이 되어야 함&#x20;



* 문서 정리 필요&#x20;
  * 이 알고리즘이 진짜 적용되는 건지

