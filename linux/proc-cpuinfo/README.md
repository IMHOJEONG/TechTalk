---
description: '-'
---

# /proc/cpuinfo

[https://www.thegeekdiary.com/proccpuinfo-file-explained/](https://www.thegeekdiary.com/proccpuinfo-file-explained/)



#### /proc/cpuinfo&#x20;

* 존재하는 CPU 수를 포함하여 시스템이 실행중인 프로세서 유형을 표시&#x20;
* processor - 각 프로세서에 식별 번호를 제공. 프로세서가 한 개인 경우 0으로 표시됨&#x20;
  * 프로세서가 두 개 이상인 경우 0 표기법을 사용하여 프로세서를 개별적으로 계산해 모든 프로세서 정보를 표시&#x20;
* cpu family: cpu 제품군&#x20;
  * 시스템에 있는 프로세서 유형을 정식으로 알려줌&#x20;
  * 컴퓨터가 Intel 기반 시스템인 경우 "86" 앞에 숫자를 입력하면 값이 결정됨&#x20;
  * 이전 시스템의 아키텍처 유형을 결정하는 데 도움이 되며 어떤 컴파일된 RPM 패키지가 해당 시스템에 가장 적합한지 결정하는 데 도움이 됨&#x20;
  * 어떤 컴파일된 RPM 패키지가 해당 시스템에 가장 적합한지 결정하는 데 도움이 됨&#x20;
* Model name - 프로젝트 이름을 포함해 프로세서의 일반 이름을 제공&#x20;
* cpu MHz - 프로세서의 정확한 속도를 메가 헤르츠 단위로 소수점 1000번째 자리까지 표시함&#x20;
* cache 크기 - 프로세서에서 사용할 수 있는 level 2 메모리 캐시의 양을 알려줌&#x20;
* flags - FPU(부동 소수점 장치)의 존재 및 MMX 명령어 처리 기능과 같은 다양한 프로세서 속성을 정의&#x20;
*











