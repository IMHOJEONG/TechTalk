---
description: '-'
---

# CSS Containment

* 브라우저가 페이지의 하위 트리를 페이지의 나머지 부분과 분리할 수 있도록 하여 웹 페이지의 성능을 향상시키고자 함&#x20;
* 브라우저가 페이지의 일부가 독립적이라는 것을 알고 있으면&#x20;
  * 렌더링이 최적화되고 성능이 향상될 수 있음&#x20;



* 개발자는 요소가 콘텐츠를 렌더링해야 하는지 여부와 화면 밖에 있을 때 콘텐츠를 렌더링해야 하는지 여부를 지정할 수 있음&#x20;
  * 사용자 에이전트는 적절한 경우 요소에 포함을 적용하고 실제로 필요할 때까지 잠재적으로 레이아웃 및 렌더링을 연기할 수 있음&#x20;



* ex) 많은 웹 페이지에는 서로 독립적인 여러 섹션이 포함되어 있음&#x20;

```markup
<h1>My blog</h1>
<article>
  <h2>Heading of a nice article</h2>
  <p>Content here.</p>
</article>
<article>
  <h2>Another heading of another article</h2>
  <p>More content here.</p>
</article>
```

* 위처럼 각 기사에는 CSS에 적용된 content 값이 있는 contain 속성이 있음&#x20;

```css
article {
    contain: content;
}
```

* 각 기사는 페이지의 다른 기사와 독립적
  * 브라우저에 표시하기 위해 `contain: content 가 제공됨`&#x20;
  * 그런 다음 브라우저는 이 정보를 사용해 콘텐츠를 렌더링하는 방법을 결정할 수 있음&#x20;
* ex) 볼 수 있는 영역 밖에 있는 기사는 렌더링하지 않을 수 있음&#x20;



* 각 `<article>` 에 content 값을 포함하는 속성을 지정하면, 새 요소가 삽입될 때 브라우저가 레이아웃을 다시 계산하거나 포함하는 요소의 하위 트리 외부 영역을 다시 칠할 필요가 없음&#x20;
  * 크기가 콘텐츠에 따라 달라지도록 \<article>의 스타일이 지정된 경우&#x20;
    * ex) height: auto
      * 브라우저는 크기 변경을 고려해야 할 수 있음&#x20;



* contain 속성을 통해 각 항목이 독립적이라는 것을 설명&#x20;
* content 값은 `contain:` 레이아웃 페인트 스타일의 줄임말&#x20;
* 요소의 내부 레이아웃이 페이지의 나머지 부분과 완전히 분리되어 있고 요소에 대한 모든 것이 해당 경계 내에 그려져 있음을 브라우저에 알림&#x20;
  * 아무것도 눈에 띄게 넘칠 수 없음&#x20;



* 이 정보 => 페이지를 생성하는 웹 개발자에게 일밙거으로 알려져있고 실제로 매우 분명한 정보&#x20;
  * But, 브라우저는 의도를 추측할 수 없고, 기사가 완전히 독립적일 것이라고 가정할 수 없음
* 따라서, 이 속성은 브라우저에 이 사실을 설명하고 해당 정보를 기반으로 성능을 최적화할 수 있는 좋은 방법을 제공&#x20;

#### 개념 및 용어&#x20;

*   `contain`&#x20;

    * contain 속성의 값은 해당 요소에 적용하려는 포함 유형을 나타냄&#x20;



#### Layout 포함 속성&#x20;

```css
article {
    contain: layout;
}
```

*





* [https://developer.mozilla.org/en-US/docs/Web/CSS/CSS\_Containment](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS\_Containment)
