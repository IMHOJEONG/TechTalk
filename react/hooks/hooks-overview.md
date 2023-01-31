---
description: '-'
---

# Hooks Overview

#### Hook

* 하위 호환성을 가짐
* `useState` : 바로 Hook
  * Hook을 호출해 함수 컴포넌트(function component) 안에 state를 추가했음
  * 이 state는 컴포넌트가 다시 렌더링되어도 그대로 유지됨
  * `useState` : 현재의 state 값과 이 값을 업데이트하는 함수를 쌍으로 제공
    * 이 함수 ⇒ 이벤트 핸들러 or 다른 곳에서 호출할 수 있음
* class의 `this.setState`와 거의 유사하지만,
  * **이전 state와 새로운 state를 합치지 않는다는 차이점이 있음**
* `useState` 는 인자로 초기 state 값을 하나 받음
  * `this.state` 와는 달리 `useState` Hook의 state는 객체일 필요가 없음
  * 이 초기값은 첫 번째 렌더링에만 딱 한 번 사용됨

#### 여러 state 변수 선언하기

* 하나의 컴포넌트 내에서 State Hook을 여러 개 사용할 수도 있음

```javascript
function ExampleWithManyStates() {
  // 상태 변수를 여러 개 선언했습니다!
  const [age, setAge] = useState(42);
  const [fruit, setFruit] = useState('banana');
  const [todos, setTodos] = useState([{ text: 'Learn Hooks' }]);

}
```

* 배열 구조 분해 문법
  * `useState`로 호출된 state 변수들을 다른 변수명으로 할당할 수 있게 해줌
  * 이 변수명은 `useState` API와 관련이 없음
* 대신 React는 매번 렌더링할 때 `useState` 가 사용된 순서대로 실행할 것

#### 근데 Hook이 뭔가요?

* Hook: 함수 컴포넌트에서 React State와 생명주기 기능(lifeCycle features)를 연동(hook into)할 수 있게 해주는 함수
  * Hook은 class 안에서는 동작하지 않음
  * class없이 React를 사용할 수 있게 해주는 것
* React는 `useState` 같은 내장 Hook을 몇 가지 제공
  * 컴포넌트 간에 상태 관련 로직을 재사용하기 위해 Hook을 직접 만드는 것도 가능

















































[https://ko.reactjs.org/docs/hooks-overview.html](https://ko.reactjs.org/docs/hooks-overview.html)
