---
description: '- 데이터 의존성'
---

# Data Dependency?

[https://namu.wiki/w/%EB%8D%B0%EC%9D%B4%ED%84%B0%20%EC%9D%98%EC%A1%B4%EC%84%B1](https://namu.wiki/w/%EB%8D%B0%EC%9D%B4%ED%84%B0%20%EC%9D%98%EC%A1%B4%EC%84%B1)

{% embed url="https://en.wikipedia.org/wiki/Data_dependency" %}

#### 데이터 의존성&#x20;

* 프로그램 명령문(명령어)이 앞선 명령문의 데이터를 참조하는 상황&#x20;



#### 의존성 분석(Dependency Analysis)

* 컴파일러 이론에서 각 구문(or 어셈블리 인스트럭션)의 데이터 의존성을 찾아내는 것&#x20;
* 데이터 의존성&#x20;
  * flow dependency&#x20;
  * name dependency
  * control dependency&#x20;



$$
{\displaystyle \left[I(S_{1})\cap O(S_{2})\right]\cup \left[O(S_{1})\cap I(S_{2})\right]\cup \left[O(S_{1})\cap O(S_{2})\right]\neq \varnothing }
$$

* S1과 S2 명령어가 있고, S2가 S1에 의존한다고 가정하면&#x20;
* I(S1)은 S1에 의해 읽은 메모리 위치 집합&#x20;
* O(S1)은 S1에 의해 쓰여진 메모리 위치 집합&#x20;
* S1부터 S2까지 실행 가능한 런타임 실행 경로가 있음&#x20;

#### Anti-dependence

* S2가 그것을 덮어쓰기 전에 S1이 읽으려고 하는 행동&#x20;

#### Flow(Data) dependence

* S2에 의해 읽어지기 전에 S1이 쓰는 것&#x20;

#### Output Dependence

* S1, S2 모두 같은 메모리 위치에 쓰는 것&#x20;

#### Flow Dependency&#x20;

* RAW (Read After Write)
  * 명령이 이전 명령의 결과에 의존할 때 발생&#x20;

```
1. A = 3
2. B = A
3. C = B
```

* 명령 3은 명령 2에 실제로 종속되어 있음&#x20;
* C의 최종 값은 명령 업데이트 B에 따라 달라지기 때문&#x20;
  * B의 최종 값이 명령 업데이트 A에 따라 달라짐&#x20;
  * 명령 2는 명령 1에 실제로 종속됨&#x20;
* 명령 2에 따라 명령 2가 명령 1에 실제로 종속되고 명령 3도 명령 1에 실제로 종속됨&#x20;

#### Anti-dependency

* WAR(읽기 후 쓰기)라고도 하는 반종속성은 명령어에 나중에 업데이트되는 값이 필요할 때 발생&#x20;

```
1. B = 3
2. A = B + 1
3. B = 7
```

* 명령 2는 명령 3에 의존하지 않음&#x20;
* 명령의 순서 변경 불가 및 병렬로 실행할 수 없음&#x20;
  * 이는 A의 최종 값에 영향을 미치기 때문&#x20;

```
 MUL R3,R1,R2
 ADD R2,R5,R6
```

* 위의 예, 처음에 R2를 읽은 다음 두 번째 명령어에서 우리는 R2에 대한 새 값을 쓰고 있음&#x20;
* Anti 종속성 = 이름 종속성의 예&#x20;
  * 즉, 변수의 이름을 바꾸면 다음 예제와 같이 종속성을 제거할 수 있음&#x20;

```
1. B = 3
N. B2 = B
2. A = B2 + 1
3. B = 7
```

* 새로운 변수 B2가 새로운 명령인 명령 N에서 B의 복사본으로 선언되었음&#x20;
* 2와 3 사이의 anti-dependency가 제거됨&#x20;
  * 그러나 수정으로 인해 새로운 종속성이 도입됨&#x20;
* 명령 2는 이제 명령 N에 실제로 종속되고 명령 1에 실제로 종속됨&#x20;
  * Flow 종속성으로서 이러한 새로운 종속성은 안전하게 제거할 수 없음&#x20;

#### Output dependency&#x20;

* WAW라고도 하는 출력 종속성&#x20;
* 명령어 순서가 변수의 최종 출력 값에 영향을 미칠 때 발생&#x20;

```
1. B = 3
2. A = B + 1
3. B = 7
```

* 명령 3과 1 사이에 출력 종속성이 있음&#x20;
* 이 예에서 명령의 순서를 변경하면 A의 최종값이 변경됨&#x20;
  * 이러한 명령을 병렬로 실행할 수 없음&#x20;
* Anti 의존성과 마찬가지로, Output 종속성은 name 종속성
  * 변수 이름 변경을 통해 제거?

```
1. B2 = 3
2. A = B2 + 1
3. B = 7
```

* 데이터 종속성에 일반적으로 사용되는 명명 규칙은 RAW, WAR, WAW

### Control Dependency&#x20;

* 명령 B는 A의 결과가 B의 실행 여부를 결정하는 경우 선행 명령 A에 대한 제어 종속성을 가짐&#x20;

```
S1.         if (a == b)
S2.             a = a + b
S3.         b = a + b
```

* S2는 S1에 대한 제어 종속성을 가짐&#x20;
* 그러나, S3는 S1에 의존하지 않음, S3는 항상 S1의 결과와 관계없이 실행되기 때문&#x20;



1. B는 A 이후에 실행될 수 있음&#x20;
2. A 실행 결과에 따라 B 실행 여부가 결정됨&#x20;

* 위의 1,2에 따라 두 문장 A와 B 사이에 제어 종속성이 있다고 볼 수 있음&#x20;
* 제어 종속성은 본질적으로 제어 흐름 그래프(CFG)의 역 그래프에서 dominance frontier입니다.&#x20;
  * 따라서 이를 구성하는 한 가지 방법은 CFG의 지배 후 경계를 구성한 다음 이를 역으로 제어 종속성 그래프를 얻는 것&#x20;
  *


