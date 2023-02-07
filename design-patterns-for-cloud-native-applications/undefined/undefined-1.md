---
description: '-'
---

# 비동기 메시징 패턴

* 비동기 통신 패턴&#x20;
  * 하나의 마이크로서비스가 다른 서비스에 메시지를 전송할 때 즉각적인 응답을 기대하지 않음&#x20;
  * 데이터를 전달받은 마이크로서비스&#x20;
    * 응답을 전혀 하지 않을 수도, 아니면 별도의 큐와 같은 다른 통신 채널을 통해 비동기 응답을 보낼 수도 있음&#x20;



* 비동기 메시징 패턴에서 일어나는 마이크로서비스 간 통신&#x20;
  * 메시지 브로커나 이벤트 브로커에 의해 이루어짐&#x20;
  * 브로커 : 데이터 소스 또는 생산자 역할을 맡는 마이크로서비스로부터 메시지를 받아서 소비자에게 전달함&#x20;
  * 소비자 : 메시지 브로커에게서 받고자 하는 메시지들만 전달받을 수 있음&#x20;



* 클라우드 네이티브 애플리케이션에서 브로커는 최소한의 비즈니스 로직만으로도 뛰어난 메시징 인프라스트럭처를 제공할 수 있음&#x20;
  * 마이크로서비스의 비즈니스 로직은 생산자와 소비자 각각에 구현하며 메시지 브로커 수준에서 비즈니스 로직을 구현할 필요는 없음&#x20;



* 메시징 인프라스트럭처로서 비즈니스 로직이 전혀 필요가 없기 때문에 브로커를 중앙화된 메시지 플랫폼으로 사용할 수 있음&#x20;
  * 비동기 메시징 패턴을 구현할 때마다 새로운 브로커 런타임을 구축할 필요가 없다는 뜻&#x20;

#### 단일 수신자 패턴&#x20;

* 하나의 마이크로서비스가 메시징 인프라스트럭처를 통해 단 하나의 마이크로서비스 혹은 시스템에만 메시지를 전달&#x20;
  * 전송하는 메시지 = 명령&#x20;
* 단일 소비자에게 메시지를 전달하고 이 소비자는 메시지를 통해 요청한 작업을 처리하기 때문
* ex) 온라인 쇼핑몰 시스템에서 주문을 처리하는 경우&#x20;
  * 비동기 메시지를 메시지 브로커 큐에 집어넣으면 주문 처리 서비스가 이 메시지를 전달받아서 필요한 작업을 처리할 것&#x20;
* 단일 생산자와 단일 소비자 사이에 정보 교환이 이루어짐&#x20;
  * \=> 점대점 비동기 메시징

#### 어떻게 동작?

* 메시지 브로커의 큐에 메시지를 만들어서 집어넣는 것으로 시작&#x20;
* 단일 소비자 서비스 또는 시스템은 큐에서 메시지를 가져와서 처리&#x20;
* 생산자 서비스의 관심 = 오직 메시지가 큐로 제대로 들어갔는지 여부&#x20;
  * 메시지가 언제 누구에게 전달되었는지 신경쓰지 않음&#x20;



*   생산자로부터 소비자에게 메시지를 전달할 때 메시지 큐를 사용하기 때문 => 메시지를 순서대로 전달할 것을 보장&#x20;

    * 메시지 브로커 = 브로커가 사용하는 통신 프로토콜의일부&#x20;
      * 최소 1회 전달과 같은 메시지 전달을 보장&#x20;


* 생산자와 소비자는 브로커가 제공하는 메시지 생성 또는 수신 후 응답 기능 등을 이용해 메시지가 전달되었음을 보장받을 수 있음&#x20;



* 메시지 브로커의 큐는 단일 소비자에게만 메시지가 전달된다는 것을 보장&#x20;
* 단일 수신자 패턴 = 메시지가 전달되는 것을 보장해야 하는 경우 자주 사용



* AMQP : 큐 기반 단일 소비자 메시징을 구현할 때 가장 널리 사용되는 프로토콜&#x20;



