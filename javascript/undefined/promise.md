---
description: '-'
---

# Promise 공식 문서

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Promise)

* Promise 객체 : 비동기 작업의 최종 완료(또는 실패)와 그 결과 값을 나타냄
* Promise는 Promise가 생성될 때 반드시 알려지지 않은 값에 대한 프록시
* handler를 비동기 작업의 최종 성공 값 또는 실패 이유와 연결할 수 있음
  * 비동기 메서드는 동기 메서드와 같은 값을 반환할 수 있음
* 즉, 최종 값을 즉시 반환하는 대신
  * 비동기 메서드는 미래의 특정 시점에 값을 제공하겠다는 Promise를 반환
*   Promise는 다음 상태 중 하나입니다.

    #### Pending: `fulfilled` 도 아니고 `rejected` 도 아닌 초기 상태

    #### Fulfilled: 작업이 성공적으로 완료되었음을 의미

    #### Rejected: 작업이 실패했음을 의미
* `Pending` 중인 Promise의 최종 상태는 값으로 `Fulfilled` 되거나 `Error`와 같은 이유가 있어 `Rejected` 될 수 있습니다
  * 위의 option 중 하나가 발생하면 Promise의 then 메서드에 의해 대기 중인 관련 handler가 호출됩니다.
  * 해당 handler가 연결될 때 약속이 이미 Fulfilled 되었거나 Rejected된 경우 핸들러가 호출되므로
    * 비동기 작업 완료와 연결된 핸들러 사이에 `Race Condition`이 없습니다.
* Promise가 fulfilled 되거나 rejected 되었지만 Pending이 아닌 경우에는 `settled` 되었다고 합니다.

<figure><img src="../../.gitbook/assets/Screenshot from 2023-01-18 11-07-10.png" alt=""><figcaption></figcaption></figure>

* Resolved라는 용어
  * Promise가 다른 Promise의 최종 상태와 일치하도록 `lock-in`(잠금)되어 있으며
  * 더 이상 Resolving 또는 Rejecting해도 아무런 효과가 없음을 의미
  * 일반적으로, resolved Promise는 fulfilled Promise와 동일하지만
    * resolved promises는 pending 또는 rejected 될 수 있습니다.

```jsx
new Promise((resolveOuter) => {
  resolveOuter(
    new Promise((resolveInner) => {
      setTimeout(resolveInner, 1000);
    })
  );
});
```

* 위 코드에서, 이 Promise는 생성될 때 이미 resolved 되었음
  * resolveOuter가 동기식으로 호출되기 때문에!
  * 다른 promise로 resolved 되었으므로 내부 Promise가 Fulfilled되는 1초 후까지 Fulfilled되지 않습니다
* 실제로 `resolution`은 종종 배후에서 이루어지며 관찰할 수 없으며
  * fulfilled 또는 rejected만 가능합니다

***

#### 참고!

* 몇몇 다른 언어들은 지연 평가 및 계산 지연을 위한 메커니즘이 있는데, 이를 `Promise`라고도 합니다
  * JS의 Promise는 콜백 함수와 연결될 수 있는 이미 발생하고 있는 Process를 나타냅니다
  * 식을 느리게 평가하려는 경우 인수가 없는 함수를 사용하는 것이 좋습니다
    * `f = () => expression` : 지연 평가되는 표현식을 생성하고
      * `f()`는 표현식을 즉시 평가합니다.

***

#### Chained Promises - 연결된 Promises

* `Promise.prototype.then()`, `Promise.prototype.catch()`, `Promise.prototype.finally()`
  * Settled된 Promise와 추가 작업을 연결하는 데 사용됨
  * 이러한 메소드들은 Promise를 반환
    * Chained(연결될) 수 있습니다.
* `then()` 메소드 : 최대 2개의 인수를 사용
  * 첫 번째 인수 : Promise가 fulfilled된 경우에 대한 콜백 함수
  * 두 번째 인수 : Promise가 rejected된 경우에 대한 콜백 함수
  * 각 `then()`은 선택적으로 연결에 사용할 수 있는 새로 생성된 Promise 객체를 반환

```jsx
const myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("foo");
  }, 300);
});

myPromise
  .then(handleFulfilledA, handleRejectedA)
  .then(handleFulfilledB, handleRejectedB)
  .then(handleFulfilledC, handleRejectedC);
```

* `then()`에 Promise 객체를 반환하는 콜백 함수가 없는 경우에도
  * 연결된 체인의 다음 링크로 처리가 계속됨
  * 따라서 체인은 마지막 `catch()` 까지 모든 Rejected 콜백 함수를 안전하게 생략할 수 있음
* 각 `then()`에서 Rejected된 Promise를 처리하면 Promise Chain이 더 아래로 내려갑니다.
  * 오류를 즉시 처리해야 하므로 때때로 선택의 여지가 없음
  * 이러한 경우, 체인 아래에서 오류 상태를 유지하기 위해 일부 유형의 오류를 발생시켜야 함
* 반면에 즉각적인 필요가 없는 경우엔, 최종 `.catch()` 문까지 오류 처리를 생략하는 것이 더 간단
  * `.catch()`는 Promise가 fulfilled되는 경우에 대한 콜백 함수를 위한 slot이 없는 `.then()`에 불과

```jsx
myPromise
  .then(handleFulfilledA)
  .then(handleFulfilledB)
  .then(handleFulfilledC)
  .catch(handleRejectedAny);
```

* 콜백 함수에 화살표 함수를 사용할 수 있음

