---
description: '-'
---

# We have a problem with promises

[https://pouchdb.com/2015/05/18/we-have-a-problem-with-promises.html](https://pouchdb.com/2015/05/18/we-have-a-problem-with-promises.html)

* Promise에 문제가 있다?
  * ㄴㄴ ⇒ Promise를 실제로 이해하지 못한 채 Promise를 사용하고 있음
* 아래 4개의 식의 차이는?

```jsx
doSomething().then(function () {
  return doSomethingElse();
});

doSomething().then(function () {
  doSomethingElse();
});

doSomething().then(doSomethingElse());

doSomething().then(doSomethingElse);
```

* 콜백 지옥의 문헌을 보면, 화면 오른쪽으로 꾸준히 확장되는 끔찍한 콜백 코드와 함께 피라미드라는 참조를 볼 수 있음
  * Promise는 실제로 이 문제를 해결하지만
  * 단순한 들여쓰기 이상의 문제

#### 콜백의 진짜 문제!

[https://www.youtube.com/watch?v=hf1T\_AONQJU](https://www.youtube.com/watch?v=hf1T\_AONQJU)

* return 및 throw와 같은 키워드를 박탈한다는 것
  * 대신 우리 프로그램의 전체 흐름은 한 함수가 우연히 다른 함수를 호출하는 부작용을 기반으로 함
* 콜백은 훨씬 더 안 좋은 일을 함
  * 프로그래밍 언어에서 당연하게 여기는 스택을 빼앗아감
  * 스택 없이 코드를 작성하는 것은 브레이크 페달 없이 자동차를 운전하는 것과 매우 비슷
    * 손을 뻗어도 거기에 없을 때까지 그것이 얼마나 절실히 필요한지 깨닫지 못함
* Promise의 요점은 비동기로 갔을 때 언어의 기본인
  * `return` , `throw` , `stack` 을 돌려주는 것
  * Promise를 올바르게 사용하는 방법을 알아야 함

***

* 이 예제에서는 PouchDB를 사용하는 예제로 잘못된 Promise 패턴을 살펴보자

```jsx
remotedb.allDocs({
  include_docs: true,
  attachments: true
}).then(function (result) {
  var docs = result.rows;
  docs.forEach(function(element) {
    localdb.put(element.doc).then(function(response) {
      alert("Pulled doc with id " + element.doc._id + " and added to local db.");
    }).catch(function (err) {
      if (err.name == 'conflict') {
        localdb.get(element.doc._id).then(function (resp) {
          localdb.remove(resp._id, resp._rev).then(function (resp) {
```

* 마치 콜백인 것처럼 Promise을 사용할 수 있다는 것이 밝혀졌음
  * 오래된 콜백 습관은 죽기 어려움

```jsx
remotedb.allDocs(...).then(function (resultOfAllDocs) {
  return localdb.put(...);
}).then(function (resultOfPut) {
  return localdb.get(...);
}).then(function (resultOfGet) {
  return localdb.put(...);
}).catch(function (err) {
  console.log(err);
});
```

* 이것을 Promise 합성이라고 하며 Promise의 강력한 기능 중 하나
* Promise는 이전 Promise가 resolved된 경우에만 호출됨
  * 해당 Promise의 출력과 함께 호출됨

#### 실수 2: Promise와 함께 forEach()를 어떻게 사용?

* 익숙한 forEach() 루프(또는 for 루프 또는 while 루프)에 도달하는 즉시 Promise와 함께 작동하는 방법은?

```jsx
// 잘못된 코드
// I want to remove() all docs
db.allDocs({include_docs: true}).then(function (result) {
  result.rows.forEach(function (row) {
    db.remove(row.doc);  
  });
}).then(function () {
  // I naively believe all docs have been removed() now!
});
```

* 문제 ⇒ `첫 번째 함수가 실제로 정의되지 않은 값을 반환한다는 것`
  * 두 번째 함수는 모든 문서에서 `db.remove()`가 호출되기를 기다리지 않음
  * 실제로 아무것도 기다리지 않고, 문서가 제거되면 실행할 수 있음
* 이것은 교활한 버그일 수 있습니다
  * PouchDB가 UI를 업데이트할 수 있을 만큼 빠르게 해당 문서를 제거한다고 가정하면
    * 잘못된 것을 알아차리지 못할 수 있기 때문
  * 이 버그 : 특이한 경합 상황이나 특정 브라우저에서만 나타날 수 있으며
    * 이 경우 디버깅이 거의 불가능
* `forEach(), for, while` 이 찾고 있는 구조가 아님
  * `Promise.all()`을 원함

```jsx
db.allDocs({include_docs: true}).then(function (result) {
  return Promise.all(result.rows.map(function (row) {
    return db.remove(row.doc);
  }));
}).then(function (arrayOfResults) {
  // All docs have really been removed() now!
});
```

* 기본적으로 `Promise.all()`은 일련의 Promise를 입력으로 받은 다음
  * 다른 Promise들이 모두 해결되었을 때만 해결되는 또 다른 Promise를 제공
  * 이는 for 루프와 같은 비동기식
* `Promise.all()`은 또한 다음 함수에 결과 배열을 전달
  * ex) PouchDB에서 여러 항목을 가져오려고 하는 경우 매우 유용할 수 있음
  * `all()` Promise는 하위 Promise 중 하나라도 거부되면 거부되고 훨씬 더 유용하다

#### 실수 3: `.catch()`를 추가하는 것을 잊음

* 또 다른 일반적인 실수
* Promise가 결과 오류를 던질 수 없다는 행복한 확신을 가지고
  * 많은 개발자들이 코드의 아무 곳에나 `.catch()`를 추가하는 것을 잊어버림
  * 불행히도, `던져진 오류가 swallowed(삼켜지고), 콘솔에서 볼 수조차 없다는 것을 의미`
    * 이것은 디버그하기 정말 어려울 수 있음

```jsx
somePromise().then(function () {
  return anotherPromise();
}).then(function () {
  return yetAnotherPromise();
}).catch(console.log.bind(console)); // <-- this is badass
```

* 오류를 예상하지 않더라도 항상 `.catch()`를 추가하는 것이 좋음
  * 가정이 잘못된 것으로 판명된다면 ⇒ 삶을 더 쉽게 만들어 줌

#### 실수 4: `deferred` 를 사용하는 것

* Promise는 오랜 역사를 가지고 있고 JS 커뮤니티가 이를 올바르게 만드는 데 오랜 시간이 걸림
* 초기에 jQuery와 Angular는 모든 곳에서 이 “지연된” 패턴을 사용햇음
  * Q, When, RSVP, Bluebird, Lie, 등등
* 대부분의 Promise 라이브러리는 타사 라이브러리에서 Promise를 가져오는 방법을 제공
  * ex) Angular의 $q 모듈 ⇒ $q.when을 사용해 $q가 아닌 Promise를 래핑할 수 있음

