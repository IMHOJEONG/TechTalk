---
description: '-'
---

# 생각해볼 부분

* 100개 연결에서 성능 시험&#x20;
  * PQC => 큰 차이는 없다?
  * 메모리는 미 적용이 적게 먹는다&#x20;
* 100%일 때 코어 1개가 작용?
  * TOP의 측정이 정확한가?
* 클라이언트의 개수가 증가할 때, idle 상태 측정&#x20;
* 계산의 의미를 두어야 함&#x20;
  * cAdvisor 2800%는 무슨 의미?
* 1000% 넘어가는 것을 어떻게 설명해야 하는가?
  * Server Temp Key&#x20;
* 보안 프로그램은 대부분 one Thread?
  * 과연 멀티쓰레드로 돌아가는가?



* PQC 적용 안 했을 때, 적용했을 때&#x20;
  * 걸리는 접속 시간, 여러 개가 동시에 붙었을 때 성능&#x20;
* 우리는 전체의 시간이 필요한 것&#x20;
  * apps/openssl을 수정해야?
  * 시작 시점, 끝 시점에 타임을 찍고 elapsed time을 측정&#x20;
  *










