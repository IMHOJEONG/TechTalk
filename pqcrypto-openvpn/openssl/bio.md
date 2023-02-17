---
description: https://wiki.openssl.org/index.php/BIO
---

# BIO

* I/O stream abstraction&#x20;
* 본질적으로 C 라이브러리에 대한 OpenSSL의 답변&#x20;
* OpenSSL은 미리 정의된 여러 가지 유용한 BIO 유형과 함께 제공되거나 직접 만들 수 있음



* BIO는 source/sink 또는 filter의 두 가지 유형으로 제공됨&#x20;
  * BIO는 서로 연결될 수 있음&#x20;
  * 각 체인에는 항상 정확히 하나의 source/sink가 있지만 원하는 수(0 이상)의 필터를 가질 수 있음&#x20;



* BIO에서 읽기는 BIO\_read 및 BIO\_gets를 사용하여 수행할 수 있음&#x20;
* BIO에 쓰기는 BIO\_write, BIO\_puts, BIO\_printf, BIO\_vprintf로 수행할 수 있음&#x20;

