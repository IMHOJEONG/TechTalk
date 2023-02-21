---
description: '-'
---

# We Now Have OpenVPN Data Channel Offload: Here's What That Means



* [https://openvpn.net/blog/openvpn-data-channel-offload/](https://openvpn.net/blog/openvpn-data-channel-offload/)



* 보안은 온라인 상태에서 고려해야 할 가장 중요한 사항 중 하나&#x20;
* 온라인 통신이 암호화로 더 많이 보호될수록 더 좋음&#x20;
  * 과거에는 데이터 암호화로 인해 컴퓨팅 속도가 느려졌으나 최신 CPU로 개선되었음&#x20;
  * 그러나 우리는 더 많은 것을 할 수 있음&#x20;
* OpenVPN은 커널 공간이 부족하여 사용자의 속도를 높이는 새로운 개발인 OpenVPN 데이터 채널 오프로드(DCO)를 도입&#x20;

#### Kernel Space?

* 커널은 컴퓨터를 켤 때 로드되는 것
  * 운영 체제에 관계 없이&#x20;
  * 다른 모든 레이어의 기본 레이어&#x20;
  * 하드웨어는 기초, 그 위에 있는 커널 공간, 사용자 공간을 구성&#x20;
  * 맨 위에는 사용하는 프로그램이 있음&#x20;
* 계층이 높을수록 하드웨어에서 멀어지고 프로그램 실행속도가 느려짐&#x20;
  * 따라서 데이터 암호화에 대해 생각할 때 어려울 수 있음&#x20;















