---
description: DHCP 중계 에이전트
---

# DHCP Relay Agent

#### DHCP Relay Agent

* DHCP 서버가 없는 서브넷으로부터 다른 서브넷에 존재하는 1 이상의 DHCP 서버에게 DHCP 또는 BOOTP 요청을 중계(Relay) 해줌&#x20;

#### 동작&#x20;

* DHCP 서버가 동일 서브넷의 밖에 있을 경우에 다음과 같이 동작&#x20;
* 동일 서브넷 내부에는 반드시 DHCP Relay Agent가 있어야 함&#x20;
  * DHCP 관리자는 네트워크 각 Subnetwork에 모두 DHCP Server를 둘 필요없이 통상적인 라우터 또는 기타 호스트로 하여금 DHCP 메시지들을 전달(Relay)토록 할 수 있음&#x20;
* DHCP 클라이언트가 정보를 요청하면&#x20;
  * DHCP relay agent는 그 요청을 지정된 DHCP 서버 목록으로 전송함&#x20;
  * DHCP 서버가 이에 응답을 하면, 원래 요청을 보낸 네트워크 상으로 그 응답을 브로드캐스트하거나 유니캐스트함&#x20;
* DHCP relay agent의 동작은 DHCP 클라이언트에게는 투명하여 그 존재를 알지못함&#x20;
