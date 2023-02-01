# Code 분할

* 기다리는 것을 좋아하는 사람은 없음&#x20;
  * **사용자의 50% 이상이 로드하는 데 3초 이상이 걸리는 경우 웹사이트를 이탈**&#x20;



*   대용량 Javascript 페이로드를 보내면 사이트 속도에 상당한 영향을 미침&#x20;

    * 애플리케이션의 첫 페이지가 로드되는 즉시 모든 Javascript를 사용자에게 제공하는 대신 번들을 여러 조각으로 나누고 맨 처음에 필요한 것만 보내자&#x20;


* Lighthouse는 페이지에서 모든 Javascript를 실행하는 데 상당한 시간이 걸리면 실패한 감사라고 표시&#x20;
* 사용자가 애플리케이션을 로드할 때&#x20;
  * 초기 경로에 필요한 코드만 보내도록 Javascript 번들을 분할
  * 구문 분석 및 컴파일해야 하는 스크립트의 양이 최소화되어 페이지 로드 시간이 빨라짐&#x20;
*   webpack, Parcel, Rollup과 같은 인기 있는 모듈 번들러를 사용하면&#x20;

    * 동적 가져오기를 사용해 번들을 분할할 수 있음&#x20;



```javascript
import moduleA from 'library';

form.addEventListener("submit", e => {
    
    e.preventDefault();
    someFunction();

});

const someFunction = () => {
    // uses moduleA
}
```

* someFunction은 특정 라이브러리에서 가져온 모듈을 사용함&#x20;
  * 다른 곳에서 사용되지 않는 경우 동적 가져오기를 사용해 사용자가 양식을 제출한 경우에만 가져오도록 코드 블록을 수정할 수 있음&#x20;

```javascript
form.addEventListener("submit", e => {
    
    e.preventDefault();
    import('library.moduleA')
        .then(module => module.default) // using the default export 
        .then(someFunction())
        .catch(handleError()); 
        
});

const someFunction = () => {
    // uses moduleA
}
```

* 모듈을 구성하는 코드는 초기 번들에 포함되지 않고 이제 지연 로드되거나 양식 제출 후 필요할 때만 사용자에게 제공됨&#x20;
* 페이지 성능을 더욱 향상시키려면 중요한 청크를 미리 로드하여 우선순위를 지정하고 더 빨리 가져오자&#x20;



*   위 코드는 간단한 예지만, 지연 로드 타사 종속성은 대규모 애플리케이션에서 일반적인 패턴이 아님&#x20;

    * 일반적으로 타사 종속성은 자주 업데이트되지 않기 때문에 캐시될 수 있는 별도의 공급업체 번들로 분할됨&#x20;
    * SplitChunksPlugin 참고

    [https://webpack.js.org/plugins/split-chunks-plugin/](https://webpack.js.org/plugins/split-chunks-plugin/)



* 클라이언트 측 프레임워크를 사용할 때 경로 또는 구성 요소 수준에서 분할하는 것
  * 애플리케이션의 다른 부분을 지연 로드하는 더 간단한 방법&#x20;
* Webpack을 사용하는 많은 인기 프레임워크는 구성을 직접 살펴보는 것보다 지연 로딩을 쉽게 하기 위해 추상화를 제공함&#x20;
*







* [https://web.dev/reduce-javascript-payloads-with-code-splitting/](https://web.dev/reduce-javascript-payloads-with-code-splitting/)
