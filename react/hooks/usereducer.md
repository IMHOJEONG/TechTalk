---
description: '-'
---

# useReducer

* useReducer
  * const \[state, dispatch] = useReducer(reducer, initialArg, init);
  * useState의 대체 함수
  * `(statem, action) => newState` 를 형태로 reducer를 받고 `dispatch` 메서드와 짝의 형태로 현재 state를 반환
* 다수의 하위값을 포함하는 복잡한 정적 로직을 만드는 경우나
  * 다음 state가 이전 state에 의존적인 경우에, 보통 `useState`보다 `useReducer` 를 선호
* `useReducer`는 자세한 업데이트를 트리거하는 컴포넌트의 성능을 최적화할 수 있게 함
  * 콜백 대신 dispatch를 전달할 수 있기 때문
* React는
  * `dispatch` 함수의 동일성이 안정적이고 리렌더링 시에도 변경되지 않으리라는 것을 보장
  * `useEffect`나 `useCallback` 의존성 목록에 이 함수를 포함하지 않아도 괜찮은 이유

#### 초기 state의 구체화

* `useReducer` state의 초기화에는 두 가지 방법이 있음
  * 유스케이스에 따라서 한 가지를 선택
  * 가장 간단한 방법은 초기 state를 두 번째 인자로 전달하는 것

```jsx
const [state, dispatch] = useReducer(
    reducer,
    {count: initialCount}
  );
```

* React에서는 Reducer의 인자로써 `state=initialState` 와 같은 초기값을 나타내는
  * Redux에서는 보편화된 관습을 사용하지 않음
  * 때때로 초기값은 props에 의존할 필요가 있어 Hook 호출에서 지정되기도 합니다.
  * 초기값을 나타내는 것이 정말 필요하다면 `useReducer(reducer, undefined, reducer)` 를 호출하는 방법으로
    * Redux를 모방할 수는 있겠지만, 이 방법을 권장하지는 않습니다.

#### 초기화 지연

* 초기 state를 조금 지연해서 생성할 수도 잇음
* 이를 위해서는 init 함수를 세 번째 인자로 전달
* 초기 state는 `init(initialArg)` 에 설정될 것
  * 이것은 reducer 외부에서 초기 state를 계산하는 로직을 추출할 수 있도록 함
  * 어떤 행동에 대한 대응으로 나중에 state를 재설정하는 데에도 유용함

#### dispatch의 회피

* Reducer Hook에서 현재 state와 같은 값을 반환하는 경우 React는 자식을 리렌더링하거나 effect를 발생하지 않고 이것들을 회피할 것
  * React는 `[Object.is](<http://Object.is>)` 비교 알고리즘을 사용합니다.
* 실행을 회피하기 전에 React에서 특정 컴포넌트를 다시 렌더링하는 것이 여전히 필요할 수도 있다는 것에 주의
  * React가 불필요하게 트리에 그 이상으로 `더 깊게` 까지는 가지 않을 것이므로 크게 신경쓰지 않아도 됨
  * 렌더링 시에 고비용의 계산을 하고 있다면 `useMemo`를 사용하여 그것들을 최적화할 수 있음
