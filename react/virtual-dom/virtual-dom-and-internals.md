# Virtual DOM and Internals

* [https://reactjs.org/docs/faq-internals.html](https://reactjs.org/docs/faq-internals.html)

#### Virtual DOM (가상 DOM이란 무엇인가요?)

* 가상 DOM(VDOM)은 UI의 이상적인 또는 `가상`표현이 메모리에 유지되고
  * ReatDOM과 같은 라이브러리에 의해 `실제` DOM과 동기화되는 프로그래밍 개념
* 이 과정을 `reconciliation` 이라고 함
* 이 접근 방식은 React의 선언적 API를 활성화함
  * React에 원하는 UI 상태를 지정하면 DOM이 해당 상태와 일치하는지 확인
  * App을 빌드하는 데 사용해야 하는 속성 조작, 이벤트 처리 및 수동 DOM 업데이트를 추상화
* `Virtual DOM`은 특정 기술보다 패턴에 가깝기 때문에 사람들은 때때로 다른 것을 의미한다고 말함
* React 세계에서 `가상 DOM`이라는 용어는 일반적으로 사용자 인터페이스를 나타내는 객체라서,
  * React 요소와 연관됨
* React는 `fibers`라는 내부 객체를 사용해 Component Tree에 대한 추가 정보를 보유함
  * React에서 `가상 DOM` 구현의 일부로 간주될 수 있음
