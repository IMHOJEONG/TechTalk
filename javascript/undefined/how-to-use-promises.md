---
description: '-'
---

# How to use promises

[https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Promises](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Promises)

### Promise

* 최신 JavaScript에서 비동기 프로그래밍의 기초
* 작업의 현재 상태를 나타내는 비동기 함수에 의해 반환되는 객체
* Promise가 호출자에게 반환될 때 작업이 완료되지 않은 경우가 많지만
  * Promise 객체는 작업의 최종 성공 또는 실패를 처리하는 메서드를 제공
* 콜백을 사용한 비동기 함수 구현
  * 비동기 함수를 호출해 콜백 함수를 전달
    * 함수는 즉시 반환되고 작업이 완료되면 콜백을 호출
* Promise 기반 API를 사용
  * 비동기 함수가 작업을 시작하고 Promise 객체를 반환
    * 그 후, 이 Promise 객체에 핸들러를 연결할 수 있으며
    * 이러한 핸들러는 작업이 성공하거나 실패할 때 실행됨

***

#### fetch API를 사용

* HTTP 요청에서 원격 서버에 요청 메시지를 보내고 응답을 다시 보냄
  * 이 경우 서버에서 JSON 파일을 가져오기 위한 요청을 보냄
  * XMLHttpRequest에 대한 Promise 기반 교체인 fetch() API를 사용할 것

```jsx
const fetchPromise = fetch('<https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json>');
// #1. fetch() API 호출 및 반환 값을 fetchPromise 변수에 할당 

console.log(fetchPromise);
// #2. 그 직후에, fetchPromise 변수를 기록 

fetchPromise.then((response) => {
  console.log(`Received response: ${response.status}`);
});

console.log("Started request…");
```

```jsx
Promise { <state>: "pending" }
```

* Promise 객체가 있고 값이 `pending`인 상태가 있음을 알려줌
  * `Pending` 상태는 fetch 작업이 계속 진행 중임을 의미
  * Promise의 `then()` 메서드에 핸들러 함수를 전달
* fetch 작업이 성공하면, Promise는 핸들러를 호출하여 서버의 응답이 포함된 `Response` 객체를 전달함
*
* `fetch()` 는 요청이 계속 진행되는 동안 반환되므로 프로그램이 응답성을 유지할 수 있음
  * 응답에는 요청이 성공했음을 의미하는 `200(OK)` 상태 코드가 표시됨
* XMLHttpRequest 대신 반환된 Promise의 `then()` 메서드에 handler를 전달합니다.

***

#### Chaining Promises

* fetch() API를 사용하면 Response 객체를 받으면
  * 다른 함수를 호출하여 응답 데이터를 가져와야 합니다
  * 이 경우 응답 데이터를 JSON으로 가져오려고 하므로 Response 객체의 `json()` 메소드를 호출
  * `json()` 도 비동기식 이라는 것이 밝혀짐
    * 따라서 두 개의 연속적인 비동기 함수를 호출해야 하는 경우가 존재

```jsx
const fetchPromise = fetch('<https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json>');

// fetch()가 반환한 Promise에 then 핸들러를 추가 
fetchPromise.then((response) => {
  // 핸들러가 response.json을 호출한 다음 새로운 then() 핸들러를 response.json()이 
  // 반환한 Promise로 전달합니다 
  const jsonPromise = response.json();
  jsonPromise.then((data) => {
    console.log(data[0].name);
  });
});
```

* 다른 콜백 내에서 콜백을 호출함으로써 연속적으로 더 많은 중첩 수준의 코드를 얻는다?
  * 이 “콜백 지옥”이 우리 코드를 이해하기 어렵게 만들었다
* Promise의 우아한 특징은 `then()` 자체가 전달된 함수의 결과로 완료될 Promise을 반환한다는 것

```jsx
const fetchPromise = fetch('<https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json>');

fetchPromise
// 첫 번째 then()에 대한 handler 내에서 
// 두 번째 then()을 호출하는 대신 json()이 반환한 Promise을 반환하고 
// 해당 반환 값에서 두 번째 then()을 호출할 수 있음 
  .then((response) => response.json())
  .then((data) => {
    console.log(data[0].name);
  });
```

#### Promise Chaining

* 연속저인 비동기 함수 호출이 필요할 때, 계속 증가하는 들여쓰기 수준을 피할 수 있음을 의미
* 다음 단계로 이동하기 전에 추가해야 할 부분이 하나 더 있음
  * 요청을 읽기 전에 서버가 요청을 수락하고 처리할 수 있는지 확인해야 함

```jsx
const fetchPromise = fetch('<https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json>');

fetchPromise
  .then((response) => {
// 응답에서 상태 코드를 확인하고 OK가 아닌 경우 
    if (!response.ok) {
// 오류를 발생 시켜 이를 수행 
      throw new Error(`HTTP error: ${response.status}`);
    }
    return response.json();
  })
  .then((data) => {
    console.log(data[0].name);
  });
```

