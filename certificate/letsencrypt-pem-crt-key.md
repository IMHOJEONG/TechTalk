---
description: '-'
---

# letsencrypt로 받은 pem 인증서를 crt, key 확장자로 바꾸는 방법

```bash
openssl rsa -in privkey -text > [도메인 주소].key

openssl x509 -inform PEM -in fullchain.pem -out [도메인 주소].crt

```

* 사용한 이유
  * parcel을 사용하면서 .pem 확장자를 사용할 수 없었던 문제를 해결하고자 함
