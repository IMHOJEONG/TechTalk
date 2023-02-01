---
description: '- React Suspense'
---

# Suspense



* React.lazy 및 Suspense를 사용한 코드 분할
  * 사용자에게 필요한 것보다 더 많은 코드를 제공할 필요가 없으므로 번들을 분할하여 이러한 일이 발생하지 않도록 하자&#x20;



* React.lazy 메서드&#x20;
  * 동적 가져오기를 사용해 구성 요소 수준에서 React 애플리케이션을 쉽게 코드 분할할 수 있음&#x20;

```jsx
import React, { lazy } from 'react';

const AvatarComponent = lazy(() => import('./AvatarComponent'));

const DetailsComponent = () => (
    
    <div>
        <AvatarComponent />
    </div>
)

```

* 큰 React 애플리케이션&#x20;
  * 일반적으로 많은 구성 요소, 유틸리티 메서드 및 타사 라이브러리로 구성됨&#x20;
* 필요할 때만 애플리케이션의 다른 부분을 로드하려고 노력하지 않으면 사용자가 첫 페이지를 로드하는 즉시&#x20;
  * 대규모 단일 Javascript 번들이 사용자에게 전송됨&#x20;
  * 페이지 성능에 상당한 영향을 줄 수 있음&#x20;



* React.lazy 함수 : 응용 프로그램의 구성 요소를 손쉽게 개별 Javascript chunk로 분리하는 기본 제공 방법을 제공&#x20;
  * Suspense 구성 요소와 결합할 때 로드 상태를 처리할 수 있음&#x20;

#### Suspense&#x20;

* 사용자에게 큰 Javascript 페이로드를 전송할 때의 문제&#x20;
  * 특히 저사양 장치와 저속 네트워크 연결에서 페이지 로드를 완료하는 데 걸리는 시간&#x20;
  * 코드 분할 및 지연 로딩이 매우 유용한 이유&#x20;



* 네트워크를 통해 코드 분할 구성 요소를 가져올 때 사용자가 경험해야 하는 약간의 지연이 항상 있음&#x20;
  * 유용한 로드 상태를 표시하는 것이 중요
  * Suspense 구성 요소와 함께 React.lazy를 사용하면 이 문제를 해결하는 데 도움이 됨&#x20;

```jsx
import React, { lazy, Suspense } from 'react';

const AvatarComponent = lazy(() => import('./AvatarComponent'));

const renderLoader = () => <p>Loading</p>;

const DetailsComponent = () => (
    
    <Suspense fallback={renderLoader()}>
        <AvatarComponent />
    </Suspense>
    
)

```

* fallback = Suspense는 모든 React 구성 요소를 로드 상태로 표시할 수 있음&#x20;
  * ex) 아바타가 버튼을 클릭할 때만 렌더링되고 일시 중단된 AvatarComponent에 필요한 코드를 검색하라는 요청이 생성됨&#x20;
    * 그 동안 fallback 로드 구성 요소가 표시됨&#x20;
* AvatarComponent를 구성하는 코드가 작기 때문에 로딩 스피너가 짧은 시간 동안만 표시됨&#x20;
  * 더 큰 구성 요소는 특히 약한 네트워크 연결에서 로드하는 데 훨씬 더 오래 걸릴 수 있음&#x20;
* 주의!
  * React는 현재 구성 요소가 서버 측에서 렌더링될 때 Suspense를 지원하지 않음&#x20;
  * 서버에서 렌더링하는 경우 React 문서에서 권장하는 loadable-components와 같은 다른 라이브러리를 사용하는 것이 좋음&#x20;



* 여러 구성 요소 일시 중단&#x20;
  * Suspense의 또 다른 기능은 여러 구성 요소가 모두 지연 로드된 경우에도 로드에서 여러 구성 요소를 일시 중단할 수 있다는 것&#x20;

```jsx
import React, { lazy, Suspense } from 'react';

const AvatarComponent = lazy(() => import('./AvatarComponent'));
const InfoComponent = lazy(() => import('./InfoComponent'));
const MoreInfoComponent = lazy(() => import('./MoreInfoComponent'));

const renderLoader = () => <p>Loading</p>;

const DetailsComponent = () => (
    
    <Suspense fallback={renderLoader()}>
        <AvatarComponent />
        <InfoComponent />
        <MoreInfoComponent />
    </Suspense>
)


```

* 단일 로드 상태만 표시하면서 여러 구성 요소의 렌더링을 지연시키는 매우 유용한 방법
* 모든 구성 요소 가져오기가 완료되면 사용자는 동시에 표시되는 모든 구성 요소를 보게 됨&#x20;

















[https://web.dev/i18n/ko/code-splitting-suspense/](https://web.dev/i18n/ko/code-splitting-suspense/)



