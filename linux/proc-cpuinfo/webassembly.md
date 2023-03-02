---
description: '-'
---

# WebAssembly

[https://tech.kakao.com/2021/05/17/frontend-growth-08/](https://tech.kakao.com/2021/05/17/frontend-growth-08/)



* JS로 개발되던 패러다임을 떠나, C, C++, Rust를 사용한 효과적이면서 더 넓은 기능을 제공할 수 있도록 개발 중&#x20;

[https://caniuse.com/?search=webassembly](https://caniuse.com/?search=webassembly)



* WebAssembly를 사용하기 위해서는 emscripten을 설치해야 한다?
* WASM 파일은 바로 읽어서 사용할 수 없음&#x20;
  * html과 wasm을 이어주기 위한 연결 다리가 필요&#x20;
  * 그 연결 다리 역할을 해주는 것이 js 파일&#x20;
  * 이것을 glue 코드라고 함&#x20;



#### 성능 차이?

* 원인 : JS 엔진이 처리하는 과정에 존재&#x20;
* parse-decode(구문 해석)
  * js 파일이 브라우저에 전달되면, 추상 구문 트리(AST)로 변환이 되어야 함&#x20;
  * AST는 각 엔진마다 다르지만 바이트 코드 형태로 변환이 됨&#x20;
  * 반면, wasm 파일은 이미 바이트 코드 형태이기 때문에&#x20;
    * 변환 과정이 필요없이 해석(decode)만 하여 검증과정만 거치면 됨&#x20;
* compile + optimize (컴파일 및 최적화)
  * js는 각 브라우저마다 다르지만 컴파일 과정을 거침&#x20;
  * 기본 컴파일러나, JIT(Just-in-time) 컴파일러를 사용하는 방식&#x20;
  * 해당 과정은 여러 요인이 있지만, WASM은 LLVM 컴파일러를 사용해 이미 많은 최적화가 진행된 상태&#x20;
  * 따라서 JS에 비해 최적화 과정이 적어짐&#x20;
* re-optimize (재-최적화)
  * 재 최적화는 js에서만 일어나는 과정&#x20;
  * JIT 컴파일러를 사용하는 과정에서 간혹 여러 이유로 인해 기존 최적화를 버리고 다시 최적화를 하는 과정&#x20;
  * WASM은 명시적인 타입 같은 이유로, JIT에서 데이터를 수집, 타입 추론 같은 과정이 필요없음&#x20;
* Execute - 실행&#x20;
  * js가 빠르게 실행되기 위해선 JIT를 자세히 알아야 함&#x20;
  * 하지만, 이는 실제로 알기도 어렵고, 모든 브라우저에서 동일하게 적용되지는 않음&#x20;
  * 반면 WASM은 이를 위한 노력이 필요 없음&#x20;
    * 이미 최적화가 많이 진행되었기 대문&#x20;
  * js가 프로그래머를 위해 작성된 언어라면, wasm은 컴파일러를 위해 작성됨&#x20;
  * 결과적으로 더 이상적인 명령어 셋을 제공해 10%\~800%의 성능 차이를 가져옴&#x20;
* GC - garbage collection
  * 아직까지 WASM에서는 GC를 제공하지 않음&#x20;
  * C, C++처럼 메모리를 수동으로 관리하는 것이 어렵지만, 안정적인 성능을 제공할 수 있음&#x20;



* 직접 테스트&#x20;
  * emscripten으로 설치 후, c++ 코드 작성&#x20;
  * emrun index.html을 통해 webassembly가 실행되는 것을 확인 ![](<../../.gitbook/assets/Screenshot from 2023-03-02 14-40-04.png>)

### golang으로도 테스트 해보자

* [https://github.com/golang/go/wiki/WebAssembly](https://github.com/golang/go/wiki/WebAssembly)

















