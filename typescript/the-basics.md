# The Basics

* Javascript의 모든 값에는 서로 다른 작업을 실행하여, 관찰할 수 있는 일련의 동작이 존재

```javascript
message.toLowerCase(); // toLowerCase 프로퍼티에 접근 후 이를 호출

message(); // message를 호출
```

* message의 값을 모른다면 ⇒ 이 코드를 실행하려고 할 때, 어떤 결과를 얻게 될 지 확실하게 말 할 수 없음
  * 각 작업의 동작은 전적으로 우리가 처음에 어떤 값을 가지고 있었는지에 따라 달라짐
* 아래 사항을 고려!
  * `message`가 callable한 가?
  * `toLowerCase`라는 속성이 있는가?
    * 그렇다면, `toLowerCase`도 호출 가능한가?
  * `message`, `toLowerCase` 모두 호출가능하면 무엇을 반환하는가?

```javascript
const message = "Hello World!";

message.toLowerCase(); // 동일한 문자열이 소문자로만 표시됨 

message(); // TypeError: message is not a function
```

* Javascript 런타임이 수행할 작업을 선택하는 방식
  * 값의 유형을 파악
    * 어떤 종류의 동작 및 기능이 있는지 ⇒ 이것을 파악하는 것
    * TypeError가 암시하는 것의 일부
* Primitive 문자열 및 숫자와 같은 일부 값의 경우 `typeof` 연산자를 사용해 런타임에 해당 유형을 식별할 수 있음
  * 함수처럼 다른 경우에는, type을 식별하는 해당 런타임 메커니즘이 없음

```javascript
function fn(x) {
	return x.flip();
}
```

* 호출 가능한 flip property가 있는 객체가 제공되는 경우에만 이 함수가 작동한다는 것을 코드를 읽으면 알 수 있음
  * Javascript는 코드가 실행되는 동안, 확인할 수 있는 방식으로 이 정보를 표시하지 않음
*   순수 Javascript에서 `fn`이 특정 값으로 수행하는 작업을 알려주는 유일한 방법

    * 해당 값을 호출하고 어떤 일이 발생하는지 확인하는 것

    ⇒ 코드가 실행되기 전에 수행할 작업을 예측하기 어렵게 만듬
* 즉, 코드를 작성하는 동안 코드가 수행할 작업을 파악하기가 더 어려움
  * type ⇒ fn에 전달할 수 있는 값과 충돌할 값을 설명하는 개념
* JavaScript : 동적 typing을 제공 ⇒ 코드를 실행하여 어떤 일이 발생하는지 확인하는 것
  * 대안! 정적 유형 시스템을 사용해 실행되기 전에 예상되는 코드를 예측하는 것

#### 정적 유형 검사

* TypeError를 다시 생각해 보십시오
* 대부분의 사람들은 코드를 실행할 때 어떤 종류의 오류도 발생하는 것을 좋아하지 않음
  * 버그로 간주됨 ⇒ 새 코드를 작성할 때 새 버그가 발생하지 않도록 최선을 다해야 함
* 약간의 코드만 추가하고, 파일을 저장하고, 코드를 다시 실행하고, 즉시 오류를 확인하면
  * 문제를 신속하게 격리할 수 있음
  * 항상 그런 것은 아님!
* 기능을 충분히 철저하게 테스트하지 않았을 수 있음
  * 발생할 수 있는 잠재적인 오류가 실제로 발생하지 않을 수도 있음
  * 또는 우리가 오류를 목격할만큼 운이 좋았다면 결국 대규모 리팩토링을 수행하고 파헤쳐야 하는 다양한 코드를 많이 추가했을 수도 있음
* 이상적으로 ⇒ 코드를 실행하기 전에 이러한 버그를 찾는 데 도움이 되는 도구가 있을 수 있음
  * Typescript와 같은 정적 유형 검사기가 하는 일
* 정적 유형 시스템은 프로그램을 실행할 때 값의 모양과 동작을 설명
  * Typescript와 같은 타입 검사기는 해당 정보를 사용해 문제가 발생할 수 있는 시기를 알림

```javascript
const message = "hello!";
 
message();
```





#### 예외가 아닌 실패

* 런타임 오류와 같은 특정 사항에 대해 논의했음
* JS 런타임이 무언가 무의미하다고 생각한다고 알려주는 경우
  * 이러한 경우는 ECMAScript 사양에 예기치 않은 상황이 발생했을 때 언어가 어떻게 작동해야 하는지에 대한 명시적인 지침이 있기 때문에 발생
* ex) 사양에 따르면 호출할 수 없는 것을 호출하려고 하면 오류가 발생해야 함
  * “명백한 동작”처럼 들릴 수 있지만, 객체에 존재하지 않는 속성에 액세스하면 오류도 발생한다고 상상할 수 있음
    * 대신 Javascript는 다른 동작을 제공하고 `정의되지 않은 값`(undefined)을 반환

```javascript
const user = {
  name: "Daniel",
  age: 26,
};
user.location; // returns undefined
```

* 궁극적으로 정적 유형 시스템은 즉시 오류를 발생시키지 않는 유효한 Javascript인 경우에도
  * 시스템에서 오류로 표시되어야 하는 코드를 호출해야 함
  * Typescript에서 ⇒ 정의되지 않은 위치에 대한 오류를 생성
* 때때로, 우리가 표현할 수 있는 것의 절충(trade-off)를 의미
  * Typescript는 합법적인 버그를 많이 잡아냄
  * ex) 오타

```typescript
// 오타 
const announcement = "Hello World!";
 
// How quickly can you spot the typos?
announcement.toLocaleLowercase();
announcement.toLocalLowerCase();
 
// We probably meant to write this...
announcement.toLocaleLowerCase();

// 호출되지 않은 함수 
function flipCoin() {
  // Meant to be Math.random()
  return Math.random < 0.5;
 // Operator '<' cannot be applied to types '() => number' and 'number'.
}


// 기본 논리 오류 
const value = Math.random() < 0.5 ? "a" : "b";
if (value !== "a") {
  // ...
} else if (value === "b") {
// This condition will always return 'false' since the types '"a"' and '"b"' have no overlap.
  // Oops, unreachable
}
```

#### Tooling을 위한 Type

* Typescript는 코드에서 실수를 할 때 버그를 잡아낼 수 있음
  * 훌륭하지만 Typescript는 애초에 이러한 실수를 방지할 수 있음
* Type-checker에는 변수 및 기타 속성에서 올바른 속성에 액세스하고 있는지 여부와 같은 것을 확인하는 정보가 있음
  * 해당 정보가 있으면 사용하려는 속성을 제안하기 시작할 수 있음
* 즉, Typescript를 코드 편집 + 핵심 타입 검사기는 편집기에 입력할 때
  * 오류 메시지와 코드 완성을 제공할 수 있음
* 이 부분! Typescript의 도구에 대해 이야히할 때 자주 언급하는 부분

#### `tsc` : Typescript 컴파일러

* Typescript 컴파일러 ⇒ 먼저 npm을 통해 가져와야 함











