---
description: '-'
---

# Container queries - Designing in the Browser

#### Container Queries, Macro to Micro layouts, darj themes, Performant images, Art direction



* Container queries are not yet available on stable browsers



* use the parent container of an element you're targeting to adjust its styles.
  * most optimal layout
* use container queries as an entry point to style anything from backgrounds to borders



* ex) the card inside of it shifts from, at smaller sizes, it could be this vertical layout to a more horizontal layouts at this minimum size&#x20;



* The cool thing about container queries&#x20;
  * is that the component itself owns all of its own responsive information
  * This means that container queries play really well in with component-based architectures and environments&#x20;



* Co-locating styles with component logic just makes a lite bit more sense&#x20;
  * when you actually have component level responsive styles.



* Don't need an additional higher level file to manage that component's responsive styles&#x20;
  * or futz around trying to make it work elsewhere



* Container queries let you divorce the idea that your page layout needs to perfectly tie into your component layout and designs&#x20;



* page layouts, macro layouts, component layouts, micro layouts => making interfaces much more flexible&#x20;



*   ResizeObserver&#x20;

    * the effects of doing so are heavy and can cause some weird layout shift jank


* polyfill that takes advantage of ResizeObserver and unlocks container queries for use cross browser today

#### Container queries polyfill

* [https://github.com/GoogleChromeLabs/container-query-polyfill](https://github.com/GoogleChromeLabs/container-query-polyfill)



* The global viewport doesn't know where this card lives and it would style them all identically if we only had that global entry point





* Container queries are enabled by CSS Containments&#x20;
  * the first step int getting container queries to work&#x20;
    * is setting containment on the parent element&#x20;



* CSS Containment provides predictable isolation of a DOM subtree from the rest of the page.



* a lot of rendering performance improvements and enables the browser's ability to isolate queries for container queries



* With containment, a developer can tell the browser what parts of the page are encapsulated as a set of content
  * allowing the browser to reason about the content  without needing to consider the state outside of the subtree



*   CSS Containment&#x20;

    * four types => size, layout, style, apint&#x20;



```css
.card-container {
    contain: style layout inline-size;
    width: 100%;
}
```

```css
@container (mid-width: 250px) {
    .product-card {
        display: grid;
        grid-template-columns: 1fr 1fr;
        align-items: center;
        gap: 2rem;
    }
}
```

* [https://www.youtube.com/watch?v=gCNMyYr7F6w](https://www.youtube.com/watch?v=gCNMyYr7F6w)