***

#### Catching errors

* 마지막 부분인 오류를 어떻게 처리해야 할까요?
  * `fetch()` API는 여러 가지 이유로 오류를 발생시킬 수 있음
    * 네트워크 연결이 없거나 URL 형식이 잘못된 경우 등
    * 또한, 서버에서 오류를 반환하면 자체적으로 오류를 발생시킴
* 중첩 콜백으로 오류 처리가 매우 어려워 모든 중첩수준에서 오류를 처리할 수 있음을 살펴봄
* 오류 처리를 지원하기 위해 `Promise` 객체는 `catch()` 메서드를 제공
  * `then()` 과 매우 유사
  * 호출하고 handler 함수를 전달함
* `then()`에 전달된 핸들러는 비동기 작업이 성공할 때 호출되지만
  * `catch()`에 전달된 핸들러는 비동기 작업이 실패할 때 호출됨

```jsx
const fetchPromise = fetch('bad-scheme://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json');

fetchPromise
  .then((response) => {
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }
    return response.json();
  })
  .then((data) => {
    console.log(data[0].name);
  })
  .catch((error) => {
    console.error(`Could not get products: ${error}`);
  });
```

***

### ❤ Promise 용어 정리

*   Promise는 3가지 상태 중 하나일 수 있음!

    #### Pending - Promise가 생성되었으며 연결된 비동기 함수가 아직 성공하거나 실패하지 않았음

    * `fetch()` 에 대한 호출에서 반환되고 요청이 아직 만들어질 때 Promise가 있는 상태

    #### Fulfilled - 비동기 함수가 성공했음, 약속이 이행되면 then() 핸들러가 호출됨

    #### Rejected - 비동기 함수가 실패했음, 약속이 거부되면 catch() 핸들러가 호출됨
* 여기서 `Succeeded` 또는 `Failed` 의 의미는 API에 달려 있음
  * `fetch()`는 서버가 404 Not Found와 같은 오류를 반환한 경우
    * 요청을 성공한 것으로 간주하지만 네트워크 오류로 인해 요청이 차단된 경우가 아님
    * 보내지고 있음
* 경우에 따라, `Settled` 라는 용어를 `Fulfilled` 와 `Rejected` 를 모두 포함하는 용어로 사용하기도 합니다.
* Promise가 `settled`되었거나 다른 Promise의 상태를 따르기 위해 `lock in`(고정) 된 경우에는 Promise가 `resolved` 된 것

***

### 여러 개의 Promises들의 혼합

* `Promise Chain`은 작업이 여러 비동기 함수로 구성되고 다음 함수를 시작하기 전에 각 함수를 완료해야 할 때 필요한 것
* 비동기 함수 호출을 결합하는 데 필요한 다른 방법이 잇음
  * `Promise API`는 이를 위한 몇가지 도우미를 제공

#### Promise.all() - 때로는 모든 Promise를 Fulfilled해야 하지만, 서로 의존하지 않는 경우

* 모두 함께 시작한 다음, 모두 fullfilled되었을 때 알림을 받는 것이 훨씬 더 효율적!
* Promise 배열을 사용하고 단일 Promise를 반환
* Promise.all()이 반환한 Promise는…
  * 배열의 모든 Promise가 `fulfilled`되는 경우 `fulfilled`됩니다.
    * 이 경우, `then()` 핸들러는 Promise가 `all()`에 전달된 것과 동일한 순서로 모든 응답의 배열과 함께 호출됨
  * 배열의 Promise 중 하나라도 `Rejected`되는 경우 `Reject`됩니다.
    * 이 경우, `catch()` 핸들러가 Rejected된 Promise에 의해 발생한 오류와 함께 호출됨

```jsx
const fetchPromise1 = fetch('<https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json>');
const fetchPromise2 = fetch('<https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/not-found>');
const fetchPromise3 = fetch('<https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json>');

/*

// 잘못된 형식의 요청 -> 3번째!
const fetchPromise1 = fetch('<https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json>');
const fetchPromise2 = fetch('<https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/not-found>');
const fetchPromise3 = fetch('bad-scheme://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json');

Promise.all([fetchPromise1, fetchPromise2, fetchPromise3])
  .then((responses) => {
    for (const response of responses) {
      console.log(`${response.url}: ${response.status}`);
    }
  })
  .catch((error) => {
    console.error(`Failed to fetch: ${error}`)
  });
*/

// 예시에서 제공해주신 요청 중 두 번째 요청의 경우 파일이 존재하지 않아 
// 404를 반환합니다. 

Promise.all([fetchPromise1, fetchPromise2, fetchPromise3])
  .then((responses) => {
    for (const response of responses) {
      console.log(`${response.url}: ${response.status}`);
    }
  })
  .catch((error) => {
    console.error(`Failed to fetch: ${error}`)
  });
```

