# Using the State Hook

```jsx
import React, { useState } from 'react';

function Example() {

	const [count, setCount] = useState(0);
	
	return (
		<div>
			<p>{count}</p>
			<button onClick={()=> setCount(count+1) }>Click me</button>
		</div>
	)

}
```

#### Hook

* Hook은 특별한 함수!
* `useState` 는 state를 함수 컴포넌트 안에서 사용할 수 있게 해줌
* 함수 컴포넌트 안에서 Hook을 이용하여 state를 사용할 수 있음

#### state 변수 선언하기

* 함수 컴포넌트는 `this`를 가질 수 없기 때문에 `this.state`를 할당하거나 읽을 수 없음
  * 대신 `useState` Hook을 직접 컴포넌트에 호출

`useState를 호출하는 것은 무엇을 하는 걸까?`

* state 변수를 선언할 수 있음
* 일반적으로 일반 변수는 함수가 끝날 때 사라지지만, state 변수는 React에 의해 사라지지 않음

`useState의 인자로 무엇을 넘겨주어야 할까요?`

* `useState()` Hook의 인자로 넘겨주는 값은 state의 초기 값
* 함수 컴포넌트의 state는 클래스와 달리 객체일 필요는 없고
  * 숫자 타입과 문자 타입을 가질 수 있음

`useState는 무엇을 반환할까요?`

* state 변수, 해당 변수를 갱신할 수 있는 함수 두 가지 쌍을 반환
* `const [count, setCount] = useState()`

```jsx
import React, { useState } from 'react';

function Example() {
	const [count, setCount] = useState(0);
}
```

* `count`라고 부르는 state 변수를 선언하고 `0`으로 초기화함
* React는 해당 변수를 re-rendering 할 때 기억하고, 가장 최근에 갱신된 값을 제공
* 주의!
  * useState로 왜 이름을 지었을까요?
  * 컴포넌트가 렌더링할 때 오직 한 번만 생성되기 때문에 “Create”라는 이름은 꽤 정확하지 않을 수 있음
  * 컴포넌트가 다음 렌더링을 하는 동안 `useState`는 현재 state를 줌
  * Hook 이름이 항상 `use` 로 시작하는 이유도 있음

[https://ko.reactjs.org/docs/hooks-state.html](https://ko.reactjs.org/docs/hooks-state.html)

#### Tip : 여러 개의 state 변수를 사용하기

`[something, setSomething]` 의 쌍처럼 state 변수를 선언하는 것은 유용

* 여러 개의 변수를 선언할 때 각각 다른 이름을 줄 수 있음

```jsx
function ExampleWithManyStates() {
  // 여러 개의 state를 선언할 수 있습니다!
  const [age, setAge] = useState(42);
  const [fruit, setFruit] = useState('banana');
  const [todos, setTodos] = useState([{ text: 'Learn Hooks' }]);

```

* age, fruit, todos 라는 지역 변수를 가지며 개별적으로 갱신할 수 있음
* 여러 개의 state 변수를 `사용하지 않아도 됨`
  * state 변수는 객체와 배열을 잘 가지고 있을 수 있음
  * 서로 연관있는 데이터를 묶을 수 있음
* 하지만 클래스 컴포넌트의 `this.setState`와는 달리 state를 갱신하는 것은 병합이 아닌
  * 대체하는 것
* 독립적인 state 변수 분할에 대한 추가적인 권장 사항은?



#### 하나 또는 여러 state 변수를 사용해야 하는가?

* Class를 배우고, `useState()`를 한 번만 호출하고 모든 state를 단일 객체에 넣고 싶을 수 있음

```jsx
function Box() {
  const [state, setState] = useState({ left: 0, top: 0, width: 100, height: 100 });
  // ...
}

// ...
  useEffect(() => {
    function handleWindowMouseMove(e) {
      // "... state"를 spread 하여 너비와 높이가 "손실"되지 않습니다
      setState(state => ({ ...state, left: e.pageX, top: e.pageY }));
    }
    // 주의: 이 구현은 약간 단순화되었습니다
    window.addEventListener('mousemove', handleWindowMouseMove);
    return () => window.removeEventListener('mousemove', handleWindowMouseMove);
  }, []);
  // ...
```

* 사용자가 마우스를 움직일 때 `left` 와 `top`의 포지션을 변경하는 로직을 작성하고 싶다면?
* 이는 state 변수를 업데이트 시, 그 값을 대체하기 때문
  * 업데이트된 필드를 객체에 병합하는 class의 `this.setState`와 다름
*   `**함께 변경되는 값에 따라 state를 여러 state 변수로 분할하는 것을 추천**`

    * ex) 컴포넌트 state를 `position` 및 `size` 객체로 분할하고 병합할 필요 없이 항상 `position`을 대체할 수 있음

    ```jsx
    function Box() {
      const [position, setPosition] = useState({ left: 0, top: 0 });
      const [size, setSize] = useState({ width: 100, height: 100 });

      useEffect(() => {
        function handleWindowMouseMove(e) {
          setPosition({ left: e.pageX, top: e.pageY });
        }
        // ...
    ```

    * 독립된 state 변수를 분리하면 또 다른 이점이 존재!
      * 나중에 관련 로직을 커스텀 Hook으로 쉽게 추출할 수 있음

    ```jsx
    function Box() {
      const position = useWindowPosition();
      const [size, setSize] = useState({ width: 100, height: 100 });
      // ...
    }

    function useWindowPosition() {
      const [position, setPosition] = useState({ left: 0, top: 0 });
      useEffect(() => {
        // ...
      }, []);
      return position;
    }
    ```
* 코드를 변경하지 않고 `position` state 변수에 대한 `useState` 호출과 관련 effect를 커스텀 Hook으로 옮길 수 있었던 방법에 유의!
  * 모든 state가 단일 객체에 있으면 추출하기가 더 어려울 것
* 모든 state를 단일 useState 호출에 넣고 필드마다 `useState` 호출을 두는 방법?
* 컴포넌트는 이러한 두 극단 사이의 균형을 찾고 관련 state를 몇 개의 독립 state 변수로 그룹화할 때
  * 가장 읽기 쉬운 경향이 존재
* State 로직이 복잡해지면 reducer로 관리 or 커스텀 Hook을 사용하는 것이 좋음









[https://ko.reactjs.org/docs/hooks-state.html#tip-using-multiple-state-variables](https://ko.reactjs.org/docs/hooks-state.html#tip-using-multiple-state-variables)











