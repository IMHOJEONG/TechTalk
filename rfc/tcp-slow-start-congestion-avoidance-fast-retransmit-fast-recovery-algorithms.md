---
description: '- https://www.rfc-editor.org/rfc/rfc2001'
---

# TCP Slow Start, Congestion Avoidance, Fast Retransmit, Fast Recovery Algorithms

#### TCP Slow Start, Congestion Avoidance, Fast Retransmit, Fast Recovery Algorithms

#### 1. Slow Start

* 이전 TCP는 receiver가 알려주는 window size까지 여러 세그먼트를 네트워크에 주입해 sender와 연결을 시작함&#x20;
* 두 호스트가 동일한 LAN에 있을 때는 괜찮지만,&#x20;
* Sender와 Receiver 사이에 라우터와 느린 링크가 있으면 문제가 발생할 수 있음&#x20;
* 일부 중간 라우터는 패킷을 대기열에 넣어야하며 해당 라우터의 공간이 부족할 수 있음&#x20;
  * 이 순진한 접근 방식이 TCP의 처리량을 줄일 수 있음&#x20;



* 이를 피하기 위한 알고리즘을 느린 시작이라고 함&#x20;
  * 새로운 패킷이 네트워크에 주입되어야 하는 속도가 다른 쪽 끝에서 응답이 반환되는 속도임을 관찰하여 작동함&#x20;



* Slow Start는 sender의 TCP에 cwnd라고 하는 혼잡 창이라는 또 다른 창을 추가&#x20;
* 다른 네트워크에 있는 호스트와 새로운 연결이 설정되면 Congestion Window는 하나의 segment(즉, 다른 쪽 끝에서 발표한 세그먼트 크기 또는 기본 값, 일반적으로 536 또는 512)로 초기화됨&#x20;
* ACK를 수신할 때마다 Congestion Window는 한 세그먼트 씩 증가함&#x20;
* sender는 Congestion Window와 알려진 Window의 최소값까지 전송할 수 있음&#x20;
* Congestion Window는 Sender가 부과하는 흐름 제어인 반면&#x20;
  * Advertised Window는 Receiver가 부과하는 흐름 제어&#x20;



* Congestion Window는 감지된  네트워크 정체에 대한 Sender의 평가를 기반으로 함&#x20;
  * Advertised Window는 이 연결을 위해 Receiver에서 사용 가능한 Buffer 공간의 양과 관련됨&#x20;



* Sender는 하나의 세그먼트를 전송하고 ACK를 기다리는 것으로 시작함&#x20;
  * 해당 ACK가 수신되면 Congestion Window가 1에서 2로 증가하고 2개의 세그먼트를 보낼 수 있음&#x20;
*   이 두 세그먼트가 각각 확인되면 Congestion Window가 4개로 늘어남&#x20;

    * 이는 수신자가 ACK를 지연시킬 수 있기 때문에 정확히 기하급수적이지는 않지만 기하급수적으로 증가&#x20;
    * 일반적으로 수신하는 두 세그먼트마다 하나의 ACK를 보냄&#x20;


* 어느 시점에 Internet 용량에 도달할 수 있으며, 중간 라우터는 패킷을 버리기 시작함&#x20;
  * 이는 발신자에게 Congestion Window가 너무 커졌다는 것을 알려줌&#x20;
* 초기 구현에선, 다른 쪽 끝이 다른 네트워크에 있는 경우에만 Slow Start를 수행&#x20;
  * 현재 구현은 항상 Slow Start를 수행&#x20;

#### 2. Congestion Avoidance

* 데이터가 큰 파이프(고속 LAN)에 도착하고 더 작은 파이프(느린 WAN)로 전송될 때 정체가 발생할 수 있음&#x20;
* 출력 용량이 입력의 합보다 적은 라우터에 여러 입력 스트림이 도착하는 경우에도 혼잡이 발생할 수 있음&#x20;
* 혼잡 회피는 손실된 패킷을 처리하는 방법&#x20;
* 이 알고리즘의 가정은 손상으로 인한 패킷 손실이 매우 적기 때문에(1% 미만), 패킷 손실은 소스와 목적지 사이의 네트워크 어딘가에서 혼잡 신호를 보낸다는 것&#x20;



* 패킷 손실에는 시간 초과 발생과 중복 ACK 수신의 두 가지 징후가 있음&#x20;



* 혼잡 회피, 느린 시작은 서로 다른 목표를 가진 독립적인 알고리즘&#x20;
  * 그러나 Congestion이 발생하면 TCP는 네트워크의 패킷 전송 속도를 늦춘 다음 slow start를 호출하여 상황을 다시 시작해야 함&#x20;
* 실제로 혼잡 회피와 Slow start는 함께 구현됨&#x20;



*   혼잡 방지 및 Slow Start를 위해서는 각 연결에 대해 Congestion Window => cwnd, a slow start threshold size => ssthresh의 두 가지 변수를 유지 관리해야 함&#x20;



1. 주어진 연결에 대한 초기화는 cwnd를 하나의 세그먼트로 설정하고, ssthresh를 65535 바이트로 설정
2. TCP 출력 루틴은 최소 cwd 및 receiver의 advertised Window보다 더 많이 보내지 않음&#x20;
3. Congestion이 발생하면(시간 초과 또는 중복 ACK 수신으로 표시됨) 현재 창 크기의 절반(최소 cwnd 및 Receiver의 Advertised Window, 그러나 최소 2개의 세그먼트)이 ssthresh에 저장됨&#x20;
   1. 또한, Congestion이 timeout으로 표시되면 cwnd는 하나의 세그먼트(즉, Slow Start)로 설정됨&#x20;
