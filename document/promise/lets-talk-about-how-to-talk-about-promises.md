# Let's talk about how to talk about promises

[https://thenewtoys.dev/blog/2021/02/08/lets-talk-about-how-to-talk-about-promises/](https://thenewtoys.dev/blog/2021/02/08/lets-talk-about-how-to-talk-about-promises/)

* Promise `fulfilled` vs Promise `resolving`
* Promise가 동시에 `pending` 및 `resolved` 될 수 있다?
* 많은 코드가 pending 중인 resolved된 Promise를 생성하고 있다?
* Promise를 resolved될 때 Promise를 `fulfilled` 하는 것이 아니라 `rejected`하는 것일 수 있다?
  * 혹은 둘다?

### Resolve

* 아마도 Promise의 땅에서 가장 오해를 많이 받는 단어
* `resolve`와 `fulfill`은 동의어가 아님
  * Javascript Promise API의 일부 ⇒ 차이점을 이해할 필요가 있음

### Promise의 기본 상태 : 상호 배타적인 세 가지 값 중 하나

#### Pending - 대부분의 Promise의 초기 상태 / Fulfilled되거나 Rejected되지 않음

#### Fulfilled - fulfill 값으로 Promise가 fulfill되었음

#### Rejected - Reject 이유(Promise를 fulfill할 수 없는 이유 설명)와 함께 Promise가 Reject됨

* `settled` : 편의를 위해 fulfilled 또는 rejected를 의미하는 총칭
* resolve라는 단어는 어디에도 없음
  * 일반적인 생각과 달리 Promise를 resolve한다고 해서 반드시 기본 상태가 변경되는 것은 아님
    * 사실 그렇지 않은 경우가 더 많음
  * `Promise resolution` 은 `Promise fulfillment`와 별개의 개념
* Promise를 resolve할 때 그 이후부터 해당 Promise에 어떤 일이 일어날 지 결정
  * 42, answer, {”example”: “result”}와 같은 것으로 resolve하면
    * 해당 값으로 Promise를 fulfilled함
* 그러나 Promise를 다른 Promise(일반적으로 thenable)으로 resolve하면
  * 다른 Promise를 따르고 수행할 작업을 할 Promise를 말하는 것
* 다른 Promise가 fulfilled되면, 원래 Promise은 다른 Promise의 fulfilled 값으로 fulfill됨
* 다른 Promise가 rejected되면, 원래 Promise은 다른 Promise의 reject 이유와 함께 reject됨
* 다른 Promise가 settled하지 않으면, 원래 Promise도 settled되지 않음
* 어떤 일이 발생하든 결과에 영향을 미치기 위해 Promise에 대해 더 이상 할 수 있는 일은 없음
* Promise는 취소가 불가능한 다른 Promise로 resolved됨
  * 다시 resolved하거나 reject하려는 시도는 효과가 없음
* 새로운 Promise 콜백이 인수로 받는 resolvce 함수를 생각해보자
  * resolved될 Promise를 통과한 적인 없는 것 같아? 그건 틈새 사용 사례일 뿐이다?
    * 이 방법은 Promise를 resolve하는 한 가지 방법일 뿐(주된 방법은 아님).
  * 아마도 주요한 것은 Promise 핸들러 함수에서 무언가를 반환하는 것
    * 핸들러 함수에서 Promise를 많이 반환했을 것

```jsx
function doStuff() {
    return first()
    .then(firstResult => {
        return second(firstResult);
    });
}
// ...
doStuff()
.then(result => {
    // ...use `result`...
});
.catch(error => {
    // ...handle/report error...
});
```

* 첫 번째 then 호출은 `.then(second)` 일 수 있음
  * 여기서 목표는 명확성!
* 비동기 함수가 등장하기 전에는?
  * pending 중인 resolved된 Promise를 만듭니다
* doStuff를 호출하면 Promise를 생성하고 반환하는 것이 먼저 호출됩니다.
  * 이 생성된 Promise를 PromiseA
  * 그 Promise를 호출하면 doStuff가 반환하는 Promise ⇒ Promise B를 생성하고 반환합니다.
* 어느 시점에서 처음부터의 Promise가 fulfilled되었다고 가정해 보자
  * doStuff의 fulfillment handler는 해당 fulfill된 값으로 두 번째를 호출하고 두 번째로 제공하는 Promise를 반환합니다. ⇒ Promise C
  * 그러면 Promise B가 Promise C로 변환됨
    * 그 시점부터 Promise B는 Promise C를 따르고 수행하는 작업을 수행
* Promise C는 해당 코드가 Promise B를 Promise B로 해결할 때 아직 settled 되지 않음
  * Promise B는 pending 상태로 유지되지만 (fulfilled 또는 rejected되지 않음)
  * 또한 resolved됨(아무것도 일어날 일을 변경할 수 없음, 무슨 일이 있어도 Promise C를 따름)
* Promise C가 settled되면 Promise B도 같은 방식으로 settled됨
* 비동기 함수는 Promise를 만들고 사용하기 위한 구문이기 때문에
  * 비동기 함수에서도 같은 일이 발생

```jsx
// async/await 를 사용하여 doStuff를 작성
async function doStuff() {
    const firstResult = await first();
    return second(firstResult);
}
```

* 첫 번째 Promise가 fulfilled되면 doStuff는 second를 호출한 다음
  * second promise를 반환하는 promise를 확인
  * 그 시점에, second의 promise가 settled될 때까지/되지 않을 때까지
    * doStuff의 Promise는 pending 및 resolved됨
  * 두 번째 Promise가 settled되면 자체적으로 fulfilled되거나 rejected됨

#### Resolving vs Fulfilling Promise

* 상황이 아직 진행 중인데 왜 resolved이라는 단어를 사용하는가?
  * `irrevocability` 때문
  * 일단 Promise가 resolved되면 그 Promise에 일어날 일을 바꿀 수 있는 것은 아무것도 없음
* 비 Promise 값으로 resolved되면 해당 값으로 fulfilled됨
  * Promise로 resolved되면 다른 Promise를 따를 것
* Resolution을 변경하거나 직접 거부할 수 없음
  * 종종 그렇게 하려고 시도조차 할 방법이 없지만, 새로운 Promise 콜백에서 Promise의 resolve 및 reject 기능이 있더라도
  * Resolution이 해결되면 둘 다 아무것도 하지 않음
* 특정 course에 있으며 진행 중인 course의 결과가 아직 명확하지 않을 수 있지만
  * 진행 중인 course를 변경할 수는 없음
