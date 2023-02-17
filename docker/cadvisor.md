---
description: '-'
---

# cAdvisor

* cAdvisor : 컨테이너 어드바이저
  * 컨테이너 사용자에게 실행 중인 컨테이너의 리소스 사용량 및 성능 특성에 대한 이해를 제공&#x20;
  * 실행 중인 컨테이너에 대한 정보를 수집, 집계, 처리 및 내보내는 실행 중인 데몬&#x20;
  * 특히 각 컨테이너에 대해 리소스 격리 매개변수, 과거 리소스 사용량, 전체 리소스 사용량 히스토그램 및 네트워크 통계를 유지&#x20;
    * 이 데이터는 컨테이너 및 머신 전체에서 내보냄&#x20;
* cAdvisor는 Docker 컨테이너를 기본적으로 지원하며, 기본적으로 다른 모든 컨테이너 유형을 지원해야 함&#x20;
  * cAdvisor 컨테이너 추상화는 Imctfy를 기반으로 하므로&#x20;
    * 컨테이너는 본질적으로 계층적으로 중첩됨&#x20;



ex) 사용되는 예상 구조&#x20;

* cAdvisor => prometheus => grafana => 매트릭을 사용한다&#x20;
  * container Memory를 얼마 쓰는지 확인&#x20;











[https://github.com/google/cadvisor](https://github.com/google/cadvisor)
