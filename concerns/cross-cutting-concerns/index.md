---
description: '-'
---

# Index

* 개발에서 cutting cross 문제는&#x20;
  * 여러 모듈에 캡슐화될 가능성 없이&#x20;
  * 여러 모듈에 영향을 미치는 프로그램의 측면&#x20;
* 설계와 구현 모두에서 시스템의 나머지 부분에서 깔끔하게 분해되지 않는 경우가 많음&#x20;
  * 분산(코드 중복), 엉킴(시스템 간 상당한 종속성) 또는 둘 다를 초래할 수 있음&#x20;
* ex) 의료 기록을 처리하기 위한 애플리케이션 작성시,&#x20;
  * 이러한 기록의 인덱싱이 핵심 관심사라면&#x20;
  * 기록 DB, 사용자 DB 또는 인증시스템에 대한 변경 내역을 기록하는 것은?
  * 프로그램에서 더 많은 부분에 있어, 상호작용하기 때문에 Cross-Cutting Concerns이 됨&#x20;

#### 배경&#x20;

* 시스템의 다른 많은 부분에 의존하거나 영향을 주어야 하는 프로그램의 일부
* aspect의 발전을 위한 기초를 형성
* Cross-Cutting Problem은 객체지향, 절차적 프로그래밍에 맞지 않음&#x20;
* 프로그램 내에서 엉킴, 시스템 상호 의존성에 직접적인 책임이 있을 수 있음&#x20;
* 절차적 및 기능적 언어 구조는 전적으로 절차 호출로 구성되기 때문&#x20;
  * 구현할 기능 및 관련 Cross-Cutting Problem을 동시에 해결할 수 있는 의미 체계가 없음&#x20;
* 결과적으로, Cross-Cutting Problem을 해결하는 코드는 다양한 관련 위치에 분산되거나 복제되어야 함&#x20;
  * 모듈성의 손실을 가져옴&#x20;



* 관점 지향 프로그래밍은 모듈성을 유지하기 위해 Cross-Cutting Problem을 Aspect로 캡슐화하는 것을 목표로 함&#x20;
  * 이를 통해서 Cross-Cutting Problem을 해결하는 코드를 완전히 분리하고 재사용할 수 있음&#x20;
  * Cross-Cutting Problem에 기반한 설계를 통해 소프트웨어 엔지니어링 이점에는 모듈성 및 단순화된 유지 관리가 포함될 수 있음&#x20;

#### 예시

* Business rules
* caching
* Code mobility
* Data validation
* Domain-specific Optimizations
* Environment variables and other global configuration settings&#x20;
* Error detection and correction
* Internationalization and localization(언어 현지화를 포함한)
* 정보 보안
* Logging
* Memory Management
* Monitoring
* Persistence
* Product features
* Real-time constraints
* Synchronization
* Transaction processing&#x20;
* Context-sensitive help
* Privacy
* Computer Security





[https://en.wikipedia.org/wiki/Cross-cutting\_concern](https://en.wikipedia.org/wiki/Cross-cutting\_concern)