```jsx
// Angular 사용자는 PouchDB 
$q.when(db.put(doc)).then(/* ... */); // <-- this is all the code you need
```

* 또 다른 전략 : `non-promise API` 를 래핑하는 데 유용한 `공개 생성자 패턴`을 사용하는 것
  * ex) Node의 fs.readFile()과 같은 콜백 기반 API를 래핑합니다

```jsx
new Promise(function (resolve, reject) {
  fs.readFile('myfile.txt', function (err, file) {
    if (err) {
      return reject(err);
    }
    resolve(file);
  });
}).then(/* ... */)
```

#### 실수 5: returning 대신 side effects(부작용)를 사용하는 것

```jsx
somePromise().then(function () {
  someOtherPromise();
}).then(function () {
  // Gee, I hope someOtherPromise() has resolved!
  // Spoiler alert: it hasn't.
});
```

* Promise의 마법은 return과 throw를 돌려준다는 것
* 모든 Promise는 then() 메서드(또는 then(null, …)의 설탕인 catch())를 제공

```jsx
// then() 함수 내부
somePromise().then(function () {
  // I'm inside a then() function!
});
```

* 3가지를 할 수 있습니다.

#### 1. return another promise ⇒ 다른 Promise 반환

#### 2. return a synchronous value (or undefined) ⇒ 동기 값 반환 (또는 정의되지 않음)

#### 3. throw a synchronous error ⇒ 동기적 오류 발생

`**1.다른 Promise 반환**`

* Promise 문서에서 볼 수 있는 일반적인 패턴

```jsx
getUserByName('nolan').then(function (user) {
  return getUserAccountById(user.id);
}).then(function (userAccount) {
  // I got a user account!
});
```

* `return getUserAccountById(user.id)` : 두 번째 Promise를 반환한다는 점에 유의
  * return이 중요
  * return을 표시하지 않으면, `getUserAccountById(user.id)` 가 실제로 부작용이 되고
    * 다음 함수는 userAccount 대신 undefined를 받게 됨

`**2.동기 또는 정의되지 않다는 값 반환**`

* 정의되지 않은 반환은 종종 실수이지만, 동기적인 값을 반환하는 것은 실제로 동기적 코드를 Promise 코드로 변환하는 멋진 방법

```jsx
// 사용자 메모리 내 캐시가 있다면?
getUserByName('nolan').then(function (user) {
  if (inMemoryCache[user.id]) {
    return inMemoryCache[user.id];    // returning a synchronous value!
  }
  return getUserAccountById(user.id); // returning a promise!
}).then(function (userAccount) {
  // I got a user account!
});
```

