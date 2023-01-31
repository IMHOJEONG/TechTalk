---
description: '-'
---

# Using the Effect Hook

```jsx
import React, { useState } from 'react';

function Example() {

	const [count, setCount] = useState(0);
	
	useEffect(()=>{
		document.title = `${count}`;
	});

	return (
		<div>
			<p>{count}</p>
			<button onClick={()=> setCount(count+1) }>Click me</button>
		</div>
	)

}
```

* Effect Hook을 사용하면 함수 컴포넌트에서 side effect를 수행할 수 있음

#### Side Effect

* 데이터 가져오기, 구독 설정하기, 수동으로 React 컴포넌트의 DOM을 수정하는 것까지 이 모든 것이 side effects
* Tip: React의 class 생명주기 메서드에 친숙하다면,
  * `useEffect` Hook을 componentDidMount, componentDidUpdate, componentWillUnmount가 합쳐진 것으로 생각해도 좋음
* React 컴포넌트에는 일반적으로 두 종류의 side effects가 있음
  * 정리(clean-up)가 필요한 것과 그렇지 않은 것

#### 정리(Clean-up)를 이용하지 않는 Effects

* `**React가 DOM을 업데이트한 뒤 추가로 코드를 실행해야 하는 경우가 있음**`
* 네트워크 리퀘스트, DOM 수동 조작, 로깅 등은 정리(clean-up)가 필요없는 경우들
  * 실행 이후 신경 쓸 것이 없기 때문
* React의 class 컴포넌트에서 `render` 메소드 그 자체는 side effect를 발생시키지 않음
  * 이러한 effect를 수행하는 것은 React가 DOM을 업데이트하고 난 이후
  * 그래서 ⇒ React class에서 side effect를 `componentDidMount` 와 `componentDidUpdate`에 두는 것이 이 때문

```jsx
componentDidMount() {
  document.title = `You clicked ${this.state.count} times`;
}
componentDidUpdate() {
  document.title = `You clicked ${this.state.count} times`;
}
```

* `**class 안의 두 개의 생명주기 메서드에 같은 코드가 중복되는 것에 주의합시다!**`
  * 왜?
    * 이는 컴포넌트가 이제 막 마운트된 단계인지 아니면 업데이트되는 것인지에 상관없이
      * 같은 side effect를 수행하기 때문
  * 개념적으로 렌더링 이후에는 항상 같은 코드가 수행되기를 바라는 것
  * 하지만 React 클래스 컴포넌트는 그러한 메서드를 가지고 있지 않음
  * 함수를 별개의 메서드로 뽑아낸다고 해도 여전히 두 장소에서 함수를 불러내야 함

```jsx
useEffect(() => {
  document.title = `You clicked ${count} times`;
});
```

* `**useEffect가 하는 일은 무엇일까요?**`
  * useEffect Hook을 이용해 React에게 컴포넌트가 렌더링 이후에 어떤 일을 수행해야 하는지를 말함
  * React는 우리가 넘긴 함수(`effect` 함수)를 기억했다가 DOM 업데이트를 수행한 이후에 불러낼 것
    * 문서 타이틀 지정 or 데이터를 가져오거나 다른 명령형 API를 불러내는 일도 할 수 있음
* `**useEffect를 컴포넌트 안에서 불러내는 이유는 무엇일까요?**`
  * `useEffect` 를 컴포넌트 내부에 둠으로써 effect를 통해 state 변수(어떤 prop에도) 접근할 수 잇게 됨
  * 함수 범위 안에 존재하기 때문에 특별한 API없이도 값을 얻을 수 있는 것
  * Hook은 JS의 클로저를 이용해 React의 한정된 API를 고안하는 것보다 JS가 이미 가지고 있는 방법을 이용해 문제를 해결
* `**useEffect는 렌더링 이후에 매번 수행되는 걸까요?**`
  * 넵!
  * 기본적으로 첫 번째 렌더링과 이후의 모든 업데이트에서 수행됨
  * 마운팅과 업데이트라는 방식으로 생각하는 대신 ⇒ effect를 렌더링 이후에 발생하는 것으로 생각하는 것이 더 쉬울 것
  * React는 effect가 수행되는 시점에 이미 DOM이 업데이트되었음을 보장!

***

#### 상세한 설명

```jsx
function Example() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });
}
```