*   메시지 브로커로 AMQP를 사용하면 생산자나 소비자 애플리케이션을 서로 다른 프로그래밍 언어로 구현하고 단일 소비자 패턴을 적용할 수 있음&#x20;

    * RabbitMQ, 아파치 ActiveMQ, 아파치 ActiveMQ Artemis&#x20;
      * 자주 사용하는 AMQP 메시지 브로커 애플리케이션&#x20;


* Queue 기반 단일 수신자 패턴&#x20;
  * 종단 간 (end-to-end) 메시지 전달을 보장해야 할 때 많이 사용&#x20;
  * 메시지 브로커 => 클라우드 네이티브 애플리케이션의 신뢰성, 스케일러빌리티, 성능에 중요한 역할을 맡음&#x20;
  * 애플리케이션의 요구사항에 잘 맞는 좋은 브로커 기술을 선택하는 것이 정말 중요&#x20;

#### 관련 패턴들&#x20;

* 최대 한 번 전달, 최소 한 번 전달과 같은 다양한 패턴을 단일 수신자 패턴에 기반하여 구현&#x20;
* fire and forget과 같은 변형 패턴들 => 메시지 브로커 없이 구현하는 경우도 있음&#x20;



### 다중 수신자 패턴&#x20;

* 단일 소비자 기반 비동기 메시징&#x20;
  * 생산자가 메시지 브로커에 전달한 메시지를 오직 하나의 소비자만 사용하는 경우에만 쓸 수 있음&#x20;
* 특정 이벤트에 관심 있는 여러 소비자에게 같은 메시지를 보내고 싶다면?
  * 다중 수신자 or 발행자-구독자 패턴을 사용할 수 있음&#x20;
* 클라우드 네이티브 애플리케이션&#x20;
  * 마이크로서비스가 특정 이벤트가 발생할 경우 비즈니스 로직을 실행하도록 만들거나
  * 다른 한 개 이상의 마이크로서비스에 이벤트가 발생했다는 사실을 알리도록 만드는 경우가 많음&#x20;

#### 어떻게 동작?

* 다중 수신자 패턴에서 메시지는 하나 이상의 마이크로서비스 소비자에게 전달됨&#x20;
  * 비동기 메시지 전달을 지원하는 메시지 브로커, 이벤트 버스를 사용&#x20;
* 어떤 마이크로서비스가 특정 토픽에 대한 메시지를 발행해서 이벤트 버스로 전달하면
  * 하나 이상의 마이크로서비스가 해당 토픽에 대해 구독하는 방식&#x20;
  * 메시지는 해당 토픽을 구독하는 모든 서비스에 비동기로 전달&#x20;
*   이벤트 버스 : 메시지 생산, 구독과 같은 요청을 처리하고 구독자 서비스들에 메시지를 전달하는 책임을 맡음&#x20;

    * 단일 수신자 패턴만큼 엄격하지 않음&#x20;
    * 이벤트 버스 => 가능한 구독자들에게 메시지를 전달하기만 하면 됨&#x20;


* 구독자 서비스가 긴 시간 동안 동작하지 않아도 메시지를 전부 받도록 만들고 싶다면&#x20;
  * 이런 기능을 지원하는 이벤트 버스 솔루션을 사용해야 함&#x20;
  * 메시지를 오랫동안 보관하고 전송할 수 있는 이벤트 버스의 경우&#x20;
    * 모든 구독자가 메시지를 전부 다 받을 때까지 메시지를 보관해야 하기 때문에 더 많은 부하가 가해짐&#x20;

#### 어떻게 사용할 수 있나요

* 발행자-구독자 메시징을 지원하는 메시지 브로커를 사용&#x20;
  * ex) ActiveMQ, RabbitMQ, 마이크로소프트 애저 서비스 버스
    * 큐 기반 단일 수신자 패턴을 지원하는 일반적인 브로커들 => 대부분 토픽 기반 메시징도 지원&#x20;