* 두 번째 함수에서 userAccount가 동기식으로 가져왔는지 비동기식으로 가져왔는지 상관하지 않음
  * 첫 번째 함수는 동기식 또는 비동기식 값을 반환할 수 있음
* 불행히도, JavaScript에서 반환되지 않는 함수는 기술적으로 정의되지 않은 반환이라는 사실이 있음
  * 무언가를 반환하려고 했을 때, 실수로 부작용이 발생하기 쉬움
* **이러한 이유로!**
  * 항상 `then()` 함수 내부에서 return 또는 throw하는 것을 개인적인 습관으로 삼음

`**3.동기적 오류 발생**`

* throw ⇒ Promise가 훨씬 더 좋을 수 있음
* 사용자가 로그아웃한 경우 동기 오류를 발생시키고 싶다면?

```jsx
getUserByName('nolan').then(function (user) {
  if (user.isLoggedOut()) {
    throw new Error('user logged out!'); // throwing a synchronous error!
  }
  if (inMemoryCache[user.id]) {
    return inMemoryCache[user.id];       // returning a synchronous value!
  }
  return getUserAccountById(user.id);    // returning a promise!
}).then(function (userAccount) {
  // I got a user account!
}).catch(function (err) {
  // Boo, I got an error!
});
```

* `catch()` : 사용자가 로그아웃하면 동기 오류를 수신하고 약속이 거부되면 비동기 오류를 수신
  * 함수는 발생한 오류가 동기식인지 비동기식인지 상관하지 않음
* 개발 중에 코딩 오류를 식별하는 데 도움이 될 수 있기 때문에 특히 유용
  * ex) then() 함수 내부의 어느 지점에서든 JSON.parse()를 수행하는 경우
    * JSON이 유효하지 않으면 동기 오류가 발생할 수 있음
  * 콜백을 사용하면 해당 오류가 삼켜지지만 Promise를 사용하면 catch() 함수 내에서 간단히 ㅓ리할 수 있음

***

#### 고급 실수 #1 : Promise.resolve()에 대해 알지 못함

* Promise는 동기 코드를 비동기 코드로 래핑하는 데 매우 유용

```jsx
new Promise(function (resolve, reject) {
  resolve(someSynchronousValue);
}).then(/* ... */);
```

* `Promise.resolve()` - 더 간결하게 표현할 수 있음

```jsx
Promise.resolve(someSynchronousValue).then(/* ... */);
```

* 이는 동기 오류를 포착하는 데에도 매우 유용
  * Promise를 반환하는 거의 모든 API 메서드를 시작하는 습관이 생김!

```jsx
function somePromiseAPI() {
  return Promise.resolve().then(function () {
    doSomethingThatMayThrow();
    return 'foo';
  }).then(/* ... */);
}
```

* **반드시 기억!**
  * 동기적으로 발생할 수 있는 모든 코드는 디버깅이 거의 불가능한 삼켜진 오류의 좋은 후보
  * 모든 것을 `Promise.resolve()`로 래핑하면 나중에 항상 `catch()` 할 수 있음
  * 마찬가지로 즉시 거부되는 Promise를 반환하는 데 사용할 수 있는 `Promise.reject()` 가 있음

```jsx
Promise.reject(new Error('some awful error'));
```

#### 고급 실수 #2 : **`then(resolveHandler).catch(rejectHandler)` 는 `then(resolveHandler, rejectHandler)` 와 완전히 같지 않아요**

* catch는 문법적 설탕입니다

```jsx
somePromise().catch(function (err) {
  // handle error
});

somePromise().then(null, function (err) {
  // handle error
});
```

* 그러나 이것은 두 snippet이 동일하다는 의미가 아님

```jsx
somePromise().then(function () {
  return someOtherPromise();
}).catch(function (err) {
  // handle error
});

somePromise().then(function () {
  return someOtherPromise();
}, function (err) {
  // handle error
});
```

* 왜? 첫 번째 함수에서 오류가 발생하면 어떻게 되는지 봐야함
  * 아래와 같이 됩니다

```jsx
somePromise().then(function () {
  throw new Error('oh noes');
}).catch(function (err) {
  // I caught your error! :)
});

somePromise().then(function () {
  throw new Error('oh noes');
}, function (err) {
  // I didn't catch your error! :(
});
```

* **알게 된 부분!**
  * **`then(resolveHandler, rejectHandler)`**
    * 이 형식에선 resolveHandler 자체에서 발생하는 경우 rejectHandler는 실제로 오류를 포착하지 않음
  * 이러한 이유 ⇒ then()의 두 번째 인수를 절대 사용하지 않는 것도 ㄱㅊ
  * 예외는 비동기 Mocha 테스트를 작성할 때 입니다
    * 여기서는 오류가 발생하는지 확인하는 테스트를 작성할 수 있음