4. 상대방이 새 데이터를 확인하면 cwnd를 높이지만, 증가하는 방식은 TCP가 Slow Start를 수행하는지 또는 혼잡 회피를 수행하는지에 따라 다름&#x20;



* cwnd가 ssthresh보다 작거나 같으면 TCP는 Slow Start 상태
  * 그렇지 않으면 TCP가 Congestion Avoidance를 수행함&#x20;
* Slow Start는 TCP가 정체가 발생했을 때의 절반에 도달할 때까지 계속되고, Congestion Avoidance가 이어짐
  * Slow Start는 cwnd가 하나의 세그먼트에서 시작하고 ACK가 수신될 때마다 하나의 세그먼트씩 증가
* 이렇게 하면 Window가 기하급수적으로 열림&#x20;
  * 한 세그먼트를 보낸 다음 두 개, 그 다음에는 네 개 등을 보냄&#x20;
* Congestion Avoidance는 cwnd가 ACK를 수신할 때마다 (segsize\*segsize/cwnd)만큼 증가하도록 지시함&#x20;
  * segsize : 세그먼트 크기&#x20;
  * cwnd : 바이트 단위로 유지됨&#x20;



* Slow start의  기하급수적  성장과 비교하여 cwnd의 선형 증가
  * cwnd의 증가는 왕복 시간 당 최대 하나의 세그먼트여야 하지만(해당 RTT에서 얼마나 많은 ACK를 수신했는지에 관계없이), Slow Start는 왕복 시간에 수신된 ACK의 수만큼 cwnd를 증가시킴&#x20;

#### 3. Fast Retransmit

* 순서가 잘못된 세그먼트가 수신되면, TCP가 즉각적인 승인(중복 ACK)을 생성할 수 있음을 인식&#x20;
* 이 중복 ACK는 지연되어서는 안 됨&#x20;
* 이 중복 ACK의 목적은 segment가 잘못된 순서로 수신되었음을 상대방에게 알리고 예상되는 시퀀스 번호를 알려주는 것&#x20;
* TCP는 중복 ACK가 손실된 세그먼트 때문인지 아니면 단순히 세그먼트의 재정렬 때문인지 알지 못함&#x20;
  * 소수의 중복 ACK가 수신될 때까지 기다림&#x20;
* Segment의 재정렬만 있는 경우, 재정렬된 세그먼트가 처리되기 전에 하나 또는 두 개의 중복 ACK만 있을 것이며&#x20;
  * 그러면 새로운 ACK가 생성될 것이라고 가정&#x20;



* 3개 이상의 중복 ACK가 연속으로 수신되면 세그먼트가 손실되었음을 나타내는 강력한 표시&#x20;
  * 그런 다음 TCP는 재전송 타이머가 만료될 때까지 기다리지 않고 누락된 세그먼트로 보이는 항목을 재전송함&#x20;

#### 4. Fast Recovery

* Fast Retransmit이 누락된 세그먼트로 보이는 것을 보낸 후 Congestion Avoidance가 수행되지만 Slow Start는 수행되지 않음&#x20;
  * 이것이 Fast Recovery Algorithm
* 특히 큰 Window의 경우 중간 정도의 혼잡 상황에서 높은 처리량을 허용하는 개선 사항&#x20;



* 이 경우 Slow Start를 수행하지 않는 이유는 중복 ACK를 수신하면 TCP는 단순히 패킷이 손실된 것 이상을 알 수 있음&#x20;
* Receiver는 다른 Segment가 수신될 때만 중복 ACK를 생성할 수 있음&#x20;
  * 해당 Segment는 네트워크를 떠나 수신자의 버퍼에 있음&#x20;
* 즉, 두 끝 사이에는 여전히 데이터 흐름이 있으며 TCP는 Slow Start로 들어가 흐름을 갑자기 줄이는 것을 원하지 않음&#x20;



* Fast Retransmit, Fast Recovery 알고리즘



1. 행의 세 번째 중복 ACK가 수신되면 ssthresh를 현재 Congestion Window의 절반인 cwnd로 설정(두 세그먼트 이상)
   1. 누락된 세그먼트를 재전송, cwnd를 ssthresh에 세그먼트 크기의 3배를 더한 값으로 설정&#x20;
   2. 이렇게 하면 네트워크를 떠났고, 다른쪽 끝이 캐시한 세그먼트 수만큼 정체 기간이 늘어남
2. 다른 중복 ACK가 도착할 때마다 Segment 크기만큼 cwnd를 증가시킴&#x20;
   1. 이렇게 하면 네트워크를 떠난 추가 세그먼트에 대한 혼잡 기간이 들어남&#x20;
   2. cwnd의 새 값에서 허용하는 경우 패킷을 전송&#x20;
3. 새로운 데이터를 확인하는 다음 ACK가 도착하면 cwnd를 ssthresh로 설정함&#x20;
   1. 이 ACK는 재전송 후 한 왕복 시간인 1단계의 재전송에 대한 확인이어야 함&#x20;
   2. 또한, 이 ACK는 손실된 패킷과 첫 번째 중복 ACK 수신 사이에 전송된 모든 중간 세그먼트를 승인해야 함&#x20;
   3. 이 단계는 TCP가 패킷이 손실되었을 때의 속도의 절반으로 떨어지기 때문에 혼잡 회피















