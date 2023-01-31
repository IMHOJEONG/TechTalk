---
description: '-'
---

# Reconciliation

* [https://reactjs.org/docs/reconciliation.html](https://reactjs.org/docs/reconciliation.html)
* React는 선언적 API를 제공 ⇒ 모든 업데이트에서 정확히 무엇이 변경되는지 걱정할 필요가 없음
  * 애플리케이션 작성이 훨씬 쉬워지지만 이것이 React 내에서 어떻게 구현되는지 명확하지 않을 수 있음
* React의 `diffing` 알고리즘에서 선택한 사항을 설명하여 구성 요소 업데이트가 고성능 앱에 대해 충분히 빠르면서도 예측 가능하도록 함

#### 동기

* React를 사용할 때 단일 시점에서 `render()` 함수를 React 요소 트리를 생성하는 것으로 생각할 수 있음
* 다음 state 또는 props 업데이트에서 해당 `render()` 함수는 다른 React 요소 트리를 반환
  * 그런 다음 React는 가장 최근 트리와 일치하도록 UI를 효율적으로 업데이트하는 방법을 찾아야 함
* 하나의 트리를 다른 트리로 변환하기 위한 최소 작업 수를 생성하는 이 알고리즘 문제에 대한 몇 가지 일반적인 솔루션이 존재
  * 최첨단 알고리즘은 `O(n^3)` 복잡성을 가지며 (n: 트리의 요소 수)
* React에서 이것을 사용했다면 1000개의 요소를 표시하려면 10억 번의 비교가 필요함
  * 대신 React는 두 가지 가정을 기반으로 휴리스틱 `O(n)` 알고리즘을 구현
* 서로 다른 유형의 두 요소는 서로 다른 트리를 생성
  * 개발자는 키 소품을 사용하여 여러 렌더링에서 어떤 하위 요소가 안정적일 수 있는지 암시할 있음
  * 실제로 이러한 가정은 거의 모든 실제 사용 사례에 유효

#### Diffing Algorithm

* 두 개의 트리를 비교할 때 React는 먼저 두 개의 루트 요소를 비교함
  * 이 동작은 루트 요소의 유형에 따라 다름

#### 다른 타입의 요소

* 루트 요소가 다른 타입을 가질 때마다 React는 이전 트리를 해체하고 처음부터 새 트리를 빌드함
  * `<a>` 에서 `<img>` 로, `<Article>` 에서 `<Comment>` 로, 또는 `<Button>` 에서 `<div>` 로 이동하면 모두 완전히 다시 빌드됨
* 트리를 분해하면 이전 DOM 노드가 파괴됨
  * Component(컴포넌트) 인스턴스는 `componentWillUnmount()`를 수신
  * 새 트리를 만들 때 새 DOM 노드가 DOM에 삽입도미
* Component 인스턴스는 `UNSAFE_componentWillMount()`를 수신한 다음
  * `componentDidMount()`를 수신함
  * 이전 트리와 관련된 모든 상태가 손실됨
* 루트 아래의 모든 컴포넌트도 마운트 해제되고 상태가 파괴됨

```html

<div>
  <Counter />
</div>

<span>
  <Counter />
</span>
```

#### 같은 타입의 DOM 요소

* 동일한 타입의 두 React DOM 요소를 비교할 때 React는 두 요소의 속성을 보고 동일한 기본 DOM 노드를 유지하며 변경된 속성만 업데이트함

```html
<div className="before" title="stuff" />

<div className="after" title="stuff" />
```

* React는 이 두 요소를 비교하여 기본 DOM 노드의 className만 수정한다는 것을 알고 있음
* 스타일을 업데이트할 때 React는 변경된 속성만 업데이트한다는 것도 알고 있음

```html
<div style={{color: 'red', fontWeight: 'bold'}} />

<div style={{color: 'green', fontWeight: 'bold'}} />
```

* 위 두 요소 사이를 변환할 때 React는 fontWeight가 아닌 색상 스타일만 수정한다는 것을 알고 있음
  * DOM 노드를 처리한 후 React는 자식에 대해 재귀함

#### 같은 타입의 Component 요소들

* Component가 업데이트되면 인스턴스가 동일하게 유지되므로 렌더링 간에 상태가 유지됨
* React는 새 요소와 일치하도록 기본 Component 인스턴스의 Props를 업데이트하고 기본 Component 인스턴스에서 `UNSAFE_componentWillReceiveProps()` `UNSAFE_componentWillUpdate()` 및 `componentDidUpdate()`를 호출
* 다음으로, `render()` 메서드가 호출되고 diff 알고리즘이 이전 결과와 새 결과에 대해 재귀

#### 자식에 대해 반복

* 기본적으로 DOM 노드의 자식에서 재귀할 때 React는 두 자식 목록을 동시에 반복하고 차이가 있을 때마다 변형을 생성
  * ex) 자삭의 끝에 요소를 추가할 때 다음 두 트리 간에 변환이 잘 작동

