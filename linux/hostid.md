---
description: '-'
---

# hostid

* `hostid` : x86기반 Linux에서 시스템의 identity로 활용

#### hostid?

* 시스템의 hostid는 호스트의 숫자 식별자
  * 호스트 간에 고유한 것으로 간주될 수 있는 정수로 구성되어야 함
* hostid : 10진수로 이루어진 `IPv4` 주소값이 `16진수(hexadecimal)` 로 `bit-twiddling` 되어 생성됨
  * 일반적인 로컬 네트워크 환경에선 동일한 IP를 사용하지 않음
  * 이렇게 생성된 hostid값은 유니크한 값을 지니게 되고 이를 통해 노드에 대한 구분이 가능
