---
description: '- the family selector'
---

# :has()



* (CSS 용어로) 시간이 시작된 이래로, 우리는 다양한 의미에서 cascade를 사용해 왔음&#x20;
* 우리의 스타일은 "Cascading Style Sheet"를 구성&#x20;
  * 선택자도 cascade됨&#x20;
* 옆으로 갈 수 있음, 대부분의 경우 아래로 내려감&#x20;
  * 그러나 결코 위로 올라가지 마십시오
  * `:has()` CSS 의사 클래스는 매개변수로 전달된 선택자가 하나 이상의 요소와 일치하는 경우 요소를 나타냄&#x20;
* parent 선택자 이상, family 선택자는 어떠한가?



#### :has 쓰는 방법&#x20;

```markup
<div class="everybody">
  <div>
    <div class="a-good-time"></div>
  </div>
</div>

<div class="everybody"></div>
```

```css
.everybody:has(.a-good-time) {
  animation: party 21600s forwards;
}
```

* `.everybody 의 첫 번째 인스턴스가 선택되고 애니메이션이 적용`&#x20;
  * .every 클래스가 있는 요소가 대상&#x20;
  * 조건은 `a-good-time` 클래스의 후손을 갖는 것&#x20;
*









* [https://developer.chrome.com/blog/has-m105/](https://developer.chrome.com/blog/has-m105/)
