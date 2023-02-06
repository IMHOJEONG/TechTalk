---
description: '-'
---

# HLS & Adaptive

#### HLS

* HTTP 라이브 스트리밍&#x20;
* 가장 널리 사용되는 비디오 스트리밍 프로토콜&#x20;
* 주문형 스트리밍이자 동시에 라이브 스트리밍입니다&#x20;
* 비디오 파일을 다운로드할 수 있는 HTTP 파일 조각으로 나누고 HTTP 프로토콜을 이용하여 전송합니다&#x20;
* 장점 중 하나 : 모든 인터넷 연결 장치가 HTTP를 지원하기 때문에 전용 서버가 필요한 스트리밍 프로토콜보다 간단하게 실행할 수 있다는 것&#x20;
* 또 다른 장점&#x20;
  * HLS 스트리밍 - 재생에 지장을 주지 않고 네트워크 상태에 따라 비디오 품질을 높이거나 낮출 수 있다는 것&#x20;
  * 사용자가 비디오를 보는 중에 품질이 나빠지거나 좋아질 수 있음&#x20;
  * 적응 비트 전송률 스트리밍&#x20;
  * 이 기능이 없으면 네트워크가 느려진 경우 비디오 재생이 완전히 멈출 수 있음&#x20;

#### 스트리밍&#x20;

* 인터넷을 통해 사용자에게 비디오 및 오디오 미디어를 전달하는 방법&#x20;
* 미디어 파일을 한 번에 모두 보내는 대신, 한 번에 조금씩 지속적으로 사용자 장치에 보냄&#x20;
* 원본 미디어 파일은 멀리 떨어진 곳에 저장되어 있거나 라이브 스트리밍의 경우 원격 카메라, 마이크를 이용하여 실시간으로 제작됨&#x20;
  * 따라서, 사용자의 장치가 먼저 모든 파일을 다운로드하지 않고 비디오 또는 오디오를 재생할 수 있음&#x20;

#### HTTP

* 네트워크에 연결된 장치 사이에 정보를 전송하기 위한 애플리케이션 계층 프로토콜&#x20;
* HTTP를 통한 스트리밍의 경우 표준 요청, 응답 패턴이 적용되지 않음&#x20;
  * 클라이언트와 서버 사이의 연결은 스트리밍 기간 동안 열려있고 서버는 비디오 데이터를 클라이언트에 보내기 때문에&#x20;
    * 클라이언트는 비디오 데이터 세그먼트마다 요청하지 않아도 됨&#x20;



[https://d2.naver.com/helloworld/7122](https://d2.naver.com/helloworld/7122)













* HLS
  * [https://developer.apple.com/streaming/](https://developer.apple.com/streaming/)
  * [https://www.cloudflare.com/ko-kr/learning/video/what-is-http-live-streaming/](https://www.cloudflare.com/ko-kr/learning/video/what-is-http-live-streaming/)
  * [https://www.netmanias.com/ko/?m=view\&id=blog\&no=5867](https://www.netmanias.com/ko/?m=view\&id=blog\&no=5867)





* [https://www.netmanias.com/ko/?m=view\&id=blog\&no=5867](https://www.netmanias.com/ko/?m=view\&id=blog\&no=5867)