```jsx
// mocha Test 예
it('should throw an error', function () {
  return doSomethingThatThrows().then(function () {
    throw new Error('I expected an error!');
  }, function (err) {
    should.exist(err);
  });
});
```

* Mocha, Chai는 Promise API 테스트를 위한 멋진 조합

#### 고급 실수 #3: **promises vs promise factories**

* 일련의 Promise를 차례대로 실행하고 싶다고 가정
  * `Promise.all()` 과 같은 것을 원하지만 Promise를 병렬로 실행하지는 않으려면

```jsx
function executeSequentially(promises) {
  var result = Promise.resolve();
  promises.forEach(function (promise) {
    result = result.then(promise);
  });
  return result;
}
```

* 안타깝게도 의도한 대로 작동하지 않음
  * `executeSequentially()`에 전달한 promises들은 여전히 병렬로 실행됨
  * 왜? Promise 배열에 대해 작업을 전혀 원하지 않기 때문
  * Promise 사양에 따라 Promise 생성되는 즉시 실행이 시작
    * 정말로 원하는 것은 Promise Factory의 배열?

```jsx
function executeSequentially(promiseFactories) {
  var result = Promise.resolve();
  promiseFactories.forEach(function (promiseFactory) {
    result = result.then(promiseFactory);
  });
  return result;
}
```

* `Promise Factory` : 단지 Promise를 반환하는 함수

```jsx
function myPromiseFactory() {
  return somethingThatCreatesAPromise();
}
```

* Promis Factory는 요청을 받을 때까지 Promise를 생성하지 않기 때문에 작동
  * then 함수와 같은 방식으로 작동

#### 고급 실수 #4: 두 가지 Promise의 결과를 원하면 어떻게 하나요?

* 종종 하나의 Promise는 다른 Promise에 의존하지만 두 Promise의 출력을 원한다면?

```jsx
// 일반적으로 이렇게 작성하는 습관이 있음
getUserByName('nolan').then(function (user) {
  return getUserAccountById(user.id);
}).then(function (userAccount) {
  // dangit, I need the "user" object too!
});
```

* 좋은 JS 개발자가 되거나 피라미드를 피하고 싶다면 사용자 개체의 더 높은 범위의 변수에 저장 가능

```jsx
var user;
getUserByName('nolan').then(function (result) {
  user = result;
  return getUserAccountById(user.id);
}).then(function (userAccount) {
  // okay, I have both the "user" and the "userAccount"
});
```

* 위 코드는 작동하지만 어색하다
  * 선입견을 버리고 피라미드를 받아들이는 전략?

```jsx
getUserByName('nolan').then(function (user) {
  return getUserAccountById(user.id).then(function (userAccount) {
    // okay, I have both the "user" and the "userAccount"
  });
});
```

* 일시적으로, 들여쓰기가 문제가 된다면 JS 개발자들이 옛날부터 해온 일을 할 수 있고
  * 함수를 명명된 함수를 추출할 수 있음

```jsx
function onGetUserAndUserAccount(user, userAccount) {
  return doSomething(user, userAccount);
}

function onGetUser(user) {
  return getUserAccountById(user.id).then(function (userAccount) {
    return onGetUserAndUserAccount(user, userAccount);
  });
}

getUserByName('nolan')
  .then(onGetUser)
  .then(function () {
  // at this point, doSomething() is done, and we are back to indentation 0
});
```

* Promise 코드가 더 복잡해지기 시작하면 점점 더 많은 함수를 이름이 지정된 함수로 추출할 수 있음

```jsx
putYourRightFootIn()
  .then(putYourRightFootOut)
  .then(putYourRightFootIn)  
  .then(shakeItAllAbout);
```

* 위와 같이 미학적으로 코드가 보일 수 있습니다

#### 고급 실수 #5: Promises가 무너지는 경우

* 난해한 예시가 존재

```jsx
Promise.resolve('foo').then(Promise.resolve('bar')).then(function (result) {
  console.log(result);
});
// bar가 아닌 foo를 출력합니다
```

* 왜?
  * `then()`에 비함수(예-Promise)를 전달하면 실제로 then(null)으로 해석되어 이전 Promise의 결과가 실패함

```jsx
Promise.resolve('foo').then(null).then(function (result) {
  console.log(result);
});
```

* 위처럼 원하는 만큼 `then(null)` 을 추가 ⇒ foo를 여전히 인쇄
  * Promisse vs Promise Factory
    * Promise를 then() 메서드에 직접 전달할 수 있지만, 생각한 대로 작동하지 않음
  *   then()은 함수를 취해야 함

      ```jsx
      Promise.resolve('foo').then(function () {
        return Promise.resolve('bar');
      }).then(function (result) {
        console.log(result);
      });
      ```

      * 위 코드는 예상대로 bar가 인쇄됨
* 항상 `then()`에 함수를 전달

***

#### 퍼즐 해결하기
