---
description: '-'
---

# RAW

* 방화벽 RAW 테이블을 사용하면 연결을 추적하기 전에 선택적으로 패킷을 우회하거나 삭제해 CPU의 부하를 크게 줄일 수 있음&#x20;
* DsS/DDoS 공격 완화에 매우 유용&#x20;



* RAW 테이블에는 연결 추적(Connection-state, layer7 등)에 의존하는 Matcher가 없음&#x20;
* 패킷이 연결추적을 우회하도록 표시된 경우 패킷 de-fragmentation(조각모음)이 발생하지 않음&#x20;

### &#x20;Chain&#x20;

* RAW 테이블에는 미리 정의된 두 개의 체인이 있음&#x20;

#### Prerouting

* 라우터에 들어오는 모든 패킷을 처리하는 데 사용&#x20;

#### Output

* 라우터에서 시작된 패킷을 처리하고 인터페이스 중 하나를 통해 나가는 데 사용됨&#x20;
* 라우터를 통과하는 패킷은 출력 체인의 규칙에 따라 처리되지 않음&#x20;





[https://help.mikrotik.com/docs/display/ROS/RAW](https://help.mikrotik.com/docs/display/ROS/RAW)
