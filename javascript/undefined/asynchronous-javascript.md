---
description: '-'
---

# Asynchronous Javascript

[https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Introducing](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Introducing)

* 비동기 프로그래밍(Asynchronous programming)
  * 잠재적으로 오래 실행되는 작업을 시작하고 해당 작업이 완료될 때까지 기다리지 않고
    * 해당 작업이 실행되는 동안 다른 이벤트에 계속 응답할 수 있도록 하는 기술
  * 해당 작업이 완료되면 프로그램에 결과가 표시됩니다.
* 브라우저에서 제공하는 많은 기능, 특히 가장 흥미로운 기능은 잠재적으로 시간이 오래 걸릴 수 있어 비동기식
  * `fetch()` : HTTP 요청
  * `getUserMedia()` : 사용자의 카메라, 마이크에 접근
  * `showOpenFilePicker()` : 사용자에게 파일을 선택하라고 요청할 때
* 자체 비동기 함수를 자주 구현할 필요는 없지만 이를 올바르게 사용해야 함

```jsx
const name = '~~~';
const greeting = `Hello, my name is ${name}!`;
console.log(greeting);
```

* 위 코드는
  1. `name` 이라는 string 선언
  2. `name` 을 사용하는 `greeting` 이라는 또 다른 string 선언
  3. Javascript Console에 greeting 결과를 출력
* 유의!
  * `위 코드에서 브라우저는 우리가 작성한 순서대로 프로그램을 한 번에 한 줄씩 효율적으로 진행한다는 점!`
  * 각 지점에서 브라우저는 다음 줄로 이동하기 전에 해당 줄이 작업을 완료할 때까지 기다림
  * 각 라인은 이전 라인에서 수행된 작업에 의존하기 때문에 이렇게 해야 함

```jsx
function makeGreeting(name) {
	return `Hello, my name is ${name}!`;
}

const name = '~~~';
const greeting = makeGreeting(name);
console.log(greeting);

```

* `makeGreeting()` 은 동기식 함수입니다.
  * 호출자가 계속하려면 함수가 작업을 마치고 값을 반환할 때까지 기다려야 하기 때문

***

#### 오래 실행되는 동기식 함수의 문제

* 필요한 것은?
  1. 함수를 호출해 장기 실행 작업을 시작
  2. 해당 함수가 작업을 시작하고 즉시 반환하도록 해 프로그램이 다른 이벤트에 계속 응답할 수 있도록 함
  3. 작업이 완료되면 작업 결과를 알림
* 위의 작업들을 비동기 함수가 처리할 수 있음

***

#### Event handlers

* 이벤트 핸들러 : 실제로 비동기 프로그래밍의 한 형태
* 즉각 호출되는 것이 아니라 이벤트가 발생할 때마다 호출되는 함수(이벤트 핸들러)를 제공
  * event가 비동기 작업이 완료됨인 경우, 해당 이벤트를 사용해 비동기 함수 호출의 결과를 호출자에게 알릴 수 있음
* 일부 초기 비동기 API는 이러한 방식으로 이벤트를 사용했음
  * XMLHttpRequest API를 사용하면 Javascript를 사용하여 원격 서버에 HTTP 요청을 할 수 있음
  * 이 작업은 시간이 오래 걸릴 수 있으므로 비동기 API이며
    * 이벤트 리스너를 XMLHttpRequest 개체에 연결해 요청의 진행 및 최종 완료에 대한 알림을 받음

***

#### Callbacks

* 이벤트 핸들러는 특정 유형의 콜백입니다.
* 콜백은 콜백이 적절한 시간에 호출될 것이라는 기대와 함께 다른 함수로 전달되는 함수일 뿐
* 콜백은 비동기 함수가 Javascript에서 구현되는 주요 방법
* 콜백 기반 코드는 콜백 자체가 콜백을 수락하는 함수를 호출해야 하는 경우 이해하기 어려울 수 있음
  * 일련의 비동기 함수로 분해되는 일부 작업을 수행해야 하는 경우 일반적인 상황
* 일련의 비동기 함수로 분해되는 일부 작업을 수행해야 하는 경우
  * 콜백 내부에서 콜백을 호출해야 하므로 읽고 디버그하기가 훨씬 더 어려운 깊게 중첩된 함수를 얻게 됨
  * callback hell(콜백 지옥), pyramid of room(파멸의 피라미드)
    * 들여쓰기가 피라미드처럼 보이기 때문
* 콜백을 중첩하면 오류를 처리하기가 매우 어려워질 수 있음
  * 최상위 수준에서 한 번만 오류를 처리하는 대신,
    * “피라미드”의 각 수준에서 오류를 처리해야 하는 경우가 많음
* 대부분의 최신 비동기 API는 콜백을 사용하지 않음
  * 대신 JavaScript에서 비동기 프로그래밍의 기반은 Promise
