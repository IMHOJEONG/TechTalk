---
description: '-'
---

# Cpu 정보 분석





* ![](<../../.gitbook/assets/Screenshot from 2023-03-02 16-10-36.png>)

\---

* [https://ko.wikipedia.org/wiki/CPUID](https://ko.wikipedia.org/wiki/CPUID)
* x86 아키텍처를 위한 프로세서 기계 명령어&#x20;
* CPUID = CPU IDentification

#### CPUID

* 어셈블리어에서 CPUID 명령 - CPUID가 EAX 레지스터를 그대로 사용하므로 매개변수를 이용하지 않음&#x20;
* 더 자세히 알아보려면...&#x20;

#### Physical ID

* 실제 cpu의 번호. CPU의 갯수를 알 수 있음&#x20;

#### CPU Cores

* 4 : Quad를 의미하며 1개의 CPU에 논리적으로 4개의 CPU의 역할을 함&#x20;
* CPU당 물리적인 Core 수&#x20;

[https://www.baeldung.com/linux/proc-cpuinfo-flags](https://www.baeldung.com/linux/proc-cpuinfo-flags)

#### [https://unix.stackexchange.com/questions/43539/what-do-the-flags-in-proc-cpuinfo-mean](https://unix.stackexchange.com/questions/43539/what-do-the-flags-in-proc-cpuinfo-mean)

####

#### lm Flag

* Long Mode&#x20;
* CPU가 64비트 아키텍처를 지원함을 나타냄&#x20;
  * 32비트 CPU와 달리 x86-64 CPU에는 이 플래그가 있음&#x20;

#### vmx and svm

* vmx(Virtual Machine Extension) 플래그는 Intel cpu에 가상 머신에 대한 하드웨어 지원이 있음을 나타냄&#x20;
* VirtualBox와 같은 가상 시스템 소프트웨어는 이 확장을 활용하여 성능 및 기타 개선 사항을 향상시킴&#x20;
* 반면에 svm 플래그는 동일한 용도로 사용되지만 AMD CPU에서만 사용할 수 있음&#x20;

#### SMX(Safer Mode Extensions)

* 64비트 프로세서에서 사용할 수 있음&#x20;
* 보호 메커니즘을 시행하는 chipset인 Intel TXT 플랫폼에서 사용할 수 있는 프로그래밍 인터페이스&#x20;

#### hypervisor flag

* CPU에 가상 머신을 실행하기 위한 하드웨어 지원이 있음을 나타냄&#x20;
* 가상 머신은 가상 운영체제를 실행하고 관리하는 SW&#x20;
* 널리 사용되는 일부 가상 머신에는 VirtualBox 및 VMware가 있음&#x20;
* CPU에 이 플래그가 없으면 vmx 또는 svm 플래그도 없을 가능성이 높음&#x20;

#### pae

* Physical Address Extension
* x86 기반 프로세서용 메모리 관리 기능&#x20;
* 이 기능을 통해 CPU는 4GB보다 큰 물리적 메모리 크기에 액세스할 수 있음&#x20;
* 2003년 이후 제조된 대부분의 프로세서에서 사용할 수 있음&#x20;

#### pn

* Intel 프로세서에는 PSN(Processor Serial Number)의 줄임말인 pn이라는 고유한 일련 번호가 있음&#x20;
* 이 일련 번호는 모든 프로세서에 고유하며 프로그램에서 개별 프로세서를 식별하는 데 사용됨&#x20;

#### ACPI

* 고급 구성 및 전원 인터페이스를 나타냄&#x20;
* 원래 Intel, Microsoft, Toshiba에서 개발한 이 표준&#x20;
* 운영체제가 하드웨어 구성 요소를 검색하고 구성할 수 있도록 설계된 표준 사양&#x20;
* ex) 운영체제에서 컴퓨터 시스템에 연결된 주변 장치의 전원 관리를 수행하는 데 사용&#x20;
* 운영 체제는 또한 이를 사용해 plug & play 장치에 대한 자동 구성을 수행하고 온도 정보와 같은 상태 모니터링을 수행&#x20;

#### SSE(Streaming SIMD Extension)

* Intel 기반 프로세서의 확장&#x20;
* SIMD 약어는 Single Instruction Multiple Data
* SSE를 사용하면  CPU가 여러 데이터 요소를 처리할 수 있으므로 병렬 처리를 통해 성능이 향상됨&#x20;
* 주로 3D 그래픽, 컴퓨터 비전 및 디지털 신호 처리와 같은 프로세스 집약적인 응용 프로그램에 사용됨&#x20;

#### SSE2&#x20;

* SSE의 확장 버전, SSE와 달리 SSE2는 64비트 값을 처리할 수 있음&#x20;
* 144개의 추가 명령이 있고 동시에 여러 정보에 대해 하나의 작업을 수행할 수 있음&#x20;

#### SSE3

* SSE3는 SSE의 세 번째 버전&#x20;
* PNI(Prescott New Instrcutions)라고도 함&#x20;
* 13개의 새로운 명령이 있으며 디지털 신호 처리 및 3D 작업의 성능이 향상&#x20;

#### SSE4\_1 및 SSE4\_2

* SSE4는 SSE의 최신 버전이며 HD Boost라는 용어로도 알려져 있음&#x20;
* flag sse4\_1, sse4\_2는 각각 SSE4.1, SSE4.2를 나타냄&#x20;
* 두 버전 모두 54개의 새 명령어 하위 집합을 표현&#x20;

#### ht&#x20;

* Hyper-threading을 나타냄&#x20;
* CPU 코어가 병렬 방식으로 둘 이상의 스레드를 실행할 수 있음&#x20;
* 결과적으로 효율성이 증가하여 여러 프로그램을 동시에 실행할 수 있음&#x20;
* 그러나 ht 플래그는 CPU에 hyper-threading이 활성화되어 있음을 나타내지 않음&#x20;
* 단지 CPU가 hyper-threading이 가능함을 나타냄&#x20;

#### tm

* Thermal Monitor는 클럭속도를 줄여 열 출력을 줄이는 Intel CPU의 기능&#x20;
* 따라서 CPU는 프로세서의 온도가 특정 한계 이상으로 상승하면 열 관리를 수행할 수 있음&#x20;
* 물론 CPU 성능을 저하시킬 수 있지만 손상을 방지할 수 있음&#x20;
* BIOS 설정에서 활성화 할 수 있음&#x20;

#### pdcm

* Intel CPU의 pdcm 플래그는 성능 및 디버깅 기능 MSR의 약어&#x20;
* MSR은 Model-Specific Register의 약자&#x20;
* 이름에서 알 수 있듯이 특수 레지스터를 사용하여 디버깅, 프로그램 실행 추적, 벤치마크 및 성능 모니터링을 수행할 수 있음&#x20;

### fpu

* Onboard FPU(Floating Point Support)
* 부동 소수점 지원&#x20;

#### bogomips&#x20;

* [https://tldp.org/HOWTO/BogoMips/bogo-faq.html](https://tldp.org/HOWTO/BogoMips/bogo-faq.html)