```html
<ul>
  <li>first</li>
  <li>second</li>
</ul>

<ul>
  <li>first</li>
  <li>second</li>
  <li>third</li>
</ul>
```

* React는 두 개의 `<li>first</li>` 트리와 두 개의 `<li>second</li>` 트리를 일치시킨 다음
  * `<li>third</li>` 트리를 삽입
* 순진하게 구현하면, 처음에 요소를 삽입하면 성능이 떨어짐

```html
<ul>
  <li>Duke</li>
  <li>Villanova</li>
</ul>

<ul>
  <li>Connecticut</li>
  <li>Duke</li>
  <li>Villanova</li>
</ul>

// 이 두 트리 간의 변환은 제대로 작동하지 않음 
```

* React는 `<li>Duke</li>` 및 `<li>Villanova</li>` 하위 트리를 그대로 유지할 수 있다는 사실을 깨닫지 못함
  * 그러다보니, 모든 자식을 변형 ⇒ 이러한 비효율성은 문제가 될 수 있음

#### Key

* 이 문제 `(아마도 - 하위 트리를 그대로 유지할 수 있는데, 모든 자식을 변형하는 경우)`를 해결하고자
  * React는 key 속성을 지원
* 자식에 key가 있으면 React는 key를 사용해 원래 트리의 자식을 후속 트리의 자식과 일치시킴
* 위의 코드는 비효율적임
  * 키를 추가해서 트리 변환을 효율적으로 만들 수 있음

```html
<ul>
  <li key="2015">Duke</li>
  <li key="2016">Villanova</li>
</ul>

<ul>
  <li key="2014">Connecticut</li>
  <li key="2015">Duke</li>
  <li key="2016">Villanova</li>
</ul>
```

* 이제 React는 키가 ‘2014’인 요소가 새 요소이고 키가 ‘2015’ 및 ‘2016’인 요소가 방금 이동했음을 알고 있음
  * 실제로 키를 찾는 것은 일반적으로 어렵지 않음
  * 표시하려는 요소에 이미 고유한 ID가 있을 수 있으므로 키는 데이터에서 가져올 수 있음

```html
<li key={item.id}>{item.name}</li>
```

* 그렇지 않은 경우 모델에 새 ID 속성을 추가 or 콘텐츠의 일부를 해시하여 키를 생성할 수 있음
  * 키는 전역적으로 고유하지 않고 형제 사이에서만 고유해야함
* 최후의 수단!
  * 배열의 항목 인덱스를 전달할 수 있음
  * 항목이 재정렬되지 않는 경우 잘 작동할 수 있지만, 재정렬이 느려짐
* 재정렬은 인덱스가 키로 사용될 때 Component 상태에 문제를 일으킬 수도 있음
  * Component 인스턴스는 해당 키를 기반으로 업데이트 및 재사용됨
  * 키가 인덱스인 경우 항목을 이동하면 변경됨
  * 결과적으로 제어되지 않는 입력과 같은 항목에 대한 Component 상태가 혼합되어
    * 예기치 않은 방식으로 업데이트될 수 있음

***

Here is [an example of the issues that can be caused by using indexes as keys](https://reactjs.org/redirect-to-codepen/reconciliation/index-used-as-key)  on CodePen, and here is [an updated version of the same example showing how not using indexes as keys will fix these reordering, sorting, and prepending issues](https://reactjs.org/redirect-to-codepen/reconciliation/no-index-used-as-key) .

#### 장단점 - Tradeoff

* 조정 알고리즘은 구현 세부사항임을 기억하는 것이 중요
* React는 모든 작업에서 전체 앱을 다시 렌더링할 수 있음
  * 최종 결과는 동일
* 이 문맥에서 다시 렌더링한다는 것은 모든 Component에 대해 렌더링을 호출하는 것을 의미
* React가 해당 Component를 마운트 해제하고 다시 마운트한다는 의미는 아님
* 이전 섹션에서 설명한 규칙에 따라 차이점만 적용됨

***

* 일반적인 사용 사례를 더 빠르게 만들기 위해 휴리스틱을 정기적으로 개선 중
  * 현재 구현에서는 하위 트리가 형제 간에 이동했다는 사실을 표현할 수 있음
  * 다른 곳으로 이동했다고 말할 수는 없음
* 알고리즘은 전체 하위 트리를 다시 렌더링
* React는 휴리스틱에 의존하기 때문에 그 뒤에 있는 가정이 충족되지 않으면 성능이 저하됨
* 알고리즘은 다른 Component 유형의 하위 트리를 일치시키려고 시도하지 않음
* 출력이 매우 유사한 두 Component 유형을 번갈아 사용하는 경우
  * 동일한 유형으로 만들고 싶을 수 있음
  * 실제로 이것이 문제가 되는 것을 발견하지 못함
* key는 안정적이고 예측 가능하며 고유해야 함
  * 불안정한 키(ex-Math.random()에 의해 생성된 것과 같은…)는
    * 많은 Component 인스턴스와 DOM 노드가 불필요하게 재생성되도록 하여
    * 하위 Component에서 성능 저하 및 손실 상태를 유발할 수 있음