```jsx
myPromise
  .then((value) => `${value} and bar`)
  .then((value) => `${value} and bar again`)
  .then((value) => `${value} and again`)
  .then((value) => `${value} and again`)
  .then((value) => {
    console.log(value);
  })
  .catch((err) => {
    console.error(err);
  });
```

#### Note! 더 빠른 실행을 위해 모든 동기 작업은 하나의 handler 내에서 수행하는 것이 좋음

* 그렇지 않으면 모든 handler를 실행하는데 몇 번의 tick(틱)이 걸림
* Promise의 종료 조건은 체인에서 다음 Promise의 `settled`된 상태를 결정
  * `Fulfilled` 상태는 Promise가 성공적으로 완료되었음을 나타내고
  * `Rejected` 상태는 성공하지 못함을 나타냄
  * 체인에서 `fulfilled`된 각 Promise의 반환 값은 다음 `.then()`으로 전달되는 반면
    * `Rejected` 이유는 체인의 다음 `Rejected` handler 함수로 전달됨

```jsx
(promise D, (promise C, (promise B, (promise A) ) ) )
```

* 연결된 Promises들은 중첩되어 있지만, 스택 맨 위에서 Pop이 됨
  * 체인의 첫 번째 Promise는 가장 깊이 들어가 있고, 가장 먼저 Pop이 됩니다.
* `nextValue`가 Promise면 그 결과는 동적 대체입니다
  * 반환으로 인해 Promise가 Pop되지만 `nextValue` Promise가 해당 위치로 Push됨
  * 중첩의 경우 `Promise B`와 연결된 `.then()`이 `Promise X`의 nextValue를 반환한다고 가정

```jsx
(promise D, (promise C, (promise X) ) )
```

* Promise는 둘 이상의 중첩에 참여할 수 있음

```jsx
const promiseA = new Promise(myExecutorFunc);
const promiseB = promiseA.then(handleFulfilled1, handleRejected1);
const promiseC = promiseA.then(handleFulfilled2, handleRejected2);
```

* promiseA가 `Settled` 상태로 전환되면 `.then()`의 두 인스턴스가 모두 호출됨
* 이미 `Settled` 된 Promise에 작업을 할당할 수 있음
  * 이 경우 Job은 적절한 경우에 첫 번째 비동기 기회에서 수행됨

#### Promise는 비동기식임을 보장

* 이미 `Settled` (정해진) Promise에 대한 작업은 Stack이 지워지고 시간이 경과한 후에만 발생
  * 이 효과는 `setTimeout(action, 10)` 과 매우 유사합니다

```jsx
const promiseA = new Promise((resolve, reject) => {
  resolve(777);
});
// At this point, "promiseA" is already settled.
promiseA.then((val) => console.log("asynchronous logging has val:", val));
console.log("immediate logging");

// produces output in this order:
// immediate logging
// asynchronous logging has val: 777
```

### Thenables

* Javascipt 생태계는 언어의 일부가 되기 오래 전에 여러 Promise를 구현했음
* 내부적으로 다르게 표현되지만 최소한 Promise와 같은 모든 객체는 `Thenable` 인터페이스를 구현
* `Thenable`은 두 개의 콜백으로 호출되는 `.then()` 메서드를 구현
  1. Promise가 fulfilled될 때
  2. Promise가 Rejected될 때
* Promise도 thenable입니다
* 기존 Promise 구현과 상호 운용하기 위해 언어는 Promise 대신 thenable을 사용할 수 있음
  * ex) `Promise.resolve`는 Promise를 해결할 뿐만 아니라 thenable을 추적

```jsx
const aThenable = {
  then(onFulfilled, onRejected) {
    onFulfilled({
      // The thenable is fulfilled with another thenable
      then(onFulfilled, onRejected) {
        onFulfilled(42);
      },
    });
  },
};

Promise.resolve(aThenable); // A promise fulfilled with 42
```

### Promise concurrency(Promise 동시성)

* Promise 클래스는 비동기 작업 동시성을 용이하게 하는 네 가지 정적 메서드를 제공

#### Promise.all()

* 모든 Promise가 fulfilled되면 fulfilled됨
* Promise가 Rejected되면 Reject됨

#### Promise.allSettled()

* 모든 Promise가 settled되면 fulfilled됩니다.

#### Promise.any()

* Promise가 fulfilled되면 fulfill합니다.
* 모든 Promise가 Rejected되면 Reject합니다.

#### Promise.race()

* Promise 중 하나라도 settled되면 settled됨
  * 즉, Promise가 fulfilled되면 fulfill됨
  * Promise가 Reject되면 Reject됨
* 이 모든 메서드는 iterable of promises(정확히는 thenable)을 사용해 새 Promise를 반환
  * 그들은 모두 하위 클래스화를 지원합니다
* 즉, Promise의 하위 클래스에서 호출할 수 있으며 결과는 subclass타입의 Promise
  * 하위 클래스의 생성자가 `Promise()` 생성자와 동일한 서명을 구현해야 함
  * 즉, `resolve` 및 `reject` 콜백을 매개변수로 사용해 호출할 수 있는 단일 실행 함수를 허용
  * 하위 클래스에는 `Promise.resolve()`와 같이 호출할 수 있는 resolve 정적 메서드가 있어야 value를 promise로 확인할 수 있음
* Javascript는 본질적으로 단일 스레드 ⇒ 주어진 순간에 하나의 작업만 실행되지만
  * 다른 Promise 간에 제어가 전환되어 Promise 실행이 동시에 수행되는 것처럼 보일 수 있음
* Javascript의 병렬 실행은 오직 worker threads를 통해 이루어질 수 있음
