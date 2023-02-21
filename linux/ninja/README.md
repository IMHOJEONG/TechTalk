---
description: '- C언어의 빌드 시스템'
---

# Ninja

* [https://ko.wikipedia.org/wiki/%EB%8B%8C%EC%9E%90\_(%EB%B9%8C%EB%93%9C\_%EC%8B%9C%EC%8A%A4%ED%85%9C)](https://ko.wikipedia.org/wiki/%EB%8B%8C%EC%9E%90\_\(%EB%B9%8C%EB%93%9C\_%EC%8B%9C%EC%8A%A4%ED%85%9C\))
* [https://ninja-build.org/](https://ninja-build.org/)



* Ninja는 속도에 중점을 둔 소규모 빌드 시스템
  * 두 가지 주요 측면에서 다른 빌드 시스템과 다름&#x20;
  * 더 높은 수준의 빌드 시스템에서 입력 파일을 생성하도록 설계되었음&#x20;
  * 가능한 한 빨리 빌드를 실행하도록 설계&#x20;



#### 또 다른 빌드 시스템이 필요한 이유는 무엇?

* 다른 빌드 시스템이 고급 언어인 경우 Ninja는 어셈블러가 되는 것을 목표로 함&#x20;
* Ninja 빌드 파일은 사람이 읽을 수 있지만, 손으로 쓰기에는 특히 편리하지 않음&#x20;
* 이러한 제한된 빌드 파일을 통해 Ninja는 증분 빌드를 빠르게 평가할 수 있음&#x20;



#### Ninja를 사용해야 하는가?

* Ninja의 저수준 접근 방식은 보다 기능이 풍부한 빌드 시스템에 포함하기에 완벽&#x20;
* Ninja는 Google Chrome, Android의 일부, LLVM을 빌드하는 데 사용되며 CMake의 Ninja 백엔드로 인해 다른 많은 프로젝트에서 사용할 수 있음&#x20;





