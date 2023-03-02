---
description: '- Advanced Vector eXtensions'
---

# AVX

[https://namu.wiki/w/%EA%B3%A0%EA%B8%89%20%EB%B2%A1%ED%84%B0%20%ED%99%95%EC%9E%A5](https://namu.wiki/w/%EA%B3%A0%EA%B8%89%20%EB%B2%A1%ED%84%B0%20%ED%99%95%EC%9E%A5)

*   2008년 4월 인텔 개발자 포럼에서 발표된 x86 SIMD 명령어 세트로 SSE 명령어 세트 시리즈의 후속작&#x20;

    * 2011년에 출시한 Intel 샌디브릿지 마이크로아키텍처에서 최초로 지원

    &#x20;
* SIMD 레지스터 폭이 128비트에서 256비트로 증가되었고, 2 피연산자 구조에서 3 피연산자 구조로 변경됨&#x20;
  * 다만 3 피연산자 연산은 VEX 인코딩을 사용하는 SIMD 명령어에 한정되고, EAX와 같은 범용 레지스터를 지원하지 않음&#x20;
  * SIMD 메모리 피연산자의 정렬 요구도 완화
  * 새롭게 VEX 코딩 방식이 도입되어 레거 시 SSE와 AVX 코드를 함께 사용하여도 속도 저하를 피할 수 있음&#x20;

