---
description: '-'
---

# Mangle



* 특수 표시로 향후 처리를 위해 패킷을 표시하는 일종의 marker
* RouterOS의 다른 많은 기능들&#x20;
  * 이러한 Mark를 사용&#x20;
  * Queue tree, NAT, Routing&#x20;
* Mark를 기반으로 패킷을 식별하고, 그에 따라 처리합니다&#x20;
* Mangle Mark는 라우터 내에만 존재하며 네트워크를 통해 전송되지 않음&#x20;



* Mangle 기능 : TOS(DSCP) 및 TTL 필드와 같은 IP 헤더의 일부 필드를 수정하는 데 사용됨
* Firewall Mangle 규칙 : 삭제할 수 없는 5개의 미리 정의된 체인으로 구성됨&#x20;



#### PREROUTING 체인

* 패킷이 네트워크 인터페이스에 막 도착할 때 패킷에 적용

#### INPUT 체인&#x20;

* 패킷이 로컬 프로세스에 제공되기 직전에 패킷에 적용됨&#x20;

#### OUTPUT 체인&#x20;

* 프로세스에 의해 생성된 직후 패킷에 적용됨&#x20;

#### FORWARD 체인&#x20;

* 현재 호스트를 통해 라우팅되는 모든 패킷에 적용됨&#x20;

#### POSTROUTING 체인&#x20;

* 패킷이 네트워크 인터페이스를 떠날 때 패킷에 적용됨&#x20;







{% embed url="https://help.mikrotik.com/docs/display/ROS/Mangle" %}





