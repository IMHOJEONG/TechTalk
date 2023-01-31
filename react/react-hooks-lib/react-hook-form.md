---
description: '-'
---

# React-hook-form

Playce Dev 팀의 FE ⇒ 강동희님

[https://tech.osci.kr/2023/01/02/introduce-react-hook-form/](https://tech.osci.kr/2023/01/02/introduce-react-hook-form/)

[https://tech.osci.kr/2023/01/05/react-hook-form-with-mui/](https://tech.osci.kr/2023/01/05/react-hook-form-with-mui/)

[https://tech.osci.kr/2023/01/09/react-hook-form-series-3/](https://tech.osci.kr/2023/01/09/react-hook-form-series-3/)

* 웹 프론트엔드 개발 시 데이터를 다루는 방법은 여러가지
  * 차트를 통해 데이터를 시각화
  * 폼을 통해 데이터를 수집
  * 테이블을 통해 데이터를 나열
* 제어 컴포넌트로 폼을 다루기 위해서 하나하나 state를 선언해주고,
  * 해당 state를 다루기 위해서 또 핸들링 함수를 만들어야 하고, 에러를 위한 state…..
  * 코드의 길이도 문제지만, 또 다른 문제점이 존재
    * React 에서 컴포넌트 리렌더링이 발생하는 조건 중 하나는 state의 값이 변했을 때
* `**만약 모든 값이 state로 연결되어 있고 하나의 값이 변할 때마다 여러 개의 자식 컴포넌트들에서 무수히 많은 리렌더링이 발생한다**`
  * 개발자가 예측한 렌더링이 아닌…
  * 불필요한 렌더링으로서 ⇒ 불필요한 연산으로 생각할 수 있음
* [https://npmtrends.com/formik-vs-react-hook-form-vs-redux-form](https://npmtrends.com/formik-vs-react-hook-form-vs-redux-form)
  * react-hook-form의 사용 비율이 높음
*
