---
description: Single instruction, Multiple dat
---

# SIMD

[https://en.wikipedia.org/wiki/Single\_instruction,\_multiple\_data](https://en.wikipedia.org/wiki/Single\_instruction,\_multiple\_data)

* Flynn 의 분류법의 병렬 처리 유형&#x20;
* SIMD는 내부일 수 있으며, ISA(명령어  세트 아키텍처)를 통해 직접 액세스 할 수 있지만, ISA와 혼동해서는 안 됨&#x20;
  * 여러 데이터 포인트에서 동시에 동일한 작업을 수행하는 여러 처리 요소가 있는 컴퓨터를 말함&#x20;



* 데이터 수준 병렬성을 이용하지만, 동시성은 이용하지 않음&#x20;
* 동시(병렬) 계산이 있지만 각 장치는 주어진 순간에 정확히 동일한 명령을 수행(단지 다른 데이터로)
* SIMD는 특히 디지털 이미지의 대비를 조정하거나 디지털 오디오의 볼륨을 조정하는 것과 같은 일반적인 작업에 적용할 수 있음&#x20;



* 대부분의 최신 CPU 설계에는 멀티 미디어 사용 성능을 개선하기 위한 SIMD 명령이 포함되어 있음



#### SIMD on the web

* Mozilla의 C/C++-to-Javascript 컴파일러인 emscripten을 사용하면 SIMD 내장 함수 또는 GCC 스타일 벡터 코드를 Javascript의 SIMD API로 C++ 프로그램을 컴파일할 수 있음&#x20;
  * 스칼라 코드와 비교해 동일한 속도 향상을 얻을 수 있음&#x20;
* 또한, WebAssembly 128비트 SIMD 제안을 지원&#x20;
*