* 아파치 카프카, NATS, 아마존 SNS, 마이크로소프트 애저 이벤트 그리드 => 이벤트 기반 다중 소비자 지원 모델들
  * 다중 수신자 패턴에 사용할 수 있는 이벤트 주도 메시징을 지원&#x20;



*   전송 기법의 경우 => 다중 수신자 패턴에선 생산자가 만든 모든 이벤트를 영구 스토어에 저장하고 관리하는 영구 전송을 지원&#x20;

    * 이벤트가 모든 구독자 서비스에 전달된다고 보장하지는 않음&#x20;
    * 대비를 하더라도 구독자에게 이벤트를 전달할 수 없는 상황이 발생할 수도 있기 때문&#x20;


* 몇몇 브로커의 경우 내구성 토픽와 같은 개념을 통해 메시지 전송을 보장하기도 함
  * 내구성 토픽 - 내구성 소비자들이 전달받은 메시지들을 모두 자체적으로 복사해서 가지고 있기 때문&#x20;
* 브로커는 모든 내구성 소비자에 대한 각 메시지별 인스턴스를 논리적으로 영구 보관함&#x20;



*   여러 소비자에게 동일한 메시지를 전달할 때 최소 한 번 전달과 같은 요구사항을 만족시키고자 할 경우&#x20;

    * 다중 소비자 패턴을 사용하기보단 단일 수신자 패턴처럼 각 소비자별로 큐를 따로 두고 이 큐에 메시지를 전달하는 방식을 더 많이 사용&#x20;



#### 고려해야 할 사항들&#x20;

* 이벤트 버스 컴포넌트가 여러 마이크로서비스가 사용하는 중앙화된 런타임 컴포넌트로 동작&#x20;
* 다중 수신자 패턴에선 여러 소비자가 하나의 브로커 인스턴스를 공유하는 것&#x20;
  * 메시지 속성 등을 통해 메시지를 라우팅하는 등 브로커에 비즈니스 로직을 구현하는 것보단&#x20;
  * 브로커가 최대한 비즈니스 로직과 독립적이게 유지하는 것이 무엇보다 중요&#x20;
* 브로커는 메시징 인프라스트럭처로만 사용하는 것이 가장 좋음&#x20;
  * 소비자가 메시지를 소비하는 방식을 더 자유롭게, 더 쉽게 구현하고 제어할 수 있음&#x20;



* 특별한 메시지 기법(내구성 토픽, 내구성 구독)은 반드시 그런 기능이 필요할 때만 사용해야 함&#x20;
*   이벤트 버스 수준에서 구독과 메시지 전달에 대한 더 세세한 제어를 제공할 수 있음&#x20;

    * 계층적 주체, 라우팅 규칙 등의 개념도 지원할 수 있기 때문&#x20;


* 이벤트 주도 아키텍처 = 다중 수신자 패턴을 핵심 기능으로 사용됨&#x20;

### 비동기 요청-응답 패턴&#x20;

* 생산자가 브로커를 통해 소비자에게 메시지를 전송하고 다른 브로커 채널을 통해 응답을 받을 수도 있음&#x20;
* 비동기 요청-응답 패턴 => 단일 수신자 패턴과 같은 메시징 모델을 사용&#x20;
* 생산자 서비스 = 메시지를 발행해서 메시지 브로커의 큐에 넣고&#x20;
  * 소비자는 큐에서 메시지를 꺼내서 사용하는 것&#x20;



*   전송하는 메시지 메타데이터에 메시지를 전송한 측에 응답을 보내달라는 요청사항과 어떻게 응답을 보낼 수 있는지를 지정할 수 있음&#x20;

    * 소비자는 메시지를 생성한 생산자 측에 다른 메시지 브로커 채널로 응답 메시지를 생성해서 전달할 수 있음&#x20;



#### 고려해야 할 사항들&#x20;

* 비동기 요청-응답 패턴 : 두 개의 단방향 메시지를 조합하는 것&#x20;
* 요청과 응답 모두에 메시지 큐를 사용하기 때문에 성능이 떨어지고, 관련 메시지 처리에 추가적인 부하가 발생하기 때문&#x20;
  *










































