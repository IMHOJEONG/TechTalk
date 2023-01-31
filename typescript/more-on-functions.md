---
description: '- 함수에 대해 더 자세히 알아보자!'
---

# More on Functions

* 함수 : 로컬 함수, 다른 모듈에서 가져온 것이든, 클래스의 메서드이든 상관없이 모든 응용 프로그램의 기본 빌딩 블록
  * 또한 값이며, 다른 값과 마찬가지로 Typescript에는 함수 호출 방법을 설명하는 다양한 방법이 존재&#x20;

#### 함수 유형 표현식&#x20;

* 함수를 설명하는 가장 간단한 방법은 함수 유형 표현식을 사용하는 것&#x20;
  * 구문적으로 화살표 함수와 유사

```typescript
function greeter(fn: (a: string) => void) {
    fn("Hello, World");
}

function printToConsole(s: string) {
    console.log(s);
}

greeter(printToConsole);

```

* `(a: string) => void 구문 : "반환 값이 없는 문자열 형식의 a라는 이름의 매개변수가 하나 있는 함수"`
* `함수 선언과 마찬가지로 매개변수 유형이 지정되지 않으면 암시적으로 any`

**``**

* **`매개변수 이름은 필수 항목`**
  * **`` 함수 유형 `(string)=>void` ``**&#x20;
    * **`'모든 유형의 문자열이라는 이름의 매개변수가 있는 함수'를 의미`**&#x20;



* 물론, 타입 별칭을 사용하여 함수 타입의 이름을 지정할 수 있음&#x20;

```typescript
type GreetFunction = (a: string) => void;
function greeter(fn: GreetFunction) {
  // ... 
}
```

#### 호출 시그니처&#x20;

* Javascript에서 함수는 호출 가능할 뿐만 아니라 속성을 가질 수 있음&#x20;
* 그러나 함수 유형 표현식 구문은 속성 선언을 허용하지 않음&#x20;
* 속성으로 호출 가능한 것을 설명하려면 개체 유형에 호출 서명을 작성할 수 있음&#x20;

```typescript
type DescribableFunction = {
    description: string;
    (someArg: number): boolean;
}

function doSomething(fn: DescribableFunction) {
    console.log(fn.description + "~~~" + fn(6));
)
```

* 구문은 함수 유형 표현식과 비교하여 약간 다름&#x20;
  * `=>` 대신 매개변수 목록과 리턴 유형 사이에 `:` 를 사용



#### Construct Signatures (생성자  시그니처)

* `new` 연산자를 사용하여 `Javascript` 함수를 호출할 수도 있음&#x20;
* `Typescript 는 일반적으로 새 객체를 생성하기 때문에 생성자라고 함`
* `호출 서명 앞에 new 키워드를 추가하여 구성 서명을 작성할 수 있음`&#x20;

```typescript
type SomeConstructor = {
    new (s: string): SomeObject;
};

function fn(ctor: SomeConstructor) {
    return new ctor('hello');
}
```

* Javascript의 Date 객체와 같은 일부 객체는 new를 사용하거나 사용하지 않고 호출할 수 있음&#x20;
* 호출을 결합하고 동일한 유형의 서명을 임의로 구성할 수 있음&#x20;

#### Generic Functions (제네릭 함수)

* 입력 Type이 출력 Type과 관련되거나 두 입력 Type이 어떤 식으로든 관련되는 함수를 작성하는 것이 일반적&#x20;
* &#x20;배열의 첫 번째 요소를 반환하는 함수&#x20;

```typescript
function firstElement(arr: any[]) {
    return arr[0];
}
```

* 이 함수는 제 역할을 하지만 불행하게도 return 타입이 any
* 함수가 배열 요소의 Type을 반환하면 더 좋을 것&#x20;



* Typescript에서 Generic은 두 값 간의 대응 관계를 설명하려는 경우에 사용됨&#x20;
* 함수 서명에서 Type 매개변수를 선언하여 이를 수행&#x20;

```typescript
function firstElement<Type>(arr: Type[]): Type | undefined {
    return arr[0];
}
```

* 이 함수에 Type 매개변수 Type을 추가하고 두 곳에서 사용하여&#x20;
  * 함수의 입력(배열)과 출력(반환 값) 사이에 링크를 만듬&#x20;
* 이제 호출하면 보다 구체적인 타입이 나옴&#x20;



```typescript
// S is of type 'string'
const s = firstElement(["a", "b", "c"]);

// n is of type 'number'
const n = firstElement([1,2,3]);

// u is of type undefined
const u = firstElement([]);

```

#### Inference (추론)

* Type을 지정할 필요가 없는 경우, Typescript에 의해 자동으로 선택되어 유추되었음&#x20;
* 여러 유형 매개변수를 사용할 수도 있음&#x20;
  * standalone(독립 실행형) 버전의 Map은?

```typescript
function map<Input, Output>(arr: Input[], func: (arg: Input => Output): Output[] {
    return arr.map(func);
}


// Parameter 'n'은 타입이 'string'
// 'parsed' 는 number[] 타입입니다 
const parsed = map(["1","2","3"], (n)=>parseInt(n));

```

* 위 예에서 Typescript는 입력 Type 매개변수(주어진 문자열 배열에서)의 Type과 함수 표현식(숫자)의 반환 값을&#x20;
  * 기반으로 하는 출력 Type 매개변수를 모두 유추할 수 있음&#x20;







[https://www.typescriptlang.org/docs/handbook/2/functions.html](https://www.typescriptlang.org/docs/handbook/2/functions.html)

