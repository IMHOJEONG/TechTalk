---
description: '- testing lib'
---

# @testing-library/react

```jsx
import React from "react";
import { render } from "@testing-library/react";

function WelcomeMessage({ message }) {
  return <h1>{message}</h1>;
}

test("renders the message", () => {
  const message = "Welcome to my website!";
  const { getByText } = render(<WelcomeMessage message={message} />);
  const welcomeMessage = getByText(message);
  expect(welcomeMessage).toBeInTheDocument();
});

```

* 간단한 예제
  * Message props을 가져와 h1 요소에 표시하는 WelComeMessage 라는 간단한 React Component가 있음
* `@testing-library/react` 의 렌더링 기능을 사용하면 Component를 렌더링하고 사용자와 동일한 방식으로 상호작용할 수 있음
* Message prop을 포함하는 요소를 찾기 위해 getByText 함수를 사용하고 있으며
* `expect(welcomeMessage).toBeInDocument()`문을 사용해 요소가 문서에 존재한다고 체크



* Jest, Mocha, Jasmine 등과 같은 다양한 테스트 프레임워크로 코드를 테스트하는 것이 중요&#x20;
* Enzyme, React Testing Library 또는 React Component를 테스트하기 위해 더 많은 기능을 제공하는 기타 라이브러리를 사용할 수도 잇음&#x20;
* 또한, 코드가 특정 코딩 표준을 준수하는지 확인하고 테스트를 실행하기 전
  * 일반적인 실수와 오류를 포착하기 위해 ESLint와 같은 Linter를 사용하는 것도 좋은 방법&#x20;
