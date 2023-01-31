---
description: '-'
---

# Using the Effect Hook (2)

### Effect를 이용하는 팁

* `useEffect`는 좀 더 깊게 알아야할 필요가 존재

#### 관심사를 구분하려고 한다면 Multiple Effect를 사용

* Hook이 탄생한 동기
  * 생명주기 class 메서드가 관련이 없는 로직들은 모아놓고, 관련이 있는 로직들은 여러 개의 메서드에 나누어 놓는 경우가 자주 있다는 것

```jsx
componentDidMount() {
  document.title = `You clicked ${this.state.count} times`;
  ChatAPI.subscribeToFriendStatus(
    this.props.friend.id,
    this.handleStatusChange
  );
}

componentDidUpdate() {
  document.title = `You clicked ${this.state.count} times`;
}

componentWillUnmount() {
  ChatAPI.unsubscribeFromFriendStatus(
    this.props.friend.id,
    this.handleStatusChange
  );
}
```

* `document.title`을 설정하는 로직이 `componentDidMount`와 `componentDidUpdate`에 나누어져 있음
* 구독 로직 또한 `componentDidMount`와 `componentDidUpdate` 에 나누어져 있음
* ComponentDidMount가 두 가지의 작업을 위한 코드를 모두 가지고 있음

#### State Hook을 여러 번 사용할 수 있는 것처럼 ⇒ effect 또한 여러 번 사용할 수 있음
