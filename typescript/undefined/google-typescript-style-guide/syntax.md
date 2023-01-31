---
description: '-'
---

# Syntax

#### [https://google.github.io/styleguide/tsguide.html#visibility](https://google.github.io/styleguide/tsguide.html#visibility)

#### 식별자&#x20;



* 식별자는 ASCII 문자, 숫자, 밑줄(상수 및 구조화된 테스트 메서드 이름용) 및 '\\(' 기호만 사용해야 함&#x20;
* 따라서 각각의 유효한 식별자 이름은 정규식 \`**\[)\w]+**\` 와 일치

| Style          | Category                                                           |
| -------------- | ------------------------------------------------------------------ |
| UpperCamelCase | class / interface / type / enum / decorator / type parameters      |
| lowerCamelCase | variable / parameter / function / method / property / module alias |
| CONSTANT\_CASE | global constant values, including enum values.                     |
| #ident         | private identifiers are never used                                 |



#### Abbreviations (약어)

* 이름의 약어를 전체 단어로 취급하자
* 즉, 플랫폼 이름에서 요구하지 않는 한 loadHTTPURL이 아닌 loadHttpUrl을 사용
  * ex) XMLHttpRequest

#### Dollar Sign (달러 표시)

* 식별자는 타사 프레임워크의 명명 규칙과 일치하는 경우를 제외하고는&#x20;
  * 일반적으로 $를 사용하면 안 됨&#x20;

#### Type parameter (유형  매개변수)

* `Array<T>`와 같은 유형 매개변수는 단일 대문자(T) 또는 UpperCamelCase를 사용할 수 있음&#x20;

#### Test 이름&#x20;

* Closure testSuites 및 유사한 xUnit 스타일 테스트 프레임워크의 테스트 메서드이름은
  * `_` 구분 기호로 구성될 수 있음&#x20;

```typescript
testX_whenY_doesZ()
```

#### \`\_\` prefix/suffix ( \_ 접두사 / 접미사 )

* 식별자는 `_` 를 접두사 또는 접미사로 사용하면 안 됨&#x20;
* `_` 가 그 자체로 식별자로 사용되어서는 안 된다는 것을 의미&#x20;
  * ex) 매개변수가 사용되지 않음을 나타냄&#x20;
  * Tip : 배열 ( 또는 Typescript 튜플)의 일부 요소만 필요한 경우
    * 구조 분해 문에 추가 쉼표를 삽입해 중간 요소를 무시할 수 있음&#x20;

```typescript
const [a, , b] = [1,5,10]; // a <- 1, b <- 10
```

#### Imports

* Module namespace imports는 lowerCamelCase이고 파일은 snake\_case
* 즉, Import가 다음과 같은 대소문자 스타일에서 올바르 게 일치하지 않음을 의미&#x20;

```typescript
import * as fooBar from './foo_bar';
```

* 일부 라이브러리는 일반적으로 이 명명 체계를 위반하는 Namespace import 접두사를 사용할 수 있지만&#x20;
  * 압도적으로 일반적인 오픈소스 사용은 위반 스타일을 더 읽기 쉽게 만듬&#x20;
* 현재 이 예외에 속하는 유일한 라이브러리는&#x20;
  * $ 접두사를 사용하는 jquery, THREE 접두사를 사용하는 three.js



#### 상수&#x20;

* Immutable : CONSTANT\_CASE는 값이 변경되지 않아야 함을 나타내며&#x20;
  * 기술적으로 수정할 수 있는 값에 사용되어 사용자에게 수정해서는 안 됨을 나타낼 수 있음&#x20;
    * 즉, 완전히 고정되지 않은 값

```typescript
const UNIT_SUFFIXS = {
    'milliseconds': 'ms',
    'seconds': 's'
};
// JavaScript UNIT_SUFFIXES의 규칙에 따라 
// 변경가능, 대문자는 사용자가 수정하지 않도록 표시
```

* 상수는 클래스의 읽기 전용 속성일 수도 있음&#x20;

```typescript
class Foo {
  private static readonly MY_SPECIAL_NUMBER = 5;

  bar() {
    return 2 * Foo.MY_SPECIAL_NUMBER;
  }
}
```

* `Global` : 모듈 수준에서 선언된 기호, 모듈 수준 클래스의 정적 필드 및 모듈 수준 열거형 값만 `CONST_CASE`를 사용할 수 있음&#x20;
* 값이 프로그램의 수명동안 두 번 이상 인스턴스화될 수 있는 경우 lowerCamelCase를 사용해야 함&#x20;
  * 예: 함수 내에서 선언된 로컬 변수 또는 함수에 중첩된 클래스의 정적 필드&#x20;
* 값이 인터페이스를 구현하는 화살표 함수인 경우, `lowerCamelCase` 로 선언될  수  있음&#x20;









