---
description: '-'
---

# useCallback

```jsx
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b],
);
```

* `메모이제이션된` 콜백을 반환
* 인라인 콜백 & 그것의 의존성 값의 배열을 전달
* `useCallback`은 콜백의 메모이제이션된 버전을 반환
  * 메모이제이션된 버전은 콜백의 의존성이 변경되었을 때에만 변경됨
  * 불필요한 렌더링을 방지하기 위해 (ex-shouldComponentUpdate를 사용해)
    * 참조의 동일성에 의존적인 최적화된 자식 컴포넌트에 콜백을 전달할 때 유용

```jsx
useCallback(fn, deps)는 useMemo(()=>fn,deps) 와 같습니다.
```

* 주의!
  * 의존성 값의 배열이 콜백에 인자로 전달되지는 않음
  * 개념적으로는, 이 기법이 콜백 함수가 무엇일지를 표현하는 방법
  * 콜백 안에서 참조되는 모든 값은 의존성 값의 배열에 나타나야 함
* `eslint-plugin-react-hooks` 패키지의 일부로써 `exhaustive-deps` 규칙을 사용하기를 권장
  * 의존성이 바르지 않게 정의되었다면 그에 대해 경고하고 수정하도록 알려줌