* 세 개의 서로 다른 URL에 대해 세 개의 `fetch()` 요청을 만들고 있음
  * 모두 성공하면 각각의 응답 상태를 기록합니다.
  * 그 중 하나라도 실패하면 실패를 기록합니다.

#### Promise.any() - 때때로 fulfilled promise 세트 중 하나가 필요할 수 있고 어느 것이든 상관하지 않을 때

* `Promise.all()` 과 비슷하지만 Promise 배열 중 하나라도 fulfilled되자마자 fulfilled 되거나
  * 모두 rejected되면 rejected 된다는 점이 다름
* 어떤 fetch 요청이 먼저 완료될지 예측할 수 없기 때문에
* Promise 참조 문서를 통해 나머지 함수를 더 알아보아야 함

***

### Async and Await

* `async` 키워드는 비동기 Promise 기반 코드로 작업하는 더 간단한 방법을 제공
* 함수 시작 부분에 `async`를 추가하면 `async` 함수가 됩니다

```jsx
async function myFunction() {

}
```

* 비동기 함수 내에서 Promise를 반환하는 함수를 호출하기 전에 `await` 키워드를 사용할 수 있음
  * Promise가 확정될 때까지 해당 지점에서 코드가 대기하게 됨
* Promise의 Fulfilled된 value가 반환 값으로 처리되거나 rejected value가 발생합니다.
* 비동기 함수를 사용하지만, 동기 코드처럼 보이는 코드를 작성할 수 있음
  * ex) fetch 예제를 다시 작성하는 데 사용할 수 있음

```jsx
async function fetchProducts() {
  try {
    // after this line, our function will wait for the `fetch()` call to be settled
    // the `fetch()` call will either return a Response or throw an error
    const response = await fetch('<https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json>');
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }
    // after this line, our function will wait for the `response.json()` call to be settled
    // the `response.json()` call will either return the parsed JSON object or throw an error
    const data = await response.json();
    console.log(data[0].name);
  }
  catch (error) {
    console.error(`Could not get products: ${error}`);
  }
}

fetchProducts();

/*
async function fetchProducts() {
  try {
    const response = await fetch('<https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json>');
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }
    const data = await response.json();
    return data;
  }
  catch (error) {
    console.error(`Could not get products: ${error}`);
  }
}

const promise = fetchProducts();
console.log(promise[0].name);   // "promise" is a Promise object, so this will not work
*/
```

* `await fetch()` 를 호출하고 있으며 Promise를 받는 대신 호출자는
  * 마치 `fetch()`가 동기 함수인 것처럼 완전히 완전한 Response 객체를 반환
* 코드가 동기식인 경우와 마찬가지로 오류 처리를 위해 `try... catch` 블록을 사용할 수도 있음
* 비동기 함수는 항상 Promise를 반환하므로 `const promise = fetchProducts(); console.log(promise[0].name);` 에서 에러가 발생합니다.
  * `promise.then((data) => console.log(data[0].name));` 와 같이 표시해야 함
* 코드가 JavaScript 모듈에 있지 않는 한 비동기 함수 내에서만 `await`를 사용할 수 있음
  * 즉, 일반 스크립트에서는 이 작업을 수행할 수 없음

```jsx
try {
  // using await outside an async function is only allowed in a module
  const response = await fetch('<https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json>');
  if (!response.ok) {
    throw new Error(`HTTP error: ${response.status}`);
  }
  const data = await response.json();
  console.log(data[0].name);
}
catch(error) {
  console.error(`Could not get products: ${error}`);
}
```

* Promise Chain을 사용할 수 있는 곳에서 비동기 함수를 많이 사용하게 될 것
  * Promise 작업을 훨씬 더 직관적으로 만들어 줌
* Promise Chain과 마찬가지로 `await`는 비동기 작업이 연속적으로 완료되도록 강제함
  * 다음 작업의 결과가 마지막 작업의 결과에 따라 달라지는 경우에 필요하지만
  * 그렇지 않은 경우에는 `Promise.all()` 과 같은 것이 더 성능이 좋음

***

* Promise : 최신 JavaScript에서 비동기 프로그래밍의 기초
  * 깊게 중첩된 콜백 없이 비동기 작업 시퀀스를 쉽게 표현하고 추론할 수 있음
  * 동기식 `try...catch`문과 유사한 오류 처리 스타일을 지원
  * async 및 await 키워드를 사용하면
    * 일련의 연속 비동기 함수 호출에서 작업을 더 쉽게 빌드할 수 있음
      * 명시적 Promise 체인을 생성할 필요가 없으며 동기 코드처럼 보이는 코드를 작성할 수 있음