* `useEffect` Hook에 함수를 전달하고 있음 ⇒ 이 함수가 바로 effect
* 같은 함수 내부에 있기 때문에 최신의 count를 바로 얻을 수 있음
* 컴포넌트를 렌더링할 때 React는 우리가 이용한 effect를 기억하엿다가 DOM을 업데이트 한 이후에 실행함
* 이는 맨 첫 번째 렌더링은 물론 그 이후의 모든 렌더링에 똑같이 적용됨

#### `useEffect`에 전달된 함수가 모든 렌더링에서 다르다는 것을 알아채자

* 의도된 부분! ⇒ 값이 제대로 업데이트 되는지에 대한 걱정 없이 effect 내부에서 그 값을 읽을 수 있게 하는 부분
* `**re-rendering하는 떄마다 모두 이전과 다른 effect로 교체하여 전달**`
  * 렌더링의 결과의 한 부분이 되게 만드는 점!
  * 각각의 effect는 특정한 렌더링에 속함

#### Tip!

* `componentDidmount` 혹은 `componentDidUpdate` 와는 달리 useEffect에서 사용되는 effect는
  * 브라우저가 화면을 업데이트하는 것을 차단하지 않음
  * 이를 통해 애플리케이션의 반응성을 향상해줌
* 대부분의 `effect`는 동기적으로 실행될 필요가 없음
* 흔하지는 않지만, (레이아웃의 측정과 같은) 동기적 실행이 필요한 경우에는
  * `useEffect`와 동일한 API를 사용하는 `useLayoutEffect`라는 별도의 Hook이 존재

#### 정리(clean-up)을 이용하는 Effects

* 정리가 필요한 effect도 있음
* 외부 데이터에 구독(subscription)을 설정해야 하는 경우
  * 메모리 누수가 발생하지 않도록 정리(clean-up)하는 것은 매우 중요
* class를 사용하는 예시
  * React class에서는 흔히 `componentDidMount` 에 구독을 설정한 뒤 `componentWillUnmount`에서 이를 정리함

```jsx
componentDidMount() {
    ChatAPI.subscribeToFriendStatus(
      this.props.friend.id,
      this.handleStatusChange
    );
  }
  componentWillUnmount() {
    ChatAPI.unsubscribeFromFriendStatus(
      this.props.friend.id,
      this.handleStatusChange
    );
  }
  handleStatusChange(status) {
    this.setState({
      isOnline: status.isOnline
    });
  }
```

* `componentDidMount`와 `componentWillUnmount`가 어떻게 대칭을 이루고 있는지를 보자
  * 두 개의 메서드 내에 개념상 똑같은 effect에 대한 코드가 있음에도 불구하고
  * 생명주기 메서드는 이를 분리하게 만듬
* Hook을 이용하는 예시
  * 정리의 실행을 위해 별개의 effect가 필요하다고 생각할 수도 있음
  * 하지만 구독의 추가와 제거를 위한 코드는 결합도가 높기 때문에 `useEffect`는 이를 함께 다루도록 고안됨
  * effect가 함수를 반환하면 React는 그 함수를 정리가 필요한 때에 실행시킬 것

```jsx
useEffect(() => {
  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }
  ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
  // effect 이후에 어떻게 정리(clean-up)할 것인지 표시합니다.
  return function cleanup() {
    ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
  };
});
```

* 주의!
  * effect에서 반드시 유명함수(named function)을 반환해야 하는 것은 아님
* `effect에서 함수를 반환하는 이유는?`
  * effect를 위한 추가적인 정리 메커니즘
  * 모든 effect는 정리를 위한 함수를 반환할 수 있음
    * ex) 구독의 추가와 제거를 위한 로직을 가까이 묶어둘 수 있음
      * 구독의 추가와 제거가 모두 하나의 effect를 구성하는 것
* `React가 effect를 정리하는 시점은 정확히 언제일까요?`
  * React는 컴포넌트가 마운트 해제되는 때에 정리를 실행
  * effect는 한번이 아니라, 렌더링이 실행되는 때마다 실행
    * React가 다음 차례의 effect를 실행하기 전에 이전의 렌더링에서 파생된 effect 또한 정리하는 이유!
* effect Hook은 두 가지 경우를 한 개의 API로 통합



[https://ko.reactjs.org/docs/hooks-effect.html](https://ko.reactjs.org/docs/hooks-effect.html)
